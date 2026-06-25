---
name: playwright-test-reviewer
description: Code review for Playwright test automation code following official best practices. Use when the user requests review, analysis, or improvements to Playwright test files (.spec.ts, .test.ts, .spec.js), Page Object Models, test utilities, fixtures, or test configuration. Triggers include: "review my Playwright tests", "check this against best practices", "improve my test code", "analyze this spec file", or when user shares Playwright test files and asks for feedback or improvements.
license: MIT
---

# Playwright Code Review

Review Playwright test automation code against official best practices from https://playwright.dev/docs/best-practices.

## Core Principles

### Auto-Waiting Over Manual Waits

Playwright auto-waits for actionability before performing actions. Never use arbitrary timeouts.

**❌ Bad:**
```typescript
await page.waitForTimeout(1000);
await element.click();
```

**✅ Good:**
```typescript
await element.click(); // Auto-waits for element to be actionable
```

**❌ Bad:**
```typescript
await page.waitForLoadState('networkidle');
await page.waitForTimeout(2000);
```

**✅ Good:**
```typescript
// Wait for specific element or response
await page.locator('h1').waitFor();
await page.waitForResponse(resp => resp.url().includes('/api/data'));
```

### Web-First Assertions

Use assertions that auto-retry until condition is met.

**❌ Bad:**
```typescript
await page.waitForTimeout(500);
const count = await page.locator('li').count();
expect(count).toBe(5);
```

**✅ Good:**
```typescript
await expect(page.locator('li')).toHaveCount(5);
```

**Common web-first assertions:**
- `toBeVisible()`, `toBeHidden()`
- `toHaveText()`, `toContainText()`
- `toHaveCount()`
- `toHaveAttribute()`, `toHaveClass()`
- `toBeEnabled()`, `toBeDisabled()`

### Prefer User-Facing Locators

Use accessible locators that reflect how users interact with the page.

**Priority order:**
1. `page.getByRole()` - Queries by ARIA role, name, state
2. `page.getByLabel()` - Form fields by label text
3. `page.getByPlaceholder()` - Inputs by placeholder
4. `page.getByText()` - Non-interactive text
5. `page.getByTestId()` - Explicit test IDs (last resort)

**❌ Bad:**
```typescript
page.locator('.btn-primary')
page.locator('#submit-button')
page.locator('button').nth(2)
```

**✅ Good:**
```typescript
page.getByRole('button', { name: 'Submit' })
page.getByLabel('Email address')
page.getByPlaceholder('Search...')
```

### Avoid `.all()` with Loops

Prefer locator filtering and chaining over materializing elements.

**❌ Bad:**
```typescript
const rows = await page.locator('tr').all();
for (const row of rows) {
  const cells = await row.locator('td').all();
  if ((await cells[1].textContent()) === 'Admin') {
    await cells[2].click();
  }
}
```

**✅ Good:**
```typescript
await page.locator('tr')
  .filter({ hasText: 'Admin' })
  .locator('td').nth(2)
  .click();
```

**When iteration is needed:**
```typescript
const count = await page.locator('tr').count();
for (let i = 0; i < count; i++) {
  const row = page.locator('tr').nth(i);
  // Work with row
}
```

## Page Object Model (POM)

### Use Fixtures Instead of beforeEach

**❌ Bad:**
```typescript
let page: Page;
let loginPage: LoginPage;

test.beforeEach(async ({ browser }) => {
  page = await browser.newPage();
  loginPage = new LoginPage(page);
});
```

**✅ Good:**
```typescript
// fixtures/auth.ts
import { test as base } from '@playwright/test';

type AuthFixtures = {
  authenticatedPage: Page;
};

export const test = base.extend<AuthFixtures>({
  authenticatedPage: async ({ page }, use) => {
    await page.goto('/login');
    await page.getByLabel('Email').fill('user@example.com');
    await page.getByRole('button', { name: 'Login' }).click();
    await use(page);
  },
});

// test.spec.ts
import { test } from './fixtures/auth';

test('dashboard loads', async ({ authenticatedPage }) => {
  await expect(authenticatedPage.locator('h1')).toHaveText('Dashboard');
});
```

### Locator Patterns

**Keep locators private, expose methods:**
```typescript
export class ProductPage {
  readonly page: Page;
  private readonly addToCartButton: Locator;
  
  constructor(page: Page) {
    this.page = page;
    this.addToCartButton = page.getByRole('button', { name: 'Add to cart' });
  }
  
  async addToCart() {
    await this.addToCartButton.click();
  }
  
  async verifyProductName(expected: string) {
    await expect(this.page.getByRole('heading', { level: 1 }))
      .toHaveText(expected);
  }
}
```

**Avoid methods that only wrap single actions:**
```typescript
// ❌ Bad - unnecessary wrapper
async clickSubmit() {
  await this.submitButton.click();
}

// ✅ Good - expose locator or add value
readonly submitButton = page.getByRole('button', { name: 'Submit' });

// OR add business logic
async submitForm(data: FormData) {
  await this.fillForm(data);
  await this.submitButton.click();
  await this.waitForSuccessMessage();
}
```

## Test Organization

### Use Descriptive Test Names

**❌ Bad:**
```typescript
test('test1', async ({ page }) => { ... });
test('should work', async ({ page }) => { ... });
```

**✅ Good:**
```typescript
test('should display validation error when email is invalid', async ({ page }) => { ... });
test('should add item to cart and update counter', async ({ page }) => { ... });
```

### Group Related Tests

```typescript
test.describe('User Authentication', () => {
  test('should login with valid credentials', async ({ page }) => { ... });
  test('should show error with invalid password', async ({ page }) => { ... });
  test('should logout successfully', async ({ page }) => { ... });
});
```

### Use test.step() for Clarity

```typescript
test('checkout flow', async ({ page }) => {
  await test.step('Add items to cart', async () => {
    await page.goto('/products');
    await page.getByRole('button', { name: 'Add to cart' }).first().click();
  });
  
  await test.step('Proceed to checkout', async () => {
    await page.getByRole('link', { name: 'Cart' }).click();
    await page.getByRole('button', { name: 'Checkout' }).click();
  });
  
  await test.step('Complete payment', async () => {
    await page.getByLabel('Card number').fill('4242424242424242');
    await page.getByRole('button', { name: 'Pay' }).click();
  });
});
```

### Use Tags for Test Filtering

```typescript
test('critical user flow @smoke @critical', async ({ page }) => { ... });
test('advanced settings @slow', async ({ page }) => { ... });

// Run: npx playwright test --grep @smoke
// Exclude: npx playwright test --grep-invert @slow
```

## Anti-Patterns to Avoid

### Don't Use test.describe.parallel() Without Isolation

**❌ Bad - shared state causes race conditions:**
```typescript
test.describe.parallel('Search tests', () => {
  let searchPage: SearchPage;
  
  test.beforeEach(async ({ page }) => {
    searchPage = new SearchPage(page);
    await searchPage.navigate();
  });
  
  test('search A', async () => {
    await searchPage.search('query A'); // Races with test B
  });
  
  test('search B', async () => {
    await searchPage.search('query B'); // Races with test A
  });
});
```

**✅ Good - each test gets fresh state:**
```typescript
test.describe('Search tests', () => {
  test('search A', async ({ page }) => {
    const searchPage = new SearchPage(page);
    await searchPage.navigate();
    await searchPage.search('query A');
  });
  
  test('search B', async ({ page }) => {
    const searchPage = new SearchPage(page);
    await searchPage.navigate();
    await searchPage.search('query B');
  });
});
```

### Don't Use Non-Null Assertions

**❌ Bad:**
```typescript
test.beforeEach(async ({ page, baseURL }) => {
  await page.goto(baseURL!); // Bypasses TypeScript safety
});
```

**✅ Good:**
```typescript
test.beforeEach(async ({ page, baseURL }) => {
  if (!baseURL) {
    throw new Error('baseURL is required');
  }
  await page.goto(baseURL);
});

// Or ensure in config:
export default defineConfig({
  use: {
    baseURL: process.env.BASE_URL || 'http://localhost:3000',
  },
});
```

### Don't Use test.fail() for Known Bugs

**❌ Bad:**
```typescript
test('sorting feature', async ({ page }) => {
  test.fail(); // CI passes when test fails, hiding regressions
  await page.getByRole('button', { name: 'Sort' }).click();
});
```

**✅ Good:**
```typescript
test.skip('sorting feature', async ({ page }) => {
  // Skipped: Known bug JIRA-123
  await page.getByRole('button', { name: 'Sort' }).click();
});

// Or conditional:
test('sorting feature', async ({ page }) => {
  test.fixme(true, 'Known bug JIRA-123');
  await page.getByRole('button', { name: 'Sort' }).click();
});
```

### Avoid Hardcoded Waits

**Replace all `waitForTimeout()` with specific conditions:**

```typescript
// ❌ Bad
await input.fill('search term');
await page.waitForTimeout(1500); // Arbitrary delay

// ✅ Good
await input.fill('search term');
await page.waitForResponse(resp => resp.url().includes('/api/search'));
```

### Don't Silently Catch Errors

**❌ Bad:**
```typescript
await page.waitForLoadState('networkidle').catch(() => {});
```

**✅ Good:**
```typescript
try {
  await page.waitForURL(/.*dashboard/);
} catch (error) {
  logger.error('Navigation failed', error);
  throw error;
}
```

## Data-Driven Tests

**Use loops for similar test cases:**

```typescript
const roles = [
  { display: 'Admin', value: 'admin' },
  { display: 'Editor', value: 'editor' },
  { display: 'Viewer', value: 'viewer' },
];

for (const { display, value } of roles) {
  test(`should filter by ${display} role`, async ({ page }) => {
    await page.getByRole('combobox').selectOption(value);
    await expect(page.locator('tr').filter({ hasText: display }))
      .toHaveCount(1, { timeout: 5000 });
  });
}
```

## Configuration Best Practices

### Use Environment Variables

```typescript
// playwright.config.ts
export default defineConfig({
  use: {
    baseURL: process.env.BASE_URL || 'http://localhost:3000',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
  },
  retries: process.env.CI ? 2 : 0,
});
```

### Extract Magic Numbers

**❌ Bad:**
```typescript
await expect(page.locator('tr')).toHaveCount(10, { timeout: 5000 });
await page.waitForTimeout(2000);
```

**✅ Good:**
```typescript
class ProductPage {
  private readonly DEFAULT_TIMEOUT = 5000;
  private readonly ITEMS_PER_PAGE = 10;
  
  async verifyPageItems() {
    await expect(this.page.locator('tr'))
      .toHaveCount(this.ITEMS_PER_PAGE, { timeout: this.DEFAULT_TIMEOUT });
  }
}
```

## Common Review Checklist

When reviewing Playwright code, check for:

- ✅ No `waitForTimeout()` - replaced with specific waits
- ✅ No `waitForLoadState('networkidle')` - use element/response waits
- ✅ Using `getByRole()` / `getByLabel()` over CSS selectors
- ✅ Web-first assertions (`toBeVisible()`, `toHaveText()`)
- ✅ Fixtures instead of `beforeEach` with variables
- ✅ No non-null assertions (`!`)
- ✅ `test.skip()` instead of `test.fail()` for known bugs
- ✅ No `.all()` with manual loops - use `.filter()` / `.nth()`
- ✅ Descriptive test names
- ✅ Test tags for filtering (@smoke, @slow, etc.)
- ✅ No hardcoded values - extract to constants
- ✅ Error handling instead of silent catches
- ✅ baseURL validated properly
- ✅ Page Object methods add value, not just wrapper clicks

## Performance Optimization

### Parallelize When Possible

```typescript
// playwright.config.ts
export default defineConfig({
  fullyParallel: true, // Run files in parallel
  workers: process.env.CI ? 2 : undefined, // Limit workers in CI
});
```

### Reuse Authentication State

```typescript
// global-setup.ts
import { test as setup } from '@playwright/test';

setup('authenticate', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill('user@example.com');
  await page.getByRole('button', { name: 'Login' }).click();
  await page.context().storageState({ path: 'auth.json' });
});

// playwright.config.ts
export default defineConfig({
  use: {
    storageState: 'auth.json',
  },
});
```

## Error Messages and Debugging

### Add Custom Error Messages

```typescript
await expect(page.locator('h1')).toHaveText('Dashboard', {
  timeout: 10000,
});
// Default error: "Locator expected to have text 'Dashboard'"

await expect(page.locator('h1'), 'Dashboard header should be visible')
  .toHaveText('Dashboard', { timeout: 10000 });
// Better error: "Dashboard header should be visible: Locator expected..."
```

### Use Soft Assertions for Multiple Checks

```typescript
test('validate form fields', async ({ page }) => {
  await expect.soft(page.getByLabel('Name')).toBeVisible();
  await expect.soft(page.getByLabel('Email')).toBeVisible();
  await expect.soft(page.getByLabel('Phone')).toBeVisible();
  // All assertions run even if one fails
});
```

## Review Response Format

When providing code review feedback, structure responses as:

1. **Critical Issues** - Must fix (breaks tests, bad practices)
2. **Performance Issues** - Slow tests, unnecessary waits
3. **Maintainability Issues** - Hard to read/maintain code
4. **Suggestions** - Nice-to-have improvements
5. **Refactored Example** - Show improved version

For each issue, provide:
- ❌ Current problematic code
- ✅ Improved alternative
- Brief explanation of why it's better
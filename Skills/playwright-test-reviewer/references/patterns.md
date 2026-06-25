# Playwright Patterns Reference

Detailed good/bad patterns for each review checklist item. Organized by category.

## Table of Contents

- [Auto-Waiting Over Manual Waits](#auto-waiting-over-manual-waits)
- [Web-First Assertions](#web-first-assertions)
- [User-Facing Locators](#user-facing-locators)
- [Avoid .all() With Loops](#avoid-all-with-loops)
- [Page Object Model](#page-object-model)
- [Fixtures Over beforeEach](#fixtures-over-beforeeach)
- [Test Organization](#test-organization)
- [Anti-Patterns](#anti-patterns)
- [Data-Driven Tests](#data-driven-tests)
- [Configuration](#configuration)
- [Error Messages and Debugging](#error-messages-and-debugging)
- [Performance Optimization](#performance-optimization)

---

## Auto-Waiting Over Manual Waits

Playwright auto-waits for actionability before performing actions. Arbitrary timeouts are flaky and slow.

❌ Bad:
```typescript
await page.waitForTimeout(1000);
await element.click();
```

✅ Good:
```typescript
await element.click(); // Auto-waits for element to be actionable
```

❌ Bad:
```typescript
await page.waitForLoadState('networkidle');
await page.waitForTimeout(2000);
```

✅ Good — wait for a specific signal:
```typescript
await page.locator('h1').waitFor();
await page.waitForResponse(resp => resp.url().includes('/api/data'));
```

---

## Web-First Assertions

Use assertions that auto-retry until the condition is met instead of snapshot checks.

❌ Bad:
```typescript
await page.waitForTimeout(500);
const count = await page.locator('li').count();
expect(count).toBe(5);
```

✅ Good:
```typescript
await expect(page.locator('li')).toHaveCount(5);
```

Common web-first assertions:
- `toBeVisible()`, `toBeHidden()`
- `toHaveText()`, `toContainText()`
- `toHaveCount()`
- `toHaveAttribute()`, `toHaveClass()`
- `toBeEnabled()`, `toBeDisabled()`

---

## User-Facing Locators

Use accessible locators that reflect how users interact with the page.

❌ Bad:
```typescript
page.locator('.btn-primary')
page.locator('#submit-button')
page.locator('button').nth(2)
```

✅ Good:
```typescript
page.getByRole('button', { name: 'Submit' })
page.getByLabel('Email address')
page.getByPlaceholder('Search...')
```

---

## Avoid .all() With Loops

Prefer locator filtering and chaining over materializing elements into arrays.

❌ Bad:
```typescript
const rows = await page.locator('tr').all();
for (const row of rows) {
  const cells = await row.locator('td').all();
  if ((await cells[1].textContent()) === 'Admin') {
    await cells[2].click();
  }
}
```

✅ Good:
```typescript
await page.locator('tr')
  .filter({ hasText: 'Admin' })
  .locator('td').nth(2)
  .click();
```

When iteration is genuinely needed, use count-based loops:
```typescript
const count = await page.locator('tr').count();
for (let i = 0; i < count; i++) {
  const row = page.locator('tr').nth(i);
  // Work with row
}
```

---

## Page Object Model

Keep locators private, expose meaningful methods:

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

Avoid thin wrappers that add no value:

❌ Bad — unnecessary wrapper:
```typescript
async clickSubmit() {
  await this.submitButton.click();
}
```

✅ Good — expose locator directly or add business logic:
```typescript
readonly submitButton = page.getByRole('button', { name: 'Submit' });

// OR add real value
async submitForm(data: FormData) {
  await this.fillForm(data);
  await this.submitButton.click();
  await this.waitForSuccessMessage();
}
```

---

## Fixtures Over beforeEach

❌ Bad — mutable shared state:
```typescript
let page: Page;
let loginPage: LoginPage;

test.beforeEach(async ({ browser }) => {
  page = await browser.newPage();
  loginPage = new LoginPage(page);
});
```

✅ Good — custom fixture:
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

---

## Test Organization

### Descriptive Names

❌ Bad:
```typescript
test('test1', async ({ page }) => { ... });
test('should work', async ({ page }) => { ... });
```

✅ Good:
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

### Use Tags for Filtering

```typescript
test('critical user flow @smoke @critical', async ({ page }) => { ... });
test('advanced settings @slow', async ({ page }) => { ... });

// Run:     npx playwright test --grep @smoke
// Exclude: npx playwright test --grep-invert @slow
```

---

## Anti-Patterns

### Parallel Tests With Shared State

❌ Bad — race condition:
```typescript
test.describe.parallel('Search tests', () => {
  let searchPage: SearchPage;

  test.beforeEach(async ({ page }) => {
    searchPage = new SearchPage(page);
    await searchPage.navigate();
  });

  test('search A', async () => {
    await searchPage.search('query A');
  });

  test('search B', async () => {
    await searchPage.search('query B');
  });
});
```

✅ Good — fresh state per test:
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

### Non-Null Assertions

❌ Bad:
```typescript
test.beforeEach(async ({ page, baseURL }) => {
  await page.goto(baseURL!);
});
```

✅ Good:
```typescript
test.beforeEach(async ({ page, baseURL }) => {
  if (!baseURL) {
    throw new Error('baseURL is required — set it in playwright.config.ts');
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

### test.fail() for Known Bugs

❌ Bad — CI passes when test fails, hiding regressions:
```typescript
test('sorting feature', async ({ page }) => {
  test.fail();
  await page.getByRole('button', { name: 'Sort' }).click();
});
```

✅ Good:
```typescript
test('sorting feature', async ({ page }) => {
  test.fixme(true, 'Known bug JIRA-123 — sorting returns stale data');
  await page.getByRole('button', { name: 'Sort' }).click();
});
```

### Hardcoded Waits

❌ Bad:
```typescript
await input.fill('search term');
await page.waitForTimeout(1500);
```

✅ Good:
```typescript
await input.fill('search term');
await page.waitForResponse(resp => resp.url().includes('/api/search'));
```

### Silent Error Swallowing

❌ Bad:
```typescript
await page.waitForLoadState('networkidle').catch(() => {});
```

✅ Good:
```typescript
try {
  await page.waitForURL(/.*dashboard/);
} catch (error) {
  logger.error('Navigation failed', error);
  throw error;
}
```

---

## Data-Driven Tests

Use loops for parameterized test cases:

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

---

## Configuration

### Environment Variables

```typescript
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

❌ Bad:
```typescript
await expect(page.locator('tr')).toHaveCount(10, { timeout: 5000 });
```

✅ Good:
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

---

## Error Messages and Debugging

### Custom Error Messages

```typescript
// Default error: "Locator expected to have text 'Dashboard'"
await expect(page.locator('h1')).toHaveText('Dashboard');

// Better: "Dashboard header should be visible after login: Locator expected..."
await expect(page.locator('h1'), 'Dashboard header should be visible after login')
  .toHaveText('Dashboard', { timeout: 10000 });
```

### Soft Assertions for Multiple Checks

```typescript
test('validate form fields', async ({ page }) => {
  await expect.soft(page.getByLabel('Name')).toBeVisible();
  await expect.soft(page.getByLabel('Email')).toBeVisible();
  await expect.soft(page.getByLabel('Phone')).toBeVisible();
  // All assertions run even if one fails
});
```

---

## Performance Optimization

### Parallelize

```typescript
export default defineConfig({
  fullyParallel: true,
  workers: process.env.CI ? 2 : undefined,
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

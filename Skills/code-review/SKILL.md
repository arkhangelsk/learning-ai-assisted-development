---
name: code-review
description: >
  Reviews the current git diff or staged changes for code quality, security vulnerabilities,
  performance issues, missing error handling, and test coverage gaps. Use this skill whenever
  the user asks to review code changes, review a diff, check staged changes, run /review,
  or wants feedback before merging a pull request. Trigger even for casual requests like
  "can you check my changes?" or "is this safe to merge?" — this skill should activate
  any time code review is implied.
---

# Code Review Skill

Perform a thorough, prioritized review of the current git diff or staged changes.
The goal is to catch real problems — not nitpick style — and give the developer
actionable, specific feedback they can act on immediately.

## Step 1: Get the diff

Run the appropriate git command to retrieve the changes:

```bash
# Staged changes (most common)
git diff --staged

# If nothing is staged, fall back to unstaged changes
git diff

# If a specific commit range or branch is given
git diff <base>..<head>
```

If the working directory is not a git repo or the diff is empty, tell the user
and ask them to paste the diff directly.

## Step 2: Analyze the diff

Work through the checklist below. For each category, look at every changed file
and every modified function. Focus on *what changed*, not the whole codebase.

### Security Vulnerabilities
- Hardcoded secrets, API keys, tokens, or passwords (even in comments or test files)
- SQL injection: string interpolation into queries, raw `execute()` calls with user input
- XSS: unsanitized user input inserted into the DOM via `innerHTML`, `document.write`, etc.
- CSRF: new state-mutating endpoints missing CSRF token validation
- Auth bypasses: middleware removed, permission checks weakened, `if user:` removed
- Input validation gaps: new parameters that are used but never validated or sanitized
- Insecure deserialization, path traversal, or command injection patterns

### Performance Issues
- Loops inside loops where a map/set/index would suffice
- Database queries inside loops (N+1 pattern)
- Missing indexes implied by new `WHERE` or `JOIN` columns in queries
- Large synchronous operations blocking the event loop
- Bundle size regressions: new heavy dependencies imported without justification
- Missing memoization for expensive pure computations called repeatedly
- Inefficient data structures (e.g., `Array.includes` in a hot path instead of a `Set`)

### Missing Error Handling
- Promises/async functions without `.catch()` or `try/catch`
- Network or I/O calls with no timeout or fallback
- Missing null/undefined guards before property access on values that could be absent
- No error boundary or graceful degradation for UI components that fetch data
- Missing status code checks after HTTP requests (assuming `response.ok`)
- Swallowed errors: `catch (e) {}` or `catch (e) { console.log(e) }` with no recovery

### Test Coverage Gaps
- New public functions, methods, or API endpoints with no corresponding test
- New branches (`if/else`, `switch`, `try/catch`) not exercised by existing tests
- Edge cases implied by the code (empty arrays, null inputs, zero, max values) with no test
- Error paths that are tested in the implementation but not in the test suite
- Integration or E2E coverage missing for user-facing flows introduced in the diff

## Step 3: Format the findings

Output a **prioritized list** grouped by severity. Only include categories that have findings — skip empty ones.

Use these severity levels:

| Level | Meaning |
|-------|---------|
| **CRITICAL** | Must fix before merge — security hole, data loss risk, auth bypass |
| **HIGH** | Should fix before merge — significant performance regression, unhandled failures in production paths |
| **MEDIUM** | Fix soon — code quality, maintainability, test gaps for important logic |
| **LOW** | Nice to have — minor improvements, style, optional optimizations |

### Finding format

For each issue:

```
[SEVERITY] Short title
File: path/to/file.ext, line(s) N–M (or function name)
Issue: What the problem is and why it matters.
Fix: Concrete suggestion — ideally a short code snippet or pattern to use instead.
```

### Example output structure

```
## CRITICAL

[CRITICAL] Hardcoded AWS secret key
File: src/config/aws.js, line 12
Issue: AWS_SECRET_KEY is hardcoded as a string literal. This will be committed to version
       history and could be scraped by automated tools.
Fix: Move to environment variable: `const secret = process.env.AWS_SECRET_KEY`
     Rotate the exposed key immediately.

## HIGH

[HIGH] N+1 query in user dashboard loader
File: app/loaders/dashboard.py, lines 45–52 (load_user_widgets)
Issue: `get_widget_data(widget_id)` executes a DB query inside a for-loop over widgets.
       With 50 widgets this is 51 queries per page load.
Fix: Batch-fetch all widget data before the loop:
     `widget_data = Widget.objects.filter(id__in=widget_ids).in_bulk()`

## MEDIUM

[MEDIUM] Unhandled promise rejection in file upload
File: frontend/components/Upload.jsx, line 78 (handleUpload)
Issue: `uploadFile()` is called without await or .catch(). Rejections will be silently
       swallowed, leaving the UI in a loading state.
Fix: `await uploadFile(file).catch(err => setError(err.message))`

## LOW

[LOW] Missing test for empty-array edge case
File: tests/utils.test.js (formatList)
Issue: formatList() is called with empty input in production but no test covers this path.
Fix: Add: `expect(formatList([])).toBe('')`
```

## Step 4: Summary verdict

End every review with one of:

- ✅ **Safe to merge** — no critical or high issues found.
- ⚠️ **Recommend changes before merge** — list the CRITICAL/HIGH items that need resolution.

Keep the verdict short. The findings above contain the detail; the verdict is just the signal.

---

## Tips for good reviews

- Be specific: "line 42" is more useful than "somewhere in this file".
- Be constructive: suggest the fix, don't just identify the smell.
- Avoid false positives: if something looks suspicious but is probably fine given context, say so briefly rather than flagging it as CRITICAL.
- Don't over-flag style: reserve findings for things that could actually cause bugs, security issues, or meaningful performance problems.
- If the diff is very large (500+ lines), prioritize CRITICAL and HIGH issues and note that a full LOW/MEDIUM sweep may require more time.
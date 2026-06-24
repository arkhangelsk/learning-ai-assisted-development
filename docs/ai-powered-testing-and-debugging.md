# AI-powered testing & debugging

> Three workflows: test generation, TDD, and automated UX auditing

---

## Key metrics

| | |
|---|---|
| Coverage jump | 40% → 90% in minutes |
| Tests generated | 118 passing (example project) |
| Testing levels | Code-level + UX-level |

---

## Workflow 1 — Generate tests from existing code

### Setup
- Start with an app that has gaps: missing validation, unhandled edge cases, incomplete error handling
- Existing tests often only cover "happy paths" — pass rate looks fine but coverage is misleading

### Prompt approach
- Be specific: name the exact source files you want covered
- Ask Claude to cover: happy paths, edge cases, error handling, and boundary conditions
- Tell it to run the tests and fix any failures

> **Key insight:** Some tests will fail not because Claude wrote bad tests, but because they exposed *real bugs* the happy path never caught — that's the value.

### Example bugs found automatically
- Case-sensitive Bearer token handling
- Undefined user crash
- No title validation on task creation
- Invalid status transitions allowed
- Get-by-priority omitting zero-task priorities

---

## Workflow 2 — TDD with AI (spec → tests → code)

### Workflow (flipped from usual)
1. Write a feature spec first (e.g. a Markdown file describing requirements)
2. Tell Claude to write *failing* tests for every requirement before touching implementation
3. Have Claude implement until all tests pass
4. Crucially: instruct Claude **not to modify tests** after writing them

> **Key insight:** Tests become the acceptance criteria. Claude can't cheat by weakening a test to match a broken implementation. Review tests first (are these the right requirements?), then review the implementation.

### Review checklist
- **Step 1** — Are the tests actually testing the right requirements?
- **Step 2** — Does the implementation genuinely satisfy them?

---

## Workflow 3 — Automated UX testing with DevTools MCP

### Prerequisites
- Dev server must be running
- Chrome DevTools MCP configured (`claude mcp add chrome-devtools`)

### What the UX audit checks
- Screenshot every key page element
- Console errors on each navigation
- All interactive elements clickable and responsive
- Responsive layout at mobile, tablet, and desktop breakpoints
- Loading states and error states
- Core Web Vitals / performance

### Example audit output
| Status | Item |
|---|---|
| ✅ Pass | Page load performance, responsive layout |
| ⚠️ Warning | Missing inline form validation |
| ❌ Fail | Missing favicon (with suggested fix) |

> **Key insight:** Build the UX audit as a **slash command**, not a one-off prompt — that makes it repeatable. Add it as a post-commit or pre-PR hook to make it part of your CI pipeline.

---

## Key takeaway

AI flips the economics of testing. Traditionally, tests are tedious and expensive so teams underinvest. With AI, test generation is nearly free — you can write them first, write them thoroughly, and use them as both a quality gate and a bug-finding tool.

- **Unit tests** — Claude generates and runs them in the terminal
- **UX tests** — DevTools MCP lets Claude see and interact with your actual app in the browser
- **Combined** — full-stack quality coverage, code-level and user-level, from a single tool

---

## Pro tip

Build your UX audit as a slash command so it runs the same checks every time, no matter who triggers it. Add to the checklist whenever you find a new class of bug — your pipeline improves with every iteration.
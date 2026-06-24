# Autonomous Agent Patterns

In the context of agentic development, an autonomous agent is an AI system that can perform tasks and make decisions without constant human intervention. These agents can be designed to follow specific patterns that enable them to operate effectively in various scenarios. Below are some common autonomous agent patterns:

## The RALPH Loop: Self-Improving Agents

### What Is RALPH?
A bash script that runs Claude Code **repeatedly in a loop** — fully autonomously. Based on Jeff Huntley's original idea, with an open-source implementation available.

Each iteration Claude:
1. Picks the highest-priority incomplete task
2. Implements it
3. Runs quality checks (type check, tests)
4. Commits if checks pass
5. Logs learnings
6. Exits — then RALPH spawns a fresh instance for the next task

No input needed from you.

---

### How Memory Works (Without a Context Window)
Memory persists between iterations through three files on disk:

| File | Purpose |
|---|---|
| `prd.json` | Task list with pass/fail status per story |
| `progress.txt` | Append-only log of learnings across iterations |
| Git history | Record of all committed work |

Each new Claude instance starts with **clean context** but reads these files to benefit from everything previous instances learned — that's the self-improving part.

---

### The 3-Step Workflow

**Step 1 — Write a PRD**
Define your requirements as a product requirements document. Use the `/prd` skill to help generate it.

**Step 2 — Convert to `prd.json`**
Structure requirements as **user stories**, each with acceptance criteria and a `passes: false` flag. Use the `/ralph` skill to help convert. As stories are completed, RALPH flips `passes` to `true`.

**Step 3 — Run RALPH**
```
./ralph.sh --tool claude
```
RALPH creates a feature branch, picks stories one by one, and exits when everything passes.

---

### Why Fresh Context Per Iteration?
In a long Claude Code session, the context window fills up fast — the AI starts losing track of key details. RALPH avoids this entirely: each story gets a **clean slate**, and memory lives on disk, not in the context window.

---

### Story Sizing — Critical Rule
Each story must be **small enough to complete in a single context window.**

| ✅ Right-sized | ❌ Too big |
|---|---|
| Add a database column and migration | Build the entire dashboard |
| Implement form validation on checkout | Redesign the auth flow |

Think of it like sizing a pull request — small and focused.

---

Check out the Ralph Loop repository for more details: [Ralph Loop GitHub Repository](https://github.com/snarktank/ralph)


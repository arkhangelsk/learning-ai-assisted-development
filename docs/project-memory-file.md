# Project Memory File

A Project Memory File is a text file that provides persistent instructions and context to your AI coding assistant. It is automatically loaded at the start of each session, giving the AI information it cannot easily infer from the codebase alone.

A well-written memory file helps the AI understand your project's architecture, coding conventions, tooling, workflows, and team-specific rules. As a result, the AI can generate more accurate code, make fewer incorrect assumptions, and better align with your development practices.

The location of the file depends on the tool you're using:

- **Claude Code** → `CLAUDE.md` in your project root
- **GitHub Copilot (VS Code)** → `.github/copilot-instructions.md`
- **Cursor** → `.cursor/rules/project.md`

---

## Guidelines for a Better Memory File
Your content is solid, but it has a few issues:

1. It overstates technical claims about attention mechanisms without evidence.
2. It uses unnecessary buzzwords ("high-fidelity context", "attention density", "deterministic steering wheel").
3. It repeats ideas several times.
4. Some phrasing sounds AI-generated rather than written by an experienced engineer.

A tighter version:

# Architectural Deep Dive: Optimizing `CLAUDE.md` and Project Memory

Files such as `CLAUDE.md`, `AGENTS.md`, and `.github/copilot-instructions.md` provide persistent project context for AI coding assistants. They help reduce ambiguity by documenting architecture, conventions, tooling, and project-specific constraints.

A good project memory file is concise, easy to maintain, and focused on information that an agent cannot reliably infer from the codebase.

---

## Template Example

```markdown
# Project Overview
This is a modern TypeScript and Next.js web application for an e-commerce platform.

# Tech Stack & Preferences
- Framework: Next.js (App Router)
- Language: TypeScript (strict mode)
- Styling: Tailwind CSS
- State Management: Zustand

# Coding Standards
- Write clean, functional code. Prefer pure functions over classes.
- Use arrow functions for React components.
- Always implement explicit TypeScript interfaces for component props.
- Wrap asynchronous API calls in try/catch blocks with detailed logging.

# Output Preferences
- Keep responses concise and focused primarily on code implementation.
- Do not include lengthy explanations unless explicitly requested.

```

---

## Principles for Effective Project Memory

### 1. Keep It Short

Aim to keep the file under roughly 200 lines.

Why:

* Large instruction files are harder for both humans and models to navigate.
* Important constraints become less visible when buried in lengthy explanations.
* Shorter files are easier to review and maintain as the project evolves.

Focus on high-value information rather than exhaustive documentation.

---

### 2. Document Commands and Tooling

Include the commands required to build, test, lint, and run the application.

```markdown
## Common Commands

- Build: `uv run build`
- Test: `pytest -v --maxfail=2`
- Lint: `ruff check --fix`
```

This gives agents a clear way to validate changes instead of guessing how the repository works.

---

### 3. Add Rules Only When Needed

Start with architecture, conventions, and workflows.

As you work with AI agents, add constraints that address recurring mistakes.

For example:

```markdown
## Project Rules

- Never access the database directly from API routes.
- Always use the service layer for business logic.
- Use repository methods instead of raw SQL.
```

Treat the file as a collection of lessons learned rather than a comprehensive specification.

---

## Example `CLAUDE.md`

```markdown
# Project Memory

## Architecture

- Python 3.12
- FastAPI
- PostgreSQL
- SQLAlchemy

Request Flow:

Client → Router → Service Layer → Repository → Database

## Engineering Standards

- Keep business logic out of route handlers.
- Use Pydantic v2 models for request validation.
- Return consistent API error responses.
- Write integration tests for all API endpoints.

## Commands

- Start server:
  `uv run uvicorn main:app --reload`

- Run tests:
  `uv run pytest`

- Lint:
  `uv run ruff check`

- Format:
  `uv run ruff format`
```

---

## Team Benefits

Store the file in version control alongside the codebase.

Benefits:

* Engineers and AI agents work from the same project conventions.
* New contributors can understand architecture and workflows quickly.
* Common mistakes are documented once instead of corrected repeatedly.
* Project-specific knowledge stays with the repository rather than individual team members.

The best project memory files are not long. They are specific. Every line should either explain how the system works, how to validate changes, or what constraints must be followed.

# Using Custom Slash Commands in Claude Code
Custom slash commands are reusable prompt templates that you invoke by typing a slash followed by the command name. Instead of re-typing detailed instructions repeatedly, you package them into a Markdown file and call them with `/command-name`.

Claude Code has many built-in slash commands such as `/clear` to empty the conversation, `/context` to view context usage, `/init` to set up a CLAUDE.md, and `/agents` for managing subagents. 

Custom commands let you add your own on top of those.

---

## How to Configure Them

### 1. Choose your scope

Commands stored in your project's `.claude/commands/` folder are project-specific and available when working in that project. You can also create user-level commands in `~/.claude/commands/` that apply across all projects.

| Location | Scope |
|---|---|
| `.claude/commands/` | Current project only (can be git-committed and shared) |
| `~/.claude/commands/` | All projects on your machine |

> **Note:** `.claude/commands/` is the legacy format. The recommended format is now `.claude/skills/<name>/SKILL.md`, which supports the same `/name` invocation plus autonomous invocation by Claude. The CLI continues to support both formats.

### 2. Create the Markdown file

A command file is simply a Markdown file with a `.md` extension. The filename determines the command name, and the file contents become the prompt that Claude executes. Use lowercase letters and hyphens for readability — avoid spaces or special characters in filenames.

A simple example — save as `.claude/commands/commit.md`:

```markdown
Review all staged changes before committing:
- Flag any TODO comments, hardcoded test data, or debug flags
- Confirm the change does one logical thing
- Write a clear, imperative commit message
Then commit with: git commit -m "<message>"
```

Invoke it with just: `/commit`

### 3. Add arguments (optional)

Custom commands support dynamic arguments using placeholders. For example, create `.claude/commands/fix-issue.md` with frontmatter:

```txt
---
argument-hint: [issue-number]
description: Fix a GitHub issue by number
---

Fix issue #$ARGUMENTS following our coding standards.
Check the issue description and implement the necessary changes.
```

Invoke with: `/fix-issue 42`

### 4. Restrict tools via frontmatter (optional)

You can lock a command to specific tools and even pin a model:

```txt
---
allowed-tools: Bash(git *), Read, Grep
description: Run a security audit
model: claude-opus-4-6
---

Scan the codebase for SQL injection risks, exposed credentials,
XSS vulnerabilities, and insecure configurations.
```

---

## Quick Reference

| Feature | How |
|---|---|
| Project command | `.claude/commands/my-command.md` |
| Global command | `~/.claude/commands/my-command.md` |
| Pass arguments | Use `$ARGUMENTS` in the file body |
| Restrict tools | Add `allowed-tools:` in YAML frontmatter |
| Modern format | `.claude/skills/my-command/SKILL.md` |
| Invoke | Type `/my-command` in Claude Code |


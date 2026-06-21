# Claude Productivity Tips

**Switching models mid-session** — Use `/model` to swap between Claude models without starting a new session. Route simple tasks to a faster, cheaper model and complex ones to a more capable model.

**Managing context in long sessions** — Use `/compact` to summarize the conversation and free up context window space before it fills up. To pick up where you left off in a previous session, use the `--continue` flag when launching Claude Code.

**Debugging Claude's reasoning** — Pass the `--verbose` flag to see exactly what Claude is doing: which tools it's calling, what it's reading, and how it's making decisions. Useful when a response seems off and you want to understand why.

**Bootstrapping a new project** — Run `/init` in any existing codebase and Claude will scan the project structure and auto-generate a `CLAUDE.md` with relevant context. A good starting point before you customize it manually.

**Adding context explicitly** — Prefix a filename or folder with `#` to pin it into the active context (e.g. `# src/auth/`). Use this when Claude is referencing the wrong file or missing a key dependency.

**Pasting visual context** — Drag and drop screenshots, error dialogs, or UI mockups directly into the prompt. Faster than describing a visual problem in words.

**Running Claude non-interactively** — Use the `--print` flag to pass a single prompt and pipe the output to stdout. Useful for integrating Claude into CI pipelines, shell scripts, or build tools.

**Locking down tool access** — Use `--allowedTools` or `--disallowedTools` flags at startup to control exactly which tools Claude can invoke. Useful when you want Claude to read but not write, or to prevent shell command execution entirely.

Here's the additional item to slot into the list:

**Pre-approving tools in settings** — Instead of approving tool calls one by one during a session, add them to the `allowedTools` array in your `.claude/settings.json`. Claude will skip the approval prompt for anything on the list, keeping your workflow uninterrupted.
Instead of approving tool calls one by one during a session, add them to the `allowedTools` array in your `.claude/settings.json`. Claude will skip the approval prompt for anything on the list, keeping your workflow uninterrupted.

```json
{
  "allowedTools": [
    "Bash(git *)",
    "Bash(npm *)",
    "Read",
    "Edit",
    "Grep"
  ]
}
```

Use granular `Bash(command *)` patterns rather than allowing `Bash` wholesale — it gives Claude the access it needs without opening up arbitrary shell execution.

**Running parallel workstreams** — Spawn subagents to work on independent tasks simultaneously — one writing tests while another refactors a module, for example. Manage active agents with `/agents`. This can dramatically reduce wall-clock time on large tasks.

**Returning to a specific session** — Use `claude --resume <session-id>` to return to any previous session by ID, not just the most recent one. Useful when juggling multiple features or branches across days.

* **Automating repetitive workflows with custom slash commands** — Any task you find yourself describing to Claude more than once is a candidate for a custom slash command. Package the instructions into a SKILL.md file inside .claude/skills/<name>/ and invoke it with a single /name. Common candidates: committing with a pre-flight checklist, running a security audit, generating a PR description, or scaffolding a new component.

**Templating your Claude setup across projects** — Your `.claude` directory is just files, which means it's portable and version-controllable. Create it once in a template repository with your `settings.json`, pre-approved tools, and skills already configured. When you start a new project, copy or clone the `.claude` directory in and Claude Code picks it up immediately — no configuration needed. 

So your template repo might look like:

```txt
my-project-template/
└── .claude/
    ├── settings.json
    └── skills/
        ├── commit/
        │   └── SKILL.md
        ├── security-audit/
        │   └── SKILL.md
        └── pr-description/
            └── SKILL.md
```
For skills you want everywhere regardless of project, keep a global copy in `~/.claude/`.
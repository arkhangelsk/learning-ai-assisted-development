# Agent Skills Deep Dive

An **Agent Skill** is an open, standardized, file-based format used to package and extend an AI agent's capabilities with specialized, repeatable workflows and domain expertise.

Instead of treating an AI agent like a generic chatbot, an agent skill teaches it **procedural memory**—exactly how your team or organization handles specific technical tasks.

---

## The Anatomy of a Skill

At its core, an agent skill is portable and lives inside a version-controlled repository directory. The structure relies on a single folder containing a **`SKILL.md`** file, which can optionally bundle extra resources:

```text
my-custom-skill/
├── SKILL.md          <── [Required] Contains metadata and instructions
├── scripts/          <── [Optional] Executable JS, Python, or Bash scripts
├── references/       <── [Optional] Specialized docs or API schemas 
└── assets/           ─── [Optional] Static file templates or seed data

```

### The `SKILL.md` Blueprint

The markdown file dictates how and when the agent should invoke the skill, structured with a YAML front-matter header:

```markdown
---
name: security-vulnerability-audit
description: Use this skill when requested to scan package.json files for supply chain vulnerabilities, slopsquatting, or malicious packages before pushing to main.
---

## Execution Steps
1. Scan the dependencies object in package.json.
2. Check the local cache against known vulnerable npm packages.
3. If an anomaly or unverified package like a suspicious axios clone is found, halt execution and generate a security warning table.

```

---

## How it Works: Progressive Disclosure

To prevent the agent's context window from getting choked by hundreds of different instructions, skills use a three-stage runtime framework called **progressive disclosure**:

1. **Discovery:** At startup, tools like Claude Code or Gemini CLI scan your skills directory and load *only* the `name` and `description` of each skill into memory (consuming very few tokens).
2. **Activation:** When you give the agent a task, the LLM uses its semantic reasoning to match your request against the available skill descriptions. Once matched, it dynamically imports the full `SKILL.md` instructions into the context window.
3. **Execution:** The agent follows the structured checkpoints, utilizing the tool-embedded `scripts/` or `references/` only at the exact moment they are needed.

---

## The Crucial Distinction

| Concept | What It Is | Best Analogy |
| --- | --- | --- |
| **System Prompt** | Global identity and behavioral tone for the entire session. | *The agent's permanent personality and general behavior.* |
| **RAG (Retrieval)** | Pulling static data or reference text out of a database dynamically. | *Handing the agent an encyclopedia or a reference manual.* |
| **Model Context Protocol (MCP)** | An open protocol allowing an agent to securely connect to external APIs and host servers. | *Giving the agent hands, tools, and hardware access.* |
| **Agent Skill** | **Portable, repeatable workflows** that enforce quality gates, logic paths, and strict constraints. | *Teaching the agent a specific professional trade or standard operating procedure.* |

---

## Common Engineering Use Cases

In AI-assisted development, engineering teams use packs of agent skills to turn an erratic LLM into a disciplined developer that complies with corporate guardrails:

* **Lifecycle Enforcement:** Forcing an agent to build a comprehensive specification (`/spec`) and a rigorous blueprint (`/plan`) before writing a single line of actual code.
* **Security & Automation Audits:** Standardizing how an agent scans for supply chain attacks or registers dependencies in an infrastructure pipeline.
* **Test-Driven Development (TDD):** Requiring the agent to write a failing test first, verify the failure, implement the minimum code to make it pass, and commit incrementally.

> **Security Guardrail Note:** Because agent skills can execute bundled code (`scripts/`) with access to your local file systems and API environment keys, you should treat custom skills like installing third-party software—always audit the `SKILL.md` steps and attached execution code thoroughly from untrusted sources.

---
## Agent Skills Registry
* [Vercel's Skill Library](https://www.skills.sh/) - A collection of open-source agent skills covering a wide range of applications and industries.
* [Awesome Copilot](https://github.com/github/awesome-copilot/tree/main/skills) - A community-curated directory of open-source agent skills for various use cases, from software development to data analysis and beyond.
* [Anthropic's Skill Library](https://github.com/anthropics/skills/tree/main/skills) - A collection of pre-built skills designed for use with the Claude family of AI agents, covering a wide range of applications and industries.

---
## Agent Skill Scanners

* [SkillSpector](https://github.com/nvidia/skillspector) - Security scanner for AI agent skills. Detect vulnerabilities, malicious patterns, and security risks before installing agent skills.


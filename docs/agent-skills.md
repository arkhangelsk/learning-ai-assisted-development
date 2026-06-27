# Agent Skills: A Practical Guide

An **agent skill** is a portable, file-based format for packaging specialized workflows and domain expertise so an AI agent can apply them automatically — consistently and without re-explanation. Instead of treating an AI like a generic chatbot, skills teach it *procedural memory*: exactly how your team handles specific tasks.

---

## How Skills Work

Skills use a three-stage runtime called **progressive disclosure** to stay lightweight:

1. **Discovery** — At startup, the agent scans your skills directory and loads only the `name` and `description` of each skill into memory (minimal token cost).
2. **Activation** — When you give a task, the model matches it against available skill descriptions and dynamically loads the full instructions.
3. **Execution** — The agent follows the structured steps, pulling in any bundled scripts or references only when needed.

This keeps the context window clean while making deep expertise available on demand.

---

## Skill Structure

A skill lives in a single folder inside your version-controlled repo:

```text
my-custom-skill/
├── SKILL.md          # Required — metadata and instructions
├── scripts/          # Optional — JS, Python, or Bash scripts
├── references/       # Optional — API schemas or specialized docs
└── assets/           # Optional — templates or seed data
```

The `SKILL.md` file uses a YAML front-matter header to define when the skill activates, followed by the instructions themselves:

```txt
---
name: api-design
description: Use this skill when designing or modifying REST APIs.
---

## Execution Steps
1. Apply resource-oriented URL structure.
2. Enforce consistent response envelopes.
3. Validate inputs before processing.
4. Return standardized error responses.
```

---

## Anatomy of an Effective Skill

Most successful skills contain five sections.

### 1. Purpose
Defines when the skill should activate. The more specific, the better.

> *"When creating a PowerPoint presentation"* is better than *"when making content."*

### 2. Core Principles
Captures high-level expert knowledge — typically 3 to 6 rules.

**Example (API Design)**
- Use resource-oriented URLs
- Return consistent response structures
- Use appropriate HTTP status codes

### 3. Implementation Patterns
Concrete examples and templates. Models learn patterns more reliably from examples than from abstract descriptions.

```json
{
  "success": true,
  "data": {},
  "meta": { "page": 1, "total": 42 }
}
```

### 4. Anti-Patterns
Explicit "don't do this" guidance. Surprisingly effective — models respond well to it.

**Example (API Design)**
- Returning raw stack traces
- Inconsistent error formats
- Generic `200` responses for every outcome
- Missing input validation

### 5. Checklist
A self-review step the agent runs before finishing.

**Example (API Design)**
- [ ] Status codes are correct
- [ ] Inputs are validated
- [ ] Errors are standardized
- [ ] Pagination metadata is included

---

## How Skills Differ from Other Concepts

| Concept | What It Is |
|---|---|
| **System Prompt** | Global identity and behavioral tone for the entire session |
| **RAG** | Pulling reference text from a database dynamically |
| **MCP** | Protocol connecting an agent to external APIs and tools |
| **Agent Skill** | Portable, repeatable workflows enforcing quality gates and logic paths |

Skills are the difference between an agent that *can* do something and one that does it *your way*, every time.

---

## Common Use Cases

- **Lifecycle enforcement** — Require a spec and plan before writing any code
- **Security audits** — Standardize scanning for supply chain vulnerabilities
- **Test-driven development** — Enforce write-test → verify failure → implement → commit
- **Presentation creation** — Apply layout, typography, and hierarchy rules automatically
- **API design** — Catch the same recurring review issues before they happen
- **Documentation standards** — Consistent structure and tone across all docs

---

## Creating Your First Skill

Start by identifying where reviews repeatedly catch the same issues. Ask:

- What standards do we enforce over and over?
- What mistakes come up most often?
- What does "good output" look like in this domain?

Then encode the answer once. A skill with three clear principles that gets used every day is more valuable than a comprehensive document that never gets maintained.

> **Start small. Capture the highest-leverage expertise. Let the AI apply it automatically.**

---

## Security Note

Skills that bundle executable scripts (`scripts/`) have access to your local file system and environment variables. Treat third-party skills like third-party software — always audit the `SKILL.md` steps and any bundled code before use.

---
## Agent Skills Registry
* [Vercel's Skill Library](https://www.skills.sh/) - A collection of open-source agent skills covering a wide range of applications and industries.
* [Awesome Copilot](https://github.com/github/awesome-copilot/tree/main/skills) - A community-curated directory of open-source agent skills for various use cases, from software development to data analysis and beyond.
* [Anthropic's Skill Library](https://github.com/anthropics/skills/tree/main/skills) - A collection of pre-built skills designed for use with the Claude family of AI agents, covering a wide range of applications and industries.
---

## Popular Agent Skills Repositories

### Design Skills:
* [GetDesign.md](https://getdesign.md/) - A collection of design-related agent skills.
* [Stitch Design.md](https://stitch.withgoogle.com/docs/design-md/overview) - A set of design-related agent skills by Google.
* [Google Labs Design.md](https://github.com/google-labs-code/design.md) - A collection of design-related agent skills by Google Labs.
* [Open Design](https://github.com/nexu-io/open-design/tree/main/skills/8-bit-orbit-video-template) - A collection of open-source design-related agent skills.

### SDLC Skills:
* [Addy Osmani's Agent Skills](https://github.com/addyosmani/agent-skills) - A collection of agent skills for software development lifecycle tasks.
* [Superpowers](https://github.com/obra/Superpowers) - A set of agent skills for enhancing development workflows.
* [Andrej Karpathy's Skills](https://github.com/multica-ai/andrej-karpathy-skills) - Agent skills based on Andrej Karpathy's work for various development tasks.
* [GStack](https://github.com/garrytan/gstack) - A collection of agent skills for full-stack development.
* [GSD Core](https://github.com/open-gsd/gsd-core) - Core agent skills for managing software development processes.
* [Matt Pocock's Skills](https://github.com/mattpocock/skills) - A set of agent skills for various development tasks.
* [Warp's OZ for OSS](https://github.com/warpdotdev/oz-for-oss/tree/main/.agents/skills) - A collection of open-source agent skills for various use cases, maintained by Warp.

## Agent Skill Scanners
* [Cisco's Skill Scanner](https://github.com/cisco-ai-defense/skill-scanner) - A security scanner for AI agent skills that detects vulnerabilities, malicious patterns, and security risks before installing agent skills.
* [SkillSpector](https://github.com/nvidia/skillspector) - Security scanner for AI agent skills. Detect vulnerabilities, malicious patterns, and security risks before installing agent skills.
* [Agent Scan Skill Inspector](https://labs.snyk.io/experiments/skill-scan/) - A tool by Snyk for inspecting AI agent skills for security vulnerabilities and risks.
* [NVIDIA's Agent Skill Scanner](https://docs.nvidia.com/skills/scanning-agent-skills) - A tool by NVIDIA for scanning AI agent skills for security vulnerabilities and risks.
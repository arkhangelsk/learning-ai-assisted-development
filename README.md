## AI-Assisted Development: A Guide to Using AI for Software Development

This repo is organized into three parts: `docs/` (long-form guides, grouped below in a suggested reading order — start with the foundations, then setup, then workflow patterns, then advanced topics), `Prompts/` (reusable prompt templates you can paste directly into an AI chat), and `Skills/` (agent skills that package repeatable AI workflows).

### Table of Contents

- [AI-Assisted Development: A Guide to Using AI for Software Development](#ai-assisted-development-a-guide-to-using-ai-for-software-development)
  - [Table of Contents](#table-of-contents)
  - [Foundations](#foundations)
  - [Getting Started and Setup](#getting-started-and-setup)
  - [Workflow and Patterns](#workflow-and-patterns)
  - [Maturity and Productivity](#maturity-and-productivity)
  - [Further Resources](#further-resources)
- [Prompts for AI-Assisted Development](#prompts-for-ai-assisted-development)
- [Agent Skills](#agent-skills)

### Foundations

Core concepts for understanding how AI coding tools work and where they fall short.

| Doc                                                                                   | Description                                                                                                                                                        |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [AI Changed Engineering. Here's What Matters Now](./docs/ai-changed-engineering.md)   | How AI tools are changing the software development landscape, which skills are becoming more important, and how to adapt to this new environment.                  |
| [When to Use Agents](./docs/when-use-agents.md)                                                | A guide to identifying workflows that are good candidates for autonomous AI agents, and those that are not.                                                        |
| [Agentic Development Terminologies](./docs/agentic-development-terminologies.md)      | A glossary of key agentic development terms (AI Agent, Agentic Loop, Ralph Loop, MCP, Agent Skills) for quick reference.                                           |
| [LLMs Under the Hood: How AI Coding Models Actually Work](./docs/llm-fundamentals.md) | An introduction to how large language models (LLMs) generate code, including their strengths, limitations, and how to use them effectively.                        |
| [The Limits of AI-Generated Code](./docs/the-limits-of-ai-generated-code.md)          | A deep dive into the common failure modes of AI-generated code and how to defend against them through careful review and testing.                                  |
| [Agentic Development Tools](./docs/agentic-development-tools.md)                      | An overview of the best tools and platforms for AI-assisted software development, including code generation, testing, and deployment.                              |
| [Model Context Protocols](./docs/model-context-protocols.md)                          | A deep dive into the Model Context Protocol (MCP) and how it lets AI agents connect to external tools and data sources like Context7, Chrome DevTools, and GitHub. |

### Getting Started and Setup

Setup, configuration, and customization guides for your AI coding agent.

| Doc                                                                                           | Description                                                                                                                                                  |
| --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [Getting Started With Agentic Development](./docs/getting-started-agentic-development.md)     | A quick-start guide to using AI tools for software development, including setup instructions and best practices for getting the most out of these tools.     |
| [Setting Up Your AI Development Environment](./docs/setting-up-ai-development-environment.md) | Step-by-step setup guides for configuring AI coding agents like VS Code Copilot and Claude Code.                                                             |
| [Setting Up settings.json Configuration Files](./docs/settings-json-file.md)                  | A reference for configuring `settings.json`, including allow/deny lists and tool access permissions.                                                         |
| [Project Memory Files](./docs/project-memory-file.md)                                         | Best practices for writing project memory files (`CLAUDE.md`, `.github/copilot-instructions.md`) that give AI agents persistent context about your codebase. |
| [Using Custom Slash Commands](./docs/custom-slash-command.md)                                 | How to set up custom slash commands to trigger repeatable workflows directly from the chat interface.                                                        |
| [Agent Skills](./docs/agent-skills.md)                                                        | An introduction to Agent Skills, the standardized format for packaging reusable, domain-specific AI agent workflows.                                         |
| [Claude Common Commands](./docs/claude-common-commands.md)                                    | A reference of commonly used Claude Code slash commands for managing context, models, and permissions.                                                       |
| [Working With Hooks](./docs/working-with-hooks.md)                                            | How to use hooks (PreToolUse, PostToolUse, etc.) to automate actions around AI agent tool calls.                                                             |

### Workflow and Patterns

Practices and prompting patterns for getting reliable results out of an AI agent.

| Doc                                                                         | Description                                                                                                                                                  |
| --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [The 70% Problem With AI-Generated Code](./docs/the-70-percent-problem.md)  | Understanding the limitations and challenges of AI-generated code, and how to address them.                                                                  |
| [Prompting Patterns That Improve AI Code Output](./docs/prompt-patterns.md) | Reusable prompting patterns that lead to higher-quality AI code generation.                                                                                  |
| [Context Engineering](./docs/context-engineering.md)                        | A guide to structuring and providing context to AI models for improved code generation and problem-solving.                                                  |
| [Agentic Development Workflow](./docs/agentic-development-workflow.md)      | A step-by-step plan for integrating AI into your software development process, from inline code suggestions to full agent mode.                              |
| [Autonomous Agent Patterns](./docs/autonomous-agent-patterns.md)            | Patterns for running AI agents autonomously, including the Ralph Loop technique.                                                                             |
| [Spec-Driven Development](./docs/spec-driven-development.md)                | How to create detailed specifications that enable AI models to generate complete applications or prototypes, and best practices for writing effective specs. |

### Maturity and Productivity

Frameworks and tips for leveling up how your team uses AI agents day-to-day.

| Doc                                                                            | Description                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [The Agent Coding Maturity Curve](./docs/agent-coding-maturity-curve.md)       | A framework for understanding the stages of AI-assisted development, from initial experimentation to fully integrated agentic workflows (see original article [here](https://engineering.salesforce.com/the-agent-coding-maturity-curve-9-stages-from-code-generation-to-trusted-automation/)). |
| [Productivity Tips for Claude Code](./docs/claude-productivity-tips.md)        | Tips and tricks for getting the most out of Claude Code, including model switching, context management, and debugging techniques.                                                                                                                                                               |
| [AI-Powered Testing and Debugging](./docs/ai-powered-testing-and-debugging.md) | How to use AI tools to automate testing, identify bugs, and generate fixes, including best practices for integrating these tools into your development workflow.                                                                                                                                |

### Further Resources

Curated learning material and people/sources to follow.

| Doc                                                      | Description                                                                                                                             |
| -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| [AI Learning Resources](./docs/ai-learning-resources.md) | A curated list of courses and resources for learning AI and prompt engineering fundamentals.                                            |
| [AI Watchlist](./docs/ai-watchlist.md)                   | A curated list of top AI builders and content sources (blogs, podcasts, YouTube, X) to follow for insights and updates in the AI space. |

## Prompts for AI-Assisted Development

Ready-to-paste prompt templates for common AI-assisted workflows.

| Prompt                                                | Description                                                                 |
| ----------------------------------------------------- | --------------------------------------------------------------------------- |
| [Code Review Prompt](./Prompts/code-review-prompt.md) | An 8-part framework for reviewing AI-generated code changes before merging. |

## Agent Skills

Packaged, repeatable AI agent workflows for specialized tasks.

| Skill                                                                    | Description                                                                                                                                                    |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Code Review](./Skills/code-review/SKILL.md)                             | Reviews the current git diff or staged changes for code quality, security vulnerabilities, performance issues, missing error handling, and test coverage gaps. |
| [Dependency Security Audit](./Skills/dependency-security-audit/SKILL.md) | Audits npm and Python project dependencies for known vulnerabilities, supply chain risks, and maintenance issues.                                              |
| [Playwright Test Reviewer](./Skills/playwright-test-reviewer/SKILL.md)   | Reviews Playwright test code against official best practices.                                                                                                  |

# Agentic Development Terminologies

A glossary of key terms used throughout this repo's docs to describe agentic development concepts, patterns, and protocols.

## AI Agent:

An AI agent is a software entity that can perform tasks autonomously or semi-autonomously, often using natural language processing and machine learning to understand and respond to user inputs.

## Agentic Engineering:

A term that describes the practice of using AI agents to assist in software development tasks. This can include code generation, testing, debugging, and more. The term "agentic" emphasizes the active role that AI tools play in the development process, as opposed to being passive assistants.

## Agentic Loop:

An Agentic Loop is an architectural pattern in AI-assisted development where an LLM doesn't just answer a single question in a one-off response (single-shot text generation), but instead cycles continuously through an autonomous sequence: Perceive -> Plan -> Act -> Reflect/Verify. In coding environments, a standard agentic loop allows an AI tool to check a codebase, call terminal tools to implement edits, inspect the output, and correct its own mistakes until it decides the task is finished.

## Ralph's Loop:

The **Ralph Loop** (or the Ralph Wiggum Technique) is a specific, highly influential variant of an agentic loop designed for software engineering. Coined by open-source developer Geoffrey Huntley, it is named after the character Ralph Wiggum from The Simpsons (known for being hilariously clueless yet stubbornly persistent). The underlying philosophy is simple: **Dumb loops beat clever workflows**. Instead of building overly fragile multi-agent orchestration structures or complex planning graphs, a Ralph Loop is quite literally a shell script (while loop) that repeatedly hammers an AI coding tool against the same prompt or requirements file until the objective is achieved.

## MCP (Model Context Protocol):

An open protocol that allows an AI agent to securely connect to external APIs and host servers. This enables the agent to perform actions that go beyond text generation, such as running code, accessing databases, or interacting with other software tools.

## Agent Skills:

An **Agent Skill** is an open, standardized, file-based format used to package and extend an AI agent's capabilities with specialized, repeatable workflows and domain expertise.

Instead of treating an AI agent like a generic chatbot, an agent skill teaches it **procedural memory**—exactly how your team or organization handles specific technical tasks.

SEE: [Agent Skills Deep Dive](./agent-skills.md)

## Multiagent Orchestration:

The practice of coordinating multiple AI agents to work together on complex tasks. This can involve assigning different roles to each agent, managing their interactions, and ensuring that they collaborate effectively to achieve a common goal.

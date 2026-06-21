# Context Engineering

Context engineering is about controlling what the AI knows before answering. The same prompt can produce dramatically different results depending on the context provided.

### Main Idea

The same prompt can produce dramatically different results depending on the context provided.

Generic prompt + no context → generic boilerplate.

Basic prompt + rich context → code that fits the actual codebase.

Example: Prompt = “Write an error handling middleware for Express.”

### Without Context

* Generic boilerplate middleware

* Plain error responses

* Basic logging

* No awareness of project structure or conventions

### With Context

* Uses project-specific types and classes

* Follows existing middleware patterns

* Integrates with the app's logging system

* Returns errors in the project's standard JSON format

* Handles application-specific error types

### What is Context Engineering?

Context engineering means supplying the AI with relevant information such as:

* Code files

* Type definitions

* Documentation

* Requirements

* Examples

* Conversation history

* Constraints

* Desired reasoning style (e.g., “think step-by-step”)

### Why Context Matters

A perfect prompt with no context usually produces generic code.

A basic prompt with rich context produces code that fits the actual codebase.

Analogy:

“Build a shelf” → generic shelf.

“Build a shelf for this room, matching this furniture and wood type” → tailored shelf.

### Types of Context

### 1. Code Context (Highest Impact)

* Existing files

* Functions

* Patterns used in the project

### 2. Documentation Context

* README files

* API documentation

* Architecture decision records

### 3. Constraint Context

* What the AI should not do

* Technical limitations

* Team rules

### 4. Example Context

* Sample inputs/outputs

* Existing implementations to mimic

### Relative Impact of Context

| Context Type               | Impact      |
| -------------------------- | ----------- |
| Relevant project code: Existing files, functions, patterns     | Highest     |
| Constraints & requirements: Constraints
What it must / must not do	 | High        |
| Examples: Sample inputs, outputs, implementations                   | Medium–High |
| Documentation: README files, API documentation, architecture decision records              | Medium      |
| General instructions
“Be clean”, “think step by step”     | Lowest      |

### Practical Workflow

### In VS Code

1. Start a chat.

2. Attach relevant files using @ or #.

3. Include type definitions, middleware, configs, or examples.

4. Ask the AI to implement the feature.

### In Claude Code / Terminal Agents

* Let the agent read the project automatically.

* Explicitly point it to important directories/files.

* Ask it to follow existing patterns before writing new code.

## Rules worth remembering
1. Attach the smallest set of files that defines the pattern.
2. State constraints explicitly (“don’t add libraries”, “reuse existing types”).
3. Provide one good example if a style already exists.
4. Ask the model to read before writing (“look at middleware.ts first”).
5. Spend less time polishing the prompt and more time curating the context.

### Key Takeaways

1. Context engineering is often more important than prompt engineering.

2. Same prompt + better context = dramatically better output.

3. Attach relevant project files whenever possible.

4. Provide constraints and examples, not just instructions.

5. Spend less time polishing prompts and more time curating context.

---

# Feeding AI the Right Information

## Key Idea

The quality of AI output depends heavily on the quality of context provided. Effective context engineering means supplying the right information, in the right amount, at the right time.

---

## 1. Use Project Memory Files

### What are Project Memory Files?

Files such as:

* `agents.md`
* `claude.md`
* Project-specific markdown files

These files act as persistent project instructions that are automatically loaded into AI conversations.

### What to Include

A project memory file can contain:

#### Architecture

* Frameworks and languages used
* System design details
* Infrastructure components

Example:

* Express + TypeScript
* Redis for caching and rate limiting
* Prisma ORM

#### Development Conventions

* RESTful endpoint naming
* Database transaction rules
* Data storage standards

Example:

* Store prices as integers
* Use Prisma transactions for multi-table operations

#### Tooling Information

* Build commands
* Test commands
* Common workflows

#### Known Issues (Optional)

* Temporary project constraints
* Current limitations

### Why It Matters

Instead of re-explaining project rules in every prompt, AI automatically understands:

* How the project is structured
* Coding conventions
* Architectural decisions
* Preferred implementation patterns

### Team Onboarding Analogy

A project memory file is like giving a new team member a:

> "Here's how we do things here" document.

---

## 2. Build a Learning System for AI

### Practical Habit

Whenever AI makes a mistake that could have been prevented by project knowledge:

Add that information to:

* `lessons.md`
* `agents.md`
* `claude.md`

### Result

Over time, the project memory becomes a growing knowledge base that prevents recurring mistakes.

---

## 3. Use Targeted Context

### Avoid Giving Everything

Do not dump your entire codebase into the context window.

Instead, provide only the files relevant to the task.

### Common Methods

#### VS Code Mentions

Use:

* `@file`
* `@folder`
* `#references`

To include:

* Files
* Directories
* Documentation
* Git changes

#### Claude Code

Examples:

* Read files from a specific directory
* Analyze Git diffs before making changes
* Reference selected documentation

### Benefits

* More focused responses
* Better use of context window
* Less noise
* More accurate code generation

### Principle

> Give AI the specific files needed for the current task.

---

## 4. Use Negative Context (Most Underused Technique)

### Definition

Tell AI what NOT to do.

Many developers only specify requirements and forget constraints.

### Example

Task:

> Implement caching for a user profile endpoint.

#### Avoid

* In-memory caching
* New caching libraries
* Changing endpoint signatures
* Unapproved design patterns

#### Require

* Existing Redis client
* Cache-aside pattern
* 5-minute TTL
* Cache invalidation on updates
* Cache hit/miss headers for debugging

### Why It Works

Without these constraints, AI might:

* Install new libraries
* Use in-memory caching
* Choose different caching patterns
* Modify APIs unnecessarily

### Principle

> Telling AI what to avoid is often as important as telling it what to do.

---

## 5. Provide External Documentation

### Problem

AI models have knowledge cut-off dates.

They may:

* Miss new SDK versions
* Use outdated APIs
* Hallucinate methods that do not exist

### Solution

Provide official documentation.

Examples:

* SDK docs
* API references
* Example code
* Function signatures

### Example Prompt

Instead of:

> Implement refunds using Stripe.

Use:

> Here's the Stripe documentation. Using this API, implement a refund function.

### Benefits

* Uses real API signatures
* Reduces hallucinations
* Produces current implementations
* Increases accuracy

### Principle

> AI performs best when working from facts instead of memory.

---

## 6. Leverage Skills and Framework-Specific Guidance

Many modern tools provide:

* Skills
* Framework guides
* Library-specific experts
* Tool integrations

These often contain:

* Best practices
* Recommended workflows
* Documentation references
* Framework conventions

### Benefit

They provide richer context than a generic prompt.

---

# Context Engineering Checklist

Before asking AI to make changes:

### Project Context

* [ ] Project memory file exists
* [ ] Architecture documented
* [ ] Coding conventions documented
* [ ] Common commands documented

### Task Context

* [ ] Relevant files included
* [ ] Relevant folders included
* [ ] Git changes included if needed
* [ ] Unrelated files excluded

### Constraints

* [ ] Clearly state what AI should do
* [ ] Clearly state what AI should NOT do
* [ ] Specify required patterns
* [ ] Specify forbidden patterns

### Documentation

* [ ] Provide SDK docs
* [ ] Provide API references
* [ ] Provide examples if available
* [ ] Use official sources

### Continuous Improvement

* [ ] Add recurring mistakes to project memory
* [ ] Update lessons learned
* [ ] Refine project instructions over time

## Core Takeaway

Prompt engineering focuses on **what you ask**.

Context engineering focuses on **what the AI knows when you ask**.

The most effective AI workflows combine:

1. Project memory files
2. Targeted context selection
3. Negative constraints
4. Official documentation
5. Continuous accumulation of project knowledge

These techniques consistently produce more accurate, maintainable, and project-aligned outputs.




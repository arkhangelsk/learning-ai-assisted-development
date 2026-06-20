# Prompting Patterns That Improve AI Code Output

Well-structured prompts consistently produce better code than simple requests. Three patterns are particularly effective across coding assistants such as Claude Code, Copilot, Cursor, and others.

---

## 1. Chain of Thought Prompting

### What It Is

Ask the AI to reason through a problem step-by-step before generating code.

### When to Use

* Complex algorithms
* Business logic
* System design decisions
* Problems with multiple edge cases

### Example

Instead of:

> Implement a function to detect scheduling conflicts.

Use:

> Before writing the code, think through the algorithm step-by-step, identify edge cases, and explain the approach.

### Benefits

* Encourages deeper reasoning.
* Identifies assumptions before coding.
* Surfaces edge cases early.
* Produces more thoughtful implementations.

### Example Outputs

The AI may first explain:

* Event data structure
* Sorting strategy
* Overlap detection logic
* Edge cases (same start time, adjacent events, all-day events)

Then generate the implementation.

---

## 2. Few-Shot Examples

### What It Is

Provide examples of the pattern you want the AI to follow.

### When to Use

* Maintaining coding standards
* Creating similar components
* API development
* Consistent naming conventions
* Team-specific patterns

### Example

Show the AI one existing API handler and ask it to create additional handlers using the same structure.

### Benefits

* Improves consistency.
* Reduces variation in coding style.
* Helps AI understand project conventions.
* Produces outputs that match existing codebases.

### Result

Instead of four different implementations, the AI generates four handlers following:

* The same structure
* The same naming conventions
* The same validation approach
* The same architectural patterns

---

## 3. Role and Constraints Prompting

### What It Is

Assign the AI a specific role and clearly define its scope.

### When to Use

* Code reviews
* Security analysis
* Performance optimization
* Architecture reviews
* Specialized evaluations

### Example

> Act as a security-focused code reviewer. Review this authentication function and focus only on token validation, timing attacks, information leakage, and OWASP Top 10 issues. Do not suggest stylistic improvements.

### Benefits

* Produces focused feedback.
* Reduces irrelevant suggestions.
* Encourages domain-specific analysis.
* Makes reviews more actionable.

### Example Roles

* Security reviewer
* Senior backend engineer
* Performance engineer
* QA automation architect
* Accessibility specialist

---

## Combining the Patterns

The most effective prompts often combine all three approaches.

### Example Structure

**Role**

> Act as a senior backend engineer.

**Chain of Thought**

> Think through the data flow and design decisions step-by-step before coding.

**Few-Shot Example**

> Here is an existing endpoint. Follow the same pattern and conventions when building the new endpoint.

### Why It Works

This combination provides:

* Expertise and perspective (Role)
* Better reasoning (Chain of Thought)
* Consistency with existing code (Few-Shot)

---

## Quick Reference

| Pattern              | Best Used For                    | Key Phrase                                      |
| -------------------- | -------------------------------- | ----------------------------------------------- |
| Chain of Thought     | Complex logic and algorithms     | "Think through this step-by-step before coding" |
| Few-Shot Examples    | Consistency and style matching   | "Here's an example, follow this pattern"        |
| Role and Constraints | Reviews and specialized analysis | "Act as a..." and "Focus only on..."            |

### Takeaway

The quality of AI-generated code depends heavily on the quality of the prompt. Rather than asking AI to simply "write code," guide it by:

1. Making it reason first.
2. Showing examples to follow.
3. Giving it a specific role and constraints.

These techniques improve accuracy, consistency, and usefulness across virtually all AI coding tools.

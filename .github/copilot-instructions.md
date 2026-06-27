# Project Guidelines

## Purpose

This is an educational documentation repository about AI-assisted software development. It contains guides, references, and reusable prompts for developers learning to work effectively with AI tools.

## Content Structure

- `docs/` — Long-form guides and reference articles (Markdown)
- `Prompts/` — Reusable prompt templates for AI-assisted workflows
- `Skills/` — Agent skills for AI-assisted workflows
- `README.md` — Index of all docs; update this when adding new files

## Writing Style

Follow the conventions established in existing docs:

- Use clear, practical prose aimed at working software developers
- Lead each section with a brief "what it is / when to use it" explanation
- Include concrete examples (blockquote `>` style for prompt examples)
- Use `##` for top-level sections, `###` for subsections
- Use `*` for bullet lists, numbered lists for sequential steps
- Separate major sections with `---`
- Avoid filler phrases ("it's important to note that…")

## Conventions

- All content files are Markdown (`.md`)
- File names use `kebab-case`
- Don't duplicate content that already exists in another doc — link to it instead
- When adding a new doc to `docs/`, add a corresponding entry in `README.md`
- Prompt files in `Prompts/` should be ready to paste directly into an AI chat — no setup explanation needed in the file itself

## No Build or Test Pipeline

This repo has no code, build steps, or test suite. Changes are complete when the Markdown is valid and consistent with the existing style.

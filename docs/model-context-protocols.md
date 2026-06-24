# Model Context Protocols (MCP)

## What is MCP?
- **Model Context Protocol** — an open standard for connecting AI agents to external tools and data sources
- Think of it as **"USB-C for AI"** — a universal interface between AI agents and everything else
- MCP servers expose tools AI Agents can call: fetching docs, querying databases, taking screenshots, checking service health

Check out this video for a quick intro to MCP: https://www.youtube.com/watch?v=aiy9S1wND2E

---

## Recommended MCP servers to try:
- **Context7** — fetches live API docs to keep LLMs up-to-date
- **Playwright MCP** — automates browser interactions for testing, scraping, and more
- **ChromeDevTools MCP** — lets agents inspect and interact with live web pages
- **GitHub MCP** — query issues, pull requests, and repos in real-time
- **PostgreSQL MCP** — run SQL queries against your databases from the agent

---

## Context7: Real-Time API Documentation for LLMs

### The Problem Context7 Solves
- LLM's training data has a **knowledge cutoff**
- Asking it to use latest APIs (e.g. Next.js middleware, Supabase real-time channels) can result in **hallucinated or deprecated methods**
- Code looks right but throws errors because the API changed

**Context7 fixes this** by fetching real, version-specific documentation on demand.

---

### Setup
One command to install:
```bash
npx ctx7 setup --claude
```

---

### How It Works (Two Core Tools)
1. **Resolve Library ID** — converts a library name into a Context7 identifier
2. **Fetch Documentation** — pulls the actual current docs for that library

Claude calls both automatically when you include **"use Context7"** in your prompt.

---

### Example Prompts
- *"use context7 to show me how to set up middleware in Next.js 15"*
- *"use context7 for the Supabase syntax for row-level security"*

In both cases, Claude fetches live docs first, then generates code using the **current API syntax** — not outdated patterns.

---

### Why It Matters
| Without Context7 | With Context7 |
|---|---|
| May use deprecated pages-based middleware | Uses current `NextRequest`/`NextResponse` + matcher config |
| Channel method signatures may be outdated | Reads actual Supabase docs before writing code |

---

### The Broader MCP Ecosystem
- Hundreds of servers available: database connectors, monitoring tools, CI/CD, Slack, GitHub, internal docs
- Browse at the MCP documentation site
- If a service has an API, there's likely an MCP server for it

---

### Pro Tips
- If you know the library ID, use it directly to skip resolution:
```txt
"use context7 with /supabase/supabase for authentication docs

use context7 with /vercel/next.js for app router setup
```
- With ctx7 setup, a skill is installed that triggers automatically when you ask about libraries — no need to say “use context7”. But, **explicitly asking ensures it fires** for libraries you specifically care about.

---

## Chrome DevTools MCP: Inspect and Interact with Live Web Pages

### What It Does
Gives AI Agents like `Claude Code` or `GitHub Copilot` **eyes on your actual running browser** — not just code, but the live rendered app.

Capabilities:
- Take **screenshots** of the rendered page
- Read **console messages** (errors, warnings, failed assertions)
- Inspect **network requests** (slow fetches, failed requests, oversized assets)
- Record **performance traces** (Core Web Vitals, layout thrashing, long tasks)
- **Interact with the page** — not just load it, but click buttons and capture dynamic behavior

---

### Setup
One-liner from the repo docs (select your tool — Claude Code, Cursor, VS Code, etc.):
```
claude mcp add chrome-devtools
```

---

### Example Usage:

**Screenshot + analysis prompt:**
> *"Take a screenshot of localhost:3000 and analyze the page for any issues. Use the Chrome DevTools MCP."*

Type of issues it can find automatically:
- Console errors & unhandled promise rejections
- Slow API endpoints
- Oversized images (visible only in network panel)
- Deprecated APIs
- Visual/UX problems

---

### Performance Tracing
**Trace prompt:**
> *"Run a performance trace and identify the top three performance issues."*

The **performance insights tool** analyzes raw trace data automatically. No need to dig through flame charts manually.

---

### Why It Matters
| Traditional Debugging | With Chrome DevTools MCP |
|---|---|
| Manually open DevTools, poke around | Claude does it automatically |
| 30–60 min on performance panel | Minutes with automated trace analysis |
| Miss dynamic bugs (on-click behavior) | Claude interacts with the page to capture them |
| Only see source code | Claude sees **symptoms** in the live app |

---

Checkout the [Chrome DevTools MCP documentation](https://mcp.so/chrome-devtools) for more details on setup and usage.

Or Watch this demo of it in action: https://www.youtube.com/watch?v=vZPc4hKxIGA

---
# Where to Find MCP Servers
https://mcp.so/
https://glama.ai/mcp/servers
https://smithery.ai/
https://mcpmarket.com/ 
https://mcpservers.org/
https://github.com/modelcontextprotocol/servers

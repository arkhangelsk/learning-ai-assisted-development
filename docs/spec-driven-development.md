# Spec-Driven Development: The Core Idea

A well-defined specification can provide enough context for an AI coding assistant to generate a working application or prototype. The quality of the output often depends on the quality and completeness of the specification.

---

## What Makes a Good Specification

Most effective specifications include four elements:

### 1. Technology Constraints

Define the technology stack upfront, including frameworks, libraries, languages, and file structure requirements.

**Weak example (vague):**
> "Build a to-do app."

**Strong example (specific):**
> "Build a to-do app using React 18 with TypeScript. Use Vite as the build tool, Tailwind CSS for styling, and localStorage for persistence. Deliver as a single `index.html` file with all assets inlined."

The second version tells the AI exactly what tools to use, how to structure the output, and where data should live — removing guesswork that often leads to inconsistent results.

---

### 2. Visual Requirements

Describe the desired appearance, such as layout, color scheme, typography, and overall design style.

**Weak example:**
> "Make it look nice and modern."

**Strong example:**
> "Use a dark background (#0f0f0f), white primary text, and electric blue (#3b82f6) accent color. The layout should be a centered single-column, max-width 640px. Use a monospace font (JetBrains Mono or system monospace fallback). Cards should have subtle border-radius (8px) and a 1px border in #2a2a2a."

Visual descriptions like "dark", "minimal", or "retro" act as shorthand design systems. The AI interprets these cues and applies them consistently — but specific hex values, spacing units, and typography choices remove even more ambiguity.

---

### 3. Interaction Requirements

Explain how users will interact with the application, including navigation, controls, animations, and expected behaviors.

**Weak example:**
> "Users can add and remove tasks."

**Strong example:**
> "Users type a task into an input field at the top and press Enter or click 'Add' to append it to the list. Each task has a checkbox to mark it complete (strikes through text and reduces opacity to 0.4) and a delete button (×) that appears on hover. Completed tasks are sorted to the bottom automatically. A filter bar at the top lets users toggle between All / Active / Completed views."

This level of detail helps the AI make the right implementation decisions for state management, event handling, and UI feedback — instead of guessing what "add and remove tasks" looks like in practice.

---

### 4. Technical Constraints

Specify performance, responsiveness, accessibility, deployment, or packaging requirements.

**Weak example:**
> "It should work on mobile."

**Strong example:**
> "Must be fully responsive at 320px, 768px, and 1280px breakpoints. Tap targets should be at least 44×44px. The app should score 90+ on Lighthouse accessibility audit. No external API calls — all data stays client-side. The final deliverable must be a single self-contained HTML file under 200KB."

Technical constraints like these prevent the AI from, for example, pulling in a 500KB charting library for a simple counter, or building a layout that breaks on small screens.

---

## A Typical Workflow: Specify → Build → Refine

### Step 1: Create the Specification

Start with a clear description of the application and its requirements. Be specific about technologies, features, and constraints. Focus on desired outcomes rather than detailed implementation steps whenever possible.

**Example specification for a Pomodoro timer:**

```
Build a Pomodoro timer as a single self-contained HTML file.

Technology: Vanilla JavaScript (no frameworks), CSS custom properties for theming.

Visual: Minimal flat design. Background #1a1a2e, accent color #e94560. Display a large
circular countdown in the center (SVG stroke-dashoffset animation). Font: system-ui.

Interactions:
- 25-minute work sessions and 5-minute break sessions
- Circular progress ring animates in real time
- Clicking the ring toggles play/pause
- An "S" key skips to the next session
- Browser tab title updates with remaining time (e.g. "🍅 24:03 — Focus")
- A soft bell sound (Web Audio API) plays when the timer ends

Technical: No external dependencies. Must work offline. Single file under 50KB.
```

This specification is roughly 120 words, yet it provides enough context for an AI to generate a fully working, polished timer with no follow-up questions needed.

---

### Step 2: Review the Output

Evaluate whether the generated application:

- Runs correctly
- Meets the functional requirements
- Matches the intended design and user experience
- Adheres to the specified constraints

Pay attention to areas where the AI made design or implementation decisions that were not explicitly defined.

**What to look for:**

| Area | Questions to ask |
|---|---|
| Functionality | Does every feature in the spec actually work? |
| Design fidelity | Do the colors, fonts, and spacing match what was described? |
| Edge cases | What happens with very long text, empty states, rapid clicks? |
| Performance | Is the file size within the limit? Does it lag on interaction? |
| Accessibility | Can you tab through the UI? Are ARIA labels present? |

**Example review note:**
> "The timer works and the ring animates correctly. However, the bell sound is missing — the AI used `setTimeout` as a placeholder comment instead of implementing the Web Audio API. The tab title updates, but uses the emoji '⏱' instead of '🍅' as specified. The file is 38KB — within the 50KB limit."

These observations feed directly into Step 3.

---

### Step 3: Refine and Iterate

Use targeted feedback to improve the result. Small, focused revisions are often more effective than regenerating the entire project. Iteration helps align the implementation more closely with the original intent.

**Effective refinement prompt (focused):**
> "The bell sound is missing. Implement it using the Web Audio API: play a 440Hz sine wave tone for 0.8 seconds, then a 550Hz tone for 0.5 seconds, with a 0.1s gap between them. Trigger this when the countdown reaches 0."

**Less effective refinement prompt (vague):**
> "Fix the sound and also make it better overall."

Focused prompts produce targeted code changes. Vague prompts risk breaking parts that already work.

---

## Key Observations

### 1. Specifications Act as a Blueprint

The specification provides the context for many implementation decisions. Incomplete or ambiguous requirements often lead to inconsistent results.

**Illustration:** Asking for "a dashboard" without specifying data sources, chart types, or layout will produce wildly different results across attempts. Specifying "a 2-column dashboard with a bar chart of monthly revenue on the left and a KPI card grid on the right, using mock data" produces consistent, predictable output.

---

### 2. Technology Choices Influence Outcomes

Specifying libraries and frameworks helps the AI generate code that follows the conventions and capabilities of those tools.

**Example:** Requesting a modal component in plain HTML/CSS/JS will yield one approach. Requesting the same modal "using React and Headless UI's `<Dialog>` component with Framer Motion for the entry animation" tells the AI exactly which APIs to use — producing idiomatic, maintainable code instead of a DIY implementation that reinvents the wheel.

---

### 3. Design Language Matters

Descriptive terms such as "minimal", "modern", or "retro" can influence visual and interaction decisions throughout the application.

**Examples of design language in action:**

| Term used | What the AI tends to produce |
|---|---|
| "minimal" | Ample white space, no decorative elements, monochrome palette |
| "retro terminal" | Monospace fonts, green-on-black, scanline overlays |
| "glassmorphism" | Frosted backgrounds, backdrop blur, subtle borders |
| "material design" | Cards with elevation, ripple effects, roboto font |
| "brutalist" | High contrast, raw borders, oversized typography |

Combining terms compounds the effect: "dark retro terminal with neon green accents and CRT scanline effect" gives the AI a very specific visual direction to follow consistently.

---

### 4. Focus on Goals and Constraints

In many cases, it is more effective to define what the application should accomplish than to prescribe every implementation detail.

**Over-specified (fragile):**
> "Create a `useState` hook called `taskList` that holds an array of objects with `id`, `text`, and `completed` fields. Write a `handleAddTask` function that calls `setTaskList` with a spread of the previous array plus a new object..."

**Goal-oriented (flexible):**
> "Build a task manager where users can add, complete, and delete tasks. Tasks persist after page refresh. The UI should clearly distinguish between active and completed tasks."

The goal-oriented version lets the AI choose implementation details it is well-suited to handle — like state structure, ID generation strategy, and localStorage schema — while you retain control of the outcome.

---

### 5. Self-Contained Outputs Can Simplify Experimentation

For prototypes and demonstrations, a single-file application can reduce setup requirements and make sharing easier.

**Example:** Instead of scaffolding a full Vite + React project, request:

> "Deliver as a single `index.html` file. Import React and ReactDOM from `https://esm.sh/react@18` and `https://esm.sh/react-dom@18`. Use an inline `<script type="module">` tag."

The result is a file you can open directly in a browser, email to a colleague, or drop into a GitHub Gist — no `npm install` required.

---

### 6. Iteration Remains Part of the Process

Even with a strong specification, generated applications typically benefit from review, refinement, and additional requirements.

**Typical iteration arc:**

1. **First pass** → core structure and features work, some visual polish missing
2. **Second pass** → animations, edge cases, and accessibility improvements
3. **Third pass** → performance tuning, final visual alignment
4. **Freeze** → specification saved for reuse

Expecting one-shot perfection is unrealistic. Expecting a working prototype in 1–2 iterations with a well-written spec is very achievable.

---

## Where This Approach Works Well

- **Prototypes** — e.g., a clickable mockup of a new product feature to share with stakeholders before committing engineering time
- **Internal tools** — e.g., a CSV-to-JSON converter, a URL shortener dashboard, a markdown editor for the team
- **Demonstrations** — e.g., a live demo of a sorting algorithm for a technical presentation
- **Learning projects** — e.g., "build me a working implementation of the Observer pattern in TypeScript with a live event log UI"
- **Portfolio projects** — e.g., a polished single-page app showcasing a concept with consistent design
- **Proofs of concept** — e.g., validating that a WebSocket-based collaboration feature is feasible before building the real version

These scenarios often prioritize speed of development and rapid iteration.

---

## Where Additional Structure May Be Needed

Projects with complex business logic, large codebases, strict compliance requirements, or extensive testing needs typically require additional engineering practices beyond specification-driven generation.

**Examples where spec-driven generation hits limits:**

- A multi-tenant SaaS application with role-based access control and audit logging
- A payment processing integration requiring PCI-DSS compliance
- A real-time multiplayer game with server-side authoritative state
- A codebase that requires 90%+ test coverage enforced in CI

In these cases, spec-driven generation can still be useful for individual components or features — but the overall architecture, security model, and test strategy need deliberate engineering decisions.

---

## Practical Recommendation

Save specifications that consistently produce good results. Over time, a collection of reusable specifications, templates, and patterns can become a useful starting point for future projects and experiments.

**Example specification library entries:**

| Template name | What it covers |
|---|---|
| `single-file-react.md` | React 18 via ESM, Tailwind CDN, single HTML file |
| `dark-dashboard.md` | Dark UI, sidebar nav, chart layout, mock data |
| `accessible-form.md` | WCAG 2.1 AA form with validation, error messages, focus management |
| `data-table.md` | Sortable, filterable table with pagination, export to CSV |
| `cli-tool-node.md` | Node.js CLI with `commander`, `chalk`, config file support |

Starting from a proven template and adjusting for the specific project is almost always faster and more consistent than writing a new spec from scratch.
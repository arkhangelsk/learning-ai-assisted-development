# Guide: Setting Up Your AI Coding Environment

Step-by-step installation and configuration for VS Code with GitHub Copilot, and Claude Code.

This guide provides detailed setup instructions for the tools used in this course. Follow the section that matches your preferred toolset.

## Prerequisites for All Setups

* A computer running macOS, Windows, or Linux.
* Node.js version 18 or later installed (download from nodejs.org).
* Git installed and configured.
* A GitHub account (required for GitHub Copilot).
* An Anthropic Console or Claude Pro/Team/Enterprise account (required for Claude Code).

---

## Setting Up VS Code with GitHub Copilot

1. **Install VS Code:** ~5 min.
Download the installer from [code.visualstudio.com](https://code.visualstudio.com) for your operating system and run it. Open the application once installed.


2. **Install the GitHub Copilot Extension:** 1 min.
Open the Extensions view by pressing `Cmd+Shift+X` (Mac) or `Ctrl+Shift+X` (Windows/Linux). Search for **"GitHub Copilot"** and click **Install**.


3. **Authenticate Your Account:** 2 min.
When prompted by a popup in the bottom right corner, click **Sign in to GitHub**. Follow the browser prompts to link your GitHub account containing an active Copilot subscription.


4. **Test Inline Autocomplete:** Ghost Text Check.
Open a project folder and create a new code file. Start typing a standard function (e.g., `function calculateDaysBetweenDates(date1, date2) {`) and look for greyed-out **ghost text** suggestions. Press `Tab` to accept them.


5. **Test Chat & Agent Modes:** Side Panel Check.
Open the Chat view in your sidebar (or press `Cmd+L` / `Ctrl+L`). To switch to multi-file modifications, toggle the drop-down selector inside the chat panel from standard chat to **"Agent"** mode.


---

## Setting Up Claude Code

1. **Verify Your Environment:** Terminal Check.
Open your terminal and ensure Node.js is ready by running:

```bash
node --version

```


2. **Install Claude Code:** Native Installation.
Run the official installation command for your operating system:

* **macOS / Linux / WSL:** `curl -fsSL [https://claude.ai/install.sh](https://claude.ai/install.sh) | bash`
* **Windows (PowerShell):** `irm [https://claude.ai/install.ps1](https://claude.ai/install.ps1) | iex`
* *(Note: The old global NPM package installer is deprecated).*


3. **Authenticate & Initialize:** First Run.
Navigate to your code repository and launch the tool:

```bash
cd /path/to/your-project
claude

```

Follow the in-browser prompts to authorize the CLI tool using your Anthropic developer or subscription account.


4. **Configure Project Rules:** Optional Pro Tip.
Create a `CLAUDE.md` file in your project's root directory. Add your specific coding standards, testing instructions, and styling guidelines here; Claude Code will automatically index this context at the beginning of every workspace session.


---

## Verification Checklist

* [ ] VS Code Copilot Chat panel opens (`Cmd+L` or `Ctrl+L`) and answers code queries.
* [ ] Ghost text completions appear inline as you type in the editor.
* [ ] The `claude` command launches successfully from your terminal application.
* [ ] Claude Code can successfully scan your local directory files when asked: *"What files are in this directory?"*


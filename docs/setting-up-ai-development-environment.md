# Guide: Setting Up Your AI Coding Environment

This guide provides detailed setup instructions for the tools used in this course. Follow the section that matches your preferred toolset.

## Prerequisites for All Setups

* A computer running macOS, Windows, or Linux.
* Node.js version 18 or later installed (download from nodejs.org).
* Git installed and configured.
* A GitHub account (required for GitHub Copilot).
* An Anthropic Console or Claude Pro/Team/Enterprise account (required for Claude Code).

---

## Setting Up VS Code with GitHub Copilot

### 1. Install VS Code: 
Download the installer from [code.visualstudio.com](https://code.visualstudio.com) for your operating system and run it. Open the application once installed.


### 2. Setup Github Account: 
To use Copilot, you’ll need a personal GitHub account with access to a Copilot plan. 

You can:
* Start with Copilot Free to explore limited features without subscribing to a plan.
* Upgrade to Copilot Pro, Copilot Pro+, or Copilot Max to unlock more features, models, and request limits.

### 3. Connect VS Code to GitHub Account: 
Hover over the Copilot icon in the Status Bar and select Use AI Features. Choose a sign-in method and follow the prompts.

### 4. Start using Copilot in VS Code!  
#### 4.1 Test Inline Autocomplete: Ghost Text Check.
Open a project folder and create a new code file. Start typing a standard function (e.g., `function calculateDaysBetweenDates(date1, date2) {`) and look for greyed-out **ghost text** suggestions. Press `Tab` to accept them.

#### 4.2 Test Chat & Agent Modes: Side Panel Check.
Open the Chat view in your sidebar. The Github Copilot opens in the **"Agent"** mode.  Write a message to test the chat functionality, such as: *"What files are in this directory?"* or *"Write a function to fetch data from an API."* The agent will respond with suggestions and code snippets based on your project context.

### 5. Set up your Project Memory File:
Create a `.github/copilot-instructions.md` file in your project root. This file will provide persistent instructions and context to the Copilot agent, helping it generate more accurate code aligned with your project's architecture, conventions, and tooling. Refer to the guide: [Project Memory Files Deep Dive](./project-memory-file.md) for best practices on what to include in this file.

---

## Setting Up Claude Code

### 1. Verify Your Environment: Terminal Check.
Open your terminal and ensure Node.js is ready by running:

```bash
node --version
```


### 2. Install Claude Code: Native Installation.
Run the official installation command for your operating system:

* **macOS / Linux / WSL:** 
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

(Alternatively on macOS, you can use Homebrew: `brew install --cask claude-code`)

* **Windows (PowerShell):** 
```powershell
irm https://claude.ai/install.ps1 | iex
```

Alternative Method (Any Platform)If you already have Node.js 18+ installed, you can use the npm package manager:

```bash
npm install -g @anthropic-ai/claude-code
```

### 3. Verify Installation:
After installation, run the following command to verify that Claude Code is installed correctly:    

```bash
claude --version
```

### 4. Authenticate & Initialize: First Run.
To start using the tool, navigate into your project directory using cd and type claude to authenticate with your Anthropic or Claude Pro account.

```bash
cd /path/to/your-project
claude

```

Follow the prompts to log in and grant necessary permissions. Once authenticated, you can start using Claude Code's features in your terminal and supported IDEs.   


### 5. Test Claude Code Functionality: Directory Scan.
In the terminal, ask Claude Code: *"What files are in this directory?"* It should respond with a list of files in your current directory, demonstrating that it can access and analyze your local project files.

### 6. Set up your Project Memory File:
Create a `CLAUDE.md` file in your project root. `CLAUDE.md` files act as persistent instructions for Claude Code agents, dictating project standards, workflows, and preferences. You can place these files in multiple locations depending on whether you want the instructions to apply globally to all your projects, to a specific repository shared with a team, or just locally for yourself.

Here are the possible locations for CLAUDE.md and related context files:
* Global User Memory (~/.claude/CLAUDE.md)
    * Scope: Personal.
    * Purpose: Applies to all projects across your machine. Perfect for your personal coding style, preferred terminal shells, and communication rules.
* Project Root (./CLAUDE.md or ./.claude/CLAUDE.md)
    * Scope: Team-shared.
    * Purpose: The most common location. You commit this file to Git so every developer gets the same instructions, build commands, and architectural standards. 
* Local Project Overrides (./CLAUDE.local.md)
    * Scope: Personal.
    * Purpose: Personal, project-specific rules you don’t want to share with the team (e.g., local server paths). Usually added to .gitignore.
* Subdirectories and Parent Directories
    * Scope: Directory-specific.
    * Purpose: Ideal for monorepos. Claude searches recursively up to the file system root, pulling in CLAUDE.md files as you navigate subdirectories.
Consult the official 

[⁠Claude Code Memory Documentation](https://code.claude.com/docs/en/memory) for exact resolution orders and organization-wide deployment paths.







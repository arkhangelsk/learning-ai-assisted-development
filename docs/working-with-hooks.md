# Working With Hooks

## Working with Hooks in Claude Code
Hooks in Claude Code are user-defined commands or actions that execute automatically at specific points in Claude's lifecycle. While you can give Claude instructions to run tasks, hooks are deterministic. They guarantee certain actions always happen, acting as reliable guardrails and automated observers. 

For example, you can set up a hook to automatically run tests after every code generation or to log all interactions for auditing purposes. Hooks ensure that critical processes are never skipped, providing a safety net and consistent behavior in your AI-assisted development workflow.

Popular use cases include:
* **Auto-formatting:** Automatically run code formatters like Prettier immediately after Claude edits a file.
* **Blocking commands:** Intercept and cancel dangerous tool calls or destructive file operations before Claude executes them.
* **Notifications:** Send a Slack/Discord ping or native OS alert when a long-running task completes.
* **Custom logging:** Log all of Claude's interactions, tool calls, and file changes to a central dashboard for monitoring and auditing.

---

## How They Work
When an event fires (e.g., PreToolUse, PostToolUse, or Notification), Claude Code passes event data (like tool names or file paths) as JSON to your hook. Based on the hook's output, it can allow, modify, or block the action entirely. For instance, a PreToolUse hook could analyze the tool being called and its arguments, then return a response that either permits the action, modifies it (e.g., changing a file path), or blocks it with an error message. This allows you to enforce custom rules and safety checks around Claude's behavior.

## Types of Hooks
You can define hooks using multiple types of handlers: [1]
1. Command: Execute a shell command.
2. HTTP: Send event context to an HTTP endpoint as a POST request.
3. Prompt/Agent: Prompt the Claude model directly for a Yes/No evaluation or spin up a sub-agent to verify a condition.
4. MCP Tool: Call a tool on an already-connected Model Context Protocol (MCP) server. This allows you to integrate with any external system or API that has a tool interface, such as a CI/CD pipeline, monitoring dashboard, or custom internal service.

## Setting Them Up
Hooks are configured in your JSON settings files—either globally (~/.claude/settings.json) or per-project (.claude/settings.json). 
For example, this configuration automatically runs Prettier on files after Claude edits them: 

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}

```

To learn more about hooks, see the [official documentation](https://code.claude.com/docs/en/hooks).




# Claude Common Slash Commands
Claude Code has many built-in slash commands. Here are some of the most commonly used ones:

1. **/help** - Displays a list of available commands and their descriptions.
2. **`claude --resume`** - Opens an interactive picker to choose from saved sessions.
3. **/model [model_name]** - Switches the active model to the specified one (e.g., `claude-2`, `claude-instant-100k`).
4. **/compact** - Compacts the current context to save memory and improve performance.
5. **/context** - Shows the current context that Claude is using for code generation.
6. **/clear** - Clears the current context and conversation history.
7. **/settings** - Opens the settings menu where you can configure your `settings.json` file.
8. **/verbose** - Toggles verbose mode, which provides more detailed output and explanations for generated code.
9. **/hooks** - Displays the list of configured hooks and their statuses.
10. **/tools** - Lists the tools that Claude has access to and their current permissions.
11. **/memory** - Shows the contents of the current project memory, including any `CLAUDE.md` or `.github/copilot-instructions.md` files.
12. **/run [command]** - Executes a shell command directly from the chat interface (requires permission).
13. **/test** - Runs the test suite for the current project (requires permission).
14. **/lint** - Runs the linter for the current project (requires permission).
15. **/deploy** - Deploys the current project to the configured environment (requires permission).
# Tool Use

## General habits

- Check for an applicable skill before starting; a skill for the task replaces the default approach. The user's direct instructions outrank skills, and skills outrank defaults.
- Prefer the dedicated tool over a shell command when one fits: the file reader over `cat`, the editor over `sed`, search tools over ad-hoc `find`.
- Make independent calls in one batch so they run in parallel. Make dependent calls in sequence and never guess a value another call will return.
- Load deferred tools in one request listing everything expected, not one request per tool.
- Use absolute paths. Avoid `cd` inside compound commands.
- A foreground `sleep` is the wrong way to wait; run the command in the background or use a monitor.

## Files

- Read before editing a file whose text is not already known. Inside the working directory, edit directly when the exact text is known from search output.
- Match the exact existing text, indentation included, and make it unique.
- Write a new file only when one is needed; otherwise edit.
- Read large files in parts once the needed region is known.
- Keep files, functions and nesting within the project's size rules.

## Shell

- Describe each command in plain words: what it does, not its flags.
- Quote paths and variables. Do not interpolate untrusted values.
- Read the whole error output before reacting.
- Set a timeout suited to the command; run long ones in the background.
- No interactive flags. If the user must run something interactive, suggest they run it with the `!` prefix so the output lands in the session.

## Git and GitHub

- Commit or push only when asked. Branch first if on the default branch.
- Inspect status and diff before committing; stage specific files.
- Message: what changed and why, in the repository's existing style.
- Attribution lines follow the user's own rule where one exists, otherwise the session's guidance.
- No history rewriting, force push or hook skipping without explicit approval.
- Use the `gh` CLI for pull requests and issues. Write a bare `@name` only to notify a real person on purpose; put roles and package names in backticks.

## Web and research tools

- Use the standard search mode by default; the extended mode only for thin results, niche or very recent facts, prices, availability and multi-step research.
- Send planned searches together.
- Fetch the primary source instead of relying on a search snippet.
- Never include the user's private details in a query.

## Browser automation

- Get the current tab context first; work in a new tab unless told to use an existing one.
- Do not trigger alert, confirm or prompt dialogs; they block further commands. Log to the console and read the log instead.
- Filter console output with a pattern.
- Record multi-step interactions the user may want to review.
- After two or three failed attempts, or unexpected complexity, stop and ask.

## Documents, artifacts and connectors

- A finished piece meant for other people is published or placed where they can open it, not left in terminal output.
- Load the relevant design or capability guidance before writing a page.
- Hand live-charting destinations the data rows, not a picture of a chart.
- Outward-facing sends (email, Slack, comments) are confirmed with recipients and content first.
- If a configured server failed to connect, say so; do not conclude the capability does not exist or route around it silently.

## Testing and build

- Follow the project's testing rule. Where test-first is mandated: write the failing test, watch it fail, implement the minimum, refactor, then check coverage against the threshold.
- Run the narrowest relevant test during work and the full suite before reporting.
- Fix build errors with the smallest change; no architectural edits to make a build pass.
- Use the platform's reviewer after writing code where the project requires it.

## Feedback on the tooling itself

At a clear moment (a reproducible tool failure resolved or abandoned, a missing capability that blocked a reasonable request, a model-behaviour slip), draft one feedback item per distinct issue, with facts only, people referred to by role, and no secrets.

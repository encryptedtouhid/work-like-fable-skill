# Delegation and Orchestration

## The cost of a subagent

A fresh agent knows only what the brief says, and returns only a summary. Both handoffs lose detail. Its errors come back sounding as confident as its findings, and an agent handed a hypothesis tends to confirm it. Several agents at once spend tokens quickly. Weigh that against the accuracy or time gained.

## Decide: inline or delegate

Do it inline when:

- It is a handful of tool calls.
- The target is already known (a specific file, symbol or URL).
- The result must be seen in full to be trusted.
- The check could be run directly.

Delegate when:

- Independent pieces can run in parallel.
- Answering means sweeping many files and only the conclusion is needed.
- The user asks for a side task that should not block the main thread.
- An unanchored second opinion is wanted (review).
- A specialised agent exists for exactly this job and the project rules point to it.

When in doubt, do not spawn.

## Choosing the agent type

1. Check the project and user rules for a mandated agent (for example a reviewer required for a given platform, or a build-error resolver).
2. Use a read-only search agent for locating code across a large tree.
3. Use a planning agent for implementation strategy on Tier 4 or 5 work.
4. Use a fork when the agent needs the full conversation context.
5. Use a general-purpose agent otherwise.

## Writing the brief

The brief is the only context the agent has. Include:

- **Goal:** the outcome wanted and why it matters.
- **Known facts:** what has been established and what has been ruled out.
- **Pointers:** files, directories and documents to read, by path. Do not retype their content.
- **Scope:** what to touch and what to leave alone. State whether edits are allowed.
- **Constraints:** conventions, the user's standing rules, tools to use or avoid.
- **Return format:** what to report, in what shape, with evidence (paths, line numbers, command output).
- **Stop condition:** when to finish and what to do if blocked.

For a review, give the code and the requirement, not the conclusion hoped for.

If a clear brief cannot be written, the task is not yet understood well enough to hand off.

## Running agents

- Launch independent agents in one message so they run concurrently.
- Do not duplicate delegated work inline; wait for the result.
- Never invent or predict a pending agent's result. If asked before it returns, say it is still running.
- Continue an existing agent with a follow-up message when its context is useful; start a new one when a clean slate is better.
- Use an isolated worktree when agents edit the same repository in parallel.

## Receiving results

- Treat the report as a claim. Spot-check the load-bearing parts: open the cited file, rerun the key command.
- Reconcile conflicts between agents by going to the source, not by majority.
- Relay what matters to the user; the agent's report is not shown to them.
- Integrate and verify the combined result as a whole.

## Multi-agent workflows

- Run a scripted workflow only when the user has explicitly opted in to multi-agent orchestration, or invoked a command that calls for it.
- If a task would benefit but no opt-in was given, describe what the workflow would do and roughly what it would cost, and ask.
- Keep to the session's size guideline for agent count unless the user asks for a different scale.
- Standard shape: fan out by dimension, verify each finding independently as soon as it arrives, keep only confirmed findings.

## Layered implementation order

When layer agents build a feature, run them in dependency order: domain and architecture first, then network and UI in parallel, then data, then wiring and integration last. Test agents follow the project's test-first rule.

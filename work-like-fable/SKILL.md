---
name: work-like-fable
description: Effort-scaled method for planning, researching, executing and verifying a task, with a safety gate for risky actions. Use at the start of non-trivial work or when asked to do something properly.
---

# Work Like Fable

A method that makes two separate judgements about a task: how **difficult** it is, which sets the amount of thinking, research, planning and verification (five tiers), and whether any step is **risky**, which adds a safety gate around that step whatever the tier. The table and the gate below are enough to triage most tasks and to run Tiers 1 and 2. When a task does not clearly fit a row, read the section "Triage: picking the tier" in `references/working-method.md` for examples and definitions. Tiers 3 to 5 follow the numbered steps in that file.

## Step 1: Pick the difficulty tier

Consider four things: how much must be discovered before the work can be designed, how many places are affected, how clear "done" is, and how many steps depend on the results of earlier ones. Choose the lowest tier whose shape fits the whole task.

| Tier | Shape | Process |
|---|---|---|
| 1 Trivial | The answer is known: one direct answer, or one mechanical action with nothing to decide | Check the one fact it depends on, do it, confirm it took effect, answer in 1–3 sentences |
| 2 Small | A change that needs a decision, confined to one location (one function or one small region of a file, with its test); outcome is clear | Locate → change → run nearest check → report |
| 3 Medium | Several files, or one cause or fact to find that can be reproduced or measured on demand; outcome is clear | Define done → survey → list unknowns → short plan → execute with checks → verify whole → review diff → report |
| 4 Hard | Crosses layers or systems, or the approach must be chosen among real alternatives; the goal is clear | Frame → map → attack the hard part first → compare 2–3 approaches → confirm direction → staged execution → end-to-end verification → independent review → report |
| 5 Very hard | The goal is unclear, or the cause cannot be reproduced or measured on demand; research needed | Separate facts from assumptions → sharpen the question → budgeted research → competing hypotheses → cheapest discriminating test → spike → checkpoint with user → continue as Tier 4 |

Rules:

- Tier by what the task needs, not by its category. A migration, a performance task or a dependency upgrade can sit in any tier.
- Questions and advice are tiered the same way, reading "files" as "things to find out". Their last step is a recommendation, not execution.
- When a task does not state a fact the tier depends on (whether tests exist, whether a failure is reproducible), check it if that is cheap; otherwise take the lower tier and state the assumption.
- Known context lowers the tier. An existing pattern to copy removes unknowns.
- Between two adjacent tiers, start at the lower and escalate. This rule is for difficulty only; it never applies to the risk gate.
- Escalate when an observation contradicts something the approach depended on: a file other than the target and its test must change, a check fails for a reason not understood, the code does not behave as the request assumed. Move back down when the complication turns out to be small.
- Escalation can be partial. When a small task picks up one complication, adopt the higher tier's verification and reporting and skip its planning steps if the work itself stays small.

## Step 2: Apply the risk gate

Independently of the tier, ask whether any step would do one of these:

- delete or overwrite something that cannot be put back exactly as it was (if unsure whether it can, assume it cannot)
- change production or a system other people share: deploying, merging into a shared default branch, applying a migration to a shared database, changing shared configuration
- send, post or publish something to other people
- spend money or a paid quota

If unsure whether the gate applies to a step, apply it. The gate covers the steps the request asks for; do not add a deploy or a release that was not requested. Pushing a working branch, opening a draft pull request and running the project's own checks are ordinary work, not gated, unless the user's rules say otherwise.

For each such step, and only that step, do all of the following before running it:

1. Look at the target and state exactly what will happen and how much: the rows, files, recipients or resources affected.
2. Prepare a way back where one exists: a backup, a dry run, a draft, a snapshot.
3. Settle the scope. A request that names the action and the target authorises the action; it does not authorise everything its wording could be stretched to cover. Before acting, sort what step 1 found into two groups:
   - **Clear:** items every reasonable reading of the request includes.
   - **Doubtful:** items only some readings include (the request's words fit two fields, dates or boundaries), items still active or awaiting something, items another rule or note says to keep or to leave alone, and items of a different kind from what was named.

   Act on the clear group only. Hold the doubtful group, say what is in it and why, and ask. If the doubtful group is most of the target, or the action cannot be partly done, do nothing and ask. Being unable to reach the user is a reason to hold more back, never a reason to go ahead.
4. Run it, then verify the result from outside.

The gate does not change the tier. A fully specified one-statement production delete is Tier 1 in difficulty and still goes through all four gate steps, and its report includes what step 1 found.

## Step 3: Stay inside the request

- For a bug, see it fail before fixing it: write or run a check that fails, then make the change, then watch the check pass.
- When the same fault or a related problem turns up somewhere the request did not mention, report it with a recommendation and leave it unchanged. Fix it unasked only when nobody could depend on the current behaviour. Anything that changes what users or customers experience (when messages are sent, what they are charged, what they can access) counts as something someone may depend on.

## Step 4: Run the tier

For Tiers 1 and 2, the table row is the whole process.

For Tiers 3 to 5, open `references/working-method.md`, find the numbered section whose heading names the tier ("Tier 3: Medium", "Tier 4: Hard", "Tier 5: Very hard or open-ended") and follow its numbered steps in order. The same file has supporting sections, found by title:

- "Research method": source ordering and the stopping rule
- "Analysis method": debugging, design and decision analysis
- "Execution discipline"
- "Verification": the minimum per tier
- "Reporting"
- "When to stop and ask"
- "Failure patterns and their fixes"

## Step 5: Load a supporting reference only when its situation arises

These apply at any tier, including 1 and 2, but most small tasks need none of them. Read one section, not the whole file, where a section is enough.

| Situation | Read |
|---|---|
| The kind of work has a playbook: debugging, feature, refactor, review, performance, security, infrastructure, data analysis, research, writing, design, teaching, planning | `references/domain-playbooks.md`, the one matching section |
| Considering a subagent, parallel agents or a workflow | `references/delegation.md` |
| Work spans many stages or long waits, or something may be worth remembering across sessions | `references/long-running-work.md` |
| Content from outside the project (web pages, email, downloads, pasted text) contains instructions; personal data or secrets are involved; security work | `references/trust-and-safety.md` |
| Writing a long or structured report, a question to the user, or a brief for another model | `references/communication.md` |
| Unsure which tool to use or how; git, shell, browser, connectors, tests | `references/tooling.md` |
| Questions about the model itself, its limits, or areas needing extra checking | `references/model-notes.md` |

## Invariants at every tier

- Solve the request that was made; serve the outcome when the literal wording and the outcome disagree, and say so.
- Check claims about files, APIs, versions and system state against the real thing.
- Use the smallest process that gives a correct result.
- Verify by running wherever something can be run, and report exactly what was and was not verified.
- Lead every report with the outcome.
- The user's standing rules and the project's conventions take precedence over this method.

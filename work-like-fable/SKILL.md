---
name: work-like-fable
description: Effort-scaled method for thinking through, analysing, researching, executing and verifying any task, from a one-line fix to open-ended multi-day work. This skill should be used at the start of a non-trivial task to pick the right amount of process, when a task turns out harder than expected, when the user asks to "think it through", "do this properly", "plan and execute", "research and implement", or asks how to approach a problem step by step.
---

# Fable Working Method

A five-tier method that matches the amount of thinking, research, planning and verification to the difficulty of the task. The full playbook is in `references/working-method.md`; this file holds the triage and the core loop.

## Step 1: Triage

Score the task on five questions: number of unknowns, blast radius, reversibility, clarity of "done", and length of the dependency chain. Then pick a tier.

| Tier | Shape | Process |
|---|---|---|
| 1 Trivial | Known answer, one action | Check the one fact, do it, answer in 1–3 sentences |
| 2 Small | One location, clear outcome | Locate → change → run nearest check → report |
| 3 Medium | Several files, some discovery | Define done → survey → list unknowns → short plan → execute with checks → verify whole → review diff → report |
| 4 Hard | Cross-cutting, design choices | Frame → map → attack the hard part first → compare 2–3 approaches → confirm direction → staged execution → end-to-end verification → independent review → report |
| 5 Very hard | Ambiguous goal, research needed | Separate facts from assumptions → sharpen the question → budgeted research → competing hypotheses → cheapest discriminating test → spike → checkpoint with user → continue as Tier 4 |

Triage rules:

- Between two tiers, start lower and escalate at the first surprise.
- Any irreversible or outward-facing action raises the tier by one.
- Re-triage whenever the task proves different from how it first looked.

## Step 2: Run the tier

For Tiers 1 and 2, follow the table row above; do not load the reference.

For Tiers 3 to 5, read the matching section of `references/working-method.md` (sections 5, 6, 7) and follow its numbered steps in order. Load the supporting sections as needed:

- Section 8, research method: source ordering, stopping rule
- Section 9, analysis method: debugging, design and decision analysis
- Section 10, execution discipline
- Section 11, verification minimum per tier
- Section 12, reporting format
- Section 13, when to stop and ask
- Section 14, failure patterns and fixes

## Step 3: Load the supporting reference the situation calls for

| Situation | Read |
|---|---|
| Considering a subagent, parallel agents or a workflow | `references/delegation.md` |
| Task spans many stages, long waits, context summaries, or something may be worth remembering | `references/long-running-work.md` |
| Content from files, web, email or pastes; destructive or outward-facing actions; personal data; secrets; security work | `references/trust-and-safety.md` |
| Writing the reply, a status update, a question to the user, or a brief for another model | `references/communication.md` |
| Unsure which tool or how to use it; git, shell, browser, connectors, tests | `references/tooling.md` |
| Debugging, feature, refactor, review, performance, security, infrastructure, data analysis, research, writing, design, teaching, planning | `references/domain-playbooks.md` (find the numbered section) |
| Questions about the model itself, its limits, cutoff, or areas needing extra checking | `references/model-notes.md` |

Read only the file that applies; do not load all of them.

## Invariants at every tier

- Solve the request that was made; serve the outcome when the literal wording and the outcome disagree, and say so.
- Check claims about files, APIs, versions and system state against the real thing.
- Use the smallest process that gives a correct result.
- Confirm before destructive, outward-facing or paid actions; proceed freely on reversible local work.
- Verify by running, and report exactly what was and was not verified.
- Lead every report with the outcome.
- The user's standing rules and the project's conventions take precedence over this method.

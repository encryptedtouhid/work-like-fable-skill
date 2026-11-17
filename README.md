# work-like-fable

A Claude skill that gives an agent a disciplined, effort-scaled way of working: how to size up a task, how much to research and plan, how to execute, how to verify, and how to report. A one-line fix gets a one-line process. An ambiguous multi-day problem gets hypotheses, experiments and staged execution.

It is modelled on the working practices of Claude Fable 5.2 inside Claude Code.

> **Not an official Anthropic document.** This is an independent write-up of working practices. It does not describe the model's internals. For official information about the model, see https://www.anthropic.com/claude/fable.

## What is in the repo

```
work-like-fable/
├── SKILL.md                      Triage table, core loop, index of references
└── references/
    ├── working-method.md         The full playbook: five tiers with numbered steps
    ├── domain-playbooks.md       Step lists for 13 kinds of work
    ├── delegation.md             Subagents, briefs, checking results, workflows
    ├── long-running-work.md      Long tasks, context, background work, memory
    ├── trust-and-safety.md       Instruction sources, untrusted content, confirmation, privacy
    ├── communication.md          Replies, status updates, questions, briefing another model
    ├── tooling.md                Files, shell, git, web, browser, connectors, tests
    └── model-notes.md            Fable 5.2 facts and limits that affect how to work
```

## How it works

The skill makes two separate judgements about a task. Difficulty sets how much process to use. Risk adds a safety gate around a dangerous step, whatever the difficulty.

### 1. Pick the difficulty tier

The agent considers how much must be discovered, how many places are affected, how clear "done" is, and how many steps depend on earlier results, then picks the lowest tier that fits.

| Tier | Shape | Process |
|---|---|---|
| 1 Trivial | The answer is known: one direct answer, or one mechanical action | Check the one fact, do it, answer briefly |
| 2 Small | A change needing a decision, confined to one function or small region of a file, with its test | Locate, change, run the nearest check, report |
| 3 Medium | Several files, or one cause or fact to find that can be reproduced on demand | Define done, survey, list unknowns, short plan, execute with checks, verify, review, report |
| 4 Hard | Crosses layers or systems, or the approach must be chosen among real alternatives | Frame, map, attack the hard part first, compare approaches, confirm direction, staged execution, end-to-end verification, independent review |
| 5 Very hard | The goal is unclear, or the cause cannot be reproduced on demand | Separate facts from assumptions, sharpen the question, budgeted research, competing hypotheses, cheapest discriminating test, spike, checkpoint, then continue as Tier 4 |

The tier follows what the task needs, not its category: a migration or a dependency upgrade can sit in any tier. Between two adjacent tiers the agent starts lower and escalates when something contradicts its assumptions.

### 2. Apply the risk gate

Separately, the agent asks whether any step deletes data that cannot be restored, changes production or a shared system, sends or publishes something to other people, or spends money. For each such step it states exactly what will be affected, prepares a way back where one exists, gets confirmation, and verifies afterwards. A fully specified one-statement production delete is Tier 1 in difficulty and still goes through the whole gate. Pushing a working branch or running the project's own checks is ordinary work and is not gated.

### 3. Run the tier

Tiers 1 and 2 run straight from `SKILL.md`. Tiers 3 to 5 follow the numbered steps in `references/working-method.md`.

### 4. Load only what the situation needs

The skill uses progressive disclosure. `SKILL.md` is short and is loaded when the skill triggers. Each reference file is read only when its situation comes up: delegating, handling untrusted content, writing a long report, and so on. This keeps the context small.

### Invariants

At every tier: solve the request that was made, check claims against the real thing, use the smallest process that gives a correct result, verify by running, and lead every report with the outcome. The user's own rules and the project's conventions always take precedence over the skill.

## Install

### Claude Code, for all your projects

```bash
git clone https://github.com/encryptedtouhid/work-like-fable-skill.git
cp -R work-like-fable-skill/work-like-fable ~/.claude/skills/
```

### Claude Code, for one project (shared with the team through git)

```bash
mkdir -p .claude/skills
cp -R /path/to/work-like-fable-skill/work-like-fable .claude/skills/
```

### Keep it in sync with this repo

Symlink instead of copying, so `git pull` updates the installed skill:

```bash
ln -s /path/to/work-like-fable-skill/work-like-fable ~/.claude/skills/work-like-fable
```

### Claude apps (claude.ai, desktop)

Zip the `work-like-fable` folder so that the folder itself is the top level of the archive (`work-like-fable.zip` containing `work-like-fable/SKILL.md`):

```bash
cd work-like-fable-skill && zip -r work-like-fable.zip work-like-fable
```

Then go to **Customize > Skills**, click **+**, choose **Create skill**, then **Upload a skill**, and select the ZIP. For uploads, keep the folder name the same as the `name` field in `SKILL.md`. Plan availability and limits change; see [Using Skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude).

### Claude Agent SDK

The SDK discovers skills in the same places as Claude Code, not from an arbitrary path. Put the folder at `<project>/.claude/skills/work-like-fable/` or `~/.claude/skills/work-like-fable/`. If you set the setting sources explicitly, include `user` and `project`; if you pass an explicit tool list, include `Skill`. See the [Agent SDK skills documentation](https://code.claude.com/docs/en/agent-sdk/skills.md).

### Notes

- Claude Code watches the skill folders, so a skill added under `~/.claude/skills/` or `.claude/skills/` is picked up in the running session without a restart. If the `skills` folder itself did not exist when the session started, run `/reload-skills`.
- In Claude Code the `name` field is optional and defaults to the folder name; when set, it is the command you type.
- These install details were checked against the official documentation on 2026-11-17: [Claude Code skills](https://code.claude.com/docs/en/skills).

## Use

### Automatic

Claude loads the skill from its description: at the start of non-trivial work, or when you ask for something to be done properly.

### Explicit

```
/work-like-fable Migrate the session store from Redis to Postgres
```

Or in plain words: "Use work-like-fable for this."

### Example prompts by tier

| You say | What happens |
|---|---|
| "Rename `usr` to `user` in this file" | Tier 1: done, one-line reply |
| "The date on the invoice is off by one day, fix it" | Tier 2: locate, fix, run the nearest test |
| "Add a CSV export endpoint with tests" | Tier 3: survey existing endpoints, short plan, build, verify |
| "Add offline support to the notes feature" | Tier 4: approaches compared, direction confirmed, staged build |
| "Checkout fails for about 1% of users and we do not know why" | Tier 5: hypotheses, discriminating tests, then a fix |

## Getting the most out of it

**Say what "done" means.** One sentence of success criteria removes most ambiguity and lets the agent pick the right tier.

**Name the tier when you already know it.** "Treat this as Tier 4" forces the design comparison and staged execution. "Tier 1, just do it" stops over-processing.

**Ask for a single stage.** The method is modular:

- "Triage this and tell me the tier and why" for a quick difficulty estimate.
- "Do only the survey and list the unknowns" before committing to work.
- "Give me the two or three approaches and your pick" for a design decision.
- "Run the verification step for Tier 4 on this branch" as a quality gate.

**Use the domain playbooks directly.** "Follow the debugging playbook", "review this with the code review playbook", "use the infrastructure playbook for this server change". There are thirteen: debugging, new feature, refactoring, code review, performance, security review, infrastructure, data analysis, research report, writing, UI design, teaching, and planning only.

**Make it the default in a project.** Add one line to the project's `CLAUDE.md`:

```markdown
For any non-trivial task, use the work-like-fable skill to choose the amount of process.
```

**Give it to subagents.** When delegating, tell the agent to follow the skill, or paste the relevant tier's steps into the brief. `references/delegation.md` describes what a good brief contains.

**Combine it with specialised skills.** This skill decides how much process a task needs. Domain skills (a test-driven-development skill, a framework guide, a design system) decide how the work is done. Use both: triage here, then follow the specialised skill inside the chosen tier.

**Use it for non-coding work.** The tiers apply equally to research, data analysis, writing and operations. The playbooks for those are in `references/domain-playbooks.md`.

**Use it for long sessions.** For multi-hour work, ask for a status line at each stage boundary. `references/long-running-work.md` covers staying on track and what is worth saving to memory.

**Use it with other models or agent frameworks.** The files are plain Markdown. Put `SKILL.md` in a system prompt and supply the reference files on demand. Lines marked **(Claude Code)** in the references name features of that environment and can be skipped elsewhere. `references/model-notes.md` is specific to Fable 5.2; replace or drop it for another model.

**Use it as a team standard.** The playbook doubles as a checklist for people: a shared definition of what "verified" and "done" mean at each level of difficulty.

## Customise

- **Tier thresholds:** edit the triage table in `SKILL.md` and section 2 of `references/working-method.md`.
- **Your own rules:** add a reference file (for example `references/team-conventions.md`) and a row for it in the "Step 3" table in `SKILL.md`.
- **Trigger behaviour:** edit the `description` in the `SKILL.md` frontmatter; that text decides when Claude loads the skill.
- **Keep `SKILL.md` short.** Detail belongs in `references/`, so it is loaded only when needed.

## How it was tested

Tested once, on 2026-11-17, with fresh Claude subagents. The sample is small; treat the results as indicative.

- **Same bug, with and without the skill.** Two agents were given the same small Python project with an off-by-one bug that also existed in a second file. Both wrote failing tests first, fixed the reported bug, re-ran the suite, found the second instance and reported it for a decision. The outcome was the same. The skill cost three extra tool calls and changed only how the second instance was reasoned about and reported. On a small, well-specified bug, a capable model does not need this skill.
- **Triage consistency.** Two agents independently tiered the same 14 tasks from the written rules alone. They agreed on 12 and reported having to guess on 5 to 9 of them. Both found the same flaws: difficulty and risk were mixed into one number, so a one-statement production delete was assigned a low tier with no safety step. The triage was rewritten in response, separating the difficulty tier from the risk gate.
- **Triage retest.** Two new agents tiered the same 14 tasks from the rewritten rules. They agreed with each other on all 14 tiers and all 14 risk-gate decisions, and the production delete and the team email were both gated. They still had to guess on some tasks where the task left a deciding fact unstated, and they found further gaps (the boundary between Tiers 1 and 2, what counts as a shared system, an overstated definition of "reversible"). Those were fixed afterwards; that last round of fixes has not itself been retested.
- **Documentation check.** The install instructions and the model notes were checked against the official pages. Several claims were corrected.

Not yet tested: any task actually run at Tier 3, 4 or 5, non-coding work, other models, and the final round of triage wording.

## Limits

- It is guidance, not enforcement. The agent can still misjudge a tier; tell it to re-triage when it does.
- It adds a small amount of context to every task where it triggers.
- It reflects one write-up of good practice and one model generation. Revise it as tools and models change.
- It never overrides your own instructions, project rules, or safety confirmations.

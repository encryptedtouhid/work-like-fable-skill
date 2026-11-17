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

### 1. Triage

The agent scores the task on five questions: how many unknowns, how wide the blast radius, how reversible, how clear "done" is, and how long the chain of dependent steps is. That gives a tier.

| Tier | Shape | Process |
|---|---|---|
| 1 Trivial | Known answer, one action | Check the one fact, do it, answer briefly |
| 2 Small | One location, clear outcome | Locate, change, run the nearest check, report |
| 3 Medium | Several files, some discovery | Define done, survey, list unknowns, short plan, execute with checks, verify, review, report |
| 4 Hard | Cross-cutting, design choices | Frame, map, attack the hard part first, compare approaches, confirm direction, staged execution, end-to-end verification, independent review |
| 5 Very hard | Ambiguous goal, research needed | Separate facts from assumptions, sharpen the question, budgeted research, competing hypotheses, cheapest discriminating test, spike, checkpoint, then continue as Tier 4 |

Three rules keep triage honest: start at the lower tier when unsure and escalate at the first surprise; raise the tier by one for anything irreversible or outward-facing; re-triage when the task turns out different from how it looked.

### 2. Run the tier

Tiers 1 and 2 run straight from the table. Tiers 3 to 5 follow the numbered steps in `references/working-method.md`.

### 3. Load only what the situation needs

The skill uses progressive disclosure. `SKILL.md` is short and always loaded when the skill triggers. Each reference file is read only when its situation comes up: delegating, handling untrusted content, writing the report, and so on. This keeps the context small.

### Invariants

At every tier: solve the request that was made, check claims against the real thing, use the smallest process that gives a correct result, confirm before destructive or outward-facing actions, verify by running, and lead every report with the outcome. The user's own rules and the project's conventions always take precedence over the skill.

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

Zip the `work-like-fable` folder so that the folder itself is the top level of the archive, then upload it where your plan lets you add custom skills.

```bash
cd work-like-fable-skill && zip -r work-like-fable.zip work-like-fable
```

Start a new session after installing. The folder name must stay `work-like-fable` to match the `name` field in `SKILL.md`.

## Use

### Automatic

The skill's description tells Claude to load it at the start of a non-trivial task, when a task turns out harder than expected, or when you say things like "think it through", "do this properly", "plan and execute" or "research and implement".

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

**Use it with other models or agent frameworks.** The files are plain Markdown. Put `SKILL.md` in a system prompt and supply the reference files on demand, or load the folder as a skill in the Claude Agent SDK. `references/model-notes.md` is specific to Fable 5.2; replace or drop it for another model.

**Use it as a team standard.** The playbook doubles as a checklist for people: a shared definition of what "verified" and "done" mean at each level of difficulty.

## Customise

- **Tier thresholds:** edit the triage table in `SKILL.md` and section 2 of `references/working-method.md`.
- **Your own rules:** add a reference file (for example `references/team-conventions.md`) and a row for it in the "Step 3" table in `SKILL.md`.
- **Trigger behaviour:** edit the `description` in the `SKILL.md` frontmatter; that text decides when Claude loads the skill.
- **Keep `SKILL.md` short.** Detail belongs in `references/`, so it is loaded only when needed.

## Limits

- It is guidance, not enforcement. The agent can still misjudge a tier; tell it to re-triage when it does.
- It adds a small amount of context to every task where it triggers.
- It reflects one write-up of good practice and one model generation. Revise it as tools and models change.
- It never overrides your own instructions, project rules, or safety confirmations.

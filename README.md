# work-like-fable

A Claude skill that teaches an agent to match its effort to the task and to be careful with risky actions. It is modelled on the working habits of Claude Fable 5.2 in Claude Code.

> **Not an official Anthropic document.** It is an independent write-up of working practices, not a description of the model's internals. Official model information: https://www.anthropic.com/claude/fable

## What it does

The skill makes two separate judgements about every task.

**1. How difficult is it?** This sets how much process to use.

| Tier | Shape | Process |
|---|---|---|
| 1 Trivial | One direct answer or one mechanical action | Check the one fact, do it, answer briefly |
| 2 Small | One decision in one function or small region of a file | Locate, change, run the nearest check, report |
| 3 Medium | Several files, or one cause to find that can be reproduced | Define done, survey, short plan, execute with checks, verify, report |
| 4 Hard | Crosses layers or systems, or needs a choice between real approaches | Frame, compare approaches, confirm direction, staged execution, end-to-end verification, independent review |
| 5 Very hard | The goal is unclear, or the cause cannot be reproduced | Sharpen the question, research, competing hypotheses, cheapest test, checkpoint, then continue as Tier 4 |

**2. Is any step risky?** A step is risky if it deletes something that cannot be put back, changes production or a shared system, sends something to other people, or spends money. For that step the agent:

1. states exactly what will be affected,
2. prepares a way back (a backup, a dry run, a draft),
3. acts only on what the request clearly covers, holds back anything doubtful, and asks,
4. verifies the result.

It also keeps the agent inside the request: see a bug fail before fixing it, and report a related problem found elsewhere instead of fixing it unasked.

## Install

**Claude Code, all projects**

```bash
git clone https://github.com/encryptedtouhid/work-like-fable-skill.git
cp -R work-like-fable-skill/work-like-fable ~/.claude/skills/
```

**Claude Code, one project** (shared with the team through git)

```bash
mkdir -p .claude/skills
cp -R /path/to/work-like-fable-skill/work-like-fable .claude/skills/
```

**Stay in sync with this repo:** symlink instead of copying.

```bash
ln -s /path/to/work-like-fable-skill/work-like-fable ~/.claude/skills/work-like-fable
```

**Claude apps:** zip the folder (`zip -r work-like-fable.zip work-like-fable`), then go to **Customize > Skills > + > Create skill > Upload a skill**. See [Using Skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude).

**Claude Agent SDK:** put the folder in `<project>/.claude/skills/` or `~/.claude/skills/`. See the [Agent SDK skills documentation](https://code.claude.com/docs/en/agent-sdk/skills.md).

Claude Code picks up a new skill in the running session; no restart is needed.

## Use

Claude loads the skill by itself at the start of non-trivial work. To call it directly:

```
/work-like-fable Migrate the session store from Redis to Postgres
```

Ways to get more from it:

- **Say what "done" means.** One sentence of success criteria lets the agent pick the right tier.
- **Name the tier** when you know it: "treat this as Tier 4", or "Tier 1, just do it".
- **Ask for one stage:** "triage this and tell me the tier", "give me the approaches and your pick".
- **Use a playbook:** "follow the debugging playbook". There are thirteen, in `references/domain-playbooks.md`.
- **Make it a project default:** add `For any non-trivial task, use the work-like-fable skill.` to the project's `CLAUDE.md`.
- **Use it with other models:** the files are plain Markdown. Lines marked **(Claude Code)** can be skipped elsewhere.

## What is in the repo

```
work-like-fable/
├── SKILL.md                    Tiers, risk gate, and when to read each reference
└── references/
    ├── working-method.md       Numbered steps for each tier
    ├── domain-playbooks.md     Step lists for 13 kinds of work
    ├── delegation.md           Using subagents
    ├── long-running-work.md    Long tasks, context and memory
    ├── trust-and-safety.md     Untrusted content, confirmation, privacy
    ├── communication.md        Replies, questions, briefs
    ├── tooling.md              Files, shell, git, web, tests
    └── model-notes.md          Fable 5.2 facts, labelled by source
```

## Test results

Tested on 2026-11-17 with fresh Claude subagents, with and without the skill, scored from the files each run left on disk. Samples are small: two or three runs per cell.

| Test | Opus 5.5 | Sonnet 5.5 | Haiku 5.5 |
|---|---|---|---|
| Triage of 14 tasks | Correct | Correct | Correct |
| Risky delete with an unstated hazard | Already careful | Already careful | **Fixed:** wrong 3 of 3 without the skill, right 6 of 6 with it |
| Bug fix with the same fault in a second file | Already correct | **Fixed:** stopped changing the second file unasked, started writing failing tests first | **Fixed:** stopped changing the second file unasked |
| Log cleanup and bulk email, hazard written down | Not run | No difference | No difference |

In short:

- The skill helps smaller models most. Opus already works this way.
- It helps when the danger is not written down and has to be inferred. When a rule is stated plainly, models follow it without the skill.
- It did no harm in any test.

Known gaps:

- In the delete test, every Sonnet and Haiku run also removed 13 orders where "older than two years" could be read two ways. Only Opus held those back.
- No task was run at Tier 3, 4 or 5, and no non-coding work was tested.
- The install steps and model notes were checked against the official pages through an automated summary, so wording may not be exact.

## Customise

- **Tiers and risk gate:** edit `SKILL.md`.
- **Your own rules:** add a file under `references/` and a row for it in the last table of `SKILL.md`.
- **When it triggers:** edit the `description` in the `SKILL.md` frontmatter.

## Limits

- It is guidance, not enforcement. Tell the agent to re-triage if it picks the wrong tier.
- It adds a small amount of context to each task where it loads.
- Your own instructions, project rules and safety confirmations always take precedence.

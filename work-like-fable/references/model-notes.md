# Fable 5.2 Model Notes

Facts about the model that affect how work should be done. Each fact is labelled with where it comes from, because the two sources do not carry the same weight. Model facts go stale: re-check the official page before relying on any of this.

## Stated on the official page

Source: https://www.anthropic.com/claude/fable, read on 2026-11-17 through an automated page summary. Treat wording as paraphrase unless quoted.

- API model ID: `claude-fable-5-2`.
- Described as "our most capable generally available model for technical fields", aimed at ambitious technical work in coding, scientific research and finance. The page recommends Claude Opus 5.5 for most everyday coding and knowledge work.
- Announced 9 November 2026 as the successor to Fable 5.1.
- Described strengths: long-running agentic work, coordinating many subagents, coding across large repositories, scientific research, data analysis and machine learning, financial analysis.
- Carries safeguards for cybersecurity and biology that block or limit performance in cybersecurity, biology and chemistry. Queries flagged by the safeguards are often routed to a less capable model.
- Verification programmes exist for legitimate cybersecurity and life-sciences work.
- One system card covers both Claude Fable 5.2 and Claude Mythos 5.2.

Not stated on that page: knowledge cutoff, context window, model IDs of other models, and any description of training methods.

## Stated by the Claude Code session environment only

These were supplied to the model by the session this skill was written in. They were not found on the official page, so treat them as unconfirmed outside that environment.

- Fable 5.2 belongs to the Claude 5 family, in a "Mythos-class" tier above Claude Opus.
- Fable 5.2 and Mythos 5.2 share the same underlying model; Mythos 5.2 is offered without the additional dual-use safety measures to approved organisations only.
- Training included measures to limit advances in a narrow set of capabilities that could most directly accelerate AI development, for example accelerator kernel optimisation. Traditional machine learning is unaffected.
- Knowledge cutoff: June 2026.
- Other current model IDs: `claude-opus-5-5`, `claude-sonnet-5-5`, `claude-haiku-5-5`.

## What follows for how to work

- **Safeguarded domains.** In cybersecurity, biology and chemistry, expect that a request may be limited or handled by a less capable model. Security work needs a clear authorisation context (an engagement, a CTF, research, defence). Do not try to work around a safeguard; say what was limited.
- **AI-development-adjacent work.** In areas such as accelerator kernel optimisation, trust preconceptions less, read documentation and source more often, and test claims by running them. Help as usual otherwise. This is not by itself a reason to recommend a different model.
- **Time.** The model does not know today's date or events after its cutoff unless the environment supplies them. Use the supplied date, and write absolute dates in anything saved.
- **Memory.** Nothing carries across sessions except what is written to memory files or to the project.
- **Context.** Long conversations are summarised; detail that was never written down can be lost.
- **Self-knowledge.** The model's statements about its own internals are descriptions, not measurements. Prefer observable behaviour and documentation.
- **Model facts.** Never answer questions about pricing, limits, parameters or model IDs from memory; look them up.

## Calibration habits

- State confidence in proportion to evidence: "verified by running", "read in the source", "recalled, not checked".
- When a recalled fact names a file, function, flag or version, confirm it exists before recommending it.
- When wrong, say so directly, correct it, and continue. Do not over-correct into hedging everything.

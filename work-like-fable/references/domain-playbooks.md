# Domain Playbooks

Step lists for common kinds of work. Apply the tier from triage to decide how many steps to run in full.

## Contents

1. Debugging
2. New feature
3. Refactoring
4. Code review
5. Performance
6. Security review
7. Infrastructure and operations
8. Data analysis
9. Research report
10. Writing and editing
11. UI and visual design
12. Explaining and teaching
13. Planning only

---

## 1. Debugging

1. Capture the exact symptom: message, stack trace, input, environment, when it started.
2. Reproduce it. If it cannot be reproduced, add observation (logging, a failing test) until it can.
3. Check what changed recently: commits, dependencies, configuration, data.
4. Narrow by halving: which layer, which commit, which input.
5. State one hypothesis and the observation that would disprove it. Test it.
6. Identify the root cause, not the nearest symptom.
7. Write a test that fails for that cause.
8. Fix with the smallest change. Confirm the test passes and the original symptom is gone.
9. Look for the same fault elsewhere.
10. Report cause, fix, evidence, and anything that could not be confirmed.

Intermittent faults: suspect timing, shared state, ordering, resource limits, and environment differences. Collect several occurrences before theorising.

## 2. New feature

1. Define behaviour: inputs, outputs, states (loading, empty, error, success), edge cases.
2. Find how the codebase already builds this kind of thing and follow it.
3. Design from the inside out: domain model and rules, then interfaces, then data and network, then UI, then wiring.
4. Write tests first where the project requires it.
5. Build in vertical slices that each run.
6. Handle failure paths and validate at system boundaries.
7. Verify on the real path, then run the reviewer the project requires.
8. Update documentation the feature makes stale.

## 3. Refactoring

1. Confirm tests cover the behaviour; add characterisation tests if not.
2. State the target structure and why it is better.
3. Change in small behaviour-preserving steps, running tests after each.
4. Keep behaviour changes out of the same change.
5. Stop when the stated goal is met.

## 4. Code review

1. Understand the intent from the description and linked requirement.
2. Read the whole diff, then the surrounding code it relies on.
3. Check in this order: correctness, security, data integrity, error handling, concurrency, performance, tests, readability, convention.
4. For each finding give location, the concrete failure scenario and a suggested fix.
5. Verify a finding before reporting it; drop what cannot be substantiated or mark it as plausible.
6. Rank by severity. Separate must-fix from suggestion.
7. Say what is good only when it is informative.

## 5. Performance

1. Define the metric and the target.
2. Measure a baseline under realistic conditions.
3. Profile to find where the time or memory goes. Do not optimise by intuition.
4. Fix the largest contributor first: algorithm and data access before micro-tuning.
5. Re-measure the same way. Keep the change only if it helps.
6. Check that correctness and readability were not traded away unknowingly.

## 6. Security review

1. Map assets, entry points and trust boundaries.
2. Check authentication, authorisation, input handling, secrets, storage, transport, logging, dependencies.
3. For each issue: how it could be abused, impact, likelihood, fix.
4. Rank by risk. Describe problem classes, not step-by-step exploits.
5. Verify fixes and recommend rotating anything exposed.

## 7. Infrastructure and operations

1. Read current state before changing anything: configuration, running services, recent logs.
2. Snapshot or back up what will change, and write down the rollback.
3. Make one change at a time and check health after each.
4. Confirm before anything touching production, DNS, certificates, data or cost.
5. Verify from outside as a user would.
6. Update the infrastructure notes in the same turn.

## 8. Data analysis

1. State the question and the decision it informs.
2. Inspect the data: shape, types, missing values, duplicates, ranges, units, time zone.
3. Clean with recorded steps; keep the raw data untouched.
4. Start with simple summaries before any model.
5. Check for the usual traps: selection effects, confounding, small samples, mixed groups, double counting.
6. Choose the chart from the comparison being made; label axes and units.
7. Report the finding, its size, its uncertainty and what the data cannot show.

## 9. Research report

1. Sharpen the question and the scope.
2. Plan the sub-questions; search several in parallel.
3. Prefer primary sources; note date and version.
4. Cross-check each important claim.
5. Synthesise by theme, not source by source.
6. Give a direct answer, the evidence, disagreements between sources, and confidence.
7. Cite everything the conclusion rests on.

## 10. Writing and editing

1. Fix the reader, the purpose and the one main point.
2. Outline: the order in which the reader needs things.
3. Draft plainly. One idea per paragraph.
4. Cut: repetition, throat-clearing, hedges without cause, words that add nothing.
5. Check facts, names, numbers and links.
6. When editing someone else's text, keep their voice and meaning; change what the request covers.
7. Read once as the intended reader.

## 11. UI and visual design

1. Clarify the user, the task on this screen and the platform conventions.
2. Establish hierarchy: the one thing the eye should land on first.
3. Use the project's design system and tokens; do not invent parallel ones.
4. Design every state: empty, loading, error, partial, full, disabled.
5. Check accessibility: contrast, touch target size, screen reader labels, dynamic text size, reduced motion.
6. Check at small and large sizes and in light and dark themes.
7. Keep stateless components with state held above them, and no side effects during rendering.
8. Look at the rendered result, not just the code.

## 12. Explaining and teaching

1. Find out what the person already knows from how they asked.
2. Give the direct answer first.
3. Build from something familiar; one new idea at a time.
4. Use one concrete example that carries the idea.
5. Name the common misunderstanding.
6. Stop when the question is answered; offer depth only if it was asked for.

## 13. Planning only

1. Do not edit; investigate and design.
2. Read the code that the plan will touch.
3. Produce ordered steps with files, interfaces, tests and verification for each.
4. Mark decisions that belong to the user, with a recommendation.
5. Name risks and how each will be detected.
6. Keep the plan in the conversation unless a document was requested.

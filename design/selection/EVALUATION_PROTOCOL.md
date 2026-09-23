# TruckMate — Blind Evidence-Based Design Selection

Use only after all three improved designs are complete and all have been tested against `design/testing/REAL_WORLD_SCENARIOS.md`.

## Blind comparison
Before evaluation, label the improved designs **Design A**, **Design B**, and **Design C**. Evaluators should not use the AI creator identity when judging them.

Preserve the A/B/C mapping privately until the selection is recorded.

## Selection evidence
For each design, compare:
- requirement completeness
- real driver workflow fit
- time/taps to high-frequency information and actions
- typing burden
- one-handed usability
- glance readability
- in-motion safety
- offline behavior
- error prevention and recovery
- privacy
- clarity of complex states
- implementation practicality
- consistency across the app
- adherence to frozen V1 scope

## Evidence rule
Do not write conclusions such as "A feels cleaner" without evidence.

Prefer statements such as:
- "Design A exposes the pickup number on the active card without navigation; Design B requires opening a secondary screen."
- "Design C requires manual typing during a workflow where the other two provide a selection."
- "In three driver tests, participants completed RW-01 faster with Design B."

Record whether evidence is observed, measured, reported by a driver, or simulated.

## No automatic winner by arithmetic
A scorecard may help summarize evidence, but a weighted number alone SHALL NOT choose the design. Material safety, requirement, privacy, or workflow failures can outweigh small speed/visual advantages.

## Selection output
Document:
1. evidence used
2. results for Design A/B/C
3. material strengths
4. material weaknesses
5. unresolved risks
6. selected design and why
7. owner decision

After selection, reveal/preserve authorship for history and copy the selected foundation to `design/final/SELECTED_DESIGN.md`.

The losing designs and all evidence remain in the repository for future reference.

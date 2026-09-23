# TruckMate — Shared Design Brief

This brief is the identical starting point for GPT, Claude, and Gemini.

## Authoritative inputs
Before creating an independent design, read:
1. `AGENTS.md`
2. `08_APPROVAL_STATUS.md`
3. `06_AI_HANDOFF.md`
4. `04_CURRENT_WORK.md`
5. `02_MASTER_DECISIONS.md`
6. `01_MASTER_SRS.md`
7. `03_FEATURE_BANK.md`

If these files conflict, follow the repository source-of-truth hierarchy in `AGENTS.md`.

## Design objective
Create the clearest, fastest, safest, most practical V1 UX for real CDL drivers and owner-operators while preserving every approved V1 requirement.

## Non-negotiable principles
- Clean. Easy. Simple.
- Collect once, use everywhere.
- Current Load Card is the center of the working-day experience.
- Minimize taps, typing, scrolling, and memory burden.
- Optimize for one-handed mobile use and quick glance comprehension.
- Do not require typing-heavy interaction while driving.
- Respect offline/poor-network operation.
- Preserve privacy and user control.
- Do not add new V1 features.
- Do not remove approved V1 behavior merely to simplify the interface.

## Independence rule
During the first design round, do not read another AI's independent design. Each AI must solve the same requirements independently.

## Expected design output
The design should make the major screens, hierarchy, navigation, states, primary actions, edge cases, and transitions understandable enough that another designer or developer can implement it without inventing product behavior.

Where multiple UX approaches are reasonable, explain the tradeoff and choose one.

## No brand advantage
Do not optimize for the preferences or style of GPT, Claude, or Gemini. The design will later be judged without AI identity.

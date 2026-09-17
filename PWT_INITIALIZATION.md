# TruckMate — PWT Initialization

TruckMate uses the PWT operating model.

## Adaptive structure rule
PWT does **not** require a fixed number of SRS sections. The project structure must match actual complexity. A small project may need roughly 10 sections; a large one may need 60–90+; another project may need a different number. Never create filler sections to satisfy a count.

## Initialization checklist
- [x] Set project identity: TruckMate.
- [x] Define mission, target users, boundaries, and principles.
- [x] Create initial consolidated requirements from existing project discussions.
- [x] Record explicit decisions separately from open questions.
- [x] Establish feature bank and scope statuses.
- [x] Define current lifecycle phase and milestone.
- [x] Establish canonical AI instructions and Claude/Gemini entry points.
- [x] Identify initial architecture direction.
- [x] Identify privacy, offline, failure/recovery, and data-control gaps requiring review.
- [x] Independent AI gap review. (Claude, 2026-09-17 — see `06_AI_HANDOFF.md`)
- [ ] Project-owner reconciliation of review findings.
- [ ] Establish first approved project-specific baseline commit.
- [ ] Begin UX/design after requirements approval.

## Gap-analysis rule
Classify findings as:
- Must Have Now
- Architecture Must Allow Later
- Optional Future

## Security rule
No secrets, API keys, passwords, tokens, or private credentials may be committed.

## Completion condition
Initialization is complete only when the repository contains enough trustworthy context that a new AI can continue without the original setup conversation and the owner has approved the project-specific baseline.

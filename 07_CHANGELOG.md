# TruckMate — Changelog

## 2026-09-21 — Gemini gap review reconciled; V1 scope frozen
- Reconciled TM-Q021–TM-Q028 and recorded TM-D040–TM-D049.
- Limited V1 trailer types to Dry Van + Reefer, with trailer-type-specific fields/checks.
- Added mid-load truck/trailer swap history and Quick Trailer Check with skip reason.
- Added limited reefer fields: set point, operating mode, reefer fuel level, unit/alarm status, optional actual temperature.
- Added flexible Critical Load Instructions to avoid niche-field bloat.
- Added out-of-order Active Stop selection without fake completion; drag-and-drop is not required as the primary flow.
- Added configurable detention threshold with a 2-hour default when unknown and a local 15-minute warning.
- Defined sync-conflict rule: manual user corrections outrank AI/inference; material manual conflicts retain provenance/recovery rather than trusting device time alone.
- Separated operational, paperwork, and settlement status dimensions.
- Rejected a separate Gate Pass screen for V1; gate/check-in references stay prominent on the Current Load Card.
- Declared the V1 requirements frozen for UX/design; only TM-Q012 public naming remains intentionally open.

## 2026-09-21 — Owner closes pre-design requirements
- Resolved TM-Q001–TM-Q011 and TM-Q013; only public naming TM-Q012 remains intentionally open.
- Adopted offline-first V1 and offline check-in/out with provenance/safe sync.
- Set standard + custom wallet documents and Quick + Detailed PTI.
- Defined load-based settlement reconciliation across settlement weeks.
- Separated carrier-paid, route-estimated, and driver-adjusted mileage with audit preservation.
- Defined 30-day Trash/permanent deletion and flexible Excel + original-document export.
- Defined document-driven load stage changes with manual override.
- Added partial/full rejection, disposition-stop, and additional-compensation rules on the same load.
- Replaced live nearby-driver help with opt-in, timestamped facility intelligence from TruckMate drivers.
- Set 60-day full-feature trial and $59.98/month target.
- Confirmed TruckMate cannot be the public product name; replacement remains open.
- Defined legal/compliance claim boundary.
- Defined unified truck/business expense capture without mandatory load linkage, including recurring overhead and reporting split.

## 2026-09-18 — Owner reconciliation of Claude review
- Reconciled TM-Q014–TM-Q020.
- Made multi-stop loads V1 core.
- Added Current + Upcoming/Pre-planned Loads and explicit document-to-load attribution.
- Added system-captured vs manual/edited check-in/out timestamp provenance.
- Expanded each load package with flexible Other Load Documents.
- Made Load History + search base V1 functionality.
- Added in-motion interaction guardrails.
- Added architecture-later protection for team-driver/shared-load access and AI usage/cost controls.
- Kept TruckMate as the working/project name only; public branding remains unresolved under TM-Q012.
- Updated handoff/current-work/approval state. Final baseline approval still waits on remaining material TM-Q001–TM-Q013 decisions.

## 2026-09-17 — Claude independent gap review
- Repaired repository git history (the checked-in `.git` directory was incomplete; no prior commits existed).
- Completed the independent gap review requested in `06_AI_HANDOFF.md`. Classification: REPAIR THEN APPROVE.
- Recorded 7 new open questions (TM-Q014–TM-Q020) covering multi-stop loads, concurrent active loads, check-in/out timestamp provenance, a missing load-document category, Load History/search scope, in-motion interaction/liability constraints, and a real trademark-collision risk with an existing industry TMS product also named TruckMate.
- No redesign proposed; existing scope boundaries (§26 deferred list) confirmed sound as-is.

## 2026-09-16 — Initial PWT baseline
- Initialized TruckMate PWT working rules.
- Established adaptive SRS rule: section count follows project complexity.
- Added project brief, master decisions, feature bank, Master SRS, current work, open questions, and AI handoff.
- Captured Current Load Card, load-document workflow, detention evidence, shareable status, Essentials wallet, expiration reminders, PTI, mileage/deadhead, Pro financials, privacy, offline concerns, and current technical direction.
- Prepared repository for independent Claude gap review.

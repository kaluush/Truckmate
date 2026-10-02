# TruckMate — Changelog

## 2026-10-01 — V1 data model draft for review
- Added `specs/data-model/DATA_MODEL.md` (DRAFT v0.1, not approved): cross-cutting data rules, four relationship diagrams and a data dictionary expanding SRS §25. No new product behavior; every field traces to the SRS or a decision.
- Added `specs/data-model/REVIEW_TEMPLATE.md`: reviewers must answer what is missing, overcomplicated, wrongly related, needs flexibility (only for recorded future needs), could cause migration pain, and what to change before implementation, plus walk RW-01–RW-12 through the model.
- Opened TM-Q029 for data-structure questions DM-Q01–DM-Q07.

## 2026-10-01 — V1 load photo evidence fixed
- Added TM-D078 and TM-F068 as a field-validated repair to the existing load-document/evidence workflow.
- Photos are captured at **pickup or delivery** and typed as **load/cargo, seal, temp, or other**.
- Capture time and GPS location are recorded automatically when available; a short note is optional.
- Photo evidence is attributed to the correct load stop so the model works cleanly with multi-stop loads.
- Updated the Master SRS, Current Work, AI Handoff, and approval baseline accordingly.

## 2026-09-26 — Gemini Stage 1 Independent UX Design Complete
- Completed Gemini's independent UX design for CabPilot V1 in `design/independent/gemini/DESIGN_V1.md`.
- Created an interactive clickable HTML mockup of CabPilot V1 in `design/independent/gemini/CabPilot_V1_Gemini_Screens.html` using Vanilla CSS/JS.
- Updated `04_CURRENT_WORK.md` to indicate all three independent Stage 1 designs are now complete.

## 2026-09-26 — AGENTS freeze rule updated
- `AGENTS.md` Final V1 freeze rule now states the baseline is frozen through TM-D077 (was TM-D055) and that TM-D057–TM-D077 are owner-approved rulings. Wording only.

## 2026-09-26 — Trial billing timing clarified
- Added TM-D077 (clarifies TM-D073/TM-D076): day-10 message is a notice only; add-payment step opens at day 14 so the store's 2-week trial starts when the no-card period ends; charge-related screens show the calendar date of the first charge instead of a day count.
- Updated SRS §24, TM-F036, and the AI handoff (early-charge implementation note resolved).

## 2026-09-26 — Follow-up owner rulings
- Aligned SRS §8 (Onsite timer wording), §12 (no automatic missing-detention flag), §17 (five notification classes + no-input rule) and §37 (phone-number change) with TM-D058, TM-D059, TM-D062 and TM-D068. Wording alignment only.
- Added TM-D075: O/O → Company Driver downgrade at next billing date; O/O-only data kept hidden, never deleted, restored on re-upgrade. SRS §24 and §39 updated.
- Added TM-D076 (clarifies TM-D073): second trial period is 2 weeks, 28 days total, to fit App Store trial durations. SRS §24, TM-F036 and TM-F065 updated.

## 2026-09-26 — Owner rulings on Claude Stage 1 design issues
- Added TM-D057–TM-D074; all 15 issues flagged in `design/independent/claude/DESIGN_V1.md` resolved.
- TM-D057: UI labels **Reminders** (due items) and **Next Loads** (pre-planned loads); "Upcoming" retired as a UI label.
- TM-D058: stop timer labelled **Onsite**, starts at arrival, shows "Appt [time]"; neutral reminders; no automatic missing-detention flag in reconciliation (clarifies TM-D046).
- TM-D059: no in-motion restriction or driving mode in V1; notifications never require input (clarifies TM-D021). Driving view and fleet-admin in-motion lock moved to V2 (TM-F066, TM-F067).
- TM-D060: Company Driver / O/O naming everywhere; expenses available to both tiers; SRS §2/§4/§12/§18 terminology updated.
- TM-D061: TM-F016, TM-F019, TM-F022, TM-F027 promoted to V1 Core.
- TM-D062: phone-number change flow (OTP to new number + old number or verified email, else support/recovery).
- TM-D063–TM-D071: Claude's assumptions approved for issues #3, #6, #7, #8, #9, #10, #11, #14, #15; SRS §5/§18 literal "\n" fixed.
- TM-D072: new tiered pricing (Company Driver from $14.99/mo; O/O from $29.99/mo; 3- and 6-month plans). TM-D073: 14-day no-card trial + 16 more days with payment method. TM-D035 marked SUPERSEDED; SRS §24 updated.
- TM-D074: launch policy — first 20–50 drivers may get free access for feedback via existing admin controls.
- Restored the missing TM-D056 row in `02_MASTER_DECISIONS.md`.
- Added `design/independent/claude/OWNER_RULINGS_2026-09-26.md`; independent design files left unedited.


## 2026-09-23 — CabPilot public brand selected
- Resolved TM-Q012.
- Added TM-D056.
- Approved public product/store name: **CabPilot**.
- Approved domain: **cabpilotapp.com**.
- Preserved TruckMate as the repository/internal working name for continuity/history.
- Marked prior open-brand decisions TM-D001 and TM-D036 superseded where they conflicted with the resolved public name.


## 2026-09-23 — Final V1 additions synchronized
- Added TM-D050–TM-D055 and TM-F061–TM-F065.
- Added one unified Upcoming due/reminder area for one-time dates and recurring obligations; distinguished it from Upcoming/Pre-planned Loads.
- Added phone OTP + optional email/no-password V1 authentication and the no-automatic-recovery boundary when both access channels are lost.
- Added the lean admin/operations panel: Dashboard, Users, Subscriptions, Support/Recovery, Controls.
- Added simple Owner/Admin/Support roles and the rule that administrators cannot open private driver document contents.
- Added Company Driver and O/O working tiers with tier entitlement controls.
- Reaffirmed that Stripe/selected billing provider remains the billing source of truth when monetization is implemented.
- Declared the final V1 scope lock: all new feature ideas now go to V2 unless required to repair an approved requirement.


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

# TruckMate — AI Handoff

## Current state
TruckMate's final V1 requirements baseline is owner-approved through **TM-D082**. The previous baseline was extended by the owner's final additions and the public-brand decision. TM-Q012 is resolved: the public product name is **CabPilot** and the approved domain is **cabpilotapp.com**.

## Owner rulings — 2026-09-26 (TM-D057–TM-D074)
All 15 Claude Stage 1 design issues are resolved; see `design/independent/claude/OWNER_RULINGS_2026-09-26.md`. Key points for the next design round:
- UI labels: **Reminders** (due items) and **Next Loads** (pre-planned loads); never "Upcoming" in the UI.
- Stop timer labelled **Onsite**, starts at arrival, shows "Appt [time]"; never "Detention"; reconciliation does not auto-flag missing detention from it.
- **No in-motion restriction or driving mode** in V1. Notifications may arrive while moving but must never require input to dismiss/continue.
- Tiers are Company Driver / O/O everywhere; expenses available to both tiers.
- Pricing and trial replaced (TM-D072/TM-D073/TM-D076/TM-D077): 14 days no card (day-10 notice only), add-payment step at day 14 unlocks 2 more weeks (28 days total), no charge during the trial; charge screens show the calendar date of the first charge.
- Downgrade O/O → Company Driver takes effect at next billing date; O/O data kept hidden, never deleted (TM-D075).
- SRS §8, §12, §17, §24, §37 and §39 now match these decisions.

## Owner rulings — 2026-10-02 (TM-D080–TM-D082)
- **Cancelled** is an operational status; cancelled loads stay in history and TONU pay can be matched (TM-D080).
- In-app account deletion: export offered first, private data deleted, facility reports kept anonymously, store-subscription cancellation explained (TM-D081).
- Deleting a load sends it and all its evidence to the 30-day Trash; small records delete immediately with Undo (TM-D082).

## Owner ruling — 2026-10-02 (TM-D079)
- Marking a bill reminder Done offers "Log as expense?" pre-filled from the reminder; never created automatically. Resolves data-model DM-Q03.

## Owner ruling — 2026-10-01 (TM-D078)
- V1 load-photo evidence is fixed: stage = pickup or delivery; type = load/cargo, seal, temp, or other; capture time and GPS are automatic when available; short note optional; evidence attaches to the correct load stop. This is treated as a field-validated repair to the existing load-document/evidence workflow, not a new standalone module.

## Data model — DRAFT v0.2, 2026-10-02 (NOT APPROVED)
- `specs/data-model/DATA_MODEL.md` expands SRS §25 into cross-cutting rules, four Mermaid relationship diagrams and a data dictionary. The dictionary is the source of truth; diagrams are derived from it and must change in the same commit. All four diagrams were checked to parse with Mermaid 11.
- v0.2 merges both reviews (`specs/data-model/reviews/gpt.md`, `gemini.md`) and owner decisions TM-D079–TM-D082. All DM-Q01–DM-Q07 are resolved. One finding was rejected with a reason (Gemini `MILEAGE_PAY`). See the model's §8 change log.
- **Owner-set review order:** (1) **Gemini** — confirmation review of v0.2 DONE (`specs/data-model/reviews/gemini-v0.2.md`): no blockers; its one diagram fix applied as v0.2.1. (2) **GPT** — last and final review of the resulting version, saved as `specs/data-model/reviews/gpt-final.md`. (3) Owner approval. Only after owner approval is the model project truth. Reviewers edit only their own review file.

## Final additions now in project truth
- **Upcoming due/reminders:** one unified flow for one-time expiration/due dates or recurring schedules; supports bills, expiring documents, maintenance and similar obligations; distinct from Upcoming/Pre-planned Loads.
- **Authentication:** phone OTP, optional email, no password; no automatic self-service recovery if both access channels are lost.
- **Admin/operations:** Dashboard, Users, Subscriptions, Support/Recovery, Controls.
- **Admin roles:** Owner, Admin, Support.
- **Privacy:** admin/support may use limited metadata for support but cannot open private driver document contents.
- **Tiers:** Company Driver and O/O; O/O covers owner-operators and percentage-paid drivers needing financial/business features; entitlements may be controlled by tier.
- **Scope:** no more V1 feature additions. New ideas go to V2 unless they repair a contradiction, security/privacy flaw, implementation blocker, or field-validated requirement gap.

## Previously approved V1 foundation (TM-D035 now superseded by TM-D072/TM-D073)
TM-D001–TM-D049 remain active, including Current Load Card, multi-stop loads, load-document package, offline-first behavior, check-in/out and detention evidence, PTI, wallet/expirations, mileage provenance, settlement reconciliation, unified expenses, export, Dry Van/Reefer behavior, equipment swaps, Quick Trailer Check, Critical Load Instructions, parallel statuses, and facility intelligence.

## Next AI assignment
Read `AGENTS.md`, `08_APPROVAL_STATUS.md`, `04_CURRENT_WORK.md`, then relevant authoritative requirements. Proceed to UX/design. Do not reopen settled decisions or add V1 features. Use **CabPilot** for customer-facing branding; the repository may remain named TruckMate.

## Owner principle
**Clean. Easy. Simple. Collect once, use everywhere.**

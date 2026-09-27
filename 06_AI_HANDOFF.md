# TruckMate — AI Handoff

## Current state
TruckMate's final V1 requirements baseline is owner-approved through **TM-D056**. The previous baseline was extended by the owner's final additions and the public-brand decision. TM-Q012 is resolved: the public product name is **CabPilot** and the approved domain is **cabpilotapp.com**.

## Owner rulings — 2026-09-26 (TM-D057–TM-D074)
All 15 Claude Stage 1 design issues are resolved; see `design/independent/claude/OWNER_RULINGS_2026-09-26.md`. Key points for the next design round:
- UI labels: **Reminders** (due items) and **Next Loads** (pre-planned loads); never "Upcoming" in the UI.
- Stop timer labelled **Onsite**, starts at arrival, shows "Appt [time]"; never "Detention"; reconciliation does not auto-flag missing detention from it.
- **No in-motion restriction or driving mode** in V1. Notifications may arrive while moving but must never require input to dismiss/continue.
- Tiers are Company Driver / O/O everywhere; expenses available to both tiers.
- Pricing and trial replaced (TM-D072/TM-D073/TM-D076/TM-D077): 14 days no card (day-10 notice only), add-payment step at day 14 unlocks 2 more weeks (28 days total), no charge during the trial; charge screens show the calendar date of the first charge.
- Downgrade O/O → Company Driver takes effect at next billing date; O/O data kept hidden, never deleted (TM-D075).
- SRS §8, §12, §17, §24, §37 and §39 now match these decisions.

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

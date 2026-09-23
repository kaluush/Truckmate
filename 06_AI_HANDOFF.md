# TruckMate — AI Handoff

## Current state
TruckMate's final V1 requirements baseline is owner-approved through **TM-D055**. The previous 2026-09-21 baseline was extended only by the owner's final additions: the unified Upcoming due/reminder module and the lean admin/operations layer. TM-Q012 (public replacement name) remains intentionally open.

## Final additions now in project truth
- **Upcoming due/reminders:** one unified flow for one-time expiration/due dates or recurring schedules; supports bills, expiring documents, maintenance and similar obligations; distinct from Upcoming/Pre-planned Loads.
- **Authentication:** phone OTP, optional email, no password; no automatic self-service recovery if both access channels are lost.
- **Admin/operations:** Dashboard, Users, Subscriptions, Support/Recovery, Controls.
- **Admin roles:** Owner, Admin, Support.
- **Privacy:** admin/support may use limited metadata for support but cannot open private driver document contents.
- **Tiers:** Company Driver and O/O; O/O covers owner-operators and percentage-paid drivers needing financial/business features; entitlements may be controlled by tier.
- **Scope:** no more V1 feature additions. New ideas go to V2 unless they repair a contradiction, security/privacy flaw, implementation blocker, or field-validated requirement gap.

## Previously approved V1 foundation
TM-D001–TM-D049 remain active, including Current Load Card, multi-stop loads, load-document package, offline-first behavior, check-in/out and detention evidence, PTI, wallet/expirations, mileage provenance, settlement reconciliation, unified expenses, export, Dry Van/Reefer behavior, equipment swaps, Quick Trailer Check, Critical Load Instructions, parallel statuses, and facility intelligence.

## Next AI assignment
Read `AGENTS.md`, `08_APPROVAL_STATUS.md`, `04_CURRENT_WORK.md`, then relevant authoritative requirements. Proceed to UX/design. Do not reopen settled decisions or add V1 features. Keep TM-Q012 open until the owner selects the public name.

## Owner principle
**Clean. Easy. Simple. Collect once, use everywhere.**

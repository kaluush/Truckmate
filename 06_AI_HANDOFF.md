# TruckMate — AI Handoff

## Current state
Owner reconciliation is complete for the material V1 requirements. On 2026-09-21, TM-Q001–TM-Q011, TM-Q013, and Gemini gap-review questions TM-Q021–TM-Q028 were resolved. TM-Q012 (public replacement name) is intentionally open. V1 scope is now frozen for UX/design unless contradictory field evidence exposes a real requirement gap.

Key accepted additions:
- Offline-first V1 wherever technically possible; offline check-in/out queues safely with provenance.
- Standard + custom expiring wallet documents.
- Quick and Detailed PTI in V1.
- Line-by-line load-based settlement reconciliation across settlement weeks.
- Carrier-paid vs route-estimated vs driver-adjusted miles with audit history.
- 30-day Trash then permanent deletion; flexible Excel + documents export.
- Pickup paperwork may set In Transit; signed POD/BOL may set Delivered; manual override remains available.
- Partial/full rejection stays on the same load, with exception/disposition stops and additional compensation.
- Facility intelligence comes from proactive opt-in driver reports, not live nearby-driver maps/requests.
- 60-day full-feature trial then $59.98/month target.
- TruckMate is working name only; public replacement name remains open.
- No claims of guaranteed legal/DOT/tax compliance, admissibility, or detention payment.
- Unified expense system does not require load linkage; recurring overhead and truck expenses are included in reports/export.
- Dry Van + Reefer are the V1 trailer types; type controls visible fields/checks.
- Mid-load equipment swaps preserve assignment history; trailer hook/swap prompts a Quick Trailer Check with skip reason.
- Reefer V1 captures set point, mode, fuel level, unit/alarm status, and optional actual temperature only.
- Critical Load Instructions provide a flexible home for seal/special-handling requirements without niche-field bloat.
- Multi-stop drivers may select an out-of-order Active Stop without fake completion.
- Detention thresholds are configurable; default 2 hours when unknown with a local 15-minute warning and no payment guarantee wording.
- Material sync conflicts preserve provenance/recovery; manual corrections outrank AI/inference.
- Operational, paperwork, and settlement statuses are separate dimensions.
- No separate Gate Pass screen in V1; surface gate references on the Current Load Card.

## Next AI assignment
Read AGENTS.md and authoritative files. Treat TM-D001–TM-D049 as project truth. Proceed to UX/design. The V1 scope is frozen: do not add features or reopen settled questions without contradictory field evidence. Keep TM-Q012 open until the owner selects a public name.

## Owner principle
**Clean. Easy. Simple. Collect once, use everywhere.**

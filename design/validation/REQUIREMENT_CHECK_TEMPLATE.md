# TruckMate — Independent Design Requirement Check

Use this only after an independent design is complete and before the three AIs read one another's designs.

Purpose: catch omissions or contradictions, not choose a winner.

## Rules
- Check the design against the approved V1 requirements.
- Reference the applicable SRS section, decision ID, or feature ID.
- Mark each item PASS, MISSING, CONFLICT, or UNCLEAR.
- Repair only missing/incorrect coverage.
- Do not add new features.
- Do not compare against another AI's design during this step.

## Check areas
- Current Load Card and stage-aware hierarchy
- Multi-stop and out-of-order Active Stop behavior
- Current vs Upcoming/Pre-planned Loads
- Load creation/import/scanning and document attribution
- Load document package and source-document retention
- Check-in/check-out, dwell, detention, provenance and sharing
- Offline operation and sync/conflict recovery
- Mileage/deadhead and provenance
- Quick/Detailed PTI
- Document Wallet and expiration behavior
- Unified Upcoming due/reminder items
- Expenses, revenue, settlements and export
- Dry Van/Reefer behavior, swaps and Quick Trailer Check
- Critical Load Instructions
- Partial/full rejection and disposition
- Facility intelligence
- Parallel operational/paperwork/settlement statuses
- Authentication and recovery boundary
- Company Driver/O/O entitlements
- Lean admin panel and admin privacy boundary
- Data deletion/privacy/security
- In-motion safety and accessibility
- Final V1 scope lock

## Result format
| Requirement / ID | Status | Where covered in design | Repair needed |
|---|---|---|---|
| Example: TM-D049 Gate references | PASS | Current Load Card / pickup state | None |

The independent design may be corrected for requirement coverage after this check, but its core UX concept should remain its own.

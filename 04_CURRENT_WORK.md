# TruckMate — Current Work

## Lifecycle phase
Final V1 requirements baseline owner-approved; UX/design may proceed. Public brand is **CabPilot**.

## Current milestone
Translate the final approved V1 into UX/design without adding product scope.

**Stage 1 status:** All three independent designs (GPT, Claude, and Gemini) are now complete. All 15 issues flagged by the Claude Stage 1 design are **RESOLVED** by owner rulings on 2026-09-26 (TM-D057–TM-D074; summary in `design/independent/claude/OWNER_RULINGS_2026-09-26.md`). Independent designs stay unedited; the rulings apply from the next design round onward.

## Current focus
1. Design the Current Load Card and multi-stop flow, including out-of-order Active Stop selection and prominent gate/check-in references without a separate Gate Pass screen.
2. Design the **Reminders** area (TM-D050) for one-time and recurring obligations, clearly separated from **Next Loads** (TM-D057); "Upcoming" is not a UI label.
3. Design the lean admin/operations panel: Dashboard, Users, Subscriptions, Support/Recovery, and Controls, with Owner/Admin/Support roles and no private document viewing.
4. Design trailer-type behavior, equipment swaps/history, Quick Trailer Check, reefer fields, Critical Load Instructions, offline-first behavior, settlement reconciliation, expenses, export, PTI, wallet, and facility intelligence using already-approved rules.
5. Apply the 2026-09-26 rulings: Onsite timer from arrival with "Appt [time]" (TM-D058); no in-motion restriction/driving mode and no input-requiring notifications (TM-D059); expenses for both tiers (TM-D060); phone-number change flow (TM-D062); new trial/pricing (TM-D072/TM-D073).
6. Apply the V1 load-photo evidence rule (TM-D078): pickup/delivery stage; load/cargo, seal, temp, or other type; automatic timestamp + GPS when available; optional short note; attach evidence to the correct stop.
7. **Data model — APPROVED v1.0 (2026-10-02):** `specs/data-model/DATA_MODEL.md` is the official V1 logical data model. Use it as a cross-check in the Stage 2 requirement check: screens and model must agree; any mismatch goes to the owner. Recommended next step: a paper test — map 5–10 real rate cons, BOLs and settlement statements onto the model to confirm every real field has a place.
8. Use **CabPilot** as the public product name and **cabpilotapp.com** as the approved domain; the repository may remain named TruckMate for continuity.

## Baseline state
Owner-approved project truth now includes TM-D084. The final V1 feature additions remain Upcoming due/reminders and the lean admin/operations layer. TM-Q012 is resolved: public product name **CabPilot**, domain **cabpilotapp.com**. No material V1 product question is intentionally open.

## Scope rule
**No more V1 feature additions.** New ideas go to V2 unless field evidence reveals a contradiction, security/privacy flaw, implementation blocker, or genuine gap in an already-approved requirement.

# TruckMate — AI Handoff

## What exists
TruckMate has been initialized from the PWT operating model using requirements developed from a working CDL driver's real workflow.

The current repository captures:
- Product mission/boundaries.
- Current Load Card as the central experience.
- Load document intake/extraction and BOL/POD package.
- Check-in/out, dwell/detention evidence, and copy/share status.
- Document wallet and expiration management.
- PTI direction.
- Deadhead/mileage and driver pay estimates.
- Pro owner-operator/percentage-driver financial layer.
- Privacy, offline, reliability, and architecture requirements.
- Deferred monetization/Gmail integration.

## Claude gap review — completed 2026-09-17

Reviewed all authoritative files against the special review focus below. Repo classification: **REPAIR THEN APPROVE** — the baseline is coherent, internally consistent, and correctly scoped away from ELD/marketplace/accounting overreach. It should become trusted V1 truth after the owner resolves a small set of concrete gaps (recorded as TM-Q014–TM-Q020 in `05_OPEN_QUESTIONS.md`), not a full redesign.

**Must Have Now (decide before this becomes the implementation baseline):**
- TM-Q015 — active/pre-planned multi-load handling and document-to-load attribution (state-corruption risk if wrong).
- TM-Q016 — system-captured vs. manually-entered timestamp provenance for check-in/out (undercuts TM-F010 detention evidence otherwise).
- TM-Q017 — a fourth "other load documents" slot (lumper/scale/accessorial/damage) alongside rate-con/BOL/POD.
- TM-Q018 — explicit Load History + search as a base-tier feature, not just implied by Pro analytics.
- TM-Q019 — in-motion interaction constraints (driver-distraction/liability), currently unaddressed anywhere in the baseline.
- TM-Q020 — **TruckMate** collides with an existing, established trucking-industry TMS product of the same name (Trimble/TMW/Innovative Computing Corp's TruckMate). This is a real trademark/confusion risk, not just a domain-availability question (TM-Q012); worth resolving early since it's cheap to change now and expensive later.

**Architecture Must Allow Later (don't build now, don't block it):**
- TM-Q014 — multi-stop loads. `LoadStop` is already in the conceptual data model (§24); the Current Load Card/workflow sections should say explicitly whether V1 UI is single-stop-only so the data model doesn't imply more than the UI delivers.
- AI-usage abuse/cost controls (rate limits on Gemini extraction calls per user) — not in §21 Reliability; worth a line so it isn't bolted on under pressure later.
- Team-driver / shared-load access — not mentioned; fine to defer, but the data model's single-owner assumption (§2, §24) should be noted as a later constraint rather than silently discovered during a schema migration.

**Optional Future (no action needed now):**
- ELD/HOS-aware integration (e.g. reading HOS clock from Motive/Samsara) as a courtesy signal — stays out of scope per TM-D006, but is a plausible later partnership, not a competitor-parity feature to chase now.
- Cross-border (Canada/Mexico) customs paperwork (PARS/PAPS, e-manifest) as additional document types.
- IFTA-relevant per-state mileage splitting — currently correctly excluded as "not accounting software," but since mileage/route data is already being captured for deadhead/loaded miles (§10), the architecture should not need to be redesigned if the owner later wants to surface state-mileage splits for the driver's own IFTA prep (not TruckMate filing taxes, just organizing the numbers).

No unnecessary-complexity findings this pass — the SRS already declines several tempting features (ELD replacement, marketplace, tax software, crippled trial) and the existing §26 deferred list is sound as-is.

## Next assignment — owner reconciliation
1. Owner reviews TM-Q014–TM-Q020 in `05_OPEN_QUESTIONS.md` (kept separate from TM-Q001–TM-Q013, which are unchanged).
2. Owner and ChatGPT resolve which become `02_MASTER_DECISIONS.md` entries vs. deferred.
3. Once resolved, update `01_MASTER_SRS.md`/`03_FEATURE_BANK.md` accordingly and move `08_APPROVAL_STATUS.md` to a fully approved baseline.
4. Do not begin UX/design work on the affected areas (Current Load Card multi-stop behavior, check-in/out timestamp UI, document categories, Load History) until TM-Q014–TM-Q019 are resolved, since each changes the shape of a core screen or data model.

## Special review focus
- Can a driver use the Current Load Card quickly while actually working?
- Are we asking for unnecessary taps or typing?
- What happens with no signal?
- What happens when AI extraction is wrong?
- Can a load state accidentally advance incorrectly?
- Can detention evidence be trusted without overclaiming it?
- Can essential documents/reminders become stale or duplicate?
- Can financial estimates be confused with settlement truth?
- Are we missing a very common, easy-to-solve driver pain point?

## Owner principle
**Clean. Easy. Simple. Collect once, use everywhere.**

# TruckMate — Open Questions

These are unresolved and are NOT approved decisions.

| ID | Question | Why it matters |
|---|---|---|
| TM-Q012 | What public product name/domain/store name should replace the working name TruckMate? | TruckMate is already used in the trucking industry; owner intentionally left the replacement name open for later naming work. |

## Resolved by owner — 2026-09-21
- TM-Q001 → TM-D024: offline-first wherever technically possible.
- TM-Q002 → TM-D025: offline check-in/out with local timestamp/location provenance and later safe sync.
- TM-Q003 → TM-D026: standard wallet slots plus custom expiring documents.
- TM-Q004 → TM-D027: Quick and Detailed PTI are both V1.
- TM-Q005 → TM-D028: line-by-line, load-based settlement reconciliation across settlement weeks.
- TM-Q006 → TM-D029: carrier-paid, route-estimated, and driver-adjusted miles remain distinct; originals preserved.
- TM-Q007 → TM-D030: corrected AI/system values retain original/audit provenance.
- TM-Q008 → TM-D031/TM-D038: 30-day Trash then permanent deletion; flexible Excel + document export.
- TM-Q009 → TM-D032/TM-D033: document-driven stage automation with manual override; structured partial/full rejection and disposition flow.
- TM-Q010 → TM-D034: no live nearby-driver feature; use proactive opt-in facility intelligence from driver reports.
- TM-Q011 → TM-D035: 60-day full-feature trial; $59.98/month target.
- TM-Q013 → TM-D037: organize evidence/records without legal/compliance/admissibility guarantees.

## Resolved from Claude independent review — 2026-09-18
- TM-Q014 → TM-D017: V1 supports multi-stop loads.
- TM-Q015 → TM-D018: one Current Load plus Upcoming/Pre-planned Loads; explicit document attribution.
- TM-Q016 → TM-D019: preserve system-captured vs manual/edited check-in/out provenance.
- TM-Q017 → TM-D009/TM-F044: flexible Other Load Documents added.
- TM-Q018 → TM-D020/TM-F045: Load History + search is V1 base functionality.
- TM-Q019 → TM-D021/TM-F046: minimize in-motion interaction; no typing-heavy core workflow while moving.
- TM-Q020 → TM-D001/TM-Q012: TruckMate remains the working/project name only pending separate public-brand replacement.


## Resolved from Gemini gap-review reconciliation — 2026-09-21
- TM-Q021 → TM-D041: support mid-load equipment swaps with assignment history; default effective from swap forward.
- TM-Q022 → TM-D045: allow out-of-order Active Stop selection without fake completion; drag-and-drop is not required as the primary V1 flow.
- TM-Q023 → TM-D040/TM-D043/TM-D044: V1 trailer types are Dry Van + Reefer; reefer-specific fields are deliberately limited; use flexible Critical Load Instructions for special requirements.
- TM-Q024 → TM-D047: manual user corrections outrank AI/inference; material manual conflicts are not silently resolved solely by client timestamp.
- TM-Q025 → TM-D049: no separate Gate Pass screen in V1; make gate/check-in references prominent on the Current Load Card.
- TM-Q026 → TM-D046: configurable detention threshold; 2-hour user default when unknown; local reminder 15 minutes before threshold with non-guarantee wording.
- TM-Q027 → TM-D048: operational, paperwork, and settlement statuses remain separate parallel dimensions.
- TM-Q028 → TM-D042: trailer hook/swap prompts a Quick Trailer Check; skip allowed with reason.


## Final V1 additions — 2026-09-22/23
- Upcoming due/reminder module → TM-D050.
- Phone OTP + optional email/no-password authentication and recovery boundary → TM-D051.
- Lean five-area admin/operations panel → TM-D052.
- Owner/Admin/Support roles and admin privacy boundary → TM-D053.
- Company Driver + O/O working tiers and entitlement controls → TM-D054.
- Final V1 scope lock; all new feature ideas move to V2 → TM-D055.

No new open V1 product question was created. TM-Q012 remains intentionally open for public naming only.

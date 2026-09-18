# TruckMate — Open Questions

These are unresolved and are NOT approved decisions.

| ID | Question | Why it matters |
|---|---|---|
| TM-Q001 | Which active-load information must be available fully offline? | Drivers frequently encounter poor connectivity. |
| TM-Q002 | Should offline check-in/out be V1 core, and how should location/timestamp integrity be represented? | Detention evidence and duplicate-sync risk. |
| TM-Q003 | What exact document-wallet slots are V1-required versus optional? | Avoid both missing essentials and clutter. |
| TM-Q004 | Is Detailed PTI V1 or post-V1? | Scope vs usefulness. |
| TM-Q005 | What is the minimum useful settlement reconciliation flow for V1 Pro? | Potentially high value but can expand complexity. |
| TM-Q006 | How are carrier-paid miles distinguished from map-estimated miles? | Pay estimates must not mislead. |
| TM-Q007 | What correction/audit history is required after AI-extracted fields are edited? | Trust and provenance. |
| TM-Q008 | What retention/deletion/export policy should apply to load and essential documents? | Privacy and user control. |
| TM-Q009 | What exact conditions allow a document upload to auto-advance load stage without confirmation? | Reduce taps without false state changes. |
| TM-Q010 | Should nearby truck help be V1 core or wait until after core workflow validation? | Prevent feature dilution. |
| TM-Q011 | What trial duration and launch pricing should be tested after field validation? | Monetization deferred but architecture should allow it. |
| TM-Q012 | What public product name/domain/store name should replace or clear the working name TruckMate after conflict review? | Existing trucking-industry use means public branding needs validation before launch. |
| TM-Q013 | Which compliance/legal claims must be avoided or specifically validated around PTI, document storage, detention evidence, and financial/tax organization? | Prevent misleading product claims. |

## Resolved from Claude independent review — 2026-09-18
- TM-Q014 → TM-D017: V1 supports multi-stop loads.
- TM-Q015 → TM-D018: one Current Load plus Upcoming/Pre-planned Loads; explicit document attribution.
- TM-Q016 → TM-D019: preserve system-captured vs manual/edited check-in/out provenance.
- TM-Q017 → TM-D009/TM-F044: flexible Other Load Documents added.
- TM-Q018 → TM-D020/TM-F045: Load History + search is V1 base functionality.
- TM-Q019 → TM-D021/TM-F046: minimize in-motion interaction; no typing-heavy core workflow while moving.
- TM-Q020 → TM-D001/TM-Q012: TruckMate remains the working/project name only pending separate public-brand conflict review.

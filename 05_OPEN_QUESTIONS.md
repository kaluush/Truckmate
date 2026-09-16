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
| TM-Q012 | What product name/domain/store-name conflicts exist for TruckMate? | Branding should be validated before public launch. |
| TM-Q013 | Which compliance/legal claims must be avoided or specifically validated around PTI, document storage, detention evidence, and financial/tax organization? | Prevent misleading product claims. |

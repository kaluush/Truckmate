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
| TM-Q014 | Should V1 model multi-stop loads (multiple pickups and/or drops), not just one pickup + one receiver? | `LoadStop` already exists in the conceptual data model (§24), but the Current Load Card (§3) and stage workflow (§7) only describe a single pickup/delivery pair. If most real loads are single-stop this may be fine for V1 UI, but the workflow rules should explicitly say so rather than leave it implicit. |
| TM-Q015 | Can a driver have more than one active/pre-planned load at once (e.g. next load assigned before the current one is delivered), and if so how does the Current Load Card and document scanning distinguish which load a new scan belongs to? | Team drivers and pre-planned loads are common; scanning a document while two loads are "active" risks silently attaching it to the wrong load and corrupting state (see TM-D016 auto-advance). |
| TM-Q016 | Should check-in/out timestamps distinguish system-captured (GPS/device-clock at time of action) from manually entered/backdated timestamps? | Detention evidence credibility (§8, TM-F010) depends on this provenance. Drivers routinely forget to tap Check In until after arrival; without a provenance flag, all timestamps look equally authoritative even when some are guesses. |
| TM-Q017 | Does the Load Document Package (§5) need a fourth "other load-specific documents" category (lumper receipts, scale tickets, detention/accessorial paperwork, damage photos) beyond rate confirmation / pickup BOL / POD? | These are extremely common per-load paperwork in real trucking workflows and are currently outside the three named categories; without an explicit slot they'll either get lost or forced into the wrong category. |
| TM-Q018 | Is there a V1 Load History / past-loads list and search across loads and documents, available to all users (not just Pro analytics), or is only the single Current Load Card in scope for V1? | "Collect once, use everywhere" (TM-D004) implies drivers should be able to find a shipper/BOL from a past load without Pro financial analytics. This isn't currently in `03_FEATURE_BANK.md` as its own item — it may be assumed but should be explicit. |
| TM-Q019 | What interaction constraints apply while the vehicle is in motion (e.g. no multi-field forms, no required typing) for driver-distraction/liability reasons? | Not addressed anywhere in the current baseline. Given TruckMate's core loop happens in-cab during a commercial driver's working day, this is a safety and liability question, not just a UX preference. |
| TM-Q020 | Has "TruckMate" been checked against the existing, well-established trucking-industry product **TruckMate** (a fleet TMS sold by Trimble/TMW/Innovative Computing Corp) for trademark and market-confusion risk? | This is a stronger version of TM-Q012 (branding): the collision isn't a hypothetical domain-availability issue, it's an existing dispatcher/back-office software product already known in the trucking industry, which raises real trademark and customer-confusion risk before any public launch or store listing. |

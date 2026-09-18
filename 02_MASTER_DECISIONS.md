# TruckMate — Master Decisions

Only explicit project decisions belong here. Preserve superseded decisions rather than silently rewriting history.

| ID | Status | Decision | Rationale |
|---|---|---|---|
| TM-D001 | ACTIVE | Product name is **TruckMate** as the working/project name; public branding is not cleared until naming-conflict review is complete. | Owner chose TruckMate, but an existing trucking-industry TruckMate product creates possible trademark/customer-confusion risk. |
| TM-D002 | ACTIVE | PWT structure is adaptive; no fixed SRS section count. | Structure follows actual project complexity. |
| TM-D003 | ACTIVE | Current Load Card is the center of the driver's working-day experience. | Highest-frequency information should be immediately available. |
| TM-D004 | ACTIVE | Product principle: clean, easy, simple; collect once and reuse everywhere. | Reduce repetitive driver work. |
| TM-D005 | ACTIVE | User tiers are based on functional needs, not job title. | Percentage-paid drivers may need the same financial layer as owner-operators. |
| TM-D006 | ACTIVE | TruckMate is not an ELD replacement. | Keep V1 focused and avoid unnecessary compliance/technical scope. |
| TM-D007 | ACTIVE | Driver operational/business data is private by default and shared only by user action. | Trust and privacy are core product requirements. |
| TM-D008 | ACTIVE | Deadhead tracking is configurable/optional. | Carrier compensation practices differ. |
| TM-D009 | ACTIVE | Load documents remain grouped with the load: confirmation/rate con, pickup BOL, signed delivery/POD, plus flexible Other Load Documents. | Eliminates searching across photos/email while accommodating real paperwork without category bloat. |
| TM-D010 | ACTIVE | Use an existing/native scanning component rather than building scanner technology. | Faster, safer V1. |
| TM-D011 | ACTIVE | Gemini is the current AI direction for document classification/extraction. | Project owner's chosen AI stack. |
| TM-D012 | ACTIVE | Gmail automation is deferred; V1 can import/upload/share/scan documents. | Avoid making Gmail verification/integration a launch dependency. |
| TM-D013 | ACTIVE | Subscriptions/payment gates come after real-life product testing. | Prove utility before monetization friction. |
| TM-D014 | ACTIVE | Trial should expose the real product rather than an artificially crippled version. | User should experience actual value before subscribing. |
| TM-D015 | ACTIVE | Check-in/out events can generate reusable copy/share status messages. | Collect once, use everywhere. |
| TM-D016 | ACTIVE | Where confidence is sufficient, normal document actions may advance load stage (e.g. pickup BOL can imply pickup completion); ambiguous cases require confirmation. | Reduce redundant taps without silently corrupting trip state. |
| TM-D017 | ACTIVE | V1 supports multiple pickups and/or deliveries on a load. | Multi-stop freight is a normal real-world workflow and should not require later structural redesign. |
| TM-D018 | ACTIVE | A driver may have one Current Load plus one or more Upcoming/Pre-planned Loads. Every document/action must be attributable to the correct load; the app must not silently attach ambiguous scans. | Drivers are commonly pre-planned before the current load is complete; explicit attribution prevents state/data corruption. |
| TM-D019 | ACTIVE | Check-in/out records preserve timestamp provenance: system-captured versus manually entered/edited. Manual correction remains allowed. | Detention records should show how the time was obtained without preventing legitimate corrections. |
| TM-D020 | ACTIVE | Load History and search are V1 base functionality for all users, not Pro-only analytics. | Drivers need fast retrieval of past loads and paperwork; this directly implements collect-once/reuse-everywhere. |
| TM-D021 | ACTIVE | Driver interaction while the vehicle is moving must be minimized; core workflows shall not depend on multi-field forms or required typing while driving. | Reduce distraction and keep the product aligned with safe in-cab use. |
| TM-D022 | ACTIVE | Architecture should allow future team-driver/shared-load access without making it V1 scope. | Avoid a single-owner schema dead end while keeping V1 simple. |
| TM-D023 | ACTIVE | Gemini/document-processing endpoints require usage/cost-abuse controls such as server-side limits/quotas before production. | Prevent runaway API cost and abuse without changing the user-facing V1 workflow. |

# TruckMate — Master Decisions

Only explicit project decisions belong here. Preserve superseded decisions rather than silently rewriting history.

| ID | Status | Decision | Rationale |
|---|---|---|---|
| TM-D001 | ACTIVE | Product name is **TruckMate**. | Chosen by project owner. |
| TM-D002 | ACTIVE | PWT structure is adaptive; no fixed SRS section count. | Structure follows actual project complexity. |
| TM-D003 | ACTIVE | Current Load Card is the center of the driver's working-day experience. | Highest-frequency information should be immediately available. |
| TM-D004 | ACTIVE | Product principle: clean, easy, simple; collect once and reuse everywhere. | Reduce repetitive driver work. |
| TM-D005 | ACTIVE | User tiers are based on functional needs, not job title. | Percentage-paid drivers may need the same financial layer as owner-operators. |
| TM-D006 | ACTIVE | TruckMate is not an ELD replacement. | Keep V1 focused and avoid unnecessary compliance/technical scope. |
| TM-D007 | ACTIVE | Driver operational/business data is private by default and shared only by user action. | Trust and privacy are core product requirements. |
| TM-D008 | ACTIVE | Deadhead tracking is configurable/optional. | Carrier compensation practices differ. |
| TM-D009 | ACTIVE | Load documents remain grouped with the load: confirmation/rate con, pickup BOL, signed delivery/POD. | Eliminates searching across photos/email. |
| TM-D010 | ACTIVE | Use an existing/native scanning component rather than building scanner technology. | Faster, safer V1. |
| TM-D011 | ACTIVE | Gemini is the current AI direction for document classification/extraction. | Project owner's chosen AI stack. |
| TM-D012 | ACTIVE | Gmail automation is deferred; V1 can import/upload/share/scan documents. | Avoid making Gmail verification/integration a launch dependency. |
| TM-D013 | ACTIVE | Subscriptions/payment gates come after real-life product testing. | Prove utility before monetization friction. |
| TM-D014 | ACTIVE | Trial should expose the real product rather than an artificially crippled version. | User should experience actual value before subscribing. |
| TM-D015 | ACTIVE | Check-in/out events can generate reusable copy/share status messages. | Collect once, use everywhere. |
| TM-D016 | ACTIVE | Where confidence is sufficient, normal document actions may advance load stage (e.g. pickup BOL can imply pickup completion); ambiguous cases require confirmation. | Reduce redundant taps without silently corrupting trip state. |

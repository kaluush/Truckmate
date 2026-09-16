# TruckMate — Master Software Requirements Specification

**Status:** Initial requirements baseline for independent gap review. Not yet owner-approved for implementation.

The structure is intentionally sized to TruckMate. PWT does not impose a fixed section count.

## 1. Product Purpose and Boundaries
TruckMate SHALL provide a private, simple mobile work companion for CDL drivers and owner-operators. It SHALL organize active-load information, load documents, check-in/out records, essential documents, inspections, mileage, and applicable financial information. It SHALL NOT present itself as an ELD replacement, tax professional, accounting replacement, or guarantee of detention payment.

## 2. Users, Modes, and Access
TruckMate SHALL support at minimum:
- Standard/mileage driver needs.
- Pro financial needs for percentage-paid drivers and owner-operators.
Functional capability SHALL be based on need/tier rather than job-title assumptions. A user SHALL control their own operational/business records. Sharing SHALL require an intentional user action unless a future integration is explicitly authorized.

## 3. Current Load Card
The Current Load Card SHALL be the primary active-trip interface. It SHALL display stage-relevant information first and secondary reference information with minimal scrolling.

At pickup stage, high-priority fields include pickup number, shipper, address, appointment, navigation, and Check In.

After pickup, delivery reference/BOL number, receiver, address, appointment, navigation, and delivery-stage actions become primary.

Secondary reference fields SHOULD include truck number, trailer number, truck/trailer plate, company/carrier information, DOT number, contacts, and other extracted load references where available.

The user SHALL be able to correct extracted information.

## 4. Load Creation and Document Intake
V1 SHALL allow load creation from practical intake methods such as PDF/image upload, in-app scan, and OS share/import. The system SHOULD minimize manual transcription.

AI extraction SHOULD identify, when present: shipper, receiver, addresses, pickup/delivery references, load number, BOL/reference number, appointments, contacts, rate/pay for authorized Pro use, and other necessary operational fields.

Extraction confidence/failure SHALL NOT silently create trusted incorrect data. Important ambiguous fields SHOULD be surfaced for quick confirmation/correction.

Gmail automatic detection/import is deferred and SHALL NOT be a V1 launch dependency.

## 5. Load Document Package
Each load SHALL maintain one organized package containing source documents and structured data. Expected document categories include:
1. Rate/load confirmation.
2. Pickup BOL/shipping papers.
3. Signed delivery BOL/POD.

Multi-page documents SHALL remain associated with the same load. Original source files SHALL remain retrievable even after AI extraction.

## 6. Scanner and AI Processing
TruckMate SHOULD use an established/native scanning capability for capture, crop, page handling, and compression rather than building scanning technology from scratch.

Multi-page documents SHOULD be processed together when practical. AI processing SHALL be designed to avoid unnecessary calls and storage duplication. Extraction errors SHALL be recoverable through user correction/retry.

## 7. Trip Stage and Workflow Automation
TruckMate SHALL maintain an explicit load stage/state. The interface SHALL adapt to that state.

Where a normal user action strongly indicates a stage transition—for example scanning the pickup BOL after loading—the system MAY suggest or safely infer pickup completion. Low-confidence or consequential transitions SHALL request confirmation. Automation SHALL reduce taps without making hidden state changes that are difficult to correct.

## 8. Check-In, Check-Out, and Detention Evidence
A Check In action SHALL record timestamp, load/facility context, and location when permission is available. Check Out SHALL record departure and calculate dwell duration.

The system SHOULD support both shipper and receiver dwell records. A normal workflow action MAY provide a checkout signal when reliable, but manual correction SHALL remain possible.

TruckMate MAY flag possible detention based on configurable/free-time rules. It SHALL describe this as supporting evidence/recordkeeping, not a guarantee of eligibility or payment.

## 9. Copy and Share Status
After arrival/check-in/check-out, TruckMate SHALL generate a concise reusable status summary. Fields MAY include facility, city/location, pickup/delivery number, truck/trailer, arrival/check-in/departure timestamps, and dwell time.

A single Copy/Share action SHOULD allow use through the device's normal share mechanisms such as SMS, WhatsApp, email, or carrier/dispatch apps. The user SHOULD be able to avoid sharing fields they do not want to disclose.

## 10. Mileage and Deadhead
TruckMate SHOULD calculate deadhead from an appropriate starting/current location to pickup and loaded miles from pickup to receiver. Total trip miles SHOULD combine applicable deadhead and loaded miles.

Deadhead tracking and compensation assumptions SHALL be configurable because carrier practices differ. Route-derived miles SHALL be distinguished from carrier-paid/settlement miles when those differ.

## 11. Mileage Driver Pay Estimate
Where enabled, a mileage-paid driver MAY configure cents-per-mile and deadhead compensation behavior. TruckMate SHOULD estimate weekly paid miles/pay while clearly identifying estimates versus confirmed settlement amounts.

## 12. Pro Financial Layer
Pro functionality SHOULD support load revenue, fuel, expenses, miles, deadhead, loaded miles, total miles, revenue per mile, and weekly/monthly/yearly summaries.

Financial analytics SHALL preserve source/provenance where practical so a user can understand where a number came from. Net/profit figures SHALL be clearly labeled as estimates unless all required costs are known.

Settlement reconciliation SHOULD support comparing known load/expense history against settlement charges and highlighting possible duplicates, omissions, or mismatches for user review. It SHALL NOT silently assert that a carrier made an error without sufficient evidence.

## 13. Document Wallet / Essentials
TruckMate SHALL provide an always-accessible wallet separate from load paperwork. It SHOULD use expected placeholders so missing essentials are visible.

Candidate slots include CDL/license, medical certification/card where applicable, truck registration, trailer registration, insurance, permits where relevant, and custom Other documents.

AI SHOULD classify document type. If an uploaded document appears inconsistent with the selected slot, the app SHOULD warn the user rather than silently filing it incorrectly.

## 14. Expiration Management
Where an expiration date can be reliably extracted, TruckMate SHOULD store it and show a live countdown.

Default reminder cadence currently intended: 60, 45, 30, 15, 7, 3, 2, and 1 day before expiration, then expired. Dismissing one notification SHALL NOT automatically cancel future scheduled reminders.

When a replacement document is recognized, TruckMate SHOULD update the active expiration/reminder state and stop obsolete reminders while preserving appropriate history. Ambiguous replacement SHALL request confirmation.

## 15. PTI / Inspection Records
TruckMate SHALL support a simple Quick Inspection workflow and MAY support a more Detailed Inspection workflow in V1 depending on scope review. Records MAY include timestamp, truck/trailer, checklist, defect notes, and photos.

Open defects MAY generate follow-up reminders. TruckMate SHALL avoid implying that its PTI record replaces legally required inspection/ELD/compliance systems unless separately validated.

## 16. Nearby Essential Help
TruckMate MAY provide lightweight access to nearby truck-relevant resources such as dealer/service, repair, tires, and truck stops. V1 SHALL avoid becoming a broad services marketplace.

## 17. Notifications
Notifications SHALL be purposeful and controllable. Primary V1 notification classes include document expirations and, if approved, unresolved defect reminders. Notification logic SHALL avoid duplicate/stale alerts after state changes.

## 18. Privacy, Security, and Data Control
Authentication SHALL protect user data. Authorization SHALL prevent cross-user access. Sensitive documents SHALL be protected in transit and at rest using appropriate platform/cloud controls. Secrets SHALL never be embedded in client code or committed to the repository.

The system SHALL define retention, deletion, export, account recovery, and document access rules before production. Location SHALL be collected only when needed for user-facing functionality and with appropriate permission. Privacy-sensitive sharing SHALL remain user controlled.

## 19. Offline and Poor-Network Behavior
Truck drivers routinely operate in unreliable-connectivity environments. Before V1 implementation, the project SHALL decide which Current Load Card fields and documents must remain available offline, how check-in/out events queue safely, and how conflicts/sync recovery work. Offline actions SHALL avoid duplicate events when connectivity returns.

## 20. Architecture and Integrations
Current direction:
- Mobile client.
- Firebase Authentication.
- TruckMate-owned API/business boundary.
- Firestore or equivalent structured data store.
- Cloud Storage for source documents.
- Gemini API for document classification/extraction.
- Push notifications.
- Device location/maps/routing.
- Existing/native scanner capability.
- Subscription billing later.
- Gmail integration later.

Core business rules, authorization, sensitive operations, and integration orchestration SHOULD remain behind the project-owned boundary rather than being tightly coupled to the mobile UI.

## 21. Reliability, Recovery, and Observability
The system SHALL plan for failed uploads, partial scans, AI extraction failures, duplicate submissions, interrupted network operations, notification failures, and dependency outages. Critical operations SHOULD be idempotent where appropriate. Logging/error monitoring SHALL avoid exposing sensitive document content unnecessarily. Backup/restore and rollback expectations SHALL be defined before production.

## 22. UX and Accessibility
Common driver actions SHALL be optimized for fast comprehension and large, clear touch targets. High-priority references such as pickup/delivery numbers SHALL be visually prominent at the correct trip stage. The app SHOULD minimize typing and unnecessary popups. Accessibility, readability, contrast, and one-handed/mobile use SHALL be considered during design validation.

## 23. Trial and Monetization
Subscription enforcement is deferred until core workflows are field-tested. The intended trial philosophy is to let users experience the actual product value rather than presenting an immediate crippled paywall. Exact pricing, trial duration, billing provider, entitlement model, and store policy handling remain open decisions.

## 24. Data Model — Initial Conceptual Entities
At minimum the architecture should anticipate: User, DriverProfile, Vehicle, Trailer, CarrierProfile, Load, LoadStop, LoadReference, LoadDocument, ExtractedField/Provenance, CheckEvent, DwellRecord, SharedStatusTemplate, EssentialDocument, ExpirationReminder, Inspection, Defect, MileageRecord, Expense, FuelRecord, Settlement/SettlementLine, and Subscription/Entitlement when monetization is added.

This is conceptual, not a final database schema.

## 25. Success Criteria and Field Validation
Before monetization, TruckMate SHOULD be tested in real trucking workflows. Validation should determine whether drivers can retrieve check-in information quickly, maintain load paperwork without photo-gallery searching, capture dwell evidence with minimal effort, manage expirations reliably, and understand mileage/financial summaries without reconstruction.

## 26. Explicitly Deferred / Out of Scope
- ELD replacement.
- Large roadside/service-provider marketplace.
- Automatic Gmail import as launch dependency.
- Heavy accounting/tax-preparation functionality.
- Subscription gating before field validation.
- Features added only because competitors have them.

## 27. Gap-Review Gate
Before this SRS becomes an implementation baseline, an independent AI reviewer (initially Claude) SHOULD inspect it for:
- Missing high-frequency driver/owner-operator pain points.
- Unnecessary scope.
- UX friction/tap count.
- Offline and failure gaps.
- Privacy/security/data-retention gaps.
- Incorrect trucking workflow assumptions.
- Financial reconciliation edge cases.
- Document extraction/confidence/provenance issues.
- Architecture that blocks later growth.

Reviewer recommendations are proposals until accepted by the project owner and recorded through PWT decisions/SRS updates.

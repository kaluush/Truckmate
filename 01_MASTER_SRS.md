# TruckMate — Master Software Requirements Specification

**Status:** Owner-approved V1 requirements baseline for UX/design as of 2026-09-21. Public replacement name remains intentionally open.

The structure is intentionally sized to TruckMate. PWT does not impose a fixed section count.

## 1. Product Purpose and Boundaries
TruckMate SHALL provide a private, simple mobile work companion for CDL drivers and owner-operators. It SHALL organize active-load information, load documents, check-in/out records, essential documents, inspections, mileage, and applicable financial information. It SHALL NOT present itself as an ELD replacement, tax professional, accounting replacement, or guarantee of detention payment.

## 2. Users, Modes, and Access
TruckMate SHALL support at minimum:
- Standard/mileage driver needs.
- Pro financial needs for percentage-paid drivers and owner-operators.
Functional capability SHALL be based on need/tier rather than job-title assumptions. A user SHALL control their own operational/business records. Sharing SHALL require an intentional user action unless a future integration is explicitly authorized.

## 3. Current Load Card
The Current Load Card SHALL be the primary active-trip interface. V1 SHALL support multiple pickups and/or deliveries, with the card following the active stop. A driver MAY also have Upcoming/Pre-planned Loads without replacing the Current Load. It SHALL display stage-relevant information first and secondary reference information with minimal scrolling.

At pickup stage, high-priority fields include pickup number, shipper, address, appointment, navigation, and Check In.

After pickup, delivery reference/BOL number, receiver, address, appointment, navigation, and delivery-stage actions become primary.

Secondary reference fields SHOULD include truck number, trailer number, truck/trailer plate, company/carrier information, DOT number, contacts, and other extracted load references where available.

The user SHALL be able to correct extracted information.

## 4. Load Creation and Document Intake
V1 SHALL allow load creation from practical intake methods such as PDF/image upload, in-app scan, and OS share/import. The system SHOULD minimize manual transcription.

AI extraction SHOULD identify, when present: shipper, receiver, addresses, pickup/delivery references, load number, BOL/reference number, appointments, contacts, rate/pay for authorized Pro use, and other necessary operational fields.

Extraction confidence/failure SHALL NOT silently create trusted incorrect data. Important ambiguous fields SHOULD be surfaced for quick confirmation/correction. When more than one load could receive a scan/import, the system SHALL require reliable attribution to the intended load and SHALL request confirmation when attribution is ambiguous.

Gmail automatic detection/import is deferred and SHALL NOT be a V1 launch dependency.

## 5. Load Document Package
Each load SHALL maintain one organized package containing source documents and structured data. Expected document categories include:
1. Rate/load confirmation.
2. Pickup BOL/shipping papers.
3. Signed delivery BOL/POD.\n4. Other Load Documents, a flexible category for load-specific paperwork such as lumper receipts, scale tickets, washout receipts, accessorial/detention paperwork, damage photos, and similar items.

Multi-page documents SHALL remain associated with the same load. Original source files SHALL remain retrievable even after AI extraction.

## 6. Scanner and AI Processing
TruckMate SHOULD use an established/native scanning capability for capture, crop, page handling, and compression rather than building scanning technology from scratch.

Multi-page documents SHOULD be processed together when practical. AI processing SHALL be designed to avoid unnecessary calls and storage duplication. Extraction errors SHALL be recoverable through user correction/retry.

## 7. Trip Stage and Workflow Automation
TruckMate SHALL maintain an explicit load stage/state and adapt the interface to that state.

Pickup papers/BOL MAY automatically advance a load to **In Transit** when document classification is sufficiently reliable. A signed delivery BOL/POD MAY automatically advance a load to **Delivered**. The user SHALL always be able to correct/override an automated state.

Partial rejection SHALL be recorded as **Delivered with Exception**, including rejected quantity/reason and optional notes/photos/documents; remaining rejected freight stays associated with the same load. Full rejection SHALL keep the same load open as rejected/awaiting instructions. Return to shipper, alternate receiver, donation/food-bank, or other disposition SHALL be modeled as an additional stop on the same load, not a new load. Additional compensation for disposition/return MAY be unknown, none, or a recorded amount and SHALL be reconcilable later.

## 8. Check-In, Check-Out, and Detention Evidence
A Check In action SHALL record timestamp, load/facility context, and location when permission is available. Check Out SHALL record departure and calculate dwell duration. Check events SHALL preserve whether a timestamp was system-captured or manually entered/edited. Manual correction SHALL remain possible.

The system SHOULD support both shipper and receiver dwell records. A normal workflow action MAY provide a checkout signal when reliable, but manual correction SHALL remain possible.

TruckMate MAY flag possible detention based on configurable/free-time rules. It SHALL describe this as supporting evidence/recordkeeping, not a guarantee of eligibility or payment.

## 9. Copy and Share Status
After arrival/check-in/check-out, TruckMate SHALL generate a concise reusable status summary. Fields MAY include facility, city/location, pickup/delivery number, truck/trailer, arrival/check-in/departure timestamps, and dwell time.

A single Copy/Share action SHOULD allow use through the device's normal share mechanisms such as SMS, WhatsApp, email, or carrier/dispatch apps. The user SHOULD be able to avoid sharing fields they do not want to disclose.

## 10. Mileage and Deadhead
TruckMate SHOULD calculate deadhead and loaded miles using available routing/location services. Route-derived mileage SHALL be treated as an estimate rather than truck-route ground truth.

The system SHALL preserve distinct values when applicable: **carrier-paid miles**, **TruckMate route-estimated miles**, and **driver-adjusted miles**. The driver MAY manually adjust mileage; adjusted values SHALL be labeled as manual/driver adjusted and the original estimate SHALL remain in audit history. Deadhead tracking/compensation assumptions remain configurable.

## 11. Mileage Driver Pay Estimate
Where enabled, a mileage-paid driver MAY configure cents-per-mile and deadhead compensation behavior. TruckMate SHOULD estimate weekly paid miles/pay while clearly identifying estimates versus confirmed settlement amounts.

## 12. Pro Financial Layer
Pro functionality SHOULD support load revenue, fuel, expenses, miles, deadhead, loaded miles, total miles, revenue per mile, and weekly/monthly/yearly summaries.

Financial analytics SHALL preserve source/provenance where practical so a user can understand where a number came from. Net/profit figures SHALL be clearly labeled as estimates unless all required costs are known.

Settlement reconciliation SHALL support line-by-line matching by load rather than assuming that all loads settle in the week completed. Completed loads MAY remain Awaiting Settlement until matched on a later settlement. The system SHOULD surface missing/mismatched load pay, detention/accessorial amounts, duplicates, omissions, or other differences for user review. It SHALL NOT silently assert that a carrier made an error without sufficient evidence.

## 13. Document Wallet / Essentials
TruckMate SHALL provide an always-accessible wallet separate from load paperwork. It SHALL provide standard document slots (such as CDL/license, medical certification/card where applicable, truck registration, trailer registration, insurance, IFTA/permits/annual inspection where applicable) and SHALL allow user-created custom document types.

Custom documents SHALL support expiration date/countdown/reminders when applicable. AI SHOULD classify document type and warn on likely slot mismatch rather than silently filing it incorrectly.

## 14. Expiration Management
Where an expiration date can be reliably extracted, TruckMate SHOULD store it and show a live countdown.

Default reminder cadence currently intended: 60, 45, 30, 15, 7, 3, 2, and 1 day before expiration, then expired. Dismissing one notification SHALL NOT automatically cancel future scheduled reminders.

When a replacement document is recognized, TruckMate SHOULD update the active expiration/reminder state and stop obsolete reminders while preserving appropriate history. Ambiguous replacement SHALL request confirmation.

## 15. PTI / Inspection Records
V1 SHALL support both **Quick PTI** and **Detailed PTI**. Both SHALL be timestamped and saved to inspection history. The driver MAY record checklist results, defect notes, and photos as appropriate. Open defects MAY generate follow-up reminders.

TruckMate SHALL avoid implying that its PTI record replaces legally required inspection/ELD/compliance systems or guarantees compliance.

## 16. Facility Intelligence
V1 SHALL NOT expose a live nearby-driver map/request network. Instead, after pickup/delivery stops TruckMate MAY proactively ask the driver, on an opt-in basis, to share structured facility experience such as overnight parking, restroom availability, early loading/unloading policy, vending/food, and an optional note.

Facility reports SHALL be timestamped and source-labeled. Recent TruckMate-driver information MAY be displayed alongside public-source information, including when they conflict, without exposing the reporting driver's private load history/location.

## 17. Notifications
Notifications SHALL be purposeful and controllable. Primary V1 notification classes include document expirations and, if approved, unresolved defect reminders. Notification logic SHALL avoid duplicate/stale alerts after state changes.

## 18. Load History and Search\nV1 SHALL provide base-tier Load History and search so all users can retrieve past loads and associated documents without requiring Pro financial analytics. Search SHOULD prioritize practical driver retrieval needs such as load/reference number, facility, date, and document.\n\n## 19. Privacy, Security, and Data Control
Authentication SHALL protect user data. Authorization SHALL prevent cross-user access. Sensitive documents SHALL be protected in transit and at rest using appropriate platform/cloud controls. Secrets SHALL never be embedded in client code or committed to the repository.

Deleted documents SHALL enter a user-restorable Trash state for 30 days and then be permanently deleted. A linked load/history record MAY retain that a document was deleted but SHALL NOT retain the deleted file after permanent deletion.

Export SHALL support all data, a selected date range, one load, or selected loads. The primary financial export SHALL be an Excel workbook containing summaries, loads/income, expenses, and settlements, bundled with original documents organized by load and referenced from the workbook.

## 20. Offline and Poor-Network Behavior
V1 SHALL be offline-first wherever technically possible. Previously available Current/Upcoming Load information, locally available documents, edits, PTI, expenses, and other technically feasible actions SHALL remain usable without connectivity.

Check-in/out SHALL work offline, saving local timestamp and device location when permission/location is available, preserving provenance and pending-sync state. Sync SHALL be idempotent and avoid duplicate events when connectivity returns. Internet-dependent operations such as new cloud imports, Gemini processing, live traffic/routing refresh, and fresh public/facility data MAY wait for connectivity.

## 21. Architecture and Integrations
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

Core business rules, authorization, sensitive operations, and integration orchestration SHOULD remain behind the project-owned boundary rather than being tightly coupled to the mobile UI. The data model SHOULD allow future team-driver/shared-load access without requiring that feature in V1.

## 22. Reliability, Recovery, and Observability
The system SHALL plan for failed uploads, partial scans, AI extraction failures, duplicate submissions, interrupted network operations, notification failures, and dependency outages. Critical operations SHOULD be idempotent where appropriate. Logging/error monitoring SHALL avoid exposing sensitive document content unnecessarily. Backup/restore and rollback expectations SHALL be defined before production. AI/document-processing endpoints SHALL have server-side usage/cost-abuse controls before production.

## 23. UX and Accessibility
Common driver actions SHALL be optimized for fast comprehension and large, clear touch targets. High-priority references such as pickup/delivery numbers SHALL be visually prominent at the correct trip stage. The app SHOULD minimize typing and unnecessary popups. While the vehicle is moving, core workflows SHALL minimize interaction and SHALL NOT depend on multi-field forms or required typing. Accessibility, readability, contrast, and one-handed/mobile use SHALL be considered during design validation.

## 24. Trial and Monetization
The launch monetization target is a **60-day full-feature trial**, followed by **$59.98/month**. The trial SHALL expose the real product rather than an artificially crippled version. Billing/store implementation details remain a later implementation decision.

## 25. Data Model — Initial Conceptual Entities
At minimum the architecture should anticipate: User, DriverProfile, Vehicle, Trailer, CarrierProfile, Load, LoadStop, LoadReference, LoadDocument, ExtractedField/Provenance, CheckEvent, DwellRecord, SharedStatusTemplate, EssentialDocument, ExpirationReminder, Inspection, Defect, MileageRecord, Expense, RecurringExpense, FuelRecord, Settlement/SettlementLine, and Subscription/Entitlement when monetization is added.

This is conceptual, not a final database schema.

## 26. Success Criteria and Field Validation
Before monetization, TruckMate SHOULD be tested in real trucking workflows. Validation should determine whether drivers can retrieve check-in information quickly, maintain load paperwork without photo-gallery searching, capture dwell evidence with minimal effort, manage expirations reliably, and understand mileage/financial summaries without reconstruction.

## 27. Explicitly Deferred / Out of Scope
- ELD replacement.
- Live nearby-driver tracking/help network.
- Large roadside/service-provider marketplace.
- Automatic Gmail import as launch dependency.
- Full tax preparation/accounting replacement.
- Claims that TruckMate guarantees DOT/legal/tax compliance, court admissibility, detention payment, or legal proof.
- Public use of the TruckMate name; replacement branding remains open.
- Features added only because competitors have them.

## 28. Gap-Review Gate
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

Claude's 2026-09-17 recommendations TM-Q014–TM-Q020 were reconciled on 2026-09-18 and are recorded in Master Decisions/Feature Bank. Future reviewer recommendations remain proposals until accepted by the project owner and recorded through PWT decisions/SRS updates.

## 29. Expense Capture and Reporting
Expenses SHALL use one coherent expense system and SHALL NOT require a load association. A record SHOULD support date, amount, category, location/address when useful, description of what was fixed/purchased, vehicle/unit when useful, optional notes or line items, and optional receipt/document attachment. Common categories SHOULD be quick-pick options with an Other/custom entry path. Recurring costs such as insurance and subscriptions SHALL be supported.

Reports SHALL be able to show trip/load-related costs and truck/business overhead separately, plus combined monthly/yearly expense totals and net estimates. Receipt attachment is encouraged but not mandatory when the user manually records the expense.

## 30. Audit and Correction Provenance
AI-extracted/system-estimated values that are manually corrected SHALL retain the original value and correction provenance in history while using the corrected value operationally. This rule applies to extracted document fields and mileage adjustments and SHOULD be reused for other material corrections where practical.

## 31. Public Naming
**TruckMate is a working/project/repository name only.** The public product/domain/store name SHALL be replaced before launch. TM-Q012 remains intentionally open for naming work and does not block UX/design.

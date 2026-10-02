# Data Model Review — Gemini, 2026-10-02

**Reviewing:** `specs/data-model/DATA_MODEL.md` at commit `4d2b50be9be7ffed26cf9ae7a9ddff765f7743d8`
**Against:** `01_MASTER_SRS.md` and ACTIVE `02_MASTER_DECISIONS.md` (frozen through TM-D078)

Save this file as `specs/data-model/reviews/gemini.md`. Do not edit `DATA_MODEL.md` directly.

## Findings table

| # | Question (A–G) | Finding | Cites | Label | Proposed change |
|---|---|---|---|---|---|
| 1 | A/C/E | **Settlement document ownership contradiction:** `Settlement.sourceDocumentId` references `LoadDocument`, and `LoadDocument.otherSubtype` includes `SETTLEMENT`, while `LoadDocument.loadId` is required (`Req: Y`). A weekly settlement covers multiple loads (and non-load deductions/advances) and is not owned by a single load. Furthermore, `ExtractionJob.documentRef` only allows `LoadDocument` or `EssentialDocument`, blocking settlement scan extraction. | SRS §5, §6, §12, TM-D028, TM-D070 | **Must fix now** | Decouple settlement file intake from `LoadDocument`. Give `Settlement` direct ownership of its source scans (e.g. `sourceFileIds: list of ref StoredFile`) and update `ExtractionJob.documentRef` to support targeting `Settlement`. Remove `SETTLEMENT` from `LoadDocument.otherSubtype`. |
| 2 | A/C/E | **Appointment time zone lost when `facilityId` is absent:** `LoadStop.facilityId` is optional (`—`). `LoadStop` stores `nameSnapshot` and `addressSnapshot`, but lacks a time-zone snapshot. Per R4, appointments are wall-clock times in the facility's time zone. If `facilityId` is null (e.g. manual stop creation or offline before facility linkage), the appointment time zone is completely lost. | R4, SRS §3, TM-D045 | **Must fix now** | Add `timeZoneSnapshot: IANA tz` directly to `LoadStop` (or `appointment.timeZone: IANA tz`) alongside name and address snapshots so appointments are interpretable without requiring a linked `Facility`. |
| 3 | A/C | **Event records missing event time zones (R4 violation):** R4 explicitly requires historical events to store UTC instants plus the local IANA time zone. While `CheckEvent` includes `timeZone`, other historical event records (`LoadPhoto.capturedAt`, `Inspection.performedAt`, `ReeferReading.recordedAt`, and `LoadStatusChange.occurredAt`) omit `timeZone`. When inspected later in different time zones, historical times will shift, causing confusion during claims, detention disputes, or temperature audits. | R4, SRS §8, §15, §33, TM-D078 | **Must fix now** | Add `eventTimeZone: IANA tz` to `LoadPhoto`, `Inspection`, `ReeferReading`, and `LoadStatusChange` (or establish a standard cross-cutting rule for all event records). |
| 4 | A/C | **Disposition stop lacks originating rejection link:** On multi-stop loads with multiple delivery stops, if freight is rejected at one stop and a disposition stop is added (`kind = DISPOSITION`), `LoadStop` has no pointer linking back to the specific delivery stop where the exception occurred. | SRS §7, TM-D017, TM-D033 | **Must fix now** | Add optional `originatingStopId: ref LoadStop` (or `exceptionStopId`) to `LoadStop` when `kind = DISPOSITION`. |
| 5 | A/E | **Operational status missing `CANCELLED` (TONU reality):** `Load.operationalStatus` omits `CANCELLED`. Draft position DM-Q05 suggests deleting cancelled loads or leaving them in history with a note. In real trucking, cancellations and TONUs (Truck Order Not Used) are frequent billable events ($150–$300) requiring retained paperwork, arrival timestamps, and settlement reconciliation. Deleting a cancelled load breaks settlement matching when the TONU pay arrives. | SRS §7, §12, §35, TM-D028, TM-D048 | **Must fix now** | Add `CANCELLED` to `Load.operationalStatus` and reject the draft position in DM-Q05. Allow optional cancellation reason and timestamp. |
| 6 | A/C | **Line-by-line settlement matching requires `LoadPayLine` link:** `SettlementLine` stores `matchedLoadId: ref Load`, but lacks a reference to the specific expected pay line. On loads with multiple pay lines (linehaul, fuel surcharge, detention, stop-off), matching only at the load level leaves individual pay lines ambiguous and prevents automated detection of missing accessorials. | SRS §12, TM-D028 | **Must fix now** | Add optional `matchedLoadPayLineId: ref LoadPayLine` to `SettlementLine`. |
| 7 | A/C | **Company Driver mileage pay missing from `LoadPayLine`:** `LoadPayLine.type` includes only flat/itemized rate-con items (`LINEHAUL`, `FUEL_SURCHARGE`, etc.). A mileage-paid Company Driver (SRS §11) whose expected pay is derived from cents-per-mile has no category (e.g. `MILEAGE_PAY` or `BASE_PAY`) to record expected load pay for settlement matching. | SRS §11, §12, TM-D028, TM-D060 | **Must fix now** | Add `MILEAGE_PAY` to `LoadPayLine.type`. |
| 8 | C | **Recurring reminder lifecycle inconsistency:** `Reminder.status` uses `ACTIVE`, `DONE`, `ARCHIVED`, while the text states that recurring items advance `nextDueOn` when marked done. Setting `status = DONE` on a recurring reminder would hide the upcoming recurrence from active queries, while leaving it `DONE` is semantically contradictory. | SRS §36, TM-D050, TM-D066 | **Must fix now** | Explicitly define the lifecycle: marking a recurring reminder complete records `lastCompletedOn` and advances `nextDueOn` while preserving `status = ACTIVE`. `status = DONE` is terminal only for one-time reminders. |
| 9 | E | **Fuel unit price whole-cents precision loses accuracy:** `Expense.fuel.pricePerGallonMinor` stores integer cents. Retail diesel in the U.S. is priced in tenths of a cent ($X.XX9). Storing whole cents creates immediate rounding drift across large fill-ups (150–200 gal). | R6, SRS §29 | **Must fix now** | Treat total `amount` and `gallons` as the authoritative transaction figures. Store unit price in mills ($0.001) or decimal, or derive it. |
| 10 | A/E | **Inspection missing checklist version identity:** `Inspection.results` references `itemCode`s from app checklist catalogs. Because checklist items are versioned app content rather than user data, `Inspection` requires a checklist version identifier so historical inspections remain unambiguous when checklist definitions change. | SRS §15, §33, TM-D027 | **Must fix now** | Add `checklistVersion: text` (or `int`) to `Inspection`. |
| 11 | A/C | **Maintenance reminders lack equipment references:** `Reminder` links to `EssentialDocument` and `Defect`, but lacks `vehicleId` and `trailerId`. Yet SRS §36 explicitly mandates maintenance reminders (oil changes, scheduled maintenance, tire inspections). Without unit references, maintenance reminders cannot be filtered or grouped by vehicle or trailer. | SRS §15, §32, §36 | **Must fix now** | Add optional `vehicleId: ref Vehicle` and `trailerId: ref Trailer` to `Reminder`. |
| 12 | C/E | **Admin audit data & export privacy risks:** `AdminAuditEvent.before/after` uses unconstrained JSON, and `ExportJob` produces an `outputFileId` containing bundled driver documents. Under TM-D053 and SRS §38, staff must NEVER see driver document contents or precise locations. If audit snapshots capture unconstrained entities or if support staff can download `ExportJob` outputs, driver privacy is compromised. | SRS §19, §37, §38, TM-D053 | **Must fix now** | Constrain `AdminAuditEvent` schemas to administrative metadata only; prohibit raw document text and exact coordinates. Restrict `ExportJob` download URLs strictly to driver-authenticated clients. |
| 13 | A/C | **Missing `trashedAt` on `LoadPhoto`:** Rule R7 establishes that files and photos enter a 30-day Trash state before permanent deletion. While `LoadDocument` has `trashedAt`, `LoadPhoto` lacks a `trashedAt` field in the entity table. | R7, SRS §19, TM-D031, TM-D078 | **Must fix now** | Add `trashedAt: instant` to `LoadPhoto`. |
| 14 | C/D/E | **Facility merge lineage & reporter anonymization:** Community facility intelligence requires merging duplicate driver-created facilities over time without rewriting historical `LoadStop` snapshots. Additionally, account deletion (DM-Q02) requires `FacilityReport.reporterUserId` to be detached without destroying crowdsourced parking/restroom intelligence. | SRS §16, TM-D007, TM-D034, DM-Q02, DM-Q07 | **Architecture must allow later** | Add optional `mergedIntoFacilityId: ref Facility` to `Facility`. Make `FacilityReport.reporterUserId` nullable upon user account deletion to preserve anonymized crowdsourced data. |
| 15 | C/D | **Reefer trailer association on readings during mid-load swap:** If a trailer swap occurs during a reefer load (TM-D041), `ReeferReading` records `loadId` and `stopId`, but does not record which trailer was checked. Determining which trailer produced an alarm requires temporal cross-referencing with `EquipmentAssignment`. | SRS §32, §33, TM-D041, TM-D043 | **Optional future** | Add optional `trailerId: ref Trailer` to `ReeferReading`. |

---

## A. What is missing?

1. **Direct Settlement Document Ownership & Extraction Reference:**
   Settlements must directly reference their scanned files (`sourceFileIds: list of ref StoredFile`) rather than being forced into a `LoadDocument` that requires a single `loadId`. `ExtractionJob.documentRef` must also support `ref Settlement` (SRS §5, §6, §12, TM-D028, TM-D070).
2. **`timeZoneSnapshot` on `LoadStop`:**
   When `facilityId` is null, the stop's local appointment time zone has nowhere to live. Adding `timeZoneSnapshot: IANA tz` to `LoadStop` guarantees appointment integrity across time-zone transitions (R4, SRS §3).
3. **Event Time Zones:**
   `LoadPhoto`, `Inspection`, `ReeferReading`, and `LoadStatusChange` lack local IANA time zone fields, violating R4 (R4, SRS §8, §15, §33, TM-D078).
4. **Originating Rejection Stop Link for Dispositions:**
   A disposition stop on a multi-stop load needs `originatingStopId: ref LoadStop` to identify which delivery stop's freight was rejected (SRS §7, TM-D033).
5. **Operational Status `CANCELLED`:**
   Essential for recording cancelled loads, retaining cancellation documentation, and reconciling TONU pay lines (SRS §7, §12, §35, TM-D028).
6. **`matchedLoadPayLineId` on `SettlementLine`:**
   Enables true line-by-line settlement reconciliation against itemized pay expectations (SRS §12, TM-D028).
7. **`MILEAGE_PAY` on `LoadPayLine.type`:**
   Enables mileage drivers to track expected trip pay alongside accessorials (SRS §11, §12, TM-D060).
8. **`vehicleId` / `trailerId` on `Reminder`:**
   Enables maintenance reminders to associate with specific equipment (SRS §15, §32, §36).
9. **`checklistVersion` on `Inspection`:**
   Identifies the catalog version used for historical inspections (SRS §15, TM-D027).
10. **`trashedAt` on `LoadPhoto`:**
    Aligns `LoadPhoto` with R7's 30-day Trash rule (R7, TM-D031).

Checked every entity in §4 against SRS §3–§39 and Master Decisions TM-D001–TM-D078; no other required fields were missing.

---

## B. What is unnecessarily complicated?

1. **The overall entity count (~40 entities) is justified:**
   The decomposition cleanly mirrors approved requirements: separate entities for `LoadStop`, `CheckEvent`, `EquipmentAssignment`, `FieldRevision`, `StoredFile`, `ExtractionJob`, and `SyncConflict` are necessary to handle multi-stop loads, equipment swaps, provenance tracking, and offline sync safely.
2. **Merging PTI and Trailer Checks into `Inspection` (DM-Q04) is correct:**
   Combining `Inspection` and `TrailerInspection` with a `kind` discriminator eliminates duplicate schemas and shared logic without losing functionality.
3. **Derived Dwell & Paperwork Status:**
   Marking dwell time and paperwork completeness as derived (R10) prevents redundant, desynchronized status counters.

---

## C. What relationship is wrong or risky?

1. **`Settlement.sourceDocumentId -> LoadDocument` (WRONG & BREAKING):**
   `LoadDocument` requires `loadId: ref Load`. Weekly settlement statements cover multiple loads or general deductions/advances. Forcing settlements into `LoadDocument` breaks relational integrity and prevents scanning multi-load statements.
2. **`Reminder |o--o{ Expense` edge in Diagram 3.3 (PREMATURE):**
   Showing this as an approved relationship before DM-Q03 is decided by the owner visually promotes an open question into settled structure.
3. **`AdminAuditEvent.before / after` unconstrained JSON (PRIVACY RISK):**
   Unconstrained JSON can leak sensitive document text, financial details, or precise GPS coordinates into admin audit logs, violating TM-D053 and SRS §38.
4. **`FacilityReport.reporterUserId` non-nullable (ANONYMIZATION RISK):**
   If `reporterUserId` is mandatory and foreign-keyed without detachment rules, deleting a user account will either delete community crowdsourced data or violate referential integrity.

---

## D. What should be more flexible?

1. **Facility De-duplication and Merge Lineage (Recorded need: SRS §16, TM-D034, DM-Q07):**
   Driver-created facilities will inevitably result in duplicate entries. `Facility` should support `mergedIntoFacilityId: ref Facility` so server-side deduplication merges community intelligence without mutating historical `LoadStop` snapshots.
2. **Extensible String Enums (Recorded rule: R9):**
   `trailerType`, `documentSlot`, `expenseCategory`, `stopKind`, and `criticalInstructionKind` are properly specified as string codes, protecting clients from unknown values.
3. **Multi-Driver/Team Loads (Recorded need: SRS §21, TM-D022):**
   Rule R2 correctly places driver-specific state (`currentLoadId`) on `User` rather than `Load`, ensuring the architecture allows team drivers later without schema rewrites.

Flexibility for unrecorded needs was rejected per Rule 5.

---

## E. What could cause migration pain later?

1. **Settlement documents modeled as `LoadDocument`:**
   Migrating settlement source files out of load collections after driver data exists will require complex data reshuffling.
2. **Missing appointment time zones on `LoadStop`:**
   Historical appointment times will remain ambiguous if `facilityId` is null or if facility time zones are updated later.
3. **Whole-cent fuel unit pricing:**
   Accumulating fuel records in whole cents creates persistent discrepancy between pump prices, gallons, and transaction totals.
4. **Unversioned inspection checklists:**
   Changing inspection checklist items in later app releases will make older inspection records difficult to render accurately.
5. **Deleting cancelled loads instead of recording `CANCELLED` status:**
   Treating cancellations as deletions makes settlement reconciliation impossible when TONU pay appears later.

---

## F. What would you change before implementation?

1. **Fix Settlement Document Architecture:** Give `Settlement` direct ownership of `sourceFileIds` and update `ExtractionJob` to target `Settlement`.
2. **Preserve Appointment Time Zones:** Add `timeZoneSnapshot` to `LoadStop`.
3. **Add Event Time Zones:** Add `eventTimeZone` to `LoadPhoto`, `Inspection`, `ReeferReading`, and `LoadStatusChange`.
4. **Add `CANCELLED` Status:** Add `CANCELLED` to `Load.operationalStatus` for TONU tracking.
5. **Link Disposition Stops to Origination:** Add `originatingStopId` to `LoadStop`.
6. **Support Line-Level Settlement Matching:** Add `matchedLoadPayLineId` to `SettlementLine`.
7. **Fix Fuel Precision:** Store fuel unit price in mills/tenths of a cent.
8. **Enforce Admin Privacy Bounds:** Restrict `AdminAuditEvent` and `ExportJob` access.
9. **Resolve DM-Q01 through DM-Q07:** Apply owner decisions and synchronize diagrams with the data dictionary.

---

## G. Scenario walk-through

These are **simulated model walkthroughs** based on `design/testing/REAL_WORLD_SCENARIOS.md`, not real-world test data.

| Scenario | Records touched | Gap found? |
|---|---|---|
| **RW-01 Arrive at pickup** | `Load`, `LoadReference`, `CarrierProfile`, `Vehicle`, `Trailer`, `CheckEvent` (`CHECK_IN`), `LoadStop` (`ARRIVED`) | **Yes:** If `LoadStop.facilityId` is absent, appointment time zone is lost. Add `timeZoneSnapshot`. |
| **RW-02 No signal at shipper** | `CheckEvent` (`capturedOffline=true`, local timestamp/location), `LoadStop` (`ARRIVED/DEPARTED`) | **No missing entity:** Stable client-generated UUIDs support offline creation; clarify R1 wording from "sync exactly once" to "idempotent upsert". |
| **RW-03 Multi-stop changes order** | `Load.activeStopId`, `LoadStop.sequence`, `LoadStop.status`, `CheckEvent` | **No gap:** `Load.activeStopId` tracks the current stop independently of `LoadStop.sequence`. |
| **RW-04 Detention approaching** | `CheckEvent` (`CHECK_IN`), `LoadStop.onsiteThresholdMinutes`, derived `LoadStop.dwell`, local notification | **No gap:** Neutral Onsite timer and dwell time are cleanly derived per R10 and TM-D058. |
| **RW-05 Pickup paperwork changes stage** | `StoredFile`, `LoadDocument` (`PICKUP_BOL`), `ExtractionJob`, `Load.operationalStatus` (`IN_TRANSIT`), `LoadStatusChange` | **No gap:** `LoadStatusChange.revertsChangeId` cleanly supports undoing automated stage transitions. |
| **RW-06 Delivery with exception** | `LoadStop.exception`, `LoadPhoto` (`LOAD_CARGO`), `LoadDocument`, new `LoadStop` (`DISPOSITION`, `extraCompensation`), `LoadPayLine` (`DISPOSITION_PAY`) | **Yes:** `LoadStop` lacks `originatingStopId` linking the disposition to the delivery stop where freight was rejected. Photo stage on disposition needs confirmation (DM-Q06 -> `DELIVERY`). |
| **RW-07 Trailer swap** | `Trailer`, `EquipmentAssignment`, `Inspection` (`TRAILER_QUICK_CHECK`), `Defect`, optional `Reminder` | **Yes:** `Inspection` lacks `checklistVersion` to identify which checklist catalog was used. |
| **RW-08 Reefer load** | `ReeferProfile`, `ReeferReading`, `CriticalInstruction`, `LoadPhoto` (`TEMP`) | **Yes:** `ReeferReading` lacks `eventTimeZone` (R4). Optional link to `Trailer` is desirable if swapped mid-load. |
| **RW-09 Expiring insurance / recurring bill** | `EssentialDocument`, `Reminder` (`DOCUMENT_EXPIRATION`, `BILL`), optional `Expense` | **Yes:** Recurring `Reminder` lifecycle is inconsistent (`DONE` vs advancing `nextDueOn`). Maintenance reminders lack `vehicleId`/`trailerId`. |
| **RW-10 Settlement does not match** | `Settlement`, `SettlementLine`, `LoadPayLine`, `Load` (`settlementStatus = REVIEW_NEEDED`) | **Yes (CRITICAL):** Settlement source file cannot be a `LoadDocument` requiring one load. `SettlementLine` needs `matchedLoadPayLineId` for line-by-line reconciliation. |
| **RW-11 Find an old document** | `Load`, `LoadStop`, `LoadDocument`, `StoredFile`, `LoadReference` | **No gap:** Searchable fields (`loadNumber`, reference `value`, dates, stop names) exist on logical entities. |
| **RW-12 Support without document exposure** | `StaffUser`, `SupportCase`, `AccountActivity`, `AdminAuditEvent` | **Yes:** `AdminAuditEvent.before/after` unconstrained JSON risks exposing private document contents or precise locations to support staff. |

---

## H. Open questions DM-Q01 to DM-Q07

### DM-Q01 — Deletion of loads / expenses / reminders / settlements
**Agree with refinement.** 
- **Loads:** Must use a 30-day recoverable Trash state because a load anchors a complex evidence graph (stops, check-in events, photos, rate cons, BOLs). Accidental permanent deletion during a shift would be disastrous. Trashing a load cascades `trashedAt` to its child stops, documents, and photos.
- **Standalone records (standalone expenses, one-time reminders):** Immediate soft-delete with an immediate "Undo" snackbar is sufficient and avoids cluttering the driver's Trash with routine receipts or task check-offs. However, server tombstones should remain for sync consistency.

### DM-Q02 — Account deletion
**Agree strongly.**
In-app account deletion is mandatory under Apple App Store Guideline 5.1.1(v) and Google Play policies.
- The workflow must allow the driver to request an export first.
- The deletion process must revoke sessions, purge private files and documents from Cloud Storage within 30 days, and delete private database records.
- For community `FacilityReport`s, driver attribution must be anonymized (`reporterUserId = null` or scrubbed) to preserve crowdsourced parking and restroom intelligence while eliminating all links to the driver's identity or trip history.
- The app must display instructions for canceling store subscriptions via the platform App Store / Google Play account, as mobile apps cannot cancel platform subscriptions directly.

### DM-Q03 — Recurring bill → Expense prefill
**Agree with draft position.**
When a driver marks a recurring bill "Done" in Reminders, prompt: *"Log as Expense?"* and prefill an `Expense` using `expenseTemplate`. Do **not** auto-create the expense silently. A due date is an obligation; a payment is a financial transaction with a specific payment method and date that the driver must confirm.

### DM-Q04 — Merge PTI and trailer checks into one `Inspection` entity
**Agree.**
Both workflows share identical structures: timestamps, checklist items, defect logs, notes, and photos. Differentiating them via `kind: PTI_QUICK, PTI_DETAILED, TRAILER_QUICK_CHECK, TRAILER_DETAILED` reduces schema duplication and simplifies inspection history queries. `checklistVersion` must be added.

### DM-Q05 — Cancelled loads and TONU (Truck Order Not Used)
**DISAGREE with draft position.**
Deleting a cancelled load is a serious operational error. In trucking, carrier/broker cancellations after dispatch or arrival are frequent and billable via **TONU** fees ($150–$300).
- Drivers must retain the load record to preserve arrival timestamps, check-in evidence, and cancellation rate-con paperwork.
- When the settlement arrives next week, the settlement reconciliation engine must match the carrier's TONU payment line against the load. If the load was deleted, matching fails.
- **Proposed decision:** Add `CANCELLED` to `Load.operationalStatus` with optional cancellation reason and cancellation timestamp.

### DM-Q06 — Disposition-stop photo stage
**Agree with draft position (`DELIVERY`).**
TM-D078 restricts photo stages to `PICKUP` and `DELIVERY`. At a disposition stop (returning freight to shipper, dropping at an alternate receiver, or donating to a food bank), the driver is performing a delivery/hand-off. Labeling it `DELIVERY` complies with TM-D078 without expanding enum complexity.

### DM-Q07 — Facility de-duplication
**Agree with direction, with strict immutability for stops:**
- Matching incoming addresses to known facilities should use high-confidence normalization. If unmatched, create a driver-created `Facility`.
- Server-side deduplication can merge facilities by setting `mergedIntoFacilityId: ref Facility`.
- **Crucial rule:** Merging facilities must NEVER retroactively mutate historical `LoadStop` records. A stop's `nameSnapshot`, `addressSnapshot`, and `timeZoneSnapshot` must remain immutable records of what was contracted and serviced. Merges affect only aggregated community intelligence.

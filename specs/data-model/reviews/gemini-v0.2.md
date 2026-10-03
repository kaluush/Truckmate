# Data Model Confirmation Review (v0.2) — Gemini, 2026-10-02

**Reviewing:** `specs/data-model/DATA_MODEL.md` v0.2 at commit `f701dc711bcf97d234d3b82c1c2e3201bb10bbeb`
**Against:** `01_MASTER_SRS.md` and ACTIVE `02_MASTER_DECISIONS.md` (frozen through TM-D082)

---

## 1. Executive Summary & Verdict

**Verdict: VERIFIED & CONFIRMED (No "Must fix now" blockers remain).**

Data Model v0.2 cleanly and accurately incorporates:
1. All **15 accepted review findings** across both the data dictionary (§4) and the domain diagrams (§3).
2. All **four new owner decisions** (TM-D079, TM-D080, TM-D081, TM-D082).
3. The resolution of all open data-structure questions (**DM-Q01 through DM-Q07** / TM-Q029).

No regressions or breaking contradictions were introduced. The model is structurally sound, strictly respects driver privacy boundaries (TM-D053), enforces time-zone preservation (R4), and faithfully adheres to the V1 scope lock (TM-D055).

---

## 2. Verification of §8 Change Log (Accepted Findings)

Every accepted finding from §8 was verified against both the **Data Dictionary (§4)** and the **Mermaid Diagrams (§3)**:

| # | Item from §8 Change Log | Verified in Data Dictionary (§4)? | Verified in Diagrams (§3)? | Assessment |
|---|---|---|---|---|
| 1 | **Settlement scans decoupled from LoadDocument** (`Settlement.sourceFileIds`; `ExtractionJob` targets `Settlement`) | **Yes:** `Settlement.sourceFileIds: list of ref StoredFile` (§4.G). `LoadDocument.otherSubtype` removed `SETTLEMENT` and added `CANCELLATION` (§4.D). `ExtractionJob.documentRef` now explicitly permits `ref Settlement` (§4.D). | **Yes:** Diagram 3.3 removed old `LoadDocument \|o--o\| Settlement` edge; added `Settlement \|\|--o{ StoredFile : "source scans"` and `Settlement \|\|--o{ ExtractionJob : "AI processing"`. | **Applied correctly.** Fixes the multi-load settlement scanning blocker. |
| 2 | **`LoadStop.timeZoneSnapshot` & `eventTimeZone` across all events** | **Yes:** `LoadStop.timeZoneSnapshot: IANA tz Y` (§4.C) with appointment local times defined in that zone. `eventTimeZone: IANA tz Y` present on all 5 historical event entities: `CheckEvent` (§4.C), `LoadPhoto` (§4.D), `Inspection` (§4.E), `ReeferReading` (§4.C), and `LoadStatusChange` (§4.C). R4 updated (§2). | **Yes:** Scalar fields correctly omitted from entity relationship diagrams per §0 standard. | **Applied correctly.** Preserves facility wall-clock appointments and historical event context. |
| 3 | **Recurring reminders stay `ACTIVE` on Done** | **Yes:** `Reminder.status` (§4.F) notes that completing recurring reminders records `lastCompletedOn`, advances `nextDueOn`, and stays `ACTIVE`; `DONE` is reserved for one-time reminders. | **Yes:** Lifecycle rule; no diagram change needed. | **Applied correctly.** Resolves query filter contradictions for recurring obligations. |
| 4 | **Fuel precision (mills unit price; total + gallons authoritative)** | **Yes:** `Expense.fuel` (§4.G) stores `printedPricePerGallonMills` (optional); `amount` + `gallons` are source of truth. Rule R6 (§2) explicitly defines unit-price tenths-of-a-cent handling in mills. | **Yes:** Structure rule; no diagram change needed. | **Applied correctly.** Prevents commercial diesel price rounding drift. |
| 5 | **`Inspection.checklistVersion`** | **Yes:** `Inspection.checklistVersion: text Y` (§4.E) preserves the catalog version for historical interpretation. | **Yes:** Scalar field; no diagram change needed. | **Applied correctly.** Preserves checklist auditability across app updates. |
| 6 | **`AdminAuditEvent` restricted allow-list** | **Yes:** `AdminAuditEvent.before / after` (§4.I) explicitly constrained to administrative metadata; strictly prohibits document contents, extracted text, financial data, or exact GPS coordinates. | **Yes:** Payload constraint; no diagram change needed. | **Applied correctly.** Enforces TM-D053 privacy boundary. |
| 7 | **Idempotency wording & server contract (R1)** | **Yes:** R1 (§2) updated from ambiguous "exactly once" to explicit device-generated UUID idempotency key with server merge/ignore contract. | **Yes:** Architectural rule; no diagram change needed. | **Applied correctly.** Aligns with distributed offline sync realities. |
| 8 | **Facility merge lineage & reporter anonymization** | **Yes:** `Facility.mergedIntoFacilityId: ref Facility` (§4.B). `FacilityReport.reporterUserId` is optional (`—`), cleared upon account deletion while preserving the crowdsourced report (§4.B, TM-D081). | **Yes:** Diagram 3.4 includes `Facility \|o--o{ Facility : "merged into"` and `User \|o--o{ FacilityReport : "submits (private, cleared on deletion)"`. | **Applied correctly.** Protects crowdsourced data while enabling deduplication without rewriting historical trip stops. |
| 9 | **Reminder → Expense link kept (TM-D079)** | **Yes:** `Reminder.expenseTemplate` (§4.F) pre-fills "Log as expense?" on Done (never automatic). `Expense.fromReminderId: ref Reminder` (§4.G). | **Yes:** Diagram 3.3 shows `Reminder \|o--o{ Expense : "logged when paid"`. | **Applied correctly.** Eliminates double entry between bills and expenses. |
| 10 | **`SettlementLine.matchedLoadPayLineId`** | **Yes:** `SettlementLine.matchedLoadPayLineId: ref LoadPayLine` (§4.G) added for true line-by-line matching. | **Yes:** Diagram 3.3 includes `SettlementLine }o--o\| LoadPayLine : "matched to line"`. | **Applied correctly.** Enables line-level accessorial and rate reconciliation. |
| 11 | **`LoadStop.originatingStopId` for dispositions** | **Yes:** `LoadStop.originatingStopId: ref LoadStop` (§4.C) points to the delivery stop where freight was rejected. | **Yes:** Diagram 3.1 includes `LoadStop \|o--o{ LoadStop : "disposition of"`. | **Applied correctly.** Supports multi-stop rejection and disposition tracking. |
| 12 | **`Reminder.vehicleId` / `trailerId`** | **Yes:** `Reminder.vehicleId / trailerId: ref` (§4.F) added for maintenance reminders. | **Yes:** Diagram 3.2 shows `Vehicle \|o--o{ Reminder : "maintenance for"` and `Trailer \|o--o{ Reminder : "maintenance for"`. | **Applied correctly.** Allows filtering and organizing maintenance by equipment. |
| 13 | **`LoadPhoto.trashedAt`** | **Yes:** `LoadPhoto.trashedAt: instant` (§4.D) added to match R7's 30-day Trash rule. | **Yes:** Scalar field; no diagram change needed. | **Applied correctly.** Standardizes evidence deletion semantics. |
| 14 | **ExportJob driver-only download boundary** | **Yes:** `ExportJob.outputFileId` (§4.H) explicitly restricted to driver client downloads; staff may view metadata only. | **Yes:** Access control rule; no diagram change needed. | **Applied correctly.** Prevents bypass of driver document privacy via export archives. |
| 15 | **`ReeferReading.trailerId`** | **Yes:** `ReeferReading.trailerId: ref Trailer` (§4.C) added to attribute readings directly to specific trailers during mid-load equipment swaps. | **Yes:** Diagram 3.2 includes `Trailer \|o--o{ ReeferReading : "reading of"`. | **Applied correctly.** Eliminates temporal timestamp joins to identify alarming trailers. |

---

## 3. Verification of Owner Decisions TM-D079–TM-D082

The four owner rulings enacted on 2026-10-02 were verified across the requirements baseline, data dictionary, and diagrams:

1. **TM-D079 (Bill reminder → "Log as expense?"):**
   - **SRS & Decisions:** Recorded in `02_MASTER_DECISIONS.md`, and aligned with SRS §29 and §36.
   - **Model implementation:** Implemented in `Reminder.expenseTemplate` (§4.F), `Expense.fromReminderId` (§4.G), and Diagram 3.3 (`Reminder |o--o{ Expense`). Explicitly states that logging an expense is an opt-in prompt and never automatic.
   - **Confirmation:** Accurately reflected.

2. **TM-D080 (Cancelled loads & TONU pay lines):**
   - **SRS & Decisions:** Recorded in `02_MASTER_DECISIONS.md`, and added to SRS §7 and §35.
   - **Model implementation:** 
     - `Load.operationalStatus` (§4.C) includes `CANCELLED`.
     - `Load.cancelledAt` and `Load.cancellationReason` (§4.C) added.
     - `LoadDocument.otherSubtype` (§4.D) includes `CANCELLATION` for broker cancellation notices/rate-cons.
     - `LoadPayLine.type` (§4.G) includes `TONU`.
     - `SettlementLine.matchedLoadId` (§4.G) explicitly notes matching against cancelled loads.
   - **Confirmation:** Accurately reflected. Solves the TONU financial reconciliation gap without treating cancellations as deletions.

3. **TM-D081 (Account deletion):**
   - **SRS & Decisions:** Recorded in `02_MASTER_DECISIONS.md`, and added to SRS §19.
   - **Model implementation:** 
     - Rule R7 (§2) defines the end-to-end account deletion state machine.
     - `User.status` (§4.A) includes `DELETION_PENDING`.
     - `User.deletionRequestedAt` (§4.A) added.
     - `FacilityReport.reporterUserId` (§4.B) is made optional and cleared upon account deletion, while retaining the crowd-sourced parking/restroom record.
     - Diagram 3.4 updated to reflect `User |o--o{ FacilityReport : "submits (private, cleared on deletion)"`.
   - **Confirmation:** Accurately reflected. Meets Apple App Store and Google Play store compliance.

4. **TM-D082 (Record deletion — Load Trash vs Small records with Undo):**
   - **SRS & Decisions:** Recorded in `02_MASTER_DECISIONS.md`, and added to SRS §19.
   - **Model implementation:**
     - Rule R7 (§2) updated: deleting a `Load` cascades `trashedAt` to its stops, documents, photos, and evidence as a unit, restorable for 30 days. `Settlement` follows the same 30-day Trash rule because it anchors source financial scans.
     - Small records (`Expense`, `Reminder`) do not carry `trashedAt`; they are deleted immediately with in-app Undo and short-lived server tombstones. Attached files on small records still follow the 30-day file Trash rule.
     - `Load.trashedAt`, `LoadDocument.trashedAt`, `LoadPhoto.trashedAt`, and `Settlement.trashedAt` are correctly present.
   - **Confirmation:** Accurately reflected. Balances evidence safety for loads with clean UX for routine receipts and reminders.

---

## 4. Consistency and Regression Analysis

I inspected all cross-cutting rules, entity definitions, and diagrams to verify that the v0.2 changes did not introduce regressions or internal contradictions:

1. **Checked Stop Hierarchy and Cascade:**
   `LoadStop` maintains its immutable snapshots (`nameSnapshot`, `addressSnapshot`, `timeZoneSnapshot`) and properly references `originatingStopId` for dispositions. Its relationship to `CheckEvent`, `LoadPhoto`, and `LoadDocument` remains intact.
2. **Checked Privacy Boundary:**
   `AdminAuditEvent` restrictions and `ExportJob` client-only download rules guarantee that neither staff audits nor export jobs can expose driver documents or precise location data to administrative users (TM-D053, SRS §38).
3. **Checked Diagram Consistency:**
   All four Mermaid diagrams parse cleanly and reflect the dictionary's relationships. One minor non-blocking diagram observation is noted below.

---

## 5. Position on Rejected Finding (`MILEAGE_PAY`)

In the v0.1 review, Gemini suggested adding `MILEAGE_PAY` to `LoadPayLine.type`. Claude rejected this finding in v0.2 §8 with the rationale:
> *"Settlement reconciliation is O/O-only (TM-F031). A company driver's weekly estimate is already derived from PayProfile + MileageRecord (§11), so storing it as an expected pay line would add company-driver settlement matching, which is not V1 scope."*

### Gemini Position: **CONCUR WITH REJECTION.**

Upon detailed re-examination of the requirements and data model rules, Claude's rejection is architecturally sound and correctly preserves the V1 scope lock:

1. **Adherence to Rule R10 (Derived values are not source of truth):**
   Under SRS §11 and `PayProfile` (§4.A), a Company Driver's weekly earnings are derived dynamically:
   $$\text{Estimated Pay} = (\text{centsPerMile} \times \text{loadedMiles}) + \text{deadheadCompensation}$$
   Materializing this calculation into a discrete `LoadPayLine` record on each load would violate Rule R10 by storing derived data as a persistent operational record. If a driver retroactively updates their cents-per-mile rate, stored pay lines would desynchronize with derived estimates.
2. **Prevention of Scope Creep (TM-D055 / TM-F031):**
   `LoadPayLine` exists specifically for settlement reconciliation (`SettlementLine.matchedLoadPayLineId`). Under TM-F031, settlement reconciliation is an **O/O-only feature**. Company drivers receive pay estimates via SRS §11, not line-by-line settlement reconciliation against broker rate-con line items. Adding `MILEAGE_PAY` would create pressure to implement company-driver settlement matching in V1, violating the scope lock.
3. **Handling of Mileage-Paid Owner-Operators:**
   Owner-operators who haul on mileage-based contracts receive rate confirmations where freight compensation is negotiated and itemized as `LINEHAUL`. The existing `LoadPayLine.type = LINEHAUL` fully accommodates these contract amounts.

The rejection of `MILEAGE_PAY` is therefore correct and upheld.

---

## 6. Findings Table

| # | Question | Finding | Cites | Label | Proposed change |
|---|---|---|---|---|---|
| 1 | C | **Diagram 3.3 omits `EssentialDocument` link to `ExtractionJob`:** In the data dictionary (§4.D), `ExtractionJob.documentRef` explicitly supports `ref LoadDocument / EssentialDocument / Settlement`. Diagram 3.1 shows `LoadDocument \|\|--o{ ExtractionJob` and Diagram 3.3 shows `Settlement \|\|--o{ ExtractionJob`. However, Diagram 3.3 contains `EssentialDocument` but omits the relationship to `ExtractionJob`, even though wallet documents undergo AI classification and expiration extraction (SRS §13, §14). | SRS §6, §13, §14, §25 | **Optional future** | In a future diagram clean-up, add `EssentialDocument \|\|--o{ ExtractionJob : "AI processing"` to Diagram 3.3. This is purely cosmetic; the data dictionary in §4.D is already correct. |
| 2 | — | **No further findings:** Checked all entities in §4 against SRS §3–§39, Master Decisions TM-D001–TM-D082, and the 15 resolved v0.1 findings. All entities, relationships, constraints, and open question resolutions are consistent and complete. | TM-D001–TM-D082, SRS §1–§40 | — | None. |

---

## 7. Final Recommendation

Data Model v0.2 is **complete, coherent, and ready for owner approval**. 

Per the owner-set review order in `04_CURRENT_WORK.md` and `06_AI_HANDOFF.md`, this review satisfies step (1). The model may proceed to **GPT's final review**, followed by official owner approval and freezing.

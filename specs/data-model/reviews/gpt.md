# Data Model Review — GPT, 2026-10-01

**Reviewing:** `specs/data-model/DATA_MODEL.md` at commit `35b02902e1fc196d043dd24506186587a602707c`
**Against:** `01_MASTER_SRS.md` and ACTIVE `02_MASTER_DECISIONS.md` (frozen through TM-D078)

## Findings table

| # | Question | Finding | Cites | Label | Proposed change |
|---|---|---|---|---|---|
| 1 | A/C/E | Appointment time zone is not preserved when `LoadStop.facilityId` is absent. The model requires local facility time + IANA zone, but the stop snapshot stores name/address only. | R4, SRS §3 | **Must fix now** | Store `timeZoneSnapshot` on LoadStop (or inside appointment) whenever an appointment exists. Do not rely on a Facility link being present forever. |
| 2 | A/C | `Settlement.sourceDocumentId -> LoadDocument` conflicts with `LoadDocument.loadId` being required. A settlement may contain many loads and is not naturally owned by one load. | TM-D028, TM-D070, SRS §12 | **Must fix now** | Make settlement scan/source files belong directly to Settlement (e.g. `sourceFileIds`) or introduce a generic document owner. Ensure AI extraction can target Settlement without fabricating a load association. |
| 3 | A/C | R4 says historical moments keep UTC + IANA time zone, but several event records do not expose a time-zone field (LoadPhoto, Inspection, ReeferReading, and others). | R4, TM-D078, TM-D027, TM-D043 | **Must fix now** | Either add a standard `eventTimeZone` rule for event-like records or explicitly list it on each affected entity. |
| 4 | C | Recurring Reminder lifecycle is internally inconsistent: `status=DONE` while the same record advances `nextDueOn` for the next recurrence. A recurring reminder cannot be both finished and still active. | TM-D066, SRS §36 | **Must fix now** | For recurring reminders, completion should update `lastCompletedOn` and advance `nextDueOn` while status remains `ACTIVE`; use `DONE` only for one-time reminders, or model occurrences separately. |
| 5 | E | Fuel `pricePerGallonMinor` uses whole cents. U.S. fuel is commonly priced to tenths of a cent, so this can lose precision and make amount/gallons reconciliation drift. | R6, SRS §29 | **Must fix now** | Prefer total `amount` + `gallons` as source of truth and derive unit price, or use a higher-precision unit-price field. |
| 6 | E | R1 wording says device UUIDs make sync happen “exactly once.” Stable IDs make retries idempotent only if the server treats the ID as the idempotency key/upsert identity. | TM-D024, TM-D025, TM-D047 | **Must fix now** | Change wording to “idempotent/effectively-once record creation” and state that server create/upsert must reject/merge the same ID rather than create a second row. |
| 7 | A/E | Inspection history does not preserve which checklist version produced the stored `itemCode` results. Checklist content is explicitly versioned app content, so later label/item changes could make old history ambiguous. | SRS §15, §33, TM-D027, TM-D042 | **Must fix now** | Add `checklistVersion` (or equivalent immutable checklist definition ID) to Inspection. |
| 8 | C | `AdminAuditEvent.before/after` is unconstrained JSON. That creates a path for private document content or precise location to be copied into admin-visible audit records, conflicting with the admin privacy boundary. | TM-D053, SRS §19, §38 | **Must fix now** | Restrict audit snapshots to approved metadata fields and explicitly prohibit private document contents, extracted text, and precise location. |
| 9 | D/E | Facility merge is proposed in DM-Q07, but Facility has no canonical/merge lineage. Rewriting every historical `facilityId` later is avoidable migration work. | TM-D034, DM-Q07 | **Architecture must allow later** | Keep LoadStop snapshots as drafted; add a cheap canonical/merge mechanism such as `mergedIntoFacilityId` or an alias mapping so duplicate facilities can resolve without rewriting trip history. |
| 10 | C | Diagram 3.3 shows Reminder → Expense as if already approved, while DM-Q03 correctly says that behavior still needs owner confirmation. | DM-Q03 | **Must fix now** | Mark the relationship as proposed/pending in the diagram or omit it until DM-Q03 is approved; the draft should not visually promote an open product decision to settled structure. |
| 11 | A/C | `FacilityReport.reporterUserId` is required, but DM-Q02 proposes anonymizing reports after account deletion. Those two rules cannot both remain true without a defined unlink/anonymization mechanism. | DM-Q02, TM-D034, TM-D007 | **Must fix now** | If account deletion/anonymization is approved, make contributor identity detachable (nullable after deletion or separate private contribution linkage) while preserving aggregate facility information. |
| 12 | B | The overall entity decomposition is not obviously overcomplicated. Load/stop/evidence, equipment history, provenance, and parallel financial records map to distinct approved behaviors; no large entity group should be removed merely to reduce count. | SRS §25, TM-D017, TM-D041, TM-D048 | **Optional future** | No simplification change recommended here; focus on the concrete fixes above rather than collapsing valid concepts. |

## A. What is missing?

The main missing pieces are: appointment time-zone preservation independent of Facility, a clean settlement-document ownership path, consistent event time-zone storage, checklist version identity, and a safe facility-merge/anonymization path. These are structural gaps, not requests for new V1 features.

## B. What is unnecessarily complicated?

I checked the approximately 40 entities against SRS §25 and later approved decisions. I did **not** find a major entity group that should be removed. The separate concepts for LoadStop, EquipmentAssignment, FieldRevision, SyncConflict, SettlementLine and StoredFile are justified by approved requirements. The model should be repaired rather than broadly collapsed.

The proposed PTI/trailer-check merge into one Inspection entity is a good simplification if the owner approves DM-Q04.

## C. What relationship is wrong or risky?

The most important relationship issue is Settlement → LoadDocument. A load document requires one load, while a settlement can cover multiple loads. The next risk is relying on Facility for appointment time-zone identity even though `facilityId` is optional. The Reminder → Expense edge is also premature until DM-Q03 is decided.

## D. What should be more flexible?

Only one change is recommended for an already-recorded future need: Facility de-duplication should allow canonical/merged facility identity without rewriting historical LoadStop snapshots. Team/shared loads are already reasonably protected by R2; more trailer types are protected by string-code enums; more tiers are protected by Tier/TierEntitlement.

## E. What could cause migration pain later?

Highest risk:
1. Settlement scans forced into a load-owned document type.
2. Missing appointment time-zone snapshot.
3. No canonical facility merge lineage.
4. No checklist version on historical inspections.
5. Treating whole cents as sufficient precision for per-gallon unit price.

## F. What would you change before implementation?

1. Fix Settlement document ownership/extraction.
2. Fix time-zone persistence (appointments and event records).
3. Fix recurring Reminder lifecycle.
4. Add checklist version identity.
5. Constrain admin audit data.
6. Correct sync/idempotency wording and contract.
7. Resolve DM-Q01–DM-Q07, then update the model and diagrams together.

## G. Scenario walk-through

These are **simulated model walkthroughs**, not real-world test evidence.

| Scenario | Records touched | Gap found? |
|---|---|---|
| RW-01 Arrive at pickup | User.currentLoadId, Load, LoadStop, LoadReference, EquipmentAssignment, CheckEvent | **Yes:** appointment zone must survive even if no Facility record is linked. |
| RW-02 No signal at shipper | CheckEvent, Device, local StoredFile/LoadDocument, SyncConflict only if later manual conflict | No missing entity; stable client IDs support retry, but “exactly once” wording needs correction. |
| RW-03 Multi-stop changes order | Load.activeStopId, LoadStop.sequence/status, CheckEvent | No data with nowhere to live. |
| RW-04 Onsite threshold approaching | LoadStop.onsiteThresholdMinutes, CheckEvent, derived dwell, Reminder/notification derivation | No data with nowhere to live. |
| RW-05 Pickup paperwork changes stage | LoadDocument, StoredFile, ExtractionJob, LoadStatusChange, FieldRevision | No core gap; automatic change/revert is representable. |
| RW-06 Delivery with exception | LoadStop.exception, LoadPhoto, LoadDocument, CriticalInstruction/extraCompensation, disposition LoadStop | **Pending DM-Q06:** disposition photos need an approved stage rule. |
| RW-07 Trailer swap | EquipmentAssignment, Trailer, Inspection, Defect, Reminder | **Yes:** add checklistVersion so the historical Quick Trailer Check remains interpretable. |
| RW-08 Reefer load | ReeferProfile, ReeferReading, LoadPhoto(TEMP), CriticalInstruction, EquipmentAssignment | **Yes:** ReeferReading historical time-zone representation should follow R4 explicitly. |
| RW-09 Expiring insurance / recurring bill | EssentialDocument, Reminder, optional Expense | **Yes:** recurring Reminder status lifecycle is inconsistent; Reminder→Expense is pending DM-Q03. |
| RW-10 Settlement does not match | Settlement, SettlementLine, LoadPayLine, Load, source scan/extraction | **Yes:** settlement source scan cannot cleanly be a LoadDocument requiring one load. |
| RW-11 Find an old document | Load, LoadStop, LoadDocument, StoredFile, LoadReference | No missing logical entity; search implementation is correctly deferred. |
| RW-12 Support without document exposure | SupportCase, AccountActivity, StaffUser, AdminAuditEvent | **Yes:** constrain AdminAuditEvent before/after so private document/location data cannot leak into support-visible metadata. |

## H. Open questions DM-Q01 to DM-Q07

### DM-Q01 — deletion of loads / expenses / reminders / settlements
**Partly agree.** A load should use a recoverable soft-delete/Trash pattern because it owns a large evidence graph. I would avoid having two unrelated deletion semantics (“30-day Trash” for loads but ad-hoc undo for other user records) unless UX testing justifies it. At the logical-model level, a common `deletedAt`/trash state across user-owned business records is easier to reason about; retention duration can differ by type if the owner wants.

### DM-Q02 — account deletion
**Agree that it must be resolved before implementation.** It affects ownership, files, facility reports, audit history and billing references. The model should define an explicit deletion/anonymization state machine rather than a giant cascade performed synchronously. Export-before-delete can be offered, but deletion must not depend on export completion.

### DM-Q03 — recurring bill → expense
**Agree with the draft position:** when the user marks a bill complete, offer “Log as expense?” and prefill it. Do not auto-create the expense because “due” and “paid” are not the same fact.

### DM-Q04 — merge PTI and trailer checks
**Agree.** One Inspection entity with a `kind` discriminator is simpler and still preserves the different workflows. Add checklistVersion.

### DM-Q05 — cancelled loads
**Disagree with the draft position.** Do not treat cancellation as deletion. A cancelled/tender-withdrawn load is a real historical business event and should remain searchable without pretending it was delivered or deleting its record. I recommend owner approval of a `CANCELLED` operational status with optional cancellation reason/time. This is a product-state decision and should not be inserted silently.

### DM-Q06 — disposition-stop photo stage
**Agree:** use `DELIVERY`. The freight is being handed off at the disposition stop, and this preserves TM-D078’s two-stage model.

### DM-Q07 — facility de-duplication
**Agree with the direction, with one structural addition:** keep LoadStop snapshots exactly as drafted and give Facility a canonical/merge lineage. Matching can use normalized address/location as implementation detail, but historical stops should never be rewritten merely because two Facility records were merged.

## Review outcome

**REPAIR THEN REVIEW AGAIN.** The draft has a sound overall shape and good traceability, but the findings above should be reconciled before the owner approves it as the authoritative V1 data model. None of these findings require discarding the model or starting over.

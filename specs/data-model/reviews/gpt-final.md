# Data Model Final Review — GPT, 2026-10-02

**Reviewing:** `specs/data-model/DATA_MODEL.md` v0.2.1 at commit `f0110b466ee42c5a9c318e930d213e3e1788dd8b`  
**Against:** `AGENTS.md`, `01_MASTER_SRS.md`, ACTIVE `02_MASTER_DECISIONS.md` through TM-D082, `specs/data-model/reviews/gpt.md`, `specs/data-model/reviews/gemini.md`, `specs/data-model/reviews/gemini-v0.2.md`, and `design/testing/REAL_WORLD_SCENARIOS.md`.

This is the owner-requested **last and final review before owner approval**. All scenario walk-throughs below are **simulated model walk-throughs**, not real-world field evidence.

---

## 1. Earlier GPT review — disposition of every finding

| Earlier GPT finding | Final check | Assessment |
|---|---|---|
| 1. Appointment time zone missing without Facility | `LoadStop.timeZoneSnapshot` is required and appointments are explicitly interpreted in that zone. | **Fixed correctly.** SRS §3; R4. |
| 2. Settlement source document wrongly forced through LoadDocument | Settlement owns `sourceFileIds`; `ExtractionJob.documentRef` accepts Settlement; `SETTLEMENT` is removed from LoadDocument subtype. | **Fixed correctly.** SRS §12; TM-D070. |
| 3. Event time zones missing | R4 now requires `eventTimeZone` on CheckEvent, LoadPhoto, Inspection, ReeferReading and LoadStatusChange; fields are present. | **Fixed correctly.** SRS §8/§15/§33; TM-D078. |
| 4. Recurring Reminder could be DONE and active at once | Recurring completion records `lastCompletedOn`, advances `nextDueOn`, and remains `ACTIVE`; `DONE` is one-time only. | **Fixed correctly.** SRS §36; TM-D066. |
| 5. Fuel unit price precision | Total amount + gallons are authoritative; optional printed unit price is stored in mills. | **Fixed correctly.** SRS §29; R6. |
| 6. “Exactly once” sync wording | R1 now states device UUID is an idempotency key and server create/retry is merge/ignore, i.e. effectively-once. | **Fixed correctly.** SRS §20; TM-D025/TM-D047. |
| 7. Inspection checklist version missing | `Inspection.checklistVersion` is required. | **Fixed correctly.** SRS §15/§33; TM-D027/TM-D042. |
| 8. AdminAuditEvent unrestricted JSON | Before/after are restricted to an allow-list of administrative metadata and explicitly exclude document contents, extracted text, financial details and precise location. | **Fixed correctly.** SRS §38; TM-D053. |
| 9. Facility merge lineage missing | `Facility.mergedIntoFacilityId` exists; historical LoadStop snapshots are not rewritten. | **Fixed correctly.** SRS §16; TM-D034. |
| 10. Reminder → Expense was premature while DM-Q03 was open | TM-D079 now explicitly approves the prompt; `Reminder.expenseTemplate` and `Expense.fromReminderId` match it. | **Fixed correctly after owner decision.** SRS §29/§36; TM-D079. |
| 11. FacilityReport anonymization conflicted with required reporter | `reporterUserId` is nullable and cleared on account deletion. | **Substantive fix applied, but see Final Finding F-03 below about R1 ownership scope.** SRS §19; TM-D081. |
| 12. No large simplification needed | v0.2.1 keeps the justified decomposition rather than collapsing distinct approved concepts. | **Correctly left unchanged.** SRS §25; TM-D017/TM-D041/TM-D048. |

### Earlier-review conclusion
All concrete repairs from the earlier GPT review were either implemented correctly or, in the case of Reminder → Expense, became valid after explicit owner approval under TM-D079. The previous review’s “repair then review again” condition was therefore substantially satisfied.

---

## 2. Gemini reviews and the MILEAGE_PAY rejection

### Gemini’s v0.1 findings
I agree with the accepted Gemini findings incorporated into v0.2: settlement line-to-pay-line matching, disposition-origin linkage, maintenance reminder equipment linkage, LoadPhoto trash metadata, driver-only ExportJob downloads, and direct ReeferReading trailer attribution all improve correctness without adding a new V1 product capability.

### Gemini’s v0.2 verdict
I agree with Gemini’s statement that the **15 merged review fixes were incorporated correctly** and that its diagram-only `EssentialDocument → ExtractionJob` observation was correctly repaired in v0.2.1.

I do **not** agree with the broader final statement that no Must-fix blockers remain, because the fresh end-to-end pass below found three specification/model consistency issues that Gemini’s confirmation pass did not surface.

### MILEAGE_PAY rejection
I **agree with the rejection** of Gemini’s proposed `MILEAGE_PAY` `LoadPayLine.type`.

Reason:
- SRS §11 defines Company Driver mileage pay as an **estimate** derived from pay settings and mileage.
- Settlement reconciliation is part of the O/O financial layer (SRS §12; TM-D054/TM-D060).
- R10 says derived values are not source-of-truth records unless necessary.
- Persisting a Company Driver mileage estimate as a `LoadPayLine` would blur the boundary between an estimate and an expected settlement line, and would create pressure to add Company Driver settlement reconciliation that is not approved V1 scope.

**Label:** Optional future — if a later version explicitly adds Company Driver pay-statement reconciliation, it may introduce a separate paid-mile/pay-statement model then. No V1 change is recommended now.

---

## 3. Owner decisions TM-D079–TM-D082 — cross-file verification

| Decision | 02_MASTER_DECISIONS.md | SRS | DATA_MODEL.md v0.2.1 | Final assessment |
|---|---|---|---|---|
| **TM-D079** — bill Done → offer “Log as expense?”, never automatic | Present and ACTIVE. | SRS §36 states the one-time offer, prefill and no automatic expense creation. | `Reminder.expenseTemplate`, `Expense.fromReminderId`, and Reminder→Expense relationship match. | **Correctly reflected.** |
| **TM-D080** — Cancelled load retained; optional time/reason; TONU can reconcile later | Present and ACTIVE. | SRS §7 and §35 include Cancelled without deletion. | `Load.operationalStatus=CANCELLED`, cancellation metadata, `TONU` pay-line type, settlement matching to cancelled loads. | **Correctly reflected.** |
| **TM-D081** — in-app account deletion; export offered not required; private data deleted; facility reports anonymized; store subscription cancellation explained | Present and ACTIVE. | SRS §19 matches. | R7, `User.DELETION_PENDING`, device revocation, deletion job, nullable `FacilityReport.reporterUserId`. | **Mostly correct; Final Finding F-03 must be clarified before approval.** |
| **TM-D082** — load 30-day Trash as a whole; small records immediate delete + Undo | Present and ACTIVE. | SRS §19 matches load vs. small-record behavior. | R7 matches for Load, Expense and Reminder. | **Correctly reflected for the approved cases, but Final Finding F-02 identifies an unapproved Settlement extension.** |

---

## 4. Fresh end-to-end pass — RW-01 to RW-12

All walk-throughs below are **simulated**.

| Scenario | Simulated walk-through result | Finding / reason |
|---|---|---|
| **RW-01 — Arrive at pickup** | LoadStop snapshot preserves name/address/time zone; LoadReference holds pickup/release number; current equipment is available from EquipmentAssignment; CheckEvent captures check-in. | **Nothing found.** The earlier appointment-zone gap is repaired, and the model has a direct place for each gate/check-in datum required by SRS §3 and TM-D049. |
| **RW-02 — No signal at shipper** | Device creates CheckEvent offline with device time/location and stable ID; later sync uses idempotent identity and provenance. | **Finding F-01 — Must fix now.** `CheckEvent.serverReceivedAt` is marked required even though SRS §20/TM-D025 require the event to exist locally before any server receipt is possible. |
| **RW-03 — Multi-stop changes order** | `Load.activeStopId` can move independently of `LoadStop.sequence/status`; earlier stops are not falsely completed. | **Nothing found.** This directly represents TM-D045 without forcing fake workflow transitions. |
| **RW-04 — Onsite threshold approaching** | Dwell derives from CheckEvents; stop/user threshold is present; notification can be derived without creating entitlement/payment logic. | **Nothing found.** The model preserves neutral Onsite evidence and does not generate detention pay, consistent with SRS §8 and TM-D058. |
| **RW-05 — Pickup paperwork changes stage** | LoadDocument + ExtractionJob can trigger LoadStatusChange; trigger document is retained and a later user correction can revert the automatic change. | **Nothing found.** The automatic-stage path and recovery path are both representable under SRS §5/§7 and TM-D032/TM-D063. |
| **RW-06 — Delivery with exception** | Exception quantity/reason live on delivery LoadStop; evidence attaches to that stop; disposition remains same Load and links back through `originatingStopId`; extra compensation can be unknown/none/amount. | **Nothing found.** The full same-load rejection/disposition story required by SRS §7 and TM-D033 is represented. |
| **RW-07 — Trailer swap** | New EquipmentAssignment starts at swap; old history remains; Quick Trailer Check links to the assignment or skip reason; checklist version is retained. | **Nothing found.** SRS §32/§33 and TM-D041/TM-D042 are representable without rewriting prior history. |
| **RW-08 — Reefer load** | Load-level set point/mode live in ReeferProfile; changing fuel/status/temp observations live in ReeferReading; trailer attribution survives a swap. | **Nothing found.** The split matches SRS §33/TM-D043 and avoids Dry Van clutter. |
| **RW-09 — Expiring insurance / recurring bill** | EssentialDocument drives expiry Reminder; recurring bill stays ACTIVE across completions; bill Done may create an Expense only after user confirmation. | **Nothing found.** The lifecycle now matches SRS §14/§36, TM-D066 and TM-D079. |
| **RW-10 — Settlement does not match** | Settlement owns source scans; ExtractionJob can process it; SettlementLine can match both Load and specific LoadPayLine and retain mismatch state. | **Finding F-02 — Must fix now.** Reconciliation itself is sound, but v0.2.1 silently gives Settlement a 30-day Trash behavior that TM-D082 and SRS §19 never approve. |
| **RW-11 — Find an old document** | Load/reference/facility snapshots, document category/stop attribution and retained load history give the search layer stable fields; physical search technology is intentionally deferred. | **Nothing found.** SRS §18 is supported and implementation-specific indexing remains properly deferred. |
| **RW-12 — Account support without document exposure** | AccountActivity exposes only limited support metadata; ExportJob file is driver-only; AdminAuditEvent excludes private content and precise location. | **Finding F-03 — Must fix now.** R1’s standard `ownerUserId` rule is not explicitly exempted for retained anonymous FacilityReport rows, so TM-D081 unlinking is ambiguous. |

---

## 5. Final findings

### F-01 — Offline CheckEvent cannot satisfy required `serverReceivedAt`
**Label:** **Must fix now**  
**Cites:** SRS §20; TM-D025; TM-D047.

`CheckEvent.serverReceivedAt` is marked **Req Y**. But RW-02 explicitly requires check-in/out to be saved while offline, before a server has received anything. A locally valid CheckEvent therefore cannot satisfy the model as written.

**Required repair:** make `serverReceivedAt` nullable/pending on the client and required only after successful server receipt/sync, or state an equivalent two-phase invariant. This is a model correctness fix, not a new V1 feature.

**Migration risk if left:** implementers may either fabricate a receipt timestamp, reject legitimate offline records, or later need a nullability/data migration after offline support is already shipped.

---

### F-02 — Settlement deletion behavior is asserted without an owner decision
**Label:** **Must fix now**  
**Cites:** SRS §19; TM-D082; DATA_MODEL §6 DM-Q01.

TM-D082 approves:
- 30-day Trash for a **Load** and its evidence; and
- immediate delete + Undo for **small records such as Expense and Reminder**.

SRS §19 says the same. Neither source specifies what happens when the user deletes a **Settlement**.

v0.2.1 nevertheless states in R7 that **Settlements follow the same Trash rule as loads**, includes `Settlement.trashedAt`, and marks DM-Q01 — whose question explicitly included Settlement deletion — as fully resolved by TM-D082.

That goes beyond the approved decision.

**Required repair:** do not claim Settlement deletion semantics are owner-approved. Either remove the settlement-specific deletion rule/field from the approved V1 logical model until behavior is decided, or obtain a separate owner ruling. The safer final-model fix is to leave deletion behavior unspecified rather than invent it.

**Migration/product risk if left:** the model would freeze a retention/restoration behavior that is absent from project truth, turning a data-model assumption into accidental V1 product behavior.

---

### F-03 — FacilityReport anonymization is ambiguous under R1 ownership
**Label:** **Must fix now**  
**Cites:** SRS §19; TM-D081; TM-D007; TM-D053.

R1 says every user-data record carries `ownerUserId`. The FacilityReport table separately carries nullable `reporterUserId`, which is cleared on account deletion. TM-D081 requires retained facility reports to have the user’s identity removed.

The model does not explicitly state whether FacilityReport is exempt from R1 `ownerUserId`. If it is not exempt, clearing only `reporterUserId` still leaves a direct user link and violates the deletion/anonymization decision.

**Required repair:** explicitly classify FacilityReport as shared/reference community data that does **not** carry R1 `ownerUserId`, or explicitly require every user-link field including ownership to be cleared during account deletion. The former is cleaner because the report is retained as anonymous shared data.

**Privacy/migration risk if left:** an implementation could persist an ownership foreign key that later has to be scrubbed from every retained facility report, or worse, retain identity contrary to TM-D081.

---

## 6. Architecture must allow later

### Team/shared loads
**Label:** **Architecture must allow later**  
**Cites:** TM-D022; SRS §21.

**Nothing new found.** R2 keeps per-driver Current Load state outside Load and explicitly prohibits a physical layout that makes future membership impossible. That is the correct degree of future-proofing without building team loads in V1.

### Additional trailer types and tiers
**Label:** **Architecture must allow later**  
**Cites:** TM-D040; SRS §32; SRS §39.

**Nothing new found.** String-code enums and Tier/TierEntitlement permit extension without requiring V1 workflows for unapproved types/tiers.

---

## 7. Optional future

### Company Driver pay-statement reconciliation
**Label:** **Optional future**  
**Cites:** SRS §11; SRS §12; TM-D054/TM-D060.

No V1 change. If a later version explicitly supports Company Driver pay-statement reconciliation, model that statement as its own approved workflow rather than forcing current mileage estimates into O/O `LoadPayLine`.

### Search/index implementation
**Label:** **Optional future**  
**Cites:** SRS §18; DATA_MODEL §7.

No logical-model blocker found. Firestore indexes vs. a dedicated search index remains correctly deferred until implementation evidence requires a choice.

---

## 8. Final verdict

# NOT READY

The overall model is strong, and the previous GPT/Gemini structural repairs are correctly merged. I agree with Gemini on the correctness of those merged fixes and with the `MILEAGE_PAY` rejection.

Before the owner makes v0.2.1 official, exactly these **Must-fix** items should be repaired:

1. **F-01:** make `CheckEvent.serverReceivedAt` compatible with legitimate offline creation.
2. **F-02:** remove or explicitly decide the unapproved Settlement deletion/Trash behavior; TM-D082 does not resolve it.
3. **F-03:** make FacilityReport’s exemption from `ownerUserId` explicit (or clear all ownership identity) so TM-D081 anonymization is guaranteed.

No new V1 feature is required. After these three consistency repairs, the data model should be ready for owner approval without another broad redesign.

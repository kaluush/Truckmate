# Data Model Review — <AI name>, <date>

**Reviewing:** `specs/data-model/DATA_MODEL.md` at commit `<hash>`
**Against:** `01_MASTER_SRS.md` and ACTIVE `02_MASTER_DECISIONS.md` (frozen through TM-D078)

Save this file as `specs/data-model/reviews/<ai-name>.md`. Do not edit `DATA_MODEL.md` directly.

## Rules

1. **"Looks good" is not a review.** Answer every question below.
2. **"Nothing found" is allowed only with a reason** — say what you checked (e.g. "checked every stop-linked entity against §3, §8, TM-D078").
3. **Every finding cites** the SRS section or decision it relates to.
4. **Every finding is labeled:**
   - **Must fix now** — wrong, missing, or unsafe for the frozen V1.
   - **Architecture must allow later** — not built now, but the model must not block it.
   - **Optional future** — nice to have; no change now.
5. **Flexibility must be earned.** A "should be more flexible" finding must name a real future need already on record (e.g. TM-D022 team loads, TM-D040 more trailer types, §39 more tiers, a Future item in `03_FEATURE_BANK.md`). Flexibility for an imagined need is speculation and will be rejected.
6. **No new V1 features.** If a finding needs new product behavior, label it and send it to the owner; do not design it into the model.
7. Do not present simulated walkthroughs as real-world evidence.

## Findings table

| # | Question (A–G) | Finding | Cites | Label | Proposed change |
|---|---|---|---|---|---|
| 1 | | | | | |

## A. What is missing?
Entities, fields, relationships or states the approved requirements need but the model lacks.

## B. What is unnecessarily complicated?
Entities, fields or rules that could be removed or merged without losing an approved requirement.

## C. What relationship is wrong or risky?
Wrong cardinality, wrong owner, data attached to the wrong record, privacy leaks through relationships.

## D. What should be more flexible?
Only with a named, recorded future need (rule 5).

## E. What could cause migration pain later?
Choices that are cheap now and expensive to change once real driver data exists.

## F. What would you change before implementation?
Your top changes, in priority order. Keep it short.

## G. Scenario walk-through
Walk each scenario in `design/testing/REAL_WORLD_SCENARIOS.md` (RW-01 to RW-12) through the model. For each: which records are created or changed, and is there any data with nowhere to live?

| Scenario | Records touched | Gap found? |
|---|---|---|
| RW-01 Arrive at pickup | | |
| RW-02 No signal at shipper | | |
| RW-03 Multi-stop changes order | | |
| RW-04 Onsite threshold approaching | | |
| RW-05 Pickup paperwork changes stage | | |
| RW-06 Delivery with exception | | |
| RW-07 Trailer swap | | |
| RW-08 Reefer load | | |
| RW-09 Expiring insurance / recurring bill | | |
| RW-10 Settlement does not match | | |
| RW-11 Find an old document | | |
| RW-12 Support without document exposure | | |

## H. Open questions DM-Q01 to DM-Q07
Agree or disagree with each draft position in §6, with a reason.

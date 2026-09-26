# CabPilot V1 — Claude Independent Design (Stage 1)

**Status:** COMPLETE — awaiting Stage 2 requirement check and owner review. Not self-approved.
**Date:** 2026-09-26
**Baseline used:** `02_MASTER_DECISIONS.md` TM-D001–TM-D056, `01_MASTER_SRS.md` §1–40, `03_FEATURE_BANK.md`.
**Isolation:** GPT's and Gemini's independent designs were NOT read before completing this design. (`design/independent/gpt/DESIGN_V1.md` was present on `origin/main` at commit `e7624d6` and was deliberately left unopened.)

**Visual design (mockups, clickable between screens):** `CabPilot_V1_Claude_Screens.html` in this folder (open it in a browser). A private hosted copy was also published for the owner.

---

## 1. Core concept

**One card, one button.** The Current Load Card on **Today** changes what it puts first by stage. The next step always appears as a full-width green button in the same fixed spot above the tab bar:

`Check In → Check Out (+ share) → Scan BOL → Check In → Scan signed POD → next load`

The card scrolls; the action button never moves.

## 2. Navigation structure

Bottom bar: **Today · Loads · [Scan] · Wallet · More** (4 tabs + centre capture button).

| Destination | Contains |
|---|---|
| **Today** (launch screen) | Current Load Card (stage-aware), stop strip → Active Stop picker, dwell timer, check-out/share sheet, facility check prompt, Driving view (in motion), top-3 Upcoming due items, next Pre-planned load, empty state = scan rate con |
| **Loads** | Search (load #, PU #, BOL, facility, date) always on top; Current · Pre-planned · History in one list; Load detail (status chips, document package, stops + provenance-tagged times, equipment history, miles, pay, instructions, edit history); Miles sources; Facility page |
| **Scan** (centre) | Scan paperwork / Import PDF-photo; quick-add Expense, Upcoming item, PTI. After capture: classification + **Where does this go?** attribution; new-load extraction review |
| **Wallet** | Standard slots + custom documents, sorted by urgency with countdown chips; document detail with reminder cadence, replace, history |
| **More** | Money (O/O) or Pay estimate (Company Driver); Upcoming (full list + Add); Inspections (Quick/Detailed PTI); Equipment; Export; Trash; Settings; account/trial |

Admin/operations is a separate **web** tool: Dashboard · Users · Subscriptions · Support/Recovery · Controls.

## 3. Screen inventory (all shown in the HTML)

- **First run:** Sign in (phone) → OTP → Setup (optional backup email with an explicit recovery warning; "How are you paid?" → Company Driver / O/O; 60-day trial notice).
- **Today:** pickup stage (Pickup # hero, facility, appointment countdown, Critical Load Instructions in solid amber, on-card gate-info grid, 4-slot paperwork row); checked-in/dwell (offline variant); check-out → share status sheet with field chips; facility check (opt-in, tap-only); facility page (driver reports vs public info, conflicts labelled); stop picker (out-of-order, "D1 stays open"); In Transit with reefer strip and auto-stage Undo toast; Driving view; empty state.
- **Scan:** capture sheet; attribution (pre-selected only when the page matches a load, with the reason shown; otherwise nothing is pre-selected); extraction review (only uncertain fields highlighted; save defaults to Pre-planned when a Current load exists).
- **Delivery:** at delivery (primary = Scan signed POD); partial/full rejection (stepper + reason chips + photos, optional note); disposition as an additional stop with extra pay Unknown/None/Amount; POD saved with open stops still listed.
- **Equipment:** trailer swap (recent trailers, effective from now by default); Quick Trailer Check (all rows pre-set OK, skip with reason); reefer sheet (exactly the five approved fields).
- **Loads:** list; search results (loads + individual documents); load detail; miles (carrier / estimate / yours + history).
- **Wallet & Upcoming:** wallet grid; document detail; Upcoming list (wallet expiries flow in automatically); Add Upcoming (one-time or repeating in one form).
- **PTI:** Quick (8 groups, tap exceptions only); Detailed (sectioned, resumable).
- **Money:** O/O summary (estimate labels); settlement reconciliation (line-by-line, cross-week, neutral wording, missing detention linked to dwell record); add expense (amount + category required only); Company Driver pay estimate.
- **More:** menu; export (all / range / one / selected; size shown up front); Trash (30-day countdown + restore); Settings (detention default, pay/miles, five notification classes).
- **Offline & alerts:** manual-vs-manual sync conflict screen; lock-screen notifications (detention, expiry, recurring bill).
- **Admin (web):** user detail with support case (Support role: masked contact info, activity metadata with time + city, document count behind a lock with no viewer, role-limited actions greyed out); Dashboard; Controls (entitlement toggles by tier, maintenance message, minimum version).

## 4. Major decisions and why

1. **Fixed-position primary action**: builds muscle memory; one obvious next step.
2. **Reference number as hero** (34px, Atkinson Hyperlegible, tap to copy); switches from PU # to BOL/delivery ref after pickup (TM-F004/F005).
3. **Gate info on the card**, not a Gate Pass screen: 0 taps at the gate (TM-D049).
4. **Colour semantics**: solid amber = Critical Load Instructions only; red/amber chips = countdowns; green = action/done; every colour also carries a word.
5. **Single capture entry**: classify and attribute after capture, with the match reason shown (TM-D018, TM-F043).
6. **Undo instead of confirm**: no dialog on Check In; automated stage changes appear as a toast with Undo (TM-D032).
7. **Driving view**: large read-only facts plus one navigation button while moving, instead of a greyed-out app (TM-D021).
8. **Money and full Upcoming under More**: desk tasks. Upcoming's top 3 and a due banner stay on Today (SRS §36).
9. **Provenance as small tags** (GPS · Edited · on phone · read from page) rather than warnings (TM-D019/D030).
10. **Offline as a state**: header pill plus "on phone" tags; only real manual conflicts interrupt the driver (TM-D024/D025/D047).

## 5. UX problems found — flagged for owner review, requirements NOT changed

| # | Problem | Assumed in design | Impact |
|---|---|---|---|
| 1 | "Upcoming" names both due items (SRS §36) and future loads (TM-D018) | Due items = "Upcoming"; loads always "Pre-planned" | High |
| 2 | Tier naming Standard/Pro (SRS §2/§12) vs Company Driver/O/O (TM-D054); expenses marked "(Pro)" in F029/F054, but SRS §29 not tier-limited | UI uses Company Driver/O/O; expenses are an O/O toggle | Medium |
| 3 | Document auto-advance needs AI/connectivity; offline BOL scan can't advance stage | Saved offline with "stage updates when online" + manual Mark In Transit | Medium |
| 4 | SRS §9 lists arrival/check-in/departure but only a Check In action exists; free time may count from appointment, not arrival | Check In = arrival; timer from check-in; per-load free-time override | High |
| 5 | TM-D021 doesn't specify motion detection, passenger/team override, or what's allowed | Driving view on sustained speed, no override | High |
| 6 | "Normal workflow action may signal check-out" (SRS §8) risks short dwell records | Never implied silently; Check Out highlighted after POD | Medium |
| 7 | Facility prompt "after stops" coincides with pulling away | Only in the check-out flow while parked | Low |
| 8 | Upcoming items have no done/paid behaviour | Swipe → Done advances recurring items (needs confirmation it isn't new scope) | Medium |
| 9 | When does a delivered load stop being Current / does Pre-planned auto-promote? | Offered via Start, never automatic | Medium |
| 10 | SRS §17 lists only expiration/defect notifications; D046/D050/D034 add more | Settings lists 5 classes | Low |
| 11 | Tier-adaptive UI during a full-feature trial | Ask "How are you paid?" at setup; trial unlocks everything | Medium |
| 12 | F016/F019/F022/F027 still "V1 Candidate" in a frozen baseline | Designed in and tagged | Medium |
| 13 | Planned phone-number change (email still valid) not covered by SRS §37 | Not designed | Medium |
| 14 | Settlement intake path and deadhead start point unspecified | Settlements via Scan/Import; deadhead from previous last stop, else location at load start | Medium |
| 15 | `01_MASTER_SRS.md` §5 and §18 contain literal `\n` text | Not edited (frozen); suggest formatting-only fix | Low |

## 6. Coverage (self-check, simulated — not field data)

All check areas in `design/validation/REQUIREMENT_CHECK_TEMPLATE.md` have at least one designed screen; see §16 of the HTML for the table. Simulated tap counts: RW-01 0 taps to see PU# and gate info, 1 to check in; RW-03 2; RW-05 3; RW-06 ~6, no typing; RW-07 3; RW-09 0 (visible on Today); RW-10 3; RW-11 3 plus typing. These are walkthroughs of mockups, **not** real-world evidence.

## 7. Next step

Stage 2 requirement check of all three independent designs, then peer learning (Stage 3). The owner should rule on issues #1, #4 and #5 before Stage 3, because they shape the Today screen in every design.

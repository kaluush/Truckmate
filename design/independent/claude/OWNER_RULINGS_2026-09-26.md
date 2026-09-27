# Owner Rulings on Claude Stage 1 Design Issues — 2026-09-26

`DESIGN_V1.md` and `CabPilot_V1_Claude_Screens.html` are preserved as the original independent design and are **not** edited. These rulings apply from the next design round onward. Authoritative text is in `02_MASTER_DECISIONS.md`.

| # | Issue | Ruling | Decision |
|---|---|---|---|
| 1 | "Upcoming" names both due items and future loads | Due items = **Reminders**; future loads = **Next Loads**; "Upcoming" not a UI label. Each reminder shows its own due timing. Naming only. | TM-D057 |
| 2 | Standard/Pro vs Company Driver/O/O; expenses marked Pro | Company Driver / O/O everywhere; expenses available to **both** tiers; other O/O financial features stay O/O. | TM-D060 |
| 3 | Offline scan can't auto-advance stage | Approved as assumed: saves immediately, stage updates when online, manual Mark In Transit. | TM-D063 |
| 4 | Timer start / free time from appointment | Timer starts at arrival always; labelled **Onsite** (never Detention); "Appt [time]" shown beside it; neutral reminder wording; no automatic missing-detention flag. **Design change:** rename the Detention card/reminder and drop detention-owed wording. | TM-D058 |
| 5 | In-motion detection/override undefined | **No in-motion restriction or driving mode** in V1. Everything works at all times. Notifications never require input. **Design change:** remove the Driving view. Driving view + fleet lock → V2. | TM-D059 |
| 6 | Implied check-out | Approved as assumed: never silent; Check Out highlighted after POD, driver taps it. | TM-D064 |
| 7 | Facility prompt timing | Approved as assumed: only inside the check-out flow while parked. | TM-D065 |
| 8 | Reminder done/paid behaviour | Approved as assumed: mark done; recurring items advance. | TM-D066 |
| 9 | When a delivered load stops being Current | Approved as assumed: stays Current until driver taps Start on the next load; no auto-promotion. | TM-D067 |
| 10 | Notification classes | Approved as assumed: five switchable classes, all under the no-input rule. | TM-D068 |
| 11 | Tier-adaptive UI during trial | Approved as assumed: setup asks "How are you paid?"; trial unlocks everything. | TM-D069 |
| 12 | F016/F019/F022/F027 still V1 Candidate | Promoted to **V1 Core**. | TM-D061 |
| 13 | Phone-number change missing | Change in Settings: OTP to new number + confirmation via old number or verified email; otherwise support/recovery. **Design change:** add this flow. | TM-D062 |
| 14 | Settlement intake / deadhead start | Approved as assumed: Scan/Import; deadhead from previous last stop, else device location at load start. | TM-D070 |
| 15 | Literal `\n` in SRS §5/§18 | Approved: formatting fixed. | TM-D071 |

Also decided the same day (not design issues): pricing TM-D072, trial TM-D073 (supersede TM-D035), launch free-access policy TM-D074. Follow-up the same day: downgrade O/O → Company Driver at next billing date with O/O data kept hidden (TM-D075); second trial period is 2 weeks, 28 days total (TM-D076). The setup/paywall screens in the next round should reflect the 14 + 14-day trial and the two-tier price table.

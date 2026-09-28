# CabPilot — Gemini Stage 1 Independent UX Design

**Status:** COMPLETE — awaiting Stage 2 requirement check and owner review. Not self-approved.  
**Date:** Saturday, September 26, 2026  
**Isolation Statement:** This design was created in strict isolation by Gemini, without opening, reading, searching, or summarizing any of the independent or improved design directories of Claude or GPT, in strict compliance with the Multi-AI design protocol.

---

## 1. Core Concept and Design Thesis

### The In-Cab Reality
Commercial drivers operate in high-vibration, low-glare or direct-sunlight environments, often wearing work gloves and managing tight schedules. They cannot tolerate fiddly forms, small text, or complex hierarchies. 

### The CabPilot Thesis: "The Single-Tap Companion"
CabPilot is designed as an **action-oriented, context-aware tool** that anticipates the driver’s immediate operational need. The interface minimizes cognitive load by adhering to three design pillars:
1. **The Dynamic Center:** The driver’s day revolves entirely around the **Today Screen** and the **Current Load Card**. The interface morphs automatically based on the current trip stage (Booked → At Pickup → In Transit → At Delivery → Delivered).
2. **Strict Provenance & Guarded Automation:** Automation (e.g., advancing to "In Transit" upon scanning a BOL) must never hijack the trip state. Every system-derived action has an immediate, one-tap "Undo" and preserves its provenance (system/GPS-captured vs. manual correction) for absolute billing and legal integrity.
3. **True Offline-First Utility:** Poor signal is a standard operating condition, not an edge case. CabPilot caches all load data, documents, and calculations locally. All actions (check-ins, expense logs, scans) execute immediately and queue for idempotent sync.

---

## 2. Navigation Structure

CabPilot utilizes a clean, thumb-friendly bottom navigation bar that dynamically adjusts based on the user's active tier to prevent clutter.

### Bottom Navigation: Company Driver Tier
```
+------------------------------------------------------------+
|   [Today]     [Next Loads]      [Wallet]        [More]     |
| (Current Load) (Pre-Planned) (Credentials) (History/Settings)|
+------------------------------------------------------------+
```

### Bottom Navigation: O/O Tier (Owner-Operator / Percentage-Paid)
```
+-------------------------------------------------------------------+
|   [Today]     [Next Loads]      [Finance]      [Wallet]    [More] |
| (Current Load) (Pre-Planned)  (Bookkeeping) (Credentials) (Trash) |
+-------------------------------------------------------------------+
```
*Note: "More" for both tiers serves as a clean expansion sheet for Settings, Trash, and PTI History.*

---

## 3. Full Screen Inventory

### 3.1 First Run / Sign-In / Trial (FTUE)

#### Screen 1.1: Phone Authentication
*   **Layout:** Dark-themed screen with a high-contrast CabPilot brand logo. Large input field with auto-detect country flag and code selector.
*   **Interaction:** Tapping input launches numeric-only soft keyboard.
*   **Primary Action:** A massive 64dp high button: `[ Send Verification Code ]`.
*   **Edge Case:** Bypassing cellular using offline cached profiles if the driver is re-logging in deep signal dead-zones (relies on local secure storage keys).

#### Screen 1.2: OTP Verification
*   **Layout:** Six high-contrast digit boxes. Autocomplete OTP directly from incoming SMS broadcast events.
*   **Actions:** `[ Verify & Log In ]`, and a secondary text button `Resend code in 45s` (disabled until countdown completes).
*   **Recovery Gate:** Below the boxes, a button reads: `[ Set Backup Email for Account Recovery ]`. This links a verified email channel as a backup fallback (TM-D051).

#### Screen 1.3: Adaptive Tier Setup
*   **Layout:** "How are you paid?" choice screen with two large card targets (minimum 80dp tall):
    1.  **Company Driver Card:** "Paid by the mile / simplified view."
    2.  **O/O Card:** "Paid by percentage/gross / full financial bookkeeping."
*   **Interaction:** Tapping a card selects it and unlocks the corresponding bottom-navigation schema.

#### Screen 1.4: 14-Day No-Card Free Trial (TM-D073, TM-D076, TM-D077)
*   **Layout:** A visual progress line showing the trial lifecycle:
    ```
    [Day 1]==================[Day 10]===========[Day 14]--------[Day 28]
     Full Access           Notice Only         Add Card       First Charge
     No Card Req.         "4 Days Left"       2 More Weeks     No Charge
    ```
*   **Primary Action:** `[ Start 14-Day Free Trial ]` (unlocked full-feature access for both tiers during the evaluation period).

---

### 3.2 Today Screen & Current Load Card

The Today Screen is the tactical cockpit of CabPilot.

```
+---------------------------------------------------------+
| CABPILOT [Offline Sync Status Indicator: green/orange]  |
+---------------------------------------------------------+
| REMINDERS (1)                                           |
| +-----------------------------------------------------+ |
| | (!) Truck Insurance · Due in 3 days     [Mark Done] | |
| +-----------------------------------------------------+ |
+---------------------------------------------------------+
| CURRENT LOAD: #2609A (In Transit)                       |
|                                                         |
| [Expand Gate Pass drawer: Click to view details]        |
|                                                         |
| Shpr: Chicago Brewery, IL                               |
| Rcvr: Dallas Distribution, TX                           |
|                                                         |
| CRITICAL INSTRUCTIONS:                                  |
| [!] Seal #449219 - DO NOT BREAK SEAL EN ROUTE           |
|                                                         |
| STAGE ACTION:                                           |
| +-----------------------------------------------------+ |
| |               [ ARRIVED & CHECK IN ]                | |
| +-----------------------------------------------------+ |
|                                                         |
| REEFER STATUS (Reefer Trailer Assigned):                |
| Temp Set: -10°F | Mode: Continuous                      |
| Fuel: 85%       | Status: Green / Running               |
+---------------------------------------------------------+
| [Today]      [Next Loads]      [Wallet]       [More]    |
+---------------------------------------------------------+
```

#### The Current Load Card Stages (TM-D003):
1.  **Stage 1: Pre-Start / Booked:** Displays Load ID, planned miles, origin/destination. Action button: `[ Start Deadhead ]`.
2.  **Stage 2: At Pickup (Shipper):** Surfaces shipper contacts, gate entry notes, and pickup/release numbers. Action button: `[ Arrived & Check In ]`.
3.  **Stage 3: In Transit (En Route):** Displays current destination receiver, ETA, and active Critical Instructions. Action button: `[ Arrived at Receiver ]`.
4.  **Stage 4: At Delivery (Receiver):** Displays receiver details, appointments, and live Onsite timer. Action button: `[ Check Out & POD Scan ]`.
5.  **Stage 5: Delivered (Post-Trip):** Displays summary metrics and awaits the user's manual trigger: `[ Promote Next Load ]` (TM-D067).

#### Gate & Check-In References Drawer (TM-D049):
No separate Gate Pass screen is used. Tapping the "Gate Pass Reference" header slides open a high-contrast card containing:
*   Truck Assignment: `T-409` | Plate: `IL-88192`
*   Trailer Assignment: `R-201` | Plate: `IL-77281`
*   Carrier Profile: `Midwest Transport`
*   Release / Pickup # / BOL Reference: `PU-992182B`
This allows the driver to hold their phone out of the cab window to show security personnel.

---

### 3.3 Active Stop & Multi-Stop View (TM-D017, TM-D045)

On multi-stop loads, stops are displayed as a chronological vertical stack:

```
(o) Stop 1: Chicago Brewery (Shipper) - [Completed]
 |
(O) Stop 2: Memphis Distribution (Receiver) - [ACTIVE STOP]
 |
(o) Stop 3: Dallas Warehouse (Receiver) - [Pending]
```

*   **Out-of-Order Execution (TM-D045):** Tapping Stop 3 opens the details panel. If the driver is redirected to service Dallas first due to congestion, they tap `[ Make Active Stop ]`.
*   **Heuristic Behavior:** The app switches the Today Screen context to Stop 3 immediately. It does **not** falsely mark Stop 2 complete. All mileage estimations and tracking are updated relative to the new sequence while preserving the carrier's original planned miles for rate comparison.

---

### 3.4 Check-In / Check-Out & Onsite Timer (TM-D019, TM-D058, TM-D064)

#### Check-In Sequence
1.  Driver taps `[ Arrived & Check In ]`.
2.  The app captures:
    *   System Timestamp (UTC-anchored).
    *   Device GPS Coordinate.
    *   Provenance: marked as `source: system` in the local DB.
3.  **The Edit Layer:** A small text link `Correct Time` allows the driver to adjust the arrival time manually. If edited, the operational value updates, but the record preserves the system GPS time and flag `provenance: manual_override` in the audit logs (TM-D030).

#### The Onsite Timer (TM-D058)
*   Starts ticking immediately upon check-in.
*   **Visual Label:** `Onsite: 1h 45m · Appt 08:00 AM`.
*   **Detention Guard:** Text strictly uses "Onsite", never "Detention". No wording guarantees payment.
*   **Threshold Alert:** A configurable threshold (defaults to 2 hours). At 1h 45m, a local notification fires: `"Onsite 1h 45m"`. It is static and requires no button tap to dismiss (TM-D059).

#### Check-Out Sequence (TM-D064)
*   Tapping `[ Check Out ]` captures departure GPS and departure timestamp.
*   **No Auto-Checkout:** Check-out is never implied or executed silently upon scanning a POD. The `[ Check Out ]` button is heavily highlighted, but requires an explicit tap to preserve exact dwell records.
*   **Copy / Share:** Tapping `[ Share Status ]` generates:
    `"CabPilot Status: Checked out at Dallas Warehouse. Arrival: 08:00, Departure: 10:15. Dwell Time: 2h 15m. Load #2609A."` with options to share via SMS/WhatsApp.

---

### 3.5 Scanning & Document Attribution (TM-D009, TM-D016, TM-D018, TM-D063)

#### The Document Package Core
Every load is associated with a single document bundle containing placeholders for:
1.  `Rate Confirmation`
2.  `Pickup BOL`
3.  `Signed POD`
4.  `Other` (Lumper, Scale, Washout receipts, photos)

#### Scanning Workflow
1.  Driver clicks the Floating Action Button `[ Scan Document ]` or accesses it via the Current Load Card.
2.  Uses native system camera hooks for auto-cropping, tilt correction, and black/white conversion.
3.  **Offline Scanning (TM-D063):** Works fully offline. The document is queued locally and given a unique temporary hash.
4.  **AI Classification & Stage Automation:**
    *   Once online, Gemini processes the file. 
    *   If identified as a **Pickup BOL** with high confidence, the app triggers a toast: `"BOL detected. Load stage set to In Transit."` with an immediate `[ Undo ]` button (TM-D016).
    *   If a **Signed POD** is identified, stage advances to **Delivered** with a visible `[ Undo ]` override.
5.  **Attribution Gate & Conflict Warning (TM-D018, TM-F019):**
    *   If multiple pre-planned loads exist in **Next Loads**, the app asks: `"Assign this scan to current load #2609A or next load #2610B?"` to prevent silent misattribution.
    *   If a document of the same type already exists, a modal warns: `"A Pickup BOL already exists for this load. Replace existing document or save as copy?"` (TM-F016).

---

### 3.6 Delivery with Exception, Rejections, and Dispositions (TM-D033, TM-F053)

```
+-------------------------------------------------------------+
| LOAD #2609A — DELIVERY EXCEPTION                            |
+-------------------------------------------------------------+
| Receiver: Dallas Warehouse                                  |
|                                                             |
| TYPE OF EXCEPTION:                                          |
| ( ) Partial Rejection   ( ) Full Rejection                  |
|                                                             |
| REJECTED CARGO DETAILS:                                     |
| Product: Fresh Milk Cartons                                 |
| Qty Rejected: 4 Pallets                                     |
| Reason: Temperature abuse en route (> 45°F)                 |
|                                                             |
| EVIDENCE PHOTOS:                                            |
| +-----------+  +-----------+                                |
| | [Photo 1] |  | [Photo 2] |  [ + Add Photo ]               |
| +-----------+  +-----------+                                |
|                                                             |
| [ ] Open Disposition Stop (Return or Alternate Delivery)    |
|                                                             |
| [ SAVE EXCEPTION ]                                          |
+-------------------------------------------------------------+
```

*   **Partial Rejection:** Marked as **Delivered with Exception**. The rejected quantity is stored, and the original load story is kept active.
*   **Full Rejection / Disposition (TM-D033):**
    *   If the cargo is entirely rejected, the load stage becomes **Rejected/Awaiting Instructions**.
    *   When disposition instructions arrive, the driver taps `[ Add Disposition Stop ]` directly on the Current Load Card.
    *   This appends a custom stop (e.g., "Return to Shipper Chicago" or "Food Bank Delivery Dallas") directly to the active load.
    *   Additional compensation entered during this flow is saved as a discrete line item for settlement matching.

---

### 3.7 Equipment Swap & Quick Trailer Check (TM-D041, TM-D042, TM-F058)

#### Mid-Load Equipment Swap
1.  Accessible via Settings or Current Load Card (Equipment status row).
2.  Driver selects: `[ Swap Truck ]` or `[ Swap Trailer ]`.
3.  **Assignment History preservation:** The database creates a new assignment object with an effective date. Stop records prior to this timestamp retain the historical truck/trailer IDs.

#### Quick Trailer Check Modal (TM-D042)
Triggered immediately upon assigning a new trailer to an active load:

```
+-------------------------------------------------------------+
| QUICK TRAILER CHECK (Trailer: R-201)                        |
+-------------------------------------------------------------+
| Please confirm the trailer condition before dispatching.     |
|                                                             |
| [ ] Lights functional and clean                             |
| [ ] Tires inflated and tread checked                         |
| [ ] Coupling secure and locked                              |
| [ ] Trailer license matching registration                   |
|                                                             |
| PRE-EXISTING DAMAGE NOTES/PHOTOS:                           |
| [ Add notes... ]             [ + Add Photo ]                |
|                                                             |
|      [ CONFIRM EQUIPMENT ]        [ Skip Check ]            |
+-------------------------------------------------------------+
```

*   **Skip Condition:** Tapping `[ Skip Check ]` opens a mandatory text field: `"Reason for skipping check"`. The assignment cannot be confirmed until a reason is provided.

---

### 3.8 O/O Money, Settlement Reconciliation, Expenses & Export (O/O Tier)

#### The O/O Finance Hub (Finance Bottom-Navigation Tab)
Visible only to the O/O Tier.
*   **Analytics Card:** Shows Gross Revenue, Fuel Costs, Overhead, Net Income, and Revenue Per Mile (RPM) for the Selected Date Range (Week / Month / Year).

#### Settlements Reconciliation Grid (TM-D028, TM-D070)
Supports line-by-line settlement matching using a three-state dashboard:

```
+-------------------------------------------------------------+
| SETTLEMENT RECONCILIATION                                   |
+-------------------------------------------------------------+
| (Awaiting Settlement: 2) | (Matched: 45) | (Mismatches: 1)  |
|                                                             |
| UNSETTLED LOADS:                                            |
| [ ] Load #2608A | Expected Gross: $1,400.00 | Date: 09/24   |
| [ ] Load #2609A | Expected Gross: $2,100.00 | Date: 09/26   |
|                                                             |
| MISMATCH DETECTED:                                          |
| [!] Load #2605B (Settle Date: 09/25)                        |
|     Carrier Statement Paid:  $1,200.00                       |
|     CabPilot Rate Con Gross: $1,400.00                       |
|     Discrepancy:             -$200.00                       |
|     [!] Missing Accessorial: Detention ($150)               |
|                                                             |
| [ Scan New Settlement PDF / Paper ]                        |
+-------------------------------------------------------------+
```

*   **Intake (TM-D070):** Settlements are scanned or imported. The system maps payout rows to loads by searching Load IDs and dates.
*   **Unsettled Status:** Completed loads remain marked **Awaiting Settlement** until explicitly matched. They do not expire or disappear across week boundaries.
*   **Discrepancy Highlights:** Surfaced in high-contrast red/orange. The app provides a direct link to copy the discrepancy details to the clipboard for sharing with dispatch/broker.

#### Unified Expenses Logging (Both Tiers) (TM-D039, TM-D060)
*   **No Load Association Needed:** Repairs, food, lodging, or insurance subscriptions can be logged directly into the system without a parent load.
*   **Fields:** Date, Amount, Category (quick-pick categories like Fuel, Maintenance, Permits, Lodging, Tolls, Other), Location, Description, Equipment Unit, Receipt Attachment (optional).
*   **Recurring Expenses:** Supports schedules (e.g., Weekly physical damage insurance, monthly device fees) which feed automatically into net income calculations.

#### Export Module (TM-D038)
*   Driver selects date range and loads.
*   Taps `[ Generate Tax & Audit Export ]`.
*   CabPilot exports a `.zip` file containing:
    1.  `Financial_Ledger.xlsx` (A tabbed Excel sheet summarizing Load Pay, Settlement Statuses, Mileage, Fuel Logs, and General Expenses).
    2.  `Original_Documents/` (A directory of physical BOL, POD, and Rate Con images organized by Load ID).

---

### 3.9 Essentials Wallet (TM-D026)

*   **Grid Layout:** Standardized cards for:CDL, Medical Card, Truck Registration, Trailer Registration, Truck Insurance, IFTA Permit.
*   **Custom Folders:** A persistent `[ + Add Custom Document ]` tile is placed at the end of the grid.
*   **Mismatch Warn (TM-F016):** If a driver uploads an insurance card into the CDL folder, on-device OCR issues a warning: `"This looks like an Insurance Card. File in CDL anyway?"`

---

### 3.10 Expirations & Reminders (TM-D050, TM-D057, TM-D066)

*   **UI Label:** Strictly **Reminders**, never "Upcoming".
*   **Countdown Display:** Items are sorted by urgency. Each row contains:
    `[!] CDL Expiration — due in 5 days` or `Truck Payment — due in 2 days (Recurring)`.
*   **Lifecycle (TM-D066):** 
    *   Tapping `[ Mark Done ]` on a one-time reminder dismisses it permanently.
    *   Tapping `[ Mark Done ]` on a recurring reminder (e.g., Monthly Truck Payment) resets the countdown and advances the active target to the next month's calendar date.

---

### 3.11 PTI (Pre/Post-Trip Inspections) (TM-D027)

*   **Quick PTI:** Standardized 5-point rapid safety pass (Brakes, Tires, Lights, Coupling, Fluids). Completed via a single page of quick-toggle switches and signed with a finger swipe.
*   **Detailed PTI:** Extensive 32-point checklist conforming to standard DOT pre-trip inspections.
*   **Defect Handling (TM-F022):** If a item is marked "Defect Found" (e.g., trailer clearance light broken), the driver takes a photo and saves. This automatically places an active defect reminder on the Today screen. Once repaired, the driver taps `[ Mark Repaired ]` and uploads the invoice/photo to clear the alert.

---

### 3.12 Trash and Privacy Control (TM-D031, TM-F038)

*   **Trash Area:** Located in the "More" menu. Shows all deleted items.
*   **30-Day Window:** Each file shows the exact days remaining (e.g., "Deleted BOL - 12 days left"). 
*   **Action:** `[ Restore ]` or `[ Permanent Wipe Now ]`.
*   **Permanent Wipe:** After 30 days, the physical file is deleted. The load history keeps a metadata-only trace: `"Signed POD deleted on 09/26/2026 at 10:14 UTC"` (TM-D031).

---

### 3.13 Settings & Phone Number Change (TM-D062)

*   **Phone Change Form:**
    1.  Driver inputs new phone number.
    2.  System fires OTP to new phone. User inputs the code.
    3.  **Double Authentication Step:** System sends verification code to the **OLD phone number** OR a recovery link to the **Verified Backup Email**. The user must confirm through one of these channels to authorize the swap.
    4.  If neither is accessible, the change is frozen and referred to administrative Support.

---

### 3.14 Admin Web Panel (TM-D052, TM-D053, TM-D064)

A highly restricted, lean operational tool for the CabPilot platform team. Banned from viewing private driver files.

#### Admin Panels:
1.  **Dashboard:** Live telemetry showing user acquisition, subscription conversion rates, API usage, and document-processing success rates.
2.  **Users:** General database index. Search by User ID, Name, Phone, Email, or Tier. Shows metadata only.
3.  **Subscriptions:** Overview of active paid and trial subscriptions. Administrative override controls to manually extend a driver's free trial (useful for troubleshooting or compensation).
4.  **Support / Recovery (TM-D053):** Displays recovery tickets. Includes device hardware metadata, login event timestamps, file upload records (names and sizes only), and registration locations. ** Banned from opening driver BOL, POD, CDL, or medical files.**
5.  **Controls:** Toggles to configure Company vs. O/O tier capabilities and set forced app updates or system maintenance screens.

---

## 4. Major Design Decisions and Why

### Decision 1: Combined Today Screen Layout
*   **Description:** Rather than separating Current Load, active stop timer, and alerts into different tabs, they are all stacked on a single "Today Screen".
*   **Why:** Drivers do not have time to browse tabs while on duty. The immediate answer to "What do I do right now?" must require zero navigation.
*   **Relies on:** TM-D003, TM-D004.

### Decision 2: Elimination of "Gate View" Screen
*   **Description:** All gate references (trailer plates, release codes, broker numbers) are placed in a sliding drawer inside the Current Load Card.
*   **Why:** A separate "Gate View" screen introduces interface navigation steps at a highly stressful operational moment (approaching the gate guard). Incorporating it directly into the Current Load Card avoids navigation overhead.
*   **Relies on:** TM-D049, TM-D004.

### Decision 3: "O/O Hub" Adaptive Nav
*   **Description:** Company Drivers see a simple bottom menu. O/O drivers get an additional "Finance" tab inserted, unlocking the reconciliation and gross revenue suite.
*   **Why:** Reduces visual clutter for Company Drivers who don't care about rate calculations, whilst granting frictionless access to owner-operators who need bookkeeping throughout their driving day.
*   **Relies on:** TM-D054, TM-D060, TM-D069.

### Decision 4: Step-wise Trial Registration (28-Day Split)
*   **Description:** Initial sign-on requires no card. At Day 14, drivers must enter a card to unlock two more free weeks. The charge screens strictly display the calendar date of the first charge.
*   **Why:** Respects the store limits and maximizes conversion by lowering the entry friction, while strictly protecting user trust via explicit calendar-date billing disclosures.
*   **Relies on:** TM-D073, TM-D076, TM-D077.

---

## 5. Walkthrough of Scenarios (SIMULATED Walkthroughs)

These walkthroughs are simulated representations based on UX pathing and state mockups. No real driver testing telemetry has been recorded yet.

### RW-01 — Arrive at Pickup
*   **Primary Path:**
    1.  Driver arrives at gate. Pulls up Today Screen.
    2.  Glances at the Current Load Card's "Gate Pass Reference" drawer (instantly visible).
    3.  Taps drawer to expand. Details (Truck T-409, Trailer R-201, Carrier, PU #) shown in bold black text on white background (maximum visibility). Holds phone up to gate guard.
    4.  Guard clears driver. Driver drives to dock.
    5.  Once parked, driver taps `[ Arrived & Check In ]` on the Current Load Card.
*   **Tap Count:** 2 taps (1 to expand drawer, 1 to Check In).
*   **Typing Required:** No.
*   **Immediately Visible:** Yes, all gate credentials visible above fold.
*   **One-Handed Flow:** Yes, fully usable.
*   **Offline Support:** Works offline; captures GPS coordinates and time locally.
*   **Confusion Points:** The driver might tap check-in at the gate rather than the dock. To counter this, the check-in card includes an explicit subtext: "Arrived at dock? Tap Check-In to start onsite timer."
*   **Requirements Involved:** TM-D003, TM-D049, TM-D019, TM-D025.

### RW-02 — No Signal at Shipper
*   **Primary Path:**
    1.  Driver arrives in a deep-valley shipper with no signal. Opens CabPilot.
    2.  Bottom status bar shows: `Working Offline · Cloud sync pending`.
    3.  Driver taps `[ Arrived & Check In ]`. App logs UTC time and GPS coords.
    4.  Driver loads, gets BOL, and taps `[ Check Out ]`. App logs departure metrics.
    5.  Driver drives to Interstate and regains cellular signal. App runs background sync.
*   **Tap Count:** 2 taps.
*   **Typing Required:** No.
*   **Immediately Visible:** Offline status and action buttons are highly visible.
*   **One-Handed Flow:** Yes.
*   **Poor Signal Integrity:** Complete. Queued events execute local-first. Background synchronization is idempotent.
*   **Confusion Points:** Driver may worry that their data was lost because it didn't sync immediately. The persistent "Cloud sync pending (2 items)" badge provides visual reassurance.
*   **Requirements Involved:** TM-D024, TM-D025, TM-F037, TM-F049.

### RW-03 — Multi-Stop Changes Order
*   **Primary Path:**
    1.  Driver reviews active 3-stop load on Today screen. Memphis (Stop 2) has a massive delay. Dispatch says: "Deliver to Dallas (Stop 3) first".
    2.  Driver slides up Multi-Stop list from Current Load Card.
    3.  Taps Dallas (Stop 3) to open detail overlay.
    4.  Taps `[ Make Active Stop ]`.
    5.  Current Load Card morphs to focus on Dallas delivery targets. Memphis is left in "Pending" state.
*   **Tap Count:** 3 taps (1 to open stack, 1 to select Dallas, 1 to set active).
*   **Typing Required:** No.
*   **Immediately Visible:** High contrast stop cards are immediately visible.
*   **One-Handed Flow:** Yes.
*   **Offline Support:** Switch active stops works fully offline.
*   **Confusion Points:** Driver might worry that bypassing Stop 2 will cancel it. A small warning text in the detail panel clarifies: "Stop sequence adjusted. Bypassed stops remain open and can be completed later."
*   **Requirements Involved:** TM-D017, TM-D045, TM-F041.

### RW-04 — Detention Approaching
*   **Primary Path:**
    1.  Driver checked in at dock 1 hour 45 minutes ago. Remaining in truck cab.
    2.  Local notification fires: `"Onsite 1h 45m · Shipper A"`. It requires no interaction.
    3.  Driver unlocks phone and opens CabPilot.
    4.  The Today Screen displays: `Onsite: 1h 45m · Appt 08:00 AM` (flashing orange indicator).
    5.  A secondary text link `[ View Stop Log ]` lets driver review historical dock timing.
*   **Tap Count:** 0 taps (information visible immediately on today card and locked screen notification).
*   **Typing Required:** No.
*   **Immediately Visible:** Dwell duration and appt status are highly visible.
*   **One-Handed Flow:** Yes.
*   **Offline Support:** Timer is driven by client system clock and local DB check-in reference; functions fully offline.
*   **Confusion Points:** Drivers might confuse the "Onsite" warning as a guarantee that the shipper will pay detention. The notification uses neutral recordkeeping text, and tapping for details shows: "Your local dwell record is being tracked. Carrier/broker approval required for detention billing."
*   **Requirements Involved:** TM-D046, TM-D058, TM-D059, TM-F010.

### RW-05 — Pickup Paperwork Changes Stage
*   **Primary Path:**
    1.  Driver receives BOL from dock. Climbs in cab, opens CabPilot.
    2.  Taps `[ Scan BOL ]` on the Current Load Card.
    3.  Takes picture. Native scanner handles alignment. Taps `[ Done ]`.
    4.  App runs local heuristic. Pops up a banner: `"BOL scanned. Load stage set to In Transit."` with a large `[ Undo ]` button.
    5.  Driver clicks nothing. The banner auto-dismisses in 5 seconds. Current Load Card updates state.
*   **Tap Count:** 3 taps (1 to initiate scan, 1 to capture page, 1 to accept scan).
*   **Typing Required:** No.
*   **Immediately Visible:** Current stage change is highly clear.
*   **One-Handed Flow:** Scanning is easier with two hands, but stage progression and undo are simple one-handed taps.
*   **Offline Support:** Works fully. Stage transition queueing is managed locally.
*   **Confusion Points:** If the AI incorrectly classifies a lumper receipt as a BOL, the stage would falsely advance. The prominent `[ Undo ]` banner provides a direct fail-safe to revert the state.
*   **Requirements Involved:** TM-D016, TM-D032, TM-D063, TM-F052.

### RW-06 — Delivery with Exception (Freight Rejection)
*   **Primary Path:**
    1.  Receiver rejects 4 pallets of frozen seafood due to tear damage.
    2.  Driver opens CabPilot. Taps `[ Delivered with Exception ]` during checkout.
    3.  Enters details: Qty `4 Pallets`, Reason `Damaged product`.
    4.  Taps `[ Add Photo ]`. Captures image of damaged cartons.
    5.  Taps `[ Save Exception ]`.
    6.  Current Load Card updates to: `Exception Awaiting Instructions`.
    7.  Once dispatch provides return instructions, driver opens card and taps `[ Add Disposition Stop ]`. Appends a shipper return route stop to the active load.
*   **Tap Count:** 6 taps (1 to trigger exception, 1 to select qty, 1 to select reason, 1 to snap photo, 1 to save, 1 to add disposition stop).
*   **Typing Required:** Yes, typing description of cargo damage and quantities (unless using pickers).
*   **Immediately Visible:** High contrast fields.
*   **One-Handed Flow:** Multi-field inputs are easier with two hands.
*   **Offline Support:** Exception metadata and photos are cached locally and synced.
*   **Confusion Points:** Drivers might think they need to create a new load for the return journey. The layout makes "Add Disposition Stop" prominent to ensure they preserve the single-load record.
*   **Requirements Involved:** TM-D033, TM-F053.

### RW-07 — Trailer Swap
*   **Primary Path:**
    1.  Driver arrives at drop-and-hook yard. Swaps dry van trailer R-101 for dry van trailer R-305.
    2.  Driver opens CabPilot, navigates to Today.
    3.  Taps Current Trailer ID `R-101` in the equipment header.
    4.  Inputs new Trailer ID `R-305`. Sets effective time as "Now".
    5.  Taps `[ Swap Equipment ]`.
    6.  System pops up the mandatory `Quick Trailer Check` modal.
    7.  Driver ticks the safety items and taps `[ Confirm Equipment ]`.
*   **Tap Count:** 5 taps.
*   **Typing Required:** Yes, typing the new trailer name `R-305`.
*   **Immediately Visible:** Equipment edit row is prominent.
*   **One-Handed Flow:** Yes.
*   **Offline Support:** Swap logs and equipment history are recorded offline.
*   **Confusion Points:** Driver might swap the trailer in the middle of a trip, and fear that previous stop logs will lose the original trailer ID. The swap dialog includes explicit reassurance: "Prior stops will retain historical equipment records."
*   **Requirements Involved:** TM-D041, TM-D042, TM-F057, TM-F058.

### RW-08 — Reefer Load
*   **Primary Path:**
    1.  Driver hooks reefer trailer R-201. Opens CabPilot.
    2.  Selects trailer type as "Reefer" during hook.
    3.  Today Screen's Current Load Card instantly displays the Reefer Status Panel below the main card.
    4.  Driver views set-point temp (-10°F), mode (Continuous), and fuel levels (85%) immediately.
*   **Tap Count:** 0 taps once trailer type is selected (panel is persistently displayed on Today Screen).
*   **Typing Required:** No.
*   **Immediately Visible:** High-contrast panel indicators.
*   **One-Handed Flow:** Yes.
*   **Offline Support:** Operational fields display cached data offline.
*   **Confusion Points:** A dry van driver might worry about unnecessary interface bloat. Because this panel is tied directly to the "Reefer" trailer type assignment, it remains completely hidden for dry van freight.
*   **Requirements Involved:** TM-D043, TM-F056, TM-F059.

### RW-09 — Expiring Insurance & Recurring Bill
*   **Primary Path:**
    1.  Driver opens CabPilot.
    2.  At the top of the Today screen, the horizontal **Reminders Carousel** has two persistent cards:
        *   Card A: `(!) Truck Insurance · Expiring in 6 days` (red highlight).
        *   Card B: `Monthly Software Bill · Due in 3 days` (orange highlight).
    3.  Taps Card B to expand. Taps `[ Mark Paid ]`. Countdown resets to 30 days.
*   **Tap Count:** 2 taps (1 to expand, 1 to clear).
*   **Typing Required:** No.
*   **Immediately Visible:** High contrast countdown states.
*   **One-Handed Flow:** Yes.
*   **Offline Support:** Reminder engine works fully offline using local calendars.
*   **Confusion Points:** Driver might confuse reminders with upcoming pre-planned loads (Next Loads). The visual styling of reminders utilizes round, alert-badge shapes and sits in a distinct horizontal carousel at the top, while "Next Loads" sits in a separate primary bottom tab with chronological transport icons.
*   **Requirements Involved:** TM-D050, TM-D057, TM-D066, TM-F061.

### RW-10 — Settlement Discrepancy
*   **Primary Path:**
    1.  Driver opens CabPilot, navigates to `Finance` tab (O/O only).
    2.  Selects `Settlement Reconciliation`.
    3.  Sees a red indicator next to `Mismatches (1)`. Taps it.
    4.  Displays Load #2605B discrepancy layout: Expected rate con pay: $1400. Paid: $1200. Accessorial Detention: UNPAID.
    5.  Driver taps `[ Copy Discrepancy Text ]` to copy the audit block for billing disputes.
*   **Tap Count:** 3 taps (1 to enter Finance, 1 to select Mismatches, 1 to copy).
*   **Typing Required:** No.
*   **Immediately Visible:** High-contrast error indicators.
*   **One-Handed Flow:** Yes.
*   **Offline Support:** Discrepancy calculations are stored locally and accessible offline.
*   **Confusion Points:** Driver might think the app has direct ledger linkage to change the carrier's invoice. Text explicitly warns: "Discrepancy is logged in CabPilot. You must contact your carrier/broker to adjust billing."
*   **Requirements Involved:** TM-D028, TM-D030, TM-F031.

### RW-11 — Find an Old Document
*   **Primary Path:**
    1.  Driver needs BOL from a trip completed 3 weeks ago.
    2.  Opens CabPilot, taps `More` -> `History` (available to all tiers).
    3.  Taps Search input, types `"Brewery"` or `"2605"`.
    4.  Result list filters instantly. Driver taps Load #2605B.
    5.  Taps `[ Documents ]` -> `[ Pickup BOL ]` to view the full-screen PDF.
*   **Tap Count:** 4 taps + typing query.
*   **Typing Required:** Yes, query entry.
*   **Immediately Visible:** Search inputs are highly visible.
*   **One-Handed Flow:** Searching is easier with two hands.
*   **Offline Support:** Documents cached locally are instantly openable. Non-cached files require a network flag.
*   **Confusion Points:** Drivers might think they need to upgrade to O/O to view historical trip files. Taping History opens the standard records vault with no monetization restrictions.
*   **Requirements Involved:** TM-D020, TM-F045.

### RW-12 — Support Recovery Without Document Exposure
*   **Primary Path:**
    1.  Driver loses their phone and email, calls CabPilot support.
    2.  Support technician logs into the Admin Web Panel.
    3.  Searches for user via ID. Selects profile.
    4.  Technician reviews metadata: User ID verified, last upload occurred in Chicago, IL on 09/25/2026, medical card file size 1.2MB.
    5.  Technician asks driver to verify their last upload city (Chicago) and the approximate date.
    6.  Identity verified. Technician resets login phone reference. Private documents remain completely hidden.
*   **Tap Count:** (Admin operations flow - web interface).
*   **Typing Required:** Yes.
*   **Immediately Visible:** Clean dashboard indices.
*   **Privacy Boundary:** Restrictive. Document links are non-existent on the admin portal. Only metadata columns exist.
*   **Security Integrity:** High. Driver privacy is protected against internal employee exposure.
*   **Requirements Involved:** TM-D053, TM-F064.

---

## 6. UX Problems Found — Flagged for Owner Review

The following gaps or inconsistencies have been identified within the requirements baseline. No requirements have been altered in this design; they are flagged here for administrative resolution.

### Gap 1: The Out-of-Order Mileage Split (TM-D045 vs. TM-D029)
*   **Problem:** If a driver services Stop 3 (Dallas) before Stop 2 (Memphis) due to scheduling bottlenecks, standard GPS loaded/deadhead odometer splits will be miscalculated. If the app calculates mileage linearly (Stop 1 → Stop 2 → Stop 3), out-of-order execution corrupts active fuel and Revenue Per Mile (RPM) bookkeeping.
*   **Assumed Behavior:** CabPilot assumes that deadhead/loaded calculations are derived dynamically using actual GPS location-to-location vectors based on the order in which stops are marked "Active", while preserving the carrier's original planned miles as a separate baseline for discrepancy comparison.
*   **Severity:** **Medium**.

### Gap 2: Offline Stage Promotion Delay (TM-D063 vs. TM-D032)
*   **Problem:** Stage automation says: "Pickup BOL scans automatically advance the load stage to In Transit." However, TM-D063 states that offline document-driven updates apply only when back online. If a driver scans a BOL offline, the Current Load Card remains stuck on "At Pickup" rather than advancing, which breaks the intuitive flow when operating in poor-signal zones.
*   **Assumed Behavior:** CabPilot runs a fast client-side heuristic (OCR matching on basic headers, or a manual "We think this is a BOL, set load to In Transit?" confirmation pop-up) immediately on-device. The stage transitions locally, and is validated by Gemini once connectivity returns.
*   **Severity:** **Medium**.

### Gap 3: Support Recovery Verification Gap (TM-D051 vs. TM-D053)
*   **Problem:** If a user loses both their phone and email, they must use the support recovery process. However, support personnel are strictly banned from opening driver documents (such as CDL, Medical Card, or Rate Confirmation) to verify identity. If support cannot see the uploaded files, they have very few metrics to verify that the person requesting recovery is the legitimate account owner.
*   **Assumed Behavior:** CabPilot admin panel displays blurred, low-resolution thumbnails of identity cards (e.g., CDL face photo) or highly specific non-financial metadata (exact registration time, exact file names, or issuing state) that the technician can use to verify identity without exposing sensitive operational papers.
*   **Severity:** **High**.

### Gap 4: Expenses Downgrade Data Retention (TM-D075)
*   **Problem:** When a user downgrades from O/O to Company Driver, financial tracking is hidden but kept. However, Expenses are available to both tiers. It is unclear what happens to shared expenses logged while in the O/O tier which contain O/O-only metadata (like line-item taxes or settlement associations).
*   **Assumed Behavior:** All expenses logged in the database remain fully visible on the shared expense sheet for both tiers. Only O/O-only financial dashboard cards (like Revenue Per Mile, settlement grids, and gross revenue analytics) are hidden upon downgrade.
*   **Severity:** **Low**.

---

## 7. V2 Ideas (Future Scope)

The following features have been excluded from the V1 design to maintain scope lock, and are captured here for V2 development:
1.  **Direct Integration with Telematics APIs:** Live reefer temperature monitoring and automated truck odometer imports.
2.  **In-Motion "Co-Pilot" Mode:** A voice-activated interface that lets the driver verify pickup references or check out using simple voice commands (TM-F066).
3.  **Carrier Portal Live Sync:** Automatic, secure sharing of selected PODs with participating carriers to trigger faster payment processing.
4.  **Automatic Gmail Integration:** Secure background polling of connected email accounts to auto-import Rate Confirmations (TM-F034).
5.  **Multi-Language Audio Playback:** Text-to-speech audio playback of Critical Load Instructions for non-native English speaking drivers.

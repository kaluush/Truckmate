# TruckMate — Feature Bank

Status vocabulary: **V1 Core**, **V1 Candidate**, **Architecture Later**, **Future**, **Rejected/Out of Scope**.

| ID | Feature | Status | Notes |
|---|---|---|---|
| TM-F001 | Dynamic Current Load Card | V1 Core | Central experience; stage-aware. |
| TM-F002 | Rate/load document import | V1 Core | PDF/image/share/scan. |
| TM-F003 | Gemini document extraction | V1 Core | Extract operational fields with confidence/fallback. |
| TM-F004 | Pickup number prominence | V1 Core | Large/visible at pickup stage. |
| TM-F005 | Delivery/BOL number prominence | V1 Core | Large/visible after pickup. |
| TM-F006 | Shipper/receiver addresses + appointments | V1 Core | Load-card essentials. |
| TM-F007 | Truck/trailer/plate/company/DOT reference details | V1 Core | One-scroll access at check-in desk. |
| TM-F008 | Check In timestamp/location | V1 Core | User-triggered detention evidence. |
| TM-F009 | Check Out + dwell calculation | V1 Core | Manual or safely inferred. |
| TM-F010 | Detention timeline/evidence | V1 Core | Proof-oriented record, not guarantee of payment. |
| TM-F011 | Copy/share arrival/check-in/check-out status | V1 Core | SMS, WhatsApp, email, carrier app via native share. |
| TM-F012 | In-app multi-page document scanning | V1 Core | Existing/native scanner. |
| TM-F013 | Load document package | V1 Core | Confirmation/rate con + BOL + POD. |
| TM-F014 | Original document retention | V1 Core | Structured extraction never replaces source. |
| TM-F015 | Document Wallet / Essentials | V1 Core | Expected slots + custom Other. |
| TM-F016 | Document type mismatch detection | V1 Candidate | Warn when wrong doc placed in known slot. |
| TM-F017 | Expiration extraction/countdown | V1 Core | Registration, medical, insurance, etc. |
| TM-F018 | Expiration notifications | V1 Core | 60/45/30/15/7/3/2/1/expired default cadence. |
| TM-F019 | Replacement-document recognition | V1 Candidate | Update date and cancel old reminders. |
| TM-F020 | Quick PTI | V1 Core | Fast everyday walk-around record. |
| TM-F021 | Detailed PTI | V1 Candidate | More thorough periodic inspection. |
| TM-F022 | Defect notes/photos/reminders | V1 Candidate | Open-defect follow-up. |
| TM-F023 | Deadhead calculation | V1 Core | Current location to pickup. |
| TM-F024 | Loaded-mile calculation | V1 Core | Pickup to receiver. |
| TM-F025 | Total trip miles | V1 Core | Deadhead + loaded. |
| TM-F026 | Configurable deadhead/pay behavior | V1 Core | Optional and carrier-dependent. |
| TM-F027 | Mileage-driver estimated weekly pay | V1 Candidate | CPM + paid miles rules. |
| TM-F028 | Pro load revenue extraction | V1 Core (Pro) | Rate/pay where available. |
| TM-F029 | Pro expenses/fuel | V1 Core (Pro) | Load/business expense records. |
| TM-F030 | Pro weekly/monthly/yearly analytics | V1 Core (Pro) | Loads, gross, miles, RPM, fuel, expenses, net estimate. |
| TM-F031 | Settlement reconciliation support | V1 Candidate (Pro) | Surface possible duplicates/missing/mismatched charges. |
| TM-F032 | Tax-record organization/export | Architecture Later | Organization only; not tax advice. |
| TM-F033 | Nearby truck service/dealer/tires/truck stops | V1 Candidate | Keep lightweight. |
| TM-F034 | Gmail automatic rate-con detection/import | Future | Requires permissions/verification design. |
| TM-F035 | Subscription billing | Future before public monetization | Add after field validation. |
| TM-F036 | Full-feature trial | Future before monetization | Avoid crippled trial. |
| TM-F037 | Offline/poor-network load-card access | Must review for V1 | High-value trucking environment gap. |
| TM-F038 | Data export/account deletion/retention controls | Must review for V1 | Privacy lifecycle requirement. |
| TM-F039 | Large roadside service marketplace | Rejected/Out of Scope | Feature bloat for V1. |
| TM-F040 | ELD replacement | Rejected/Out of Scope | Explicit boundary. |

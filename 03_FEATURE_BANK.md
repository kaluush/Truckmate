# TruckMate — Feature Bank

Status vocabulary: **V1 Core**, **V1 Candidate**, **Architecture Later**, **Future**, **Rejected/Out of Scope**.

| ID | Feature | Status | Notes |
|---|---|---|---|
| TM-F001 | Dynamic Current Load Card | V1 Core | Central experience; stage-aware and multi-stop aware. |
| TM-F002 | Rate/load document import | V1 Core | PDF/image/share/scan. |
| TM-F003 | Gemini document extraction | V1 Core | Extract operational fields with confidence/fallback. |
| TM-F004 | Pickup number prominence | V1 Core | Large/visible for the active pickup stop. |
| TM-F005 | Delivery/BOL number prominence | V1 Core | Large/visible for the active delivery stop. |
| TM-F006 | Shipper/receiver addresses + appointments | V1 Core | Load-card essentials across stops. |
| TM-F007 | Truck/trailer/plate/company/DOT reference details | V1 Core | One-scroll access at check-in desk. |
| TM-F008 | Check In timestamp/location + provenance | V1 Core | Preserve system-captured vs manual/edited origin. |
| TM-F009 | Check Out + dwell calculation + provenance | V1 Core | Manual or safely inferred; preserve provenance. |
| TM-F010 | Detention timeline/evidence | V1 Core | Proof-oriented record, not guarantee of payment. |
| TM-F011 | Copy/share arrival/check-in/check-out status | V1 Core | SMS, WhatsApp, email, carrier app via native share. |
| TM-F012 | In-app multi-page document scanning | V1 Core | Existing/native scanner. |
| TM-F013 | Load document package | V1 Core | Confirmation/rate con + BOL + POD + flexible Other Load Documents. |
| TM-F014 | Original document retention | V1 Core | Structured extraction never replaces source. |
| TM-F015 | Document Wallet / Essentials | V1 Core | Expected slots + custom Other. |
| TM-F016 | Document type mismatch detection | V1 Candidate | Warn when wrong doc placed in known slot. |
| TM-F017 | Expiration extraction/countdown | V1 Core | Registration, medical, insurance, etc. |
| TM-F018 | Expiration notifications | V1 Core | 60/45/30/15/7/3/2/1/expired default cadence. |
| TM-F019 | Replacement-document recognition | V1 Candidate | Update date and cancel old reminders. |
| TM-F020 | Quick PTI | V1 Core | Fast everyday walk-around record. |
| TM-F021 | Detailed PTI | V1 Core | Driver-selectable detailed inspection; timestamped and saved to history. |
| TM-F022 | Defect notes/photos/reminders | V1 Candidate | Open-defect follow-up. |
| TM-F023 | Deadhead calculation | V1 Core | Current location to first pickup. |
| TM-F024 | Loaded-mile calculation | V1 Core | Across load stops. |
| TM-F025 | Total trip miles | V1 Core | Deadhead + loaded. |
| TM-F026 | Configurable deadhead/pay behavior | V1 Core | Optional and carrier-dependent. |
| TM-F027 | Mileage-driver estimated weekly pay | V1 Candidate | CPM + paid miles rules. |
| TM-F028 | Pro load revenue extraction | V1 Core (Pro) | Rate/pay where available. |
| TM-F029 | Pro expenses/fuel | V1 Core (Pro) | Load/business expense records. |
| TM-F030 | Pro weekly/monthly/yearly analytics | V1 Core (Pro) | Loads, gross, miles, RPM, fuel, expenses, net estimate. |
| TM-F031 | Settlement reconciliation support | V1 Core (Pro) | Line-by-line load matching across settlement weeks; awaiting-settlement status and mismatch/accessorial review. |
| TM-F032 | Excel + document export | V1 Core (Pro) | All data/date range/selected loads; Excel financial index plus original document bundle. |
| TM-F033 | Driver-sourced facility intelligence | V1 Core | Opt-in post-stop structured reports; timestamp/source; supplement conflicting public info without exposing driver history. |
| TM-F034 | Gmail automatic rate-con detection/import | Future | Requires permissions/verification design. |
| TM-F035 | Subscription billing | Future before public monetization | Add after field validation. |
| TM-F036 | Full-feature trial | Monetization target | 60 days full-feature, then $59.98/month. |
| TM-F037 | Offline/poor-network operation | V1 Core | Everything technically feasible stays usable offline; safe queued sync. |
| TM-F038 | Data deletion/retention controls | V1 Core | 30-day Trash then permanent deletion; deleted-file history may retain event only. |
| TM-F039 | Large roadside service marketplace | Rejected/Out of Scope | Feature bloat for V1. |
| TM-F040 | ELD replacement | Rejected/Out of Scope | Explicit boundary. |
| TM-F041 | Multi-stop loads | V1 Core | Multiple pickups and/or deliveries; Current Load Card follows active stop. |
| TM-F042 | Current + Upcoming/Pre-planned Loads | V1 Core | Prevent upcoming assignment from replacing current work. |
| TM-F043 | Explicit document-to-load attribution | V1 Core | Scan/import must attach to intended load; ambiguous cases require confirmation. |
| TM-F044 | Other Load Documents | V1 Core | Flexible slot for lumper/scale/washout/accessorial/damage and similar paperwork. |
| TM-F045 | Load History + search | V1 Core | Base-tier retrieval across past loads/documents. |
| TM-F046 | In-motion interaction guardrails | V1 Core UX requirement | Minimize interaction; no core workflow requiring multi-field typing while moving. |
| TM-F047 | Team-driver/shared-load access | Architecture Later | Schema should permit later sharing without building it now. |
| TM-F048 | AI usage/cost-abuse controls | Architecture Later / production requirement | Server-side quotas/rate controls for extraction calls. |

| TM-F049 | Offline check-in/out | V1 Core | Local timestamp + location when available; provenance/pending-sync state; idempotent sync. |
| TM-F050 | Custom wallet documents | V1 Core | User-defined document types with expiration/reminders when applicable. |
| TM-F051 | Mileage source + manual adjustment audit | V1 Core | Carrier-paid vs route-estimated vs driver-adjusted miles; preserve original. |
| TM-F052 | Document/state automation override | V1 Core | Pickup papers → In Transit; signed POD/BOL → Delivered when reliable; user can override. |
| TM-F053 | Partial/full rejection + disposition | V1 Core | Same load; Delivered with Exception or Awaiting Instructions; add disposition stop and compensation. |
| TM-F054 | Unified truck/business expenses | V1 Core (Pro) | No mandatory load link; categories, location, description, optional receipt/line items, recurring overhead. |
| TM-F055 | Correction audit history | V1 Core | Preserve original AI/system value and manual correction provenance. |

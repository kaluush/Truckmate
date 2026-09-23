# TruckMate — Common Real-World Design Tests

All three improved designs must be tested against the same scenarios before selection.

These scenarios do not create new product requirements. They exercise already-approved behavior.

## Test method
For every scenario, record:
- number of taps/actions for the primary path
- whether typing is required
- whether the needed information is immediately visible
- whether the flow works one-handed
- what happens with poor/no signal
- likely driver confusion points
- error/recovery behavior
- any privacy or safety concern
- requirement IDs involved

Use actual field observation or direct driver testing when available. If only a walkthrough is available, label it as simulated rather than real-world evidence.

## Scenarios

### RW-01 — Arrive at pickup
Driver is at the gate and needs the pickup/release number, carrier, truck and trailer information immediately, then checks in.

### RW-02 — No signal at shipper
Driver has no usable internet connection, checks in, later checks out, and expects the events to sync safely when connectivity returns.

### RW-03 — Multi-stop changes order
A load has multiple stops. The driver must service a later stop first without falsely marking earlier stops complete.

### RW-04 — Detention approaching
Driver checked in earlier. The configured detention threshold is approaching and the driver needs to understand dwell time and the reminder without being told payment is guaranteed.

### RW-05 — Pickup paperwork changes stage
Driver uploads/scans pickup paperwork. The design should make the resulting In Transit state clear and recoverable if classification is wrong.

### RW-06 — Delivery with exception
Receiver rejects part of the freight. Driver records quantity/reason/evidence and later receives disposition instructions without creating a fake new load.

### RW-07 — Trailer swap
Driver changes trailers during the load, records the new assignment, completes or skips the Quick Trailer Check with reason, and prior equipment history remains understandable.

### RW-08 — Reefer load
Driver needs set point, operating mode, reefer fuel, unit/alarm status and critical instructions without cluttering Dry Van workflows.

### RW-09 — Expiring insurance / recurring bill
Insurance expires in six days and a recurring bill is due monthly. Driver must understand both through the unified Upcoming experience without confusing them with Upcoming Loads.

### RW-10 — Settlement does not match
A delivered load appears on a later settlement with a mismatch or missing accessorial. Driver must find and understand the discrepancy.

### RW-11 — Find an old document
Driver needs a past load/BOL quickly using Load History/search without entering Pro analytics.

### RW-12 — Account support without document exposure
Support must identify the account and use permitted activity/upload metadata to help with a recovery/support case without opening the driver's private documents.

## Evidence priority
Prefer, in order:
1. observed real-driver behavior
2. moderated driver usability testing
3. field-test logs and task timing
4. owner/operator workflow evidence
5. simulated walkthrough when field evidence is not yet available

Do not present simulated results as real-world data.

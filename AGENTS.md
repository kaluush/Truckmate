# TruckMate — AI Working Instructions

This repository is the centralized working memory and shared operating system for TruckMate.

## Start of every session
1. Read `08_APPROVAL_STATUS.md`.
2. Read `06_AI_HANDOFF.md` and `04_CURRENT_WORK.md`.
3. Review work since the last approved work commit before new meaningful work.
4. Read only authoritative sections relevant to the task; do not reread unchanged large files unnecessarily.

## Source-of-truth hierarchy
1. Active entries in `02_MASTER_DECISIONS.md`.
2. `01_MASTER_SRS.md` and detailed requirements/specs.
3. `01_PROJECT_BRIEF.md`.
4. `04_CURRENT_WORK.md`.
5. `06_AI_HANDOFF.md`.
6. `03_FEATURE_BANK.md`.
7. Research/session history.

`05_OPEN_QUESTIONS.md` is unresolved work, not approved truth.

## Critical-review rule
Do not follow the project owner blindly. Identify better alternatives, unnecessary complexity, missing requirements, contradictions, UX friction, privacy/security/reliability risks, vendor lock-in, and maintainability problems. Push back when the downside is meaningful and explain the better alternative and tradeoffs.

## Scope rule — adaptive PWT
PWT does NOT require a predetermined number of SRS sections or modules. Project structure must follow actual complexity. A project may need 10, 30, 60, 90, or another number of sections. Never create filler sections to satisfy a count. Add a section only when it improves clarity, ownership, requirements coverage, or implementation safety.

Classify gaps as:
- **Must Have Now**
- **Architecture Must Allow Later**
- **Optional Future**

Build for extension, not speculation.

## TruckMate product principles
- Clean. Easy. Simple.
- Designed first for real CDL driver and owner-operator workflows.
- Collect information once; reuse it everywhere.
- Minimize taps and typing.
- The Current Load Card is the center of the working-day experience.
- Do not turn TruckMate into an ELD.
- User operational/business data stays private unless the user explicitly shares it.
- Avoid feature bloat; prioritize high-frequency, meaningful pain points that are straightforward to solve.

## Architecture
Prefer: **Mobile Client → TruckMate-owned API/business boundary → data/services/integrations**.
Keep business rules, authorization, validation, sensitive data access, and integration orchestration out of presentation code. Controlled direct client integrations are acceptable when intentional and secure.

Current direction: mobile app, Firebase Authentication, project-owned backend/API, Firestore or equivalent structured data, Cloud Storage for documents, Gemini for document extraction/classification, push notifications, device location/maps, scanner component, subscription billing later, Gmail integration later.

Do not commit secrets.

## Requirements/risk gate
Before major design/coding, check users/roles, normal and edge flows, offline/failure/recovery behavior, dependencies, security/privacy, data retention/deletion, accessibility, observability, and future impact. Scale analysis to the risk; do not create bureaucracy for small changes.

## Lifecycle
Discovery & Planning → Design → Development → Testing → Deployment & Review. Iteration backward is allowed whenever evidence requires it.

## End of meaningful session
Update only affected files: decisions, feature bank, current work, open questions, SRS/specs, handoff, changelog, and approval status as appropriate. Leave enough repository context that the next AI can continue without the previous chat.

## Approval rule
Do not mark your own new work approved merely because you created it. The next AI/session should independently review it unless the project owner explicitly approves that exact work.

## Core principle
**Think critically. Read efficiently. Find gaps without scope creep. Design for real drivers. Validate before building further. Build for extension, not speculation.**


## Final V1 freeze rule
The owner-approved V1 baseline is frozen through TM-D055. The unified Upcoming due/reminder module and lean admin/operations panel are the final V1 feature additions. Do not add another V1 feature during design or coding. Put new ideas in V2/future scope unless they are necessary to repair a contradiction, security/privacy flaw, implementation blocker, or field-validated gap in an already-approved requirement.


## Multi-AI design protocol
For TruckMate UX/design, follow the canonical workflow in `design/README.md`:

Requirements → 3 independent designs → 3 improved designs by learning from each other → select the strongest design using real-world data → selected final design → all AIs improve that selected design together → coding.

Isolation is mandatory during the first design round. GPT, Claude, and Gemini must not read the other independent designs until all three first versions are complete. Coding begins only after the jointly improved final design is explicitly approved by the owner.

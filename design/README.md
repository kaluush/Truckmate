# TruckMate Design Workspace

This directory preserves the approved multi-AI design process.

## Canonical design workflow

**Requirements → 3 independent designs → 3 improved designs by learning from each other → select the strongest design using real-world data → selected final design → all AIs improve that selected design together → coding**

## Stage 1 — Independent designs
GPT, Claude, and Gemini each create a complete first design from the same approved TruckMate requirements.

They MUST NOT read the other two independent designs before completing their own.

Files:
- `independent/gpt/DESIGN_V1.md`
- `independent/claude/DESIGN_V1.md`
- `independent/gemini/DESIGN_V1.md`

The originals are preserved permanently.

## Stage 2 — Improved designs
Only after all three independent designs are complete, each AI reads the other two designs, learns from them, and improves its own design.

Files:
- `improved/gpt/DESIGN_V2.md`
- `improved/claude/DESIGN_V2.md`
- `improved/gemini/DESIGN_V2.md`

Each AI may adopt better ideas from the others, but must still produce its own improved design.

## Stage 3 — Selection using real-world data
Compare the three improved designs against the same approved TruckMate requirements and real-world driver evidence.

The selection is based on evidence such as:
- real driver workflow fit
- minimal taps and typing
- one-handed mobile use
- fast comprehension at pickup/delivery
- safe in-motion behavior
- offline reality
- privacy
- error recovery
- implementation practicality
- consistency with the frozen V1 scope

Do not select based on which AI created the design.

Selection notes belong in `selection/`.

## Stage 4 — Selected final design
The strongest design becomes the selected final design and is copied/preserved in:
- `final/SELECTED_DESIGN.md`

This is the chosen foundation, not yet the coding baseline.

## Stage 5 — Improve the selected final design together
GPT, Claude, and Gemini then review the selected design together and improve that one design using the best evidence and ideas from all prior work.

The resulting owner-approved coding baseline belongs in:
- `final-improvement/FINAL_DESIGN.md`

## Stage 6 — Coding
Coding begins only after the owner approves the final improved design.

## Scope rule
No stage of the design process may add new V1 features. New feature ideas go to V2 unless they repair a contradiction, security/privacy flaw, implementation blocker, or field-validated gap in an already-approved requirement.

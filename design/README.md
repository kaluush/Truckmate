# TruckMate Design Workspace

This directory preserves the approved multi-AI design process.

## Canonical design workflow

**Frozen requirements → shared design brief → 3 independent designs → requirement check → 3 improved designs by learning from each other → same real-world tests → blind evidence-based selection → selected final design → all AIs improve that selected design together → owner approval/freeze → coding**

## Stage 0 — Shared brief
All three AIs receive the same instructions and source material from:
- `SHARED_DESIGN_BRIEF.md`

No AI gets a different private design brief.

## Stage 1 — Three independent designs
GPT, Claude, and Gemini each create a complete first design from the same approved TruckMate requirements.

They MUST NOT read the other two independent designs before completing their own.

Files:
- `independent/gpt/DESIGN_V1.md`
- `independent/claude/DESIGN_V1.md`
- `independent/gemini/DESIGN_V1.md`

The originals are preserved permanently.

## Stage 2 — Requirement completeness check
Before peer learning, each independent design is checked against:
- `validation/REQUIREMENT_CHECK_TEMPLATE.md`

This stage repairs omissions or contradictions only. It is not a design competition and does not add features.

## Stage 3 — Three improved designs
Only after all three independent designs pass the requirement check may each AI read the other two designs.

Each AI learns from the other designs and improves its own:
- `improved/gpt/DESIGN_V2.md`
- `improved/claude/DESIGN_V2.md`
- `improved/gemini/DESIGN_V2.md`

The original independent files remain unchanged.

## Stage 4 — Same real-world tests
Test all three improved designs using the identical scenarios in:
- `testing/REAL_WORLD_SCENARIOS.md`

Record whether evidence is observed/measured or only simulated. Do not call simulated walkthroughs real-world data.

## Stage 5 — Blind evidence-based selection
Temporarily label the improved designs Design A, B and C and evaluate them without AI identity using:
- `selection/EVALUATION_PROTOCOL.md`

Selection must be supported by concrete evidence rather than preference or AI reputation.

Preserve all three designs and the evidence, including the designs that are not selected.

## Stage 6 — Selected final design
The selected design is preserved in:
- `final/SELECTED_DESIGN.md`

It is the chosen foundation, not yet the coding baseline.

## Stage 7 — Improve the selected design together
GPT, Claude, and Gemini jointly challenge and improve the selected design using the requirements, test evidence, and useful ideas preserved from all three designs.

Output:
- `final-improvement/FINAL_DESIGN.md`

At this stage the goal is no longer to defend the original AI designs. The goal is to make the selected design stronger.

## Stage 8 — Owner approval and design freeze
The final improved design becomes the coding baseline only after explicit owner approval.

After approval, design changes during coding require a real implementation problem, contradiction, security/privacy problem, or field-validated issue—not casual redesign.

## Stage 9 — Coding
Coding starts from the approved frozen design.

## Scope rule
No stage of the design process may add new V1 features. New feature ideas go to V2 unless they repair a contradiction, security/privacy flaw, implementation blocker, or field-validated gap in an already-approved requirement.

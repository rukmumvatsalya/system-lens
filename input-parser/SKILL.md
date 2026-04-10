# Input Parser
## SKILL.md — Version 1.0

Conforms to AGENT_STACK_FORMAT.md §5.4 (9-section skill format). No restatement per §2.

---

## Purpose

Parse whatever design system artifact the human shares into a structured inventory of tokens, components, patterns, and metadata — so the auditor skills can review without re-reading raw input.

---

## When

Triggered at the start of every run as soon as a design system artifact is provided. Ends when all outputs are produced: `artifact_inventory`, `token_inventory`, `component_inventory`, `pattern_inventory`, `metadata`, `k1_k2_k3_verdict`.

---

## Required Inputs

| Input | Why |
|---|---|
| Design system artifact (SOURCE_OF_TRUTH, any format per AGENT_SPEC INPUT) | The thing being parsed into a structured inventory |
| Optional REFERENCE docs — benchmark target, brand guidelines, platform info (per AGENT_SPEC INPUT) | Context for disambiguation — never overrides the primary artifact |

---

## Framework

```
Step 1: Identify artifact type (Figma link, token file, component spec, markdown doc, screenshot, PDF, style guide, mixed). → artifact_type
Step 2: Run K1/K2/K3 detection — is input empty? Is it a redesign request? Is it code-only? → k1_k2_k3_verdict
Step 3: Extract token inventory — list every token found: color, typography, spacing, elevation, motion, iconography, borders, opacity, breakpoints. Note naming conventions. → token_inventory
Step 4: Extract component inventory — list every component found: name, states documented, variants documented, usage guidance present/absent. → component_inventory
Step 5: Extract pattern inventory — list every composed pattern found: empty states, loading, error, navigation, onboarding, forms, data display, feedback, modals, toasts. → pattern_inventory
Step 6: Extract metadata — platform (web/mobile/both), brand info, target audience, any design principles or values stated. → metadata
Step 7: Compile full artifact inventory — what was found, what format it was in, what sections/areas the input covers. → artifact_inventory
```

---

## Rules

R1: IF no design system input provided (empty or missing) → STOP. K1 fires. Return to human with "nothing to review — please share a design system artifact."
R2: IF input is a redesign request ("redesign this", "make this better", "create a new version") → PAUSE. K2 fires. Ask human for what to review.
R3: IF input is code-only (React components, CSS files, Tailwind config) with no design artifacts → PAUSE. K3 fires. Ask human for design artifacts.
R4: IF input format is unrecognizable → PAUSE. Ask human to clarify what was shared.
R5: IF input covers only part of a design system (e.g., only tokens, no components) → proceed. Note coverage scope in `metadata`. Downstream auditors will mark uncovered areas as "NOT PROVIDED" rather than "MISSING."
R6: IF optional REFERENCE docs are provided → extract benchmark target name and pass to `benchmark-engine`. Do not use reference to evaluate the primary artifact.
R7: IF input is in a non-English language → preserve all labels, names, and descriptions verbatim in original language.

---

## Output

| Output | Content |
|---|---|
| `artifact_type` | What kind of input was provided (Figma / token file / component spec / markdown / screenshot / mixed) |
| `artifact_inventory` | Full list of what was found in the input, organized by area (tokens, components, patterns, other) |
| `token_inventory` | Every token found: category, name, value, naming convention notes |
| `component_inventory` | Every component found: name, states, variants, usage guidance (present/absent) |
| `pattern_inventory` | Every composed pattern found: name, coverage description |
| `metadata` | Platform, brand info, target audience, design principles, coverage scope |
| `k1_k2_k3_verdict` | `pass` or `fail` with code (K1/K2/K3) — `fail` stops or pauses the run |

---

## Fail Conditions

| Condition | Severity | Action |
|---|---|---|
| No input provided (K1) | S1 | STOP |
| Input is a redesign request (K2) | S1 | PAUSE — ask human |
| Input is code-only (K3) | S1 | PAUSE — ask human |
| Input format unrecognizable | S1 | PAUSE — ask human |
| Input covers only partial system | S2 | FLAG — note scope, proceed |
| Zero tokens, components, and patterns found in input | S1 | STOP — "input does not appear to contain a design system" |

---

## Does Not Do

- Does not audit or evaluate anything — only parses and inventories
- Does not produce verdicts (PASS/GAP/MISSING)
- Does not benchmark against world-class systems
- Does not create or suggest design elements
- Does not modify the input
- Does not translate content
- Does not review code
- Does not rank or prioritize findings

---

## Traceability

| This Skill Enforces | Authority |
|---|---|
| K1 detection (no input) | AGENT_SPEC Kill K1 |
| K2 detection (redesign request) | AGENT_SPEC Kill K2 |
| K3 detection (code-only) | AGENT_SPEC Kill K3 |
| No invented content — parse only | AGENT_SPEC Constraint 2, Constraint 7 |
| Design level only — no code parsing | AGENT_SPEC Constraint 3 |

---

*Input Parser — SKILL.md v1.0*

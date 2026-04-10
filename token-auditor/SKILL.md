# Token Auditor
## SKILL.md — Version 1.0

Conforms to AGENT_STACK_FORMAT.md §5.4 (9-section skill format). No restatement per §2.

---

## Purpose

Audit the token architecture of the design system — color, typography, spacing, elevation, motion, iconography — for completeness, layered structure, naming consistency, and coverage against what world-class systems include.

---

## When

Triggered after `input-parser` emits its output. Runs in parallel with `component-auditor`, `pattern-auditor`, and `accessibility-auditor`. Ends when `token_audit_result` is produced.

---

## Required Inputs

| Input | Why |
|---|---|
| `input-parser` output — `token_inventory` | The tokens to audit |
| `input-parser` output — `metadata` | Platform, brand context, coverage scope |

---

## Framework

```
Step 1: Read token inventory from parser. → raw_tokens
Step 2: Audit COLOR tokens — palette definition, semantic naming (primary/secondary/error/warning/success/info), surface/background tokens, text-on-surface tokens, dark mode support, opacity variants. → color_audit
Step 3: Audit TYPOGRAPHY tokens — type scale (how many steps), font families, weights, line heights, letter spacing, responsive type, heading hierarchy. → typography_audit
Step 4: Audit SPACING tokens — spacing scale (consistent multiplier?), component-internal vs layout spacing, responsive spacing. → spacing_audit
Step 5: Audit ELEVATION / SHADOW tokens — shadow scale, elevation hierarchy, usage semantics (card, modal, dropdown, tooltip). → elevation_audit
Step 6: Audit MOTION tokens — easing curves, duration scale, transition types, reduced-motion support. → motion_audit
Step 7: Audit ICONOGRAPHY — icon set present, sizing tokens, consistent style, stroke/fill consistency, accessibility labels. → iconography_audit
Step 8: Audit TOKEN ARCHITECTURE — layered structure (global → alias → component?), naming convention consistency, documentation of token purpose. → architecture_audit
Step 9: Assign verdict per category (PASS / GAP / MISSING) and note specific gaps. → token_audit_result
```

---

## Rules

R1: IF `token_inventory` is empty or marked "NOT PROVIDED" by parser → mark ALL token categories as MISSING. Do not fabricate findings. Proceed with empty audit.
R2: IF a token category exists but is incomplete (e.g., colors defined but no semantic naming) → verdict is GAP with specific description of what's missing.
R3: IF a token category is fully absent → verdict is MISSING.
R4: IF a token category is complete with layered architecture, semantic naming, and documentation → verdict is PASS.
R5: IF tokens use inconsistent naming conventions across categories (e.g., camelCase for color, kebab-case for spacing) → FLAG as GAP in architecture audit.
R6: IF motion tokens are entirely absent → FLAG as GAP (not MISSING) — many systems lack these, but world-class ones include them.
R7: Do not suggest specific token values — only flag what's missing or inconsistent structurally.

---

## Output

`token_audit_result` — structured audit per category:

| Field | Content |
|---|---|
| `color_audit` | Verdict + what's here + what's missing |
| `typography_audit` | Verdict + what's here + what's missing |
| `spacing_audit` | Verdict + what's here + what's missing |
| `elevation_audit` | Verdict + what's here + what's missing |
| `motion_audit` | Verdict + what's here + what's missing |
| `iconography_audit` | Verdict + what's here + what's missing |
| `architecture_audit` | Verdict on layered structure + naming consistency |
| `token_overall_verdict` | PASS (all pass) / GAP (some gaps) / MISSING (no tokens) |

---

## Fail Conditions

| Condition | Severity | Action |
|---|---|---|
| Token inventory not provided by parser | S2 | FLAG — mark all MISSING, proceed |
| Audit produces a creation/redesign suggestion | S1 | STOP — rewrite as gap description only |
| Audit references code implementation | S1 | STOP — stay at design level |

---

## Does Not Do

- Does not create tokens or suggest specific values
- Does not redesign the token architecture
- Does not benchmark (that's `benchmark-engine`'s job)
- Does not audit components or patterns
- Does not review code or implementation
- Does not assign priority (that's `verdict-assembler`'s job)
- Does not look at other auditors' outputs

---

## Traceability

| This Skill Enforces | Authority |
|---|---|
| Per-item verdict (PASS/GAP/MISSING) | AGENT_SPEC Measure M1 |
| Coverage check — identifies what's missing | AGENT_SPEC Measure M4 |
| Findings trace to input or documented absence | AGENT_SPEC Measure M5, Constraint 7 |
| Review only — no creation | AGENT_SPEC Measure M8, Constraint 1 |
| Design level only | AGENT_SPEC Measure M6, Constraint 3 |

---

*Token Auditor — SKILL.md v1.0*

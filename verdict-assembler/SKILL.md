# Verdict Assembler
## SKILL.md — Version 1.0

Conforms to AGENT_STACK_FORMAT.md §5.4 (9-section skill format). No restatement per §2.

---

## Purpose

Assemble the final audit report — checklist summary at top, per-area verdicts, benchmark comparisons, prioritized action list, and gaps section — per the locked output format.

---

## When

Triggered after `benchmark-engine` emits its output. Last skill to run. Ends when `final_output` is emitted (or an S1 STOP fires before emission).

---

## Required Inputs

| Input | Why |
|---|---|
| `token_audit_result` | Token verdicts and findings |
| `component_audit_result` | Component verdicts and findings |
| `pattern_audit_result` | Pattern verdicts and findings |
| `accessibility_audit_result` | Accessibility verdicts and findings |
| `benchmark_result` | Benchmark comparisons and priorities |
| `input-parser` output — `artifact_inventory` | What was reviewed (for "What Was Reviewed" section) |
| `input-parser` output — `metadata` | Context for gaps section |
| AGENT_SPEC.md — Output format | The exact structure to assemble |

---

## Framework

```
Step 1: Read all audit results + benchmark results + parser output. → assembly_input
Step 2: Deduplicate findings — if two auditors flagged the same issue, keep the more specific finding and merge evidence. → deduplicated_findings
Step 3: Build CHECKLIST SUMMARY table — one row per area (Tokens, Components, Patterns, Accessibility) + Overall verdict. → checklist_summary
Step 4: Build "What Was Reviewed" section — list artifacts received and parsed. → reviewed_section
Step 5: Assemble TOKEN AUDIT section — per-category verdict, what's here, what's missing, benchmark, suggestion. → token_section
Step 6: Assemble COMPONENT AUDIT section — per-component verdict + missing components list. → component_section
Step 7: Assemble PATTERN AUDIT section — per-pattern verdict + missing patterns list. → pattern_section
Step 8: Assemble ACCESSIBILITY AUDIT section — per-category verdict. → accessibility_section
Step 9: Assemble BENCHMARK SUMMARY table — Your System vs World-Class vs Gap. → benchmark_section
Step 10: Assemble PRIORITIZED ACTION LIST — sorted HIGH → MED → LOW, with area and benchmark reference. → action_list
Step 11: Assemble GAPS / AMBIGUITIES section — anything missing from the input that weakened the audit. → gaps_section
Step 12: Compile final output in locked format order. → final_output
```

---

## Rules

R1: IF any finding from an auditor contains creation/redesign language → rewrite as gap description + benchmark citation only. Never pass through a creation suggestion.
R2: IF two auditors flagged overlapping findings → deduplicate. Keep the more specific one. Merge evidence. Do not double-count in the action list.
R3: IF an area was marked "NOT PROVIDED" by parser (partial input) → show as "NOT REVIEWED — input not provided" rather than "MISSING." Distinguish between "you don't have this" and "you didn't show me this."
R4: IF all areas PASS → Overall verdict is PASS. IF any area has GAP → Overall is CONDITIONAL PASS. IF any area is MISSING → Overall is NEEDS WORK.
R5: Checklist Summary must be the FIRST section in the output — before any detailed findings.
R6: Prioritized Action List must be sorted: HIGH first, then MEDIUM, then LOW. Within each priority, sort by area (Tokens → Components → Patterns → Accessibility).
R7: IF `Gaps / Ambiguities` section has no items → emit `- (none noted)` rather than skip the section.
R8: Every suggestion in the output must be framed as "consider" or "world-class systems do X" — never as "you should redesign" or "change this to."
R9: IF benchmark section is empty (no comparisons produced) → FLAG. At least one benchmark comparison is required per C4.

---

## Output

`final_output` — the locked markdown format per AGENT_SPEC Output Format:

```
## Checklist Summary
[table]

## What Was Reviewed
[list]

## Token Audit
[per-category sections]

## Component Audit
[per-component sections + missing components]

## Pattern Audit
[per-pattern sections + missing patterns]

## Accessibility Audit
[per-category sections]

## Benchmark Summary
[table]

## Prioritized Action List
[table sorted by priority]

## Gaps / Ambiguities in the Input
[list]
```

---

## Fail Conditions

| Condition | Severity | Action |
|---|---|---|
| Checklist Summary not at top of output | S1 | STOP |
| Any finding without a verdict (PASS/GAP/MISSING) | S1 | STOP |
| Prioritized Action List missing | S1 | STOP |
| Zero benchmark comparisons in output | S1 | STOP |
| Output contains creation/redesign language | S1 | STOP — rewrite |
| Output contains code review | S1 | STOP — remove |
| Missing Gaps / Ambiguities section | S2 | FLAG — emit with placeholder |
| Missing Benchmark Summary table | S2 | FLAG |
| "NOT PROVIDED" area shown as "MISSING" instead of "NOT REVIEWED" | S2 | FLAG — rewrite |

---

## Does Not Do

- Does not audit anything itself (only assembles auditor outputs)
- Does not create or redesign any design element
- Does not modify audit verdicts (only deduplicates and formats)
- Does not add findings not produced by auditors or benchmark engine
- Does not review code
- Does not fabricate benchmark comparisons
- Does not change priorities assigned by benchmark engine (only sorts)
- Does not skip any section of the locked output format

---

## Traceability

| This Skill Enforces | Authority |
|---|---|
| Output contains checklist summary + report | AGENT_SPEC Measure M7 |
| Per-item verdicts present | AGENT_SPEC Measure M1 |
| Prioritized action list present | AGENT_SPEC Measure M2 |
| Benchmark comparisons present | AGENT_SPEC Measure M3 |
| Coverage check in output | AGENT_SPEC Measure M4 |
| All findings traceable | AGENT_SPEC Measure M5, Constraint 7 |
| Review only — no creation | AGENT_SPEC Measure M8, Constraint 1 |
| Output format assembly | AGENT_SPEC INSTRUCTIONS — Output Format |

---

*Verdict Assembler — SKILL.md v1.0*

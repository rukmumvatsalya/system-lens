# Benchmark Engine
## SKILL.md — Version 1.0

Conforms to AGENT_STACK_FORMAT.md §5.4 (9-section skill format). No restatement per §2.

---

## Purpose

Compare all audit findings against world-class design systems and produce specific, real benchmark comparisons — citing what top systems do that the reviewed system doesn't, and what the reviewed system does well.

---

## When

Triggered after all 4 auditor skills (`token-auditor`, `component-auditor`, `pattern-auditor`, `accessibility-auditor`) have emitted their results. Ends when `benchmark_result` is produced.

---

## Required Inputs

| Input | Why |
|---|---|
| `token_audit_result` | Token findings to benchmark |
| `component_audit_result` | Component findings to benchmark |
| `pattern_audit_result` | Pattern findings to benchmark |
| `accessibility_audit_result` | Accessibility findings to benchmark |
| `input-parser` output — `metadata` | Platform and context for selecting relevant benchmarks |
| CONTEXT.md — World-Class Design System Knowledge Base | The baked-in knowledge of what top systems do |
| Optional: human-provided REFERENCE benchmark target | A specific system to compare against |

---

## Framework

```
Step 1: Read all 4 audit results + metadata + knowledge base. → benchmark_input
Step 2: For each GAP finding, identify which world-class system(s) handle this well and what specifically they do. → gap_benchmarks
Step 3: For each MISSING finding, identify which world-class systems include this and why it matters. → missing_benchmarks
Step 4: For each PASS finding, note if the reviewed system meets or exceeds world-class standard. → strength_benchmarks
Step 5: If human provided a specific benchmark REFERENCE, produce direct comparison against that system. → reference_comparison
Step 6: Assign priority to each gap/missing finding based on world-class consensus: HIGH (most Tier 1 systems include it), MEDIUM (most Tier 2 include it), LOW (advanced/niche). → prioritized_findings
Step 7: Compile benchmark summary table + per-finding benchmark citations. → benchmark_result
```

---

## Rules

R1: IF citing a world-class system → cite a REAL practice from the knowledge base. Do not fabricate what a system does.
R2: IF no world-class comparator exists for a specific finding → mark as "no direct benchmark — finding stands on its own merit."
R3: IF multiple world-class systems are relevant → cite the most relevant one (platform match, domain match, or Tier 1 first).
R4: IF human provided a specific benchmark REFERENCE → produce a dedicated comparison section, but do not limit the audit to only that system.
R5: Do not rank world-class systems against each other — only compare the input system against them.
R6: IF a finding is PASS and matches world-class standard → note it as a strength. Do not only report gaps.
R7: Priority assignment rules:
  - HIGH: ≥ 3 Tier 1 systems include this AND it affects core usability or accessibility
  - MEDIUM: ≥ 2 Tier 1 or Tier 2 systems include this OR it affects completeness
  - LOW: Advanced/niche feature OR only Tier 3 systems include this
R8: Do not suggest specific designs or solutions — only cite what benchmark systems do as reference.

---

## Output

`benchmark_result` — structured benchmark:

| Field | Content |
|---|---|
| `gap_benchmarks` | For each GAP: which system does it well + what they do specifically |
| `missing_benchmarks` | For each MISSING: which systems include it + why it matters |
| `strength_benchmarks` | For each PASS: confirmation that it meets/exceeds world-class bar |
| `reference_comparison` | If REFERENCE provided: direct comparison table |
| `prioritized_findings` | All GAP + MISSING findings with priority (HIGH/MED/LOW) and benchmark citation |
| `benchmark_summary_table` | Summary: Your System vs World-Class Benchmark vs Gap |

---

## Fail Conditions

| Condition | Severity | Action |
|---|---|---|
| No audit results provided | S1 | STOP — cannot benchmark without findings |
| Benchmark cites a fabricated practice (not in knowledge base) | S1 | STOP — rewrite with real citation |
| Benchmark suggests a specific design/solution | S1 | STOP — rewrite as reference citation only |
| Benchmark ranks world-class systems against each other | S2 | FLAG — rewrite to compare input only |

---

## Does Not Do

- Does not audit the input itself (that's the auditors' job)
- Does not create or redesign anything
- Does not fabricate what world-class systems do
- Does not rank world-class systems against each other
- Does not suggest specific solutions — only cites benchmarks
- Does not review code
- Does not assign final verdicts (that's `verdict-assembler`'s job)
- Does not modify audit results

---

## Traceability

| This Skill Enforces | Authority |
|---|---|
| Benchmark comparisons cite specific world-class systems | AGENT_SPEC Measure M3 |
| Priority assignment (HIGH/MED/LOW) | AGENT_SPEC Measure M2 |
| No fabricated benchmarks | AGENT_SPEC Constraint 6 |
| No ranking of design systems against each other | AGENT_SPEC Constraint 4 |
| Review only — no creation | AGENT_SPEC Measure M8, Constraint 1 |

---

*Benchmark Engine — SKILL.md v1.0*

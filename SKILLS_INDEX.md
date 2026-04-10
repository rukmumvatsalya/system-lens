# System Lens — Skills Index
## Version 1.0

Routing contract per AGENT_STACK_FORMAT.md §5.2. No restatement per §2.

---

## Available Skills

| # | Skill | Folder | Type | Mandatory | Purpose |
|---|---|---|---|---|---|
| 1 | Input Parser | `/input-parser/` | domain | Yes | Parse whatever design system artifact the human shares into a structured inventory of tokens, components, patterns, and metadata |
| 2 | Token Auditor | `/token-auditor/` | domain | Yes | Audit token architecture — color, typography, spacing, elevation, motion, iconography — for completeness, structure, and naming |
| 3 | Component Auditor | `/component-auditor/` | domain | Yes | Audit component coverage — states, variants, usage guidance, responsiveness, and missing components |
| 4 | Pattern Auditor | `/pattern-auditor/` | domain | Yes | Audit composed patterns — empty states, loading, error, navigation, onboarding, data display, forms, feedback |
| 5 | Accessibility Auditor | `/accessibility-auditor/` | domain | Yes | Audit accessibility — contrast, keyboard nav, focus management, ARIA, screen reader, motion sensitivity |
| 6 | Benchmark Engine | `/benchmark-engine/` | domain | Yes | Compare all audit findings against world-class design systems and produce specific benchmark comparisons |
| 7 | Verdict Assembler | `/verdict-assembler/` | domain | Yes | Assemble final report — checklist summary, verdicts, prioritized actions, benchmark table, gaps section |

---

## Execution Order

```
input-parser
    ↓
[token-auditor, component-auditor, pattern-auditor, accessibility-auditor]   ← parallel, independent
    ↓
benchmark-engine
    ↓
verdict-assembler
```

Sequential with a parallel middle step. All 7 are mandatory.

**Dependency rules:**
- `input-parser` must complete before any auditor skill starts.
- All 4 auditor skills must complete before `benchmark-engine` starts.
- `benchmark-engine` must complete before `verdict-assembler` starts.
- The 4 auditor skills run independently of each other — they must not read each other's outputs (that is `benchmark-engine` and `verdict-assembler`'s job).

---

## Conflict Resolution

| Conflict | Resolution |
|---|---|
| Two auditor skills claim the same finding (e.g., token-auditor and component-auditor both flag a color issue) | Both may flag; `verdict-assembler` deduplicates — keeps the more specific finding, merges evidence |
| Auditor skill cannot determine verdict due to ambiguous input | Mark as GAP with note "input ambiguous — could not determine"; `verdict-assembler` carries into Gaps section |
| `benchmark-engine` finds no world-class comparator for a finding | Flag as "no direct benchmark" — finding still stands on its own merit |
| `input-parser` cannot identify artifact type | PAUSE — ask human to clarify what was shared |
| Auditor produces a finding that looks like a redesign suggestion | `verdict-assembler` filters: if suggestion prescribes specific visual/interaction design → rewrite as gap description only |
| Two auditors assign different priorities to overlapping findings | `benchmark-engine` resolves by checking which world-class systems treat it as critical; `verdict-assembler` takes the higher priority |

---

## Re-Run Triggers

| Trigger | What Re-Runs |
|---|---|
| Design system input changes | All 7 skills (full re-run) |
| Human adds a REFERENCE doc (benchmark target) mid-run | `benchmark-engine` + `verdict-assembler` only |
| Human narrows scope ("only review tokens") | `input-parser` → only the relevant auditor(s) → `benchmark-engine` → `verdict-assembler` |
| Human adds context doc mid-run (brand guidelines, platform info) | `input-parser` → cascades forward |
| Output format changes | `verdict-assembler` only |
| Human requests code review mid-run | Scope change — not supported per Constraint 3 unless human explicitly overrides |

---

*System Lens — Skills Index v1.0*

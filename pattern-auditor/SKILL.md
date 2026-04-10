# Pattern Auditor
## SKILL.md — Version 1.0

Conforms to AGENT_STACK_FORMAT.md §5.4 (9-section skill format). No restatement per §2.

---

## Purpose

Audit composed patterns in the design system — empty states, loading, error handling, navigation, onboarding, data display, forms, and feedback — for coverage and completeness.

---

## When

Triggered after `input-parser` emits its output. Runs in parallel with `token-auditor`, `component-auditor`, and `accessibility-auditor`. Ends when `pattern_audit_result` is produced.

---

## Required Inputs

| Input | Why |
|---|---|
| `input-parser` output — `pattern_inventory` | The patterns to audit |
| `input-parser` output — `component_inventory` | Context — patterns are composed from components |
| `input-parser` output — `metadata` | Platform, coverage scope |

---

## Framework

```
Step 1: Read pattern inventory from parser. → raw_patterns
Step 2: Audit EMPTY STATE patterns — zero-data views, first-use experiences, search-no-results, error-recovery states. → empty_state_audit
Step 3: Audit LOADING patterns — skeleton screens, spinners, progress indicators, optimistic UI, lazy loading, pull-to-refresh (mobile). → loading_audit
Step 4: Audit ERROR patterns — inline validation, form-level errors, page-level errors, network errors, 404/500 pages, retry patterns, error recovery flows. → error_audit
Step 5: Audit NAVIGATION patterns — page transitions, deep linking, back navigation, breadcrumb trails, tab patterns, drawer/sheet patterns, command palette. → navigation_audit
Step 6: Audit ONBOARDING patterns — first-run experience, feature discovery, tooltips/coach marks, progressive disclosure, permission requests. → onboarding_audit
Step 7: Audit FORM patterns — form layout, field grouping, validation timing (on blur/submit/realtime), multi-step forms, autosave, field dependencies. → form_audit
Step 8: Audit DATA DISPLAY patterns — list/detail, master-detail, dashboard layout, data visualization, infinite scroll vs pagination, sorting/filtering, export. → data_display_audit
Step 9: Audit FEEDBACK patterns — success confirmation, destructive action confirmation, undo patterns, notification center, in-app messaging. → feedback_audit
Step 10: Check for MISSING PATTERNS — compare against standard pattern checklist (see below). → missing_patterns
Step 11: Assign verdict per pattern category and overall. → pattern_audit_result
```

**Standard pattern checklist (what world-class systems include):**

Essential: Empty states, Loading/skeleton, Error handling (inline + page), Form validation, Success/failure feedback, Confirmation dialogs, Navigation transitions

Important: Onboarding/first-run, Progressive disclosure, Search patterns, Filter/sort patterns, Infinite scroll/pagination, Pull-to-refresh (mobile), Notification patterns

Advanced: Undo/redo, Optimistic UI, Offline patterns, Multi-step flows, Drag-and-drop, Keyboard shortcuts, Command palette

---

## Rules

R1: IF `pattern_inventory` is empty or marked "NOT PROVIDED" by parser → list standard pattern checklist as MISSING. Do not fabricate findings.
R2: IF a pattern exists but covers only the happy path (e.g., loading but no error recovery) → verdict is GAP with specific missing flows listed.
R3: IF a pattern is comprehensive with edge cases covered → verdict is PASS.
R4: IF a pattern from the standard checklist is absent → list as MISSING with note on impact.
R5: IF input is mobile-only → include mobile-specific patterns (pull-to-refresh, swipe actions, bottom sheet flows) in the checklist.
R6: IF input is web-only → include web-specific patterns (keyboard shortcuts, command palette, breadcrumbs) in the checklist.
R7: Do not suggest specific pattern designs — only flag what's missing or incomplete.
R8: IF a pattern exists as a component but not as a documented flow/guideline → verdict is GAP ("component exists but usage pattern not documented").

---

## Output

`pattern_audit_result` — structured audit:

| Field | Content |
|---|---|
| `per_pattern_audit` | For each found pattern: name, coverage description, verdict |
| `missing_patterns` | Patterns from standard checklist not found in input, with impact notes |
| `pattern_overall_verdict` | PASS / GAP / MISSING |

---

## Fail Conditions

| Condition | Severity | Action |
|---|---|---|
| Pattern inventory not provided by parser | S2 | FLAG — list standard checklist as MISSING, proceed |
| Audit produces a creation/redesign suggestion | S1 | STOP — rewrite as gap description only |
| Audit references code implementation | S1 | STOP — stay at design level |

---

## Does Not Do

- Does not create or design patterns
- Does not suggest specific interaction designs
- Does not benchmark (that's `benchmark-engine`'s job)
- Does not audit tokens or individual components (only composed patterns)
- Does not review code or implementation
- Does not assign priority (that's `verdict-assembler`'s job)
- Does not look at other auditors' outputs

---

## Traceability

| This Skill Enforces | Authority |
|---|---|
| Per-item verdict (PASS/GAP/MISSING) | AGENT_SPEC Measure M1 |
| Coverage check — identifies missing patterns | AGENT_SPEC Measure M4 |
| Findings trace to input or documented absence | AGENT_SPEC Measure M5, Constraint 7 |
| Review only — no creation | AGENT_SPEC Measure M8, Constraint 1 |
| Design level only | AGENT_SPEC Measure M6, Constraint 3 |

---

*Pattern Auditor — SKILL.md v1.0*

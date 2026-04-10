# Component Auditor
## SKILL.md — Version 1.0

Conforms to AGENT_STACK_FORMAT.md §5.4 (9-section skill format). No restatement per §2.

---

## Purpose

Audit component coverage of the design system — states, variants, usage guidance, responsiveness, and identify missing components that world-class systems include.

---

## When

Triggered after `input-parser` emits its output. Runs in parallel with `token-auditor`, `pattern-auditor`, and `accessibility-auditor`. Ends when `component_audit_result` is produced.

---

## Required Inputs

| Input | Why |
|---|---|
| `input-parser` output — `component_inventory` | The components to audit |
| `input-parser` output — `metadata` | Platform, coverage scope |

---

## Framework

```
Step 1: Read component inventory from parser. → raw_components
Step 2: For each component found, audit STATE coverage — default, hover, active, focused, disabled, loading, error, empty, selected, readonly. → state_coverage
Step 3: For each component found, audit VARIANT coverage — sizes (sm/md/lg), styles (primary/secondary/ghost/destructive), density options. → variant_coverage
Step 4: For each component found, check USAGE GUIDANCE — when to use, when not to use, dos and don'ts, content guidelines. → guidance_coverage
Step 5: For each component found, check RESPONSIVE behavior — mobile vs desktop, breakpoint-specific changes, touch targets. → responsive_coverage
Step 6: Check for MISSING COMPONENTS — compare against the standard component checklist (see below). → missing_components
Step 7: Assign verdict per component (PASS / GAP) and overall verdict. → component_audit_result
```

**Standard component checklist (what world-class systems include):**

Core inputs: Button, Input/TextField, TextArea, Select/Dropdown, Checkbox, Radio, Toggle/Switch, Slider, DatePicker, TimePicker, FileUpload, Search, OTP/PIN input

Navigation: Tabs, Breadcrumb, Pagination, Sidebar/Drawer, Navbar/Header, Footer, BottomNav (mobile), Stepper/Progress

Data display: Table/DataTable, List, Card, Avatar, Badge/Tag, Tooltip, Accordion/Collapse, Timeline, Stat/Metric, EmptyState

Feedback: Alert/Banner, Toast/Snackbar, Dialog/Modal, ConfirmDialog, ProgressBar, Spinner/Loader, Skeleton

Layout: Container, Grid, Stack/Flex, Divider, Spacer

Overlay: Popover, Menu/Dropdown, Sheet/BottomSheet (mobile), Command palette

Typography: Heading, Text/Paragraph, Label, Link, Code/CodeBlock

---

## Rules

R1: IF `component_inventory` is empty or marked "NOT PROVIDED" by parser → list standard component checklist as MISSING. Do not fabricate what was found.
R2: IF a component exists but lacks state coverage (e.g., no error state, no disabled state) → verdict is GAP with specific missing states listed.
R3: IF a component exists with full state, variant, guidance, and responsive coverage → verdict is PASS.
R4: IF a component from the standard checklist is absent → list as MISSING with note on why it matters.
R5: IF input is mobile-only → adjust checklist (include BottomSheet, BottomNav, SwipeActions; de-prioritize Sidebar, Table).
R6: IF input is web-only → adjust checklist (include Table, Sidebar, CommandPalette; de-prioritize BottomSheet, BottomNav).
R7: Do not suggest specific component designs — only flag what's missing or incomplete.

---

## Output

`component_audit_result` — structured audit:

| Field | Content |
|---|---|
| `per_component_audit` | For each found component: name, state verdict, variant verdict, guidance verdict, responsive verdict, overall component verdict |
| `missing_components` | Components from standard checklist not found in input |
| `component_overall_verdict` | PASS (all found + covered) / GAP (some incomplete) / MISSING (few or none found) |

---

## Fail Conditions

| Condition | Severity | Action |
|---|---|---|
| Component inventory not provided by parser | S2 | FLAG — list standard checklist as MISSING, proceed |
| Audit produces a creation/redesign suggestion | S1 | STOP — rewrite as gap description only |
| Audit references code implementation | S1 | STOP — stay at design level |

---

## Does Not Do

- Does not create or design components
- Does not suggest specific visual designs
- Does not benchmark (that's `benchmark-engine`'s job)
- Does not audit tokens or patterns
- Does not review code or implementation
- Does not assign priority (that's `verdict-assembler`'s job)
- Does not look at other auditors' outputs

---

## Traceability

| This Skill Enforces | Authority |
|---|---|
| Per-item verdict (PASS/GAP/MISSING) | AGENT_SPEC Measure M1 |
| Coverage check — identifies missing components | AGENT_SPEC Measure M4 |
| Findings trace to input or documented absence | AGENT_SPEC Measure M5, Constraint 7 |
| Review only — no creation | AGENT_SPEC Measure M8, Constraint 1 |
| Design level only | AGENT_SPEC Measure M6, Constraint 3 |

---

*Component Auditor — SKILL.md v1.0*

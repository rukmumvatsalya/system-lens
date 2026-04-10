# Accessibility Auditor
## SKILL.md — Version 1.0

Conforms to AGENT_STACK_FORMAT.md §5.4 (9-section skill format). No restatement per §2.

---

## Purpose

Audit the design system's accessibility posture — color contrast, keyboard navigation, focus management, ARIA patterns, screen reader support, and motion sensitivity — at the design level.

---

## When

Triggered after `input-parser` emits its output. Runs in parallel with `token-auditor`, `component-auditor`, and `pattern-auditor`. Ends when `accessibility_audit_result` is produced.

---

## Required Inputs

| Input | Why |
|---|---|
| `input-parser` output — `token_inventory` | Color contrast, motion tokens |
| `input-parser` output — `component_inventory` | Component-level a11y (states, focus, ARIA) |
| `input-parser` output — `pattern_inventory` | Pattern-level a11y (keyboard flows, screen reader) |
| `input-parser` output — `metadata` | Platform context |

---

## Framework

```
Step 1: Read all inventories from parser. → a11y_input
Step 2: Audit COLOR CONTRAST — do color tokens support WCAG AA (4.5:1 text, 3:1 large text, 3:1 UI)? Is there evidence of contrast checking? Do semantic colors account for contrast? → contrast_audit
Step 3: Audit KEYBOARD NAVIGATION — do components document keyboard interaction? Tab order? Arrow key navigation for composite widgets (tabs, menus, radio groups)? Keyboard shortcuts? → keyboard_audit
Step 4: Audit FOCUS MANAGEMENT — focus ring/indicator tokens? Focus trap for modals/dialogs? Focus restoration after dismiss? Skip-to-content? → focus_audit
Step 5: Audit ARIA PATTERNS — do components specify ARIA roles, states, properties? Are live regions documented for dynamic content? Are landmarks defined? → aria_audit
Step 6: Audit SCREEN READER SUPPORT — alt text guidance for images/icons? Visually-hidden text patterns? Announcement patterns for state changes? → screenreader_audit
Step 7: Audit MOTION SENSITIVITY — prefers-reduced-motion support? Motion toggle? Are animations optional or essential? Duration limits? → motion_sensitivity_audit
Step 8: Audit TOUCH TARGETS (if mobile) — minimum 44x44px / 48x48dp? Adequate spacing between targets? → touch_audit
Step 9: Assign verdict per category and overall. → accessibility_audit_result
```

---

## Rules

R1: IF no accessibility information found in any inventory → mark ALL categories as MISSING. Note: "design system has no documented accessibility considerations."
R2: IF color tokens are present but no contrast ratios or contrast-checking guidance documented → verdict is GAP.
R3: IF components exist but none document keyboard interaction → verdict is MISSING for keyboard navigation.
R4: IF focus indicators are present as tokens/components → check if focus trap and restoration are documented for overlay components.
R5: IF ARIA patterns are documented for some components but not others → verdict is GAP with specific components listed.
R6: IF motion tokens exist but no reduced-motion alternative documented → verdict is GAP.
R7: IF platform is mobile → include touch target audit. IF web-only → skip touch targets, include skip-to-content audit.
R8: Do not test actual color values for contrast — only audit whether contrast consideration is documented in the system. This is design-level review, not code testing.

---

## Output

`accessibility_audit_result` — structured audit:

| Field | Content |
|---|---|
| `contrast_audit` | Verdict + what's documented + what's missing |
| `keyboard_audit` | Verdict + what's documented + what's missing |
| `focus_audit` | Verdict + what's documented + what's missing |
| `aria_audit` | Verdict + what's documented + what's missing |
| `screenreader_audit` | Verdict + what's documented + what's missing |
| `motion_sensitivity_audit` | Verdict + what's documented + what's missing |
| `touch_audit` | Verdict (if mobile) or "N/A — web only" |
| `a11y_overall_verdict` | PASS / GAP / MISSING |

---

## Fail Conditions

| Condition | Severity | Action |
|---|---|---|
| No inventories provided by parser | S2 | FLAG — mark all MISSING, proceed |
| Audit produces a creation/redesign suggestion | S1 | STOP — rewrite as gap description only |
| Audit tests actual color values or runs code | S1 | STOP — stay at design-level documentation review |

---

## Does Not Do

- Does not test actual contrast ratios (design-level review only)
- Does not create accessibility guidelines
- Does not run automated accessibility tests
- Does not benchmark (that's `benchmark-engine`'s job)
- Does not audit token values or component designs
- Does not review code or implementation
- Does not assign priority (that's `verdict-assembler`'s job)
- Does not look at other auditors' outputs

---

## Traceability

| This Skill Enforces | Authority |
|---|---|
| Per-item verdict (PASS/GAP/MISSING) | AGENT_SPEC Measure M1 |
| Coverage check — identifies missing a11y considerations | AGENT_SPEC Measure M4 |
| Findings trace to input or documented absence | AGENT_SPEC Measure M5, Constraint 7 |
| Review only — no creation | AGENT_SPEC Measure M8, Constraint 1 |
| Design level only — no code testing | AGENT_SPEC Measure M6, Constraint 3 |

---

*Accessibility Auditor — SKILL.md v1.0*

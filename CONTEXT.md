# System Lens — Shared Context
## Version 1.0

Implementation context per AGENT_STACK_FORMAT.md §5.3. No restatement per §2.

---

## Domain

- **Product:** Design system audits for Wiom (and general use) — reviewing design systems for completeness, consistency, and world-class benchmarking
- **Users:** Product leads, design leads, and UX engineers reviewing or building a design system — ensuring it meets world-class standards before scaling
- **Output:** Markdown audit report with checklist summary, per-area verdicts, benchmark comparisons, prioritized action list, and gaps — never a redesign or creation

---

## Current Versions

| Document | Version | Role |
|---|---|---|
| AGENT_STACK_FORMAT.md | v2.4 | SOURCE_OF_TRUTH |
| AGENT_SPEC.md | v1.0 | SOURCE_OF_TRUTH |
| SKILLS_INDEX.md | v1.0 | SOURCE_OF_TRUTH |
| CONTEXT.md | v1.0 | SOURCE_OF_TRUTH |
| input-parser/SKILL.md | v1.0 | SOURCE_OF_TRUTH |
| token-auditor/SKILL.md | v1.0 | SOURCE_OF_TRUTH |
| component-auditor/SKILL.md | v1.0 | SOURCE_OF_TRUTH |
| pattern-auditor/SKILL.md | v1.0 | SOURCE_OF_TRUTH |
| accessibility-auditor/SKILL.md | v1.0 | SOURCE_OF_TRUTH |
| benchmark-engine/SKILL.md | v1.0 | SOURCE_OF_TRUTH |
| verdict-assembler/SKILL.md | v1.0 | SOURCE_OF_TRUTH |

---

## Canonical Sources

| Data Type | Source File | Section |
|---|---|---|
| Objective + measures of success | AGENT_SPEC.md | INSTRUCTIONS — Objective, Measure of Success |
| Input manifest | AGENT_SPEC.md | INPUT |
| Output format | AGENT_SPEC.md | INSTRUCTIONS — Output Format |
| Constraints + kill conditions | AGENT_SPEC.md | INSTRUCTIONS — Constraints, Kill Conditions |
| Check table + severities | AGENT_SPEC.md | CHECKS |
| Skill execution order | SKILLS_INDEX.md | Execution Order |
| Conflict resolution rules | SKILLS_INDEX.md | Conflict Resolution |
| Re-run triggers | SKILLS_INDEX.md | Re-Run Triggers |
| Format definitions (all layers) | AGENT_STACK_FORMAT.md | §1–§12 |
| Run-time protocol | AGENT_STACK_FORMAT.md | §12 |
| JUDGMENT triggers + escalation | AGENT_STACK_FORMAT.md | §7 |
| Decision authority rule | AGENT_STACK_FORMAT.md | §12.4 |

---

## World-Class Design System Knowledge Base

This agent carries baked-in knowledge of world-class design systems, organized by tier. This knowledge is used by the `benchmark-engine` skill to produce specific, real comparisons.

### Tier 1 — Gold Standard (primary benchmarks)

| System | Company | Key Strengths |
|---|---|---|
| Carbon | IBM | Best-in-class documentation, WCAG AAA accessibility, layered token taxonomy (global → alias → component), detailed usage guidance per component |
| Polaris | Shopify | Content/copy guidelines embedded in component docs, strong opinionated merchant-facing patterns, clean token architecture |
| Primer | GitHub | Figma-to-code parity, accessibility rigor, open-source transparency, ViewComponent architecture |
| Material Design 3 | Google | Comprehensive token architecture, cross-platform coverage, robust motion guidelines, theming system |
| Apple HIG | Apple | Interaction pattern depth, platform-native guidance, accessibility-first principles, "when NOT to use" guidance |

### Tier 2 — Strong References

| System | Company | Learn From |
|---|---|---|
| Fluent | Microsoft | Adaptive theming, density controls, cross-platform story |
| Lightning | Salesforce | Multi-brand/theming support, enterprise density patterns |
| Spectrum | Adobe | Cross-platform theming, comprehensive component coverage |
| Paste | Twilio | Accessibility-first approach, inclusive design patterns |
| Radix | WorkOS | Headless primitives, best-in-class accessibility handling |
| Atlassian DS | Atlassian | Pragmatic documentation, iconography system |
| Linear | Linear | Craft, motion, visual polish, typographic restraint |
| Stripe | Stripe | Visual hierarchy, spacing system, typography scale |
| Airbnb | Airbnb | Responsive patterns, illustration system, motion |

### Tier 3 — Domain-Specific References

| System | Learn From |
|---|---|
| Ant Design | Massive component coverage (60+), data-heavy/enterprise UI |
| Vercel/Geist | Typographic restraint, minimal design philosophy |
| Base Web (Uber) | Open-source architecture, composability |
| Swiggy/Zomato | India-market mobile patterns, vernacular design |
| Groww | Fintech mobile UX for India market |
| Revolut/Ramp | Fintech density, data visualization patterns |
| Orbit (Kiwi.com) | Travel domain patterns |
| Evergreen (Segment) | Enterprise dashboard patterns |

### Common Traits of World-Class Systems (audit checklist)

1. **Layered token architecture** — global → alias → component tokens
2. **Usage guidance per component** — not just API docs but when/why to use
3. **Accessibility built in** — ARIA, keyboard, focus management as defaults
4. **Figma-code parity** — design and code components stay in sync
5. **Content/copy guidelines** — writing rules embedded in component docs
6. **Contribution model** — clear process for others to propose/extend
7. **Motion system** — easing, duration, and transition tokens
8. **Theming/density support** — multi-brand, adaptive layouts
9. **Composed patterns** — empty states, error flows, loading sequences
10. **Governance** — ownership, versioning, lifecycle management

### Common Failures to Check For

1. Documentation shows *what* but never *why*
2. No density/theming support
3. Figma-code drift within months
4. Components exist but no composed pattern guidance
5. Accessibility as afterthought
6. No motion system — transitions are ad hoc
7. Stale governance — no ownership, no contribution process

---

## Key Decisions Already Locked

| Decision | Source | Impact |
|---|---|---|
| Review only — no creation or redesign | Elicitation — human explicitly stated "only review, you will not create" | Agent must never produce design artifacts, only audit findings and suggestions |
| Design level only — no code review | Elicitation — human stated "for now lets keep it at design level" | Code review is out of scope unless human explicitly requests at run time |
| Baked-in knowledge AND optional reference input | Elicitation Q3 — human said "do both" | Agent carries world-class DS knowledge but also accepts specific systems to benchmark against |
| All measures apply (verdicts + priority list + benchmarks + coverage) | Elicitation Q5 — human said "all of the above" | All 4 audit dimensions always run; none are optional |
| Output = markdown report + checklist summary | Elicitation Q6 — human said "both" | Checklist at top, detailed report below |
| Agent name: System Lens | Elicitation — human chose from options | All files use "System Lens" as agent name |
| World-class benchmark list is not limited to user's initial list | Elicitation — human said "do not limit yourself to what I have named" | Agent learned from industry-wide best practices, tiered by quality |

---

## Completeness Checklist (for the agent at run time)

Before emitting final output, `verdict-assembler` must verify:

- [ ] Checklist Summary table present at top of output
- [ ] Every audited item has a verdict (PASS / GAP / MISSING)
- [ ] Prioritized action list present with priorities
- [ ] At least one benchmark comparison cites a specific world-class system
- [ ] Coverage check covers missing patterns, not just what's present
- [ ] Every finding traces to input or documented absence
- [ ] Zero creation/redesign in output
- [ ] Zero code review (unless human requested)
- [ ] Benchmark Summary table present
- [ ] Gaps / Ambiguities section present
- [ ] LOG.md logged every STRUCTURAL decision with AUTHORITY

---

*System Lens — Shared Context v1.0*

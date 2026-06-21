# Frontend Design Skill

A structured approach to frontend design and implementation for Claude Code — from understanding user needs to delivering production-quality Vue 3 + TypeScript components.

## Overview

This skill provides:

- **Design thinking** — Understanding users, flows, and architecture
- **Pragmatic implementation** — Working code, real components, shippable output
- **Systematic process** — Six phases from context gathering to code review

## Installation

Copy the `SKILL.md` and `references/` directory into your project's `.claude/skills/frontend-design-skill/` folder:

```
.claude/
└── skills/
    └── frontend-design-skill/
        ├── SKILL.md
        └── references/
            ├── vue3-patterns.md
            ├── design-tokens.md
            └── component-checklist.md
```

## Modes

| Mode | When to use | What it delivers |
|------|-------------|------------------|
| **Full Design** | New feature, no existing design, or major redesign | User flows → component architecture → visual design → code |
| **Design-to-Code** | User has wireframes, mockups, or detailed descriptions | Component tree → implementation with states |
| **Improvement** | Existing UI that needs better UX, a11y, or visual polish | Audit → recommendations → targeted fixes |

## Phases

1. **Context** — Understand the project, users, constraints, and tech stack
2. **Information Architecture** — Map user flows and define screen states
3. **Component Architecture** — Decompose UI into a typed component tree
4. **Visual Design** — Design tokens, layout patterns, responsive strategy
5. **Implementation** — Vue 3 + Composition API + TypeScript components
6. **Review** — States, responsiveness, a11y, consistency, code quality

## Tech Stack

- **Default**: Vue 3 + Composition API + TypeScript + `<script setup>`
- Adapts to whatever stack the project uses

## Reference Files

- `references/vue3-patterns.md` — Vue 3 Composition API patterns, composables, anti-patterns
- `references/design-tokens.md` — Complete design token system with dark mode
- `references/component-checklist.md` — Detailed checklist for component quality

## License

MIT

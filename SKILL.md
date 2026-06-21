---
name: my-fornitent
description: >
  Frontend UI/UX design and implementation skill. Use this skill when the user asks for
  UI design, UX design, component design, design system setup, design-to-code conversion,
  frontend implementation, page layout, UI refactoring, or any task involving visual
  interface creation. Triggers on mentions of: UI, UX, design, component, layout, page,
  interface, styling, design system, design tokens, wireframe, mockup, frontend, Vue,
  and similar terms — even if the user doesn't explicitly say "design."
---

# Frontend Design Skill

## Overview

This skill provides a structured approach to frontend design and implementation — from
understanding user needs to delivering production-quality Vue 3 + TypeScript components.
It adapts to your starting point: you might have a rough idea, an existing codebase,
a mockup, or just a problem to solve.

The skill balances **design thinking** (understanding users, flows, architecture) with
**pragmatic implementation** (working code, real components, shippable output). It
defaults to Vue 3 + Composition API + TypeScript but adapts to whatever stack the
project uses.

## When to use which approach

Before diving in, assess the user's starting point. The skill works in three modes:

| Mode | When to use | What it delivers |
|------|-------------|------------------|
| **Full Design** | New feature, no existing design, or major redesign | User flows → component architecture → visual design → code |
| **Design-to-Code** | User has wireframes, mockups, or detailed descriptions | Component tree → implementation with states |
| **Improvement** | Existing UI that needs better UX, a11y, or visual polish | Audit → recommendations → targeted fixes |

If the user doesn't specify, default to **Full Design** mode. If the project has
existing components or design tokens, always build on them rather than replacing them.

---

## Phase 1: Understand the context

Before writing any code, gather this information. Ask concisely — don't questionnaire
the user if the answers are obvious from context:

1. **What are we building?** — A page, a feature, a component, a design system?
2. **Who uses it?** — Internal tools, consumer-facing, admin panels?
3. **What's the current state?** — Greenfield, existing codebase, legacy UI?
4. **What are the constraints?** — Must use certain libraries? Performance targets?
   Browser/device requirements?
5. **What's the tech stack?** — Check `package.json`, existing components, config
   files. Default to Vue 3 + Composition API + TypeScript + `<script setup>`.

**Key principle:** Read existing code before writing new code. Use Glob to find
existing components, design tokens, and style files. Understand the project's
conventions before adding to them.

---

## Phase 2: Information Architecture & User Flow

For new features or pages, think through the structure before the visuals:

### User flow mapping

Describe the user's journey in plain language. What steps do they take? What
decisions do they make? What's the happy path vs. edge cases?

```
[Entry] → [Step 1: Browse] → [Step 2: Select] → [Step 3: Confirm] → [Done]
                ↓                    ↓
          [Empty state]        [Error state]
```

### Information hierarchy

- What's the most important information on each screen?
- What's secondary? What can be hidden behind interactions?
- How does the layout guide the user's eye?

### Screen/state inventory

List every state the UI needs to handle:
- **Loading** — First visit, subsequent loads, skeleton vs. spinner
- **Empty** — No data yet, no results, nothing selected
- **Error** — Network failure, validation errors, permission denied
- **Edge cases** — Very long text, missing images, extreme data values
- **Success** — Confirmation states, completion feedback

---

## Phase 3: Component Architecture

Decompose the UI into a component tree. This is the single most important design
decision — good component boundaries make everything else easier.

### Component decomposition principles

1. **Single responsibility** — Each component does one thing well
2. **Composable** — Small components combine into larger ones via slots
3. **Reusable** — Extract patterns; don't duplicate
4. **Testable** — Props in, events out; side effects at the edges

### Define the component API

For each component, sketch its interface in Vue 3 style:

```typescript
// Component: UserCard
// Responsibility: Display a user's avatar, name, and status
interface UserCardProps {
  user: User;              // The user object
  size?: 'sm' | 'md' | 'lg'; // Display size variant
  showStatus?: boolean;    // Whether to show online indicator
}
// Events: (click) - emitted when card is clicked
// Slots: #actions - slot for action buttons
```

### State management plan

- **Props** — Data flowing down from parent
- **Events** — Actions flowing up to parent
- **Local state** — UI-only state (open/closed, focused, hovered)
- **Store (Pinia)** — Shared application state (user data, auth, settings)
- **Composables** — Reusable logic (useDebounce, useMediaQuery, useFetch)

Read `references/vue3-patterns.md` for detailed Vue 3 patterns and anti-patterns.

---

## Phase 4: Visual Design

### Design tokens first

Before writing CSS, define the design tokens. If the project already has tokens,
extend them; don't replace them.

```css
/* Design tokens — the single source of truth for visual values */
:root {
  /* Colors */
  --color-primary: #3B82F6;
  --color-primary-hover: #2563EB;
  --color-primary-light: #DBEAFE;
  --color-danger: #EF4444;
  --color-success: #22C55E;
  --color-text: #111827;
  --color-text-secondary: #6B7280;
  --color-bg: #FFFFFF;
  --color-bg-secondary: #F9FAFB;
  --color-border: #E5E7EB;

  /* Typography */
  --font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --font-size-xs: 0.75rem;
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;
  --font-size-2xl: 1.5rem;

  /* Spacing */
  --spacing-1: 0.25rem;
  --spacing-2: 0.5rem;
  --spacing-3: 0.75rem;
  --spacing-4: 1rem;
  --spacing-6: 1.5rem;
  --spacing-8: 2rem;
  --spacing-12: 3rem;

  /* Borders & Shadows */
  --radius-sm: 0.25rem;
  --radius-md: 0.375rem;
  --radius-lg: 0.5rem;
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-md: 0 4px 6px rgba(0,0,0,0.1);
}
```

Read `references/design-tokens.md` for a complete token system with dark mode support.

### Layout patterns

Think in terms of common CSS layout patterns rather than pixel-pushing:
- **Stack** — Vertical spacing between elements (flexbox column + gap)
- **Grid** — Multi-column layouts (CSS Grid with responsive columns)
- **Sidebar** — Fixed sidebar + fluid content (flexbox or grid)
- **Centered** — Constrained width content (max-width + auto margins)

### Responsive strategy

Use a mobile-first approach with consistent breakpoints:
- **Default**: Mobile (320px+)
- **`md:`**: Tablet (768px+)
- **`lg:`**: Desktop (1024px+)
- **`xl:`**: Wide (1280px+)

Every component should work at every breakpoint. Don't hide critical information on
mobile — restructure it instead.

### Visual hierarchy checklist

- [ ] Primary action is visually dominant (color, size, position)
- [ ] Related information is grouped (proximity)
- [ ] Important text has sufficient contrast (≥ 4.5:1 for body, ≥ 3:1 for large text)
- [ ] Interactive elements look interactive (cursor, hover state, focus ring)
- [ ] Whitespace is used intentionally to separate sections

---

## Phase 5: Implementation

### Component template

Generate Vue 3 components using `<script setup lang="ts">` with this structure:

```vue
<script setup lang="ts">
// 1. Imports
// 2. Types & Interfaces
// 3. Props & Emits
// 4. Composables
// 5. Reactive state
// 6. Computed properties
// 7. Methods
// 8. Lifecycle hooks
// 9. Watchers
</script>

<template>
  <!-- Use semantic HTML, handle all states -->
</template>

<style scoped>
/* Scoped styles using design tokens */
</style>
```

### State handling is mandatory

Every component that deals with async data must handle these states:

```vue
<template>
  <div>
    <!-- LOADING: shown while data is being fetched -->
    <div v-if="loading" class="skeleton">
      <SkeletonCard />
    </div>

    <!-- ERROR: shown when the request failed -->
    <div v-else-if="error" class="error-state">
      <ErrorIcon />
      <p>{{ error.message }}</p>
      <button @click="retry">重试</button>
    </div>

    <!-- EMPTY: shown when data loaded but is empty -->
    <div v-else-if="!items.length" class="empty-state">
      <EmptyIcon />
      <p>暂无数据</p>
      <button v-if="canCreate" @click="create">创建第一个</button>
    </div>

    <!-- DATA: the actual content -->
    <ul v-else class="item-list">
      <li v-for="item in items" :key="item.id">
        <ItemCard :item="item" />
      </li>
    </ul>
  </div>
</template>
```

### Accessibility baseline

Every component must meet these minimum a11y requirements:
- **Keyboard navigation** — All interactive elements reachable via Tab, activatable via Enter/Space
- **Focus management** — Visible focus rings, focus trapped in modals, focus restored on close
- **Semantic HTML** — Use `<button>` for buttons, `<nav>` for navigation, `<main>` for content
- **Labels** — Every input has a visible label; icon-only buttons have `aria-label`
- **Alt text** — Images have meaningful `alt` attributes (or empty `alt=""` if decorative)
- **ARIA** — Use ARIA attributes when HTML semantics aren't enough (e.g., `aria-expanded`, `aria-haspopup`, `role="alert"`)

### Performance

- Use `v-memo` for expensive lists that rarely change
- Lazy load below-the-fold content with `defineAsyncComponent`
- Debounce search inputs and scroll handlers
- Use CSS `content-visibility: auto` for long lists
- Images: use `loading="lazy"` and appropriate sizes

---

## Phase 6: Review

After generating code, do a quick self-review:

1. **All states covered?** — Loading, empty, error, edge cases, success
2. **Responsive?** — Check mobile, tablet, desktop
3. **Accessible?** — Keyboard, screen reader, focus management
4. **Consistent with the design system?** — Tokens used, not hardcoded values
5. **Vue 3 idiomatic?** — `<script setup>`, composables, proper Props/Emits typing
6. **No dead code?** — Remove unused imports, styles, and placeholder content

Report what was implemented, what decisions were made, and any trade-offs or
alternatives the user should know about.

---

## Design system mode

When the user asks to build or extend a design system, focus on:

### Foundation tokens
- Colors (primary, neutral, semantic)
- Typography (scale, weights, line heights)
- Spacing scale
- Border radii
- Shadows / elevation
- Motion (durations, easings)
- Breakpoints

### Component primitives
Build from atoms upward:
1. **Atoms**: Button, Input, Label, Icon, Badge, Avatar
2. **Molecules**: FormField, SearchBar, Card, Modal, Toast
3. **Organisms**: Header, Sidebar, DataTable, Form

### Component API design
Every design system component needs:
- **Variants** — Visual variants (primary/secondary/danger/ghost)
- **Sizes** — Consistent size scale (sm/md/lg)
- **States** — Hover, focus, active, disabled, loading
- **Composition** — Slots for flexible content, not rigid prop-driven content

### Documentation
For each component, generate:
- Props table (name, type, default, description)
- Events table
- Slots table
- Usage examples for common scenarios
- Edge case notes

---

## Reference files

- `references/vue3-patterns.md` — Vue 3 Composition API patterns, composables, and anti-patterns
- `references/design-tokens.md` — Complete design token system with dark mode
- `references/component-checklist.md` — Detailed checklist for component quality

Read these when you need deeper guidance on a specific topic. Don't load them all
at once — pick the one relevant to the current phase.

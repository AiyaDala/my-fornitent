# Component Quality Checklist

Use this checklist to review every component before considering it done.
Not every item applies to every component — use judgment.

## States

- [ ] **Loading state** — Shown while data is being fetched or computed
  - First load: skeleton or spinner
  - Subsequent loads: subtle indicator (don't flash the whole UI)
  - If loading is < 200ms, don't show any indicator (avoid flicker)
- [ ] **Empty state** — Shown when data loaded successfully but is empty
  - Clear message explaining why it's empty
  - Call to action if applicable ("创建第一个项目")
  - Illustration or icon to soften the empty feeling
- [ ] **Error state** — Shown when loading or submission fails
  - Human-readable error message (not raw error codes)
  - Retry action if recoverable
  - Escape hatch if not ("联系支持" or "返回首页")
- [ ] **Edge cases**
  - Very long text (ellipsis or wrap decision)
  - Missing optional data (graceful degradation, not blank spaces)
  - Very large numbers (formatting, abbreviations)
  - Empty strings vs. `null` vs. `undefined`
  - Zero values (0 is not falsy-empty — show "0" not "暂无")

## Accessibility

- [ ] **Keyboard navigation**
  - All interactive elements are focusable (Tab/Shift+Tab)
  - Activatable with Enter and/or Space
  - No keyboard traps (focus can leave any element)
  - Modal: focus trapped inside, focus restored on close
- [ ] **Focus indicators**
  - Visible focus ring on all interactive elements
  - Focus ring uses `:focus-visible` (not `:focus`) — avoids rings on mouse clicks
  - Focus ring has ≥ 3:1 contrast against background
- [ ] **Semantic HTML**
  - `<button>` for buttons (not `<div onclick>`)
  - `<a>` for navigation (not `<span onclick="location=...">`)
  - `<nav>`, `<main>`, `<header>`, `<footer>` for landmark regions
  - `<form>` wrapping inputs with submit handling
  - `<ul>`/`<ol>` for lists
  - `<table>` (with `<thead>`, `<tbody>`) for tabular data
- [ ] **Labels & descriptions**
  - Every `<input>` has a visible `<label>` (or `aria-label` if truly icon-only)
  - Icon-only buttons have `aria-label` describing the action
  - Error messages linked via `aria-describedby`
- [ ] **Screen reader**
  - Dynamic content updates use `role="alert"` or `aria-live`
  - Decorative images have `alt=""` (empty, not missing)
  - Content images have descriptive `alt` text
  - Collapsible sections expose state via `aria-expanded`
  - Tabs use `role="tablist"`, `role="tab"`, `role="tabpanel"`
- [ ] **Color & contrast**
  - Text meets contrast minimums: 4.5:1 (body), 3:1 (large text ≥18px bold)
  - Color is never the sole indicator of state (add icons, text, or patterns)
  - UI works in forced-colors mode (Windows High Contrast)

## Responsiveness

- [ ] **Mobile (< 768px)**
  - Content fits without horizontal scroll
  - Touch targets ≥ 44×44px (WCAG 2.5.5)
  - Adequate spacing between touch targets (no accidental taps)
- [ ] **Tablet (768px – 1024px)**
  - Layout adapts to medium screen (2-column where desktop is 3-column, etc.)
- [ ] **Desktop (≥ 1024px)**
  - Content doesn't stretch infinitely (use `max-width` on containers)
  - Comfortable line length: 45–75 characters for body text
- [ ] **General**
  - Images use `srcset` or `max-width: 100%` to avoid overflow
  - Tables have horizontal scroll wrapper on mobile
  - Font sizes are readable at all breakpoints (never < 12px)
  - No fixed pixel widths on containers (use %, fr, or clamp)

## Performance

- [ ] **Rendering**
  - Large lists use `v-memo` or virtual scrolling
  - Expensive computed values are cached (computed is already cached)
  - Conditional content uses `v-if` (lazy) not `v-show` (eager) when rarely shown
- [ ] **Loading**
  - Code-split route-level components via `defineAsyncComponent`
  - Images use `loading="lazy"` for below-the-fold
  - Third-party scripts are loaded asynchronously
- [ ] **Interactions**
  - Event handlers that fire rapidly (scroll, resize, input) are debounced/throttled
  - Animations use `transform` and `opacity` (GPU-composited, no layout thrashing)
  - `@scroll` listeners use `{ passive: true }`

## Code quality

- [ ] **Props**
  - All props have TypeScript types
  - Required props marked as `required: true`
  - Defaults provided for optional props
  - Validators for constrained values (enums, ranges)
  - Complex objects use `PropType<T>` with an exported interface
- [ ] **Events**
  - All emitted events have typed payloads
  - Event names are kebab-case: `update:modelValue`, `item-select`
  - Mutations are communicated up; props are read-only down
- [ ] **Slots**
  - Named slots for customizable regions
  - Default slot for main content
  - Slot props documented when used (renderless patterns)
- [ ] **Styles**
  - Uses `scoped` styles
  - References design tokens (no hardcoded colors, spacing)
  - No `!important` unless overriding a third-party library
  - No deep selectors (`:deep()`) unless absolutely necessary
  - CSS order: layout → box model → typography → visual → motion
- [ ] **Cleanup**
  - `setInterval` / `addEventListener` cleaned up in `onUnmounted`
  - Async operations handle the "component unmounted mid-request" case
  - No memory leaks from retained references

## Design consistency

- [ ] Uses the project's design tokens (colors, spacing, typography)
- [ ] Follows existing component conventions (look at sibling components)
- [ ] Consistent naming: component file, CSS classes, props names
- [ ] Consistent interaction patterns (same gesture does the same thing everywhere)

## Documentation

For shared/reusable components, include a comment block at the top:

```vue
<!--
  Component: UserCard
  Description: Displays a user's avatar, name, email, and status indicator.
  Used in: UserList, ProjectMembers, AssigneePicker

  @prop {User} user - The user object to display
  @prop {'sm'|'md'|'lg'} [size='md'] - Card size variant
  @prop {boolean} [showStatus=true] - Whether to show online indicator

  @event {User} select - Emitted when card is clicked
  @event {string|number} remove - Emitted when remove action triggered

  @slot default - Override card body content
  @slot actions - Additional action buttons

  @example
  <UserCard :user="currentUser" size="lg" @select="handleSelect" />
-->
```

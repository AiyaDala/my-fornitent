# Design Tokens System

## Overview

Design tokens are the single source of truth for visual values. Every color, spacing
value, font size, shadow, and radius should reference a token — never a hardcoded value.

This reference provides a complete token system that can be used as-is or adapted to
match an existing project's design language.

## Color system

### Naming convention: `--color-{category}-{variant}`

Categories: `primary`, `neutral`, `danger`, `success`, `warning`, `info`

```css
:root {
  /* === Primary (brand color) === */
  --color-primary-50:  #EFF6FF;
  --color-primary-100: #DBEAFE;
  --color-primary-200: #BFDBFE;
  --color-primary-300: #93C5FD;
  --color-primary-400: #60A5FA;
  --color-primary-500: #3B82F6;  /* base */
  --color-primary-600: #2563EB;
  --color-primary-700: #1D4ED8;
  --color-primary-800: #1E40AF;
  --color-primary-900: #1E3A8A;

  /* === Neutral (grays) === */
  --color-neutral-50:  #F9FAFB;
  --color-neutral-100: #F3F4F6;
  --color-neutral-200: #E5E7EB;
  --color-neutral-300: #D1D5DB;
  --color-neutral-400: #9CA3AF;
  --color-neutral-500: #6B7280;
  --color-neutral-600: #4B5563;
  --color-neutral-700: #374151;
  --color-neutral-800: #1F2937;
  --color-neutral-900: #111827;

  /* === Semantic colors === */
  --color-danger-50:  #FEF2F2;
  --color-danger-500: #EF4444;
  --color-danger-600: #DC2626;

  --color-success-50:  #F0FDF4;
  --color-success-500: #22C55E;
  --color-success-600: #16A34A;

  --color-warning-50:  #FFFBEB;
  --color-warning-500: #F59E0B;
  --color-warning-600: #D97706;

  --color-info-50:  #EFF6FF;
  --color-info-500: #3B82F6;
  --color-info-600: #2563EB;
}
```

### Semantic token aliases

Map raw colors to semantic meanings. Components reference semantic tokens, not raw colors:

```css
:root {
  --color-text:            var(--color-neutral-900);
  --color-text-secondary:  var(--color-neutral-500);
  --color-text-disabled:   var(--color-neutral-400);
  --color-text-inverse:    #FFFFFF;

  --color-bg:              #FFFFFF;
  --color-bg-secondary:    var(--color-neutral-50);
  --color-bg-tertiary:     var(--color-neutral-100);
  --color-bg-disabled:     var(--color-neutral-200);

  --color-border:          var(--color-neutral-200);
  --color-border-hover:    var(--color-neutral-300);
  --color-border-focus:    var(--color-primary-500);

  --color-link:            var(--color-primary-600);
  --color-link-hover:      var(--color-primary-700);
}
```

## Typography

### Font family stack

```css
:root {
  --font-sans: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
               'Helvetica Neue', Arial, 'Noto Sans SC', sans-serif;
  --font-mono: 'SF Mono', 'Fira Code', 'Fira Mono', 'Roboto Mono',
               'Courier New', monospace;
}
```

### Type scale (using a 1.25 ratio)

```css
:root {
  --text-xs:   0.75rem;   /* 12px */
  --text-sm:   0.875rem;  /* 14px */
  --text-base: 1rem;      /* 16px */
  --text-lg:   1.125rem;  /* 18px */
  --text-xl:   1.25rem;   /* 20px */
  --text-2xl:  1.5rem;    /* 24px */
  --text-3xl:  1.875rem;  /* 30px */
  --text-4xl:  2.25rem;   /* 36px */

  --font-normal:  400;
  --font-medium:  500;
  --font-semibold: 600;
  --font-bold:    700;

  --leading-tight:  1.25;
  --leading-normal: 1.5;
  --leading-relaxed: 1.625;
}
```

## Spacing scale

Based on a 4px grid system:

```css
:root {
  --space-0:   0;
  --space-1:   0.25rem;  /* 4px */
  --space-2:   0.5rem;   /* 8px */
  --space-3:   0.75rem;  /* 12px */
  --space-4:   1rem;     /* 16px */
  --space-5:   1.25rem;  /* 20px */
  --space-6:   1.5rem;   /* 24px */
  --space-8:   2rem;     /* 32px */
  --space-10:  2.5rem;   /* 40px */
  --space-12:  3rem;     /* 48px */
  --space-16:  4rem;     /* 64px */
  --space-20:  5rem;     /* 80px */
}
```

Usage rules:
- Padding inside components: `--space-3` to `--space-6`
- Gaps between components: `--space-4` to `--space-8`
- Page-level spacing: `--space-12` to `--space-20`

## Borders & Radii

```css
:root {
  --radius-none: 0;
  --radius-sm:   0.125rem;  /* 2px  — checkboxes, tags */
  --radius-md:   0.375rem;  /* 6px  — buttons, inputs, cards */
  --radius-lg:   0.5rem;    /* 8px  — modals, dialogs */
  --radius-xl:   0.75rem;   /* 12px — large cards, panels */
  --radius-2xl:  1rem;      /* 16px — hero sections */
  --radius-full: 9999px;    /* pill — badges, avatars */

  --border-width: 1px;
  --border-width-md: 2px;
}
```

## Shadows / Elevation

```css
:root {
  --shadow-xs:  0 1px 2px  rgba(0, 0, 0, 0.05);
  --shadow-sm:  0 1px 3px  rgba(0, 0, 0, 0.1), 0 1px 2px rgba(0, 0, 0, 0.06);
  --shadow-md:  0 4px 6px  rgba(0, 0, 0, 0.07), 0 2px 4px rgba(0, 0, 0, 0.06);
  --shadow-lg:  0 10px 15px rgba(0, 0, 0, 0.1), 0 4px 6px rgba(0, 0, 0, 0.05);
  --shadow-xl:  0 20px 25px rgba(0, 0, 0, 0.1), 0 8px 10px rgba(0, 0, 0, 0.05);
}
```

### Semantic shadow mapping

```css
:root {
  --shadow-card:       var(--shadow-sm);
  --shadow-card-hover: var(--shadow-md);
  --shadow-dropdown:   var(--shadow-lg);
  --shadow-modal:      var(--shadow-xl);
  --shadow-toast:      var(--shadow-md);
}
```

## Motion tokens

```css
:root {
  --duration-instant: 0ms;
  --duration-fast:    150ms;
  --duration-normal:  200ms;
  --duration-slow:    300ms;

  --ease-default:     cubic-bezier(0.4, 0, 0.2, 1);
  --ease-in:          cubic-bezier(0.4, 0, 1, 1);
  --ease-out:         cubic-bezier(0, 0, 0.2, 1);
  --ease-in-out:      cubic-bezier(0.4, 0, 0.2, 1);
}
```

Usage:
- Hover effects: `--duration-fast`
- Page transitions: `--duration-slow`
- Micro-interactions (ripple, toggle): `--duration-instant`
- Default: `--duration-normal`

## Breakpoints

```css
/* Mobile-first breakpoints */
/* sm: not needed — mobile is the default */
/* @media (min-width: 768px)  — tablet */
/* @media (min-width: 1024px) — desktop */
/* @media (min-width: 1280px) — wide */

:root {
  --breakpoint-md:  768px;
  --breakpoint-lg:  1024px;
  --breakpoint-xl:  1280px;

  --container-sm: 640px;
  --container-md: 768px;
  --container-lg: 1024px;
  --container-xl: 1280px;
}
```

## Dark mode

Use CSS custom properties with a `[data-theme="dark"]` selector or `prefers-color-scheme`:

```css
/* Option A: Manual toggle */
[data-theme="dark"] {
  --color-text:            var(--color-neutral-100);
  --color-text-secondary:  var(--color-neutral-400);
  --color-text-disabled:   var(--color-neutral-600);

  --color-bg:              var(--color-neutral-900);
  --color-bg-secondary:    var(--color-neutral-800);
  --color-bg-tertiary:     var(--color-neutral-700);

  --color-border:          var(--color-neutral-700);
  --color-border-hover:    var(--color-neutral-600);
  --color-border-focus:    var(--color-primary-400);

  --shadow-xs:  0 1px 2px  rgba(0, 0, 0, 0.3);
  --shadow-sm:  0 1px 3px  rgba(0, 0, 0, 0.4);
  --shadow-md:  0 4px 6px  rgba(0, 0, 0, 0.5);
}

/* Option B: System preference */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    /* same dark tokens as above */
  }
}
```

**Key principle for dark mode:** Only change semantic tokens, not the base color palette.
Components that use `var(--color-bg)` and `var(--color-text)` automatically adapt.

## Applying tokens to components

Always reference tokens, never use raw values:

```css
/* BAD */
.my-button {
  background: #3B82F6;
  padding: 8px 16px;
  border-radius: 6px;
}

/* GOOD */
.my-button {
  background: var(--color-primary-500);
  padding: var(--space-2) var(--space-4);
  border-radius: var(--radius-md);
  transition: all var(--duration-fast) var(--ease-default);
}

.my-button:hover {
  background: var(--color-primary-600);
}
```

## Generating a color palette on the fly

When designing for a new brand without an existing color system, generate a palette
from a single base color using this method:

1. Pick the base color (the "500" level) — e.g., `#3B82F6`
2. Lighter shades (50–400): increase lightness, reduce saturation
3. Darker shades (600–900): decrease lightness, slight saturation shift
4. Ensure each shade has ≥ 3:1 contrast against its neighbors for distinguishable steps
5. The 500 → 600 step should be the most dramatic (it's the default hover state)

For neutral grays, start from a pure gray (`#6B7280` at 500) and shift cooler
(add slight blue) for lighter shades, warmer (add slight yellow) for darker shades.
This prevents the "concrete slab" look of pure grays.

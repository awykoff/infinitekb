# Style Guide — infinitekb.com

## Design Principles

- Clean and minimal — content is the focus, not decoration
- Dark mode first — every color must work in both modes
- Tailwind classes only — no inline styles, no CSS modules
- Consistent spacing — use the spacing scale, never arbitrary values
- Accessible — sufficient contrast, semantic HTML, keyboard navigable

---

## Typography

### Font Stack

```ts
// tailwind.config.ts
fontFamily: {
  sans: ['Inter', 'system-ui', 'sans-serif'],
  mono: ['JetBrains Mono', 'monospace'],
}
```

Load via `app/layout.tsx` using `next/font/google`:

```tsx
import { Inter, JetBrains_Mono } from 'next/font/google'

const inter = Inter({ subsets: ['latin'], variable: '--font-inter' })
const jetbrainsMono = JetBrains_Mono({ subsets: ['latin'], variable: '--font-mono' })
```

### Type Scale

| Element | Class | Size | Weight |
|---|---|---|---|
| Page title (h1) | `text-4xl font-bold` | 36px | 700 |
| Section heading (h2) | `text-2xl font-semibold` | 24px | 600 |
| Sub-heading (h3) | `text-xl font-semibold` | 20px | 600 |
| Body text | `text-base` | 16px | 400 |
| Small / meta | `text-sm` | 14px | 400 |
| Caption / label | `text-xs` | 12px | 400 or 500 |

### Prose (Article Body)

Use `@tailwindcss/typography` for MDX content:

```tsx
<article className="prose prose-gray dark:prose-invert max-w-none prose-headings:font-semibold prose-a:text-indigo-600 dark:prose-a:text-indigo-400 prose-code:text-indigo-600 dark:prose-code:text-indigo-400">
  {children}
</article>
```

---

## Color Palette

### Brand

| Token | Light | Dark | Usage |
|---|---|---|---|
| `brand-50` | `#eef2ff` | — | Subtle backgrounds |
| `brand-500` | `#6366f1` | — | Interactive elements |
| `brand-600` | `#4f46e5` | — | Hover states |
| `brand-900` | `#1e1b4b` | — | Dark backgrounds |

### Semantic Colors (Tailwind defaults)

| Purpose | Light class | Dark class |
|---|---|---|
| Page background | `bg-white` | `dark:bg-gray-950` |
| Surface / card | `bg-white` | `dark:bg-gray-900` |
| Subtle background | `bg-gray-50` | `dark:bg-gray-800` |
| Border | `border-gray-200` | `dark:border-gray-800` |
| Primary text | `text-gray-900` | `dark:text-white` |
| Secondary text | `text-gray-600` | `dark:text-gray-400` |
| Muted text | `text-gray-400` | `dark:text-gray-500` |
| Link / accent | `text-indigo-600` | `dark:text-indigo-400` |
| Link hover | `hover:text-indigo-700` | `dark:hover:text-indigo-300` |

### Status Colors

| Status | Background | Text |
|---|---|---|
| Info | `bg-blue-50 dark:bg-blue-900/20` | `text-blue-700 dark:text-blue-300` |
| Success | `bg-green-50 dark:bg-green-900/20` | `text-green-700 dark:text-green-300` |
| Warning | `bg-amber-50 dark:bg-amber-900/20` | `text-amber-700 dark:text-amber-300` |
| Danger | `bg-red-50 dark:bg-red-900/20` | `text-red-700 dark:text-red-300` |

---

## Spacing Scale

Always use Tailwind spacing tokens. Never use arbitrary values like `mt-[13px]`.

| Token | Value | Common use |
|---|---|---|
| `gap-2` / `p-2` | 8px | Tight icon gaps |
| `gap-3` / `p-3` | 12px | Badge padding |
| `gap-4` / `p-4` | 16px | Component internal padding |
| `gap-6` / `p-6` | 24px | Card padding |
| `gap-8` / `p-8` | 32px | Section internal spacing |
| `gap-12` | 48px | Between components |
| `mb-10` / `mt-10` | 40px | Section breaks |
| `mb-24` / `mt-24` | 96px | Major section separation |

---

## Layout

### Max Width

```tsx
// Page wrapper — use on every page
<div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">

// Article content — narrower for readability
<div className="max-w-3xl mx-auto px-4 sm:px-6">

// Hero / full bleed sections
<div className="w-full">
```

### Article Page Layout (two-column on desktop)

```tsx
<div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
  <div className="lg:grid lg:grid-cols-[1fr_280px] lg:gap-12">
    <main>{/* Article content */}</main>
    <aside className="hidden lg:block">
      {/* Table of contents, related articles */}
    </aside>
  </div>
</div>
```

---

## Component Patterns

### Card

```tsx
<div className="border border-gray-200 dark:border-gray-800 rounded-xl p-6 bg-white dark:bg-gray-900">
```

### Card with hover

```tsx
<div className="border border-gray-200 dark:border-gray-800 rounded-xl p-6 hover:border-gray-400 dark:hover:border-gray-600 transition-colors">
```

### Section divider

```tsx
<div className="border-b border-gray-200 dark:border-gray-800 pb-10 mb-10">
```

### Pill / Badge

```tsx
<span className="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-indigo-50 text-indigo-700 dark:bg-indigo-900/30 dark:text-indigo-300">
  Label
</span>
```

### Primary Button

```tsx
<button className="px-4 py-2 text-sm font-medium bg-indigo-600 hover:bg-indigo-700 text-white rounded-lg transition-colors">
  Button
</button>
```

### Secondary Button

```tsx
<button className="px-4 py-2 text-sm font-medium border border-gray-300 dark:border-gray-700 text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-800 rounded-lg transition-colors">
  Button
</button>
```

### Text Input

```tsx
<input
  type="text"
  className="w-full px-4 py-2 text-sm border border-gray-300 dark:border-gray-700 rounded-lg bg-white dark:bg-gray-900 text-gray-900 dark:text-white placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-indigo-500"
/>
```

---

## Dark Mode

Dark mode is enabled via Tailwind's `class` strategy. Set in `tailwind.config.ts`:

```ts
darkMode: 'class'
```

Toggle is controlled by adding/removing the `dark` class on `<html>`.
Every color class must have a `dark:` variant — no exceptions.

**Quick check:** For every `bg-`, `text-`, and `border-` class you write,
ask: does it have a `dark:` counterpart? If not, add one.

---

## Responsive Breakpoints

| Prefix | Min-width | Target |
|---|---|---|
| (none) | 0px | Mobile |
| `sm:` | 640px | Large mobile / small tablet |
| `md:` | 768px | Tablet |
| `lg:` | 1024px | Desktop |
| `xl:` | 1280px | Wide desktop |

Mobile-first always. Write the base class for mobile, then add `md:` and `lg:` overrides.

```tsx
// Correct — mobile first
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">

// Wrong — desktop first
<div className="grid grid-cols-3 gap-6">
```

---

## VegaNext Alignment

infinitekb.com shares visual language with veganext.com:

- Same font stack (Inter / system-ui)
- Same indigo accent color family
- Same card pattern (border + rounded-xl)
- Same nav/footer structure
- Same prose styling for article content

When building components for infinitekb.com that could be reused on
VegaNext, note it in a comment at the top of the component file:

```tsx
// Reusable: compatible with VegaNext insights section
```

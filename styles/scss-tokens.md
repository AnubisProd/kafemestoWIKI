## Nice setup in the Vite SCSS preprocessor options.

```scss
additionalData: `
  @use "@/assets/scss/tokens/colors";
  @use "@/assets/scss/abstracts/_mixins.scss" as *;
`,
```

This means **every SCSS file** in your project automatically gets access to:

- Your color design tokens (from `tokens/colors`)
- All your global mixins (from `abstracts/_mixins.scss`)

This is a very modern, clean way to build a small-to-medium design system. Let's break down both concepts in detail — what they are, why they matter, how they're typically structured, and best practices in 2025–2026.

### 1. SCSS Design Tokens (especially colors)

**Design tokens** are the smallest, most atomic pieces of your visual design language.

They are **named values** that represent a single design decision — usually stored as variables.

The main goal:
- **Single source of truth** — change once, everywhere updates
- **Consistency** across the whole app (and potentially across platforms if you ever export tokens to JSON)
- **Semantic naming** instead of raw values → easier to understand intent

#### Primitive vs Semantic Tokens (very common split in 2024–2026)

**Primitive tokens** — raw, foundational values (no meaning attached)

```scss
// tokens/_primitives.scss  (or directly in colors.scss)
$gray-100: #f8f9fa;
$gray-200: #e9ecef;
$gray-300: #dee2e6;
$blue-500:  #0d6efd;
$blue-600:  #0a58ca;
$success-500: #198754;
```

**Semantic tokens** — named by **purpose / context** (they usually reference primitives)

```scss
// tokens/colors.scss  (the file you're @using)
$color-background:      $gray-100;
$color-surface:         white;
$color-surface-elevated: $gray-50;

$color-text-primary:    #212529;
$color-text-secondary:  $gray-600;
$color-text-muted:      $gray-500;

$color-primary:         $blue-500;
$color-primary-hover:   $blue-600;
$color-primary-active:  darken($blue-500, 10%);
$color-success:         $success-500;
```

#### Why semantic > raw hex codes?

```scss
// Bad (2020 style)
button {
  background: #0d6efd;
  border-color: #0a58ca;
  color: white;
}

// Good (2025 style)
button {
  background: $color-primary;
  border-color: $color-primary-hover;
  color: $color-text-inverted;   // usually white or near-black
}
```

- Theme switch (dark mode)? Just reassign semantic tokens.
- Brand refresh? Change primitives, semantics stay the same.
- Designer ↔ developer communication improves dramatically.

#### Typical modern folder structure (matches your project)

```
assets/
  scss/
    tokens/
      _colors.scss          ← semantic + maybe primitives
      _spacing.scss
      _typography.scss
      _shadows.scss
      _radii.scss
    abstracts/
      _mixins.scss
      _functions.scss
    vendors/
    base/
    components/
    layouts/
    main.scss             ← usually just @forward or @use everything
```

### 2. SCSS Mixins

A **mixin** is a reusable block of styles that can accept arguments (like a function for CSS).

Think of it as "copy-paste code with parameters" — but centralized and maintainable.

#### Basic syntax

```scss
// abstracts/_mixins.scss

@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

@mixin respond-to($breakpoint) {
  @if $breakpoint == mobile {
    @media (max-width: 767px) { @content; }
  } @else if $breakpoint == tablet {
    @media (min-width: 768px) and (max-width: 991px) { @content; }
  } @else if $breakpoint == desktop {
    @media (min-width: 992px) { @content; }
  }
}

@mixin button-variant($bg, $text: white, $hover: scale-color($bg, $lightness: -15%)) {
  background: $bg;
  color: $text;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: $border-radius-md;

  &:hover {
    background: $hover;
  }
}
```

#### Usage (anywhere in your project — thanks to additionalData)

```scss
.card {
  @include flex-center;
  flex-direction: column;
  gap: 1.5rem;
  padding: 2rem;
  background: $color-surface-elevated;
  border-radius: $radius-lg;
  box-shadow: $shadow-md;

  @include respond-to(mobile) {
    padding: 1.25rem;
  }
}

.btn-primary {
  @include button-variant($color-primary);
}

.btn-success {
  @include button-variant($color-success, $text: #fff);
}
```

### Best Practices in 2025–2026 (SCSS + modern frontend)

| Aspect              | Recommendation                                                                 | Why / Notes |
|---------------------|--------------------------------------------------------------------------------|-------------|
| **Naming**          | `kebab-case` or `camelCase` — be consistent (`button-primary`, `flex-center`)  | Readability |
| **When to mixin**   | Complex/repeated patterns (flex setups, button families, responsive helpers)   | Avoid for single properties — use variables instead |
| **Avoid overuse**   | Don't make mixins for everything (e.g. no `@mixin margin($m)` for 1 line)      | Increases abstraction debt |
| **Content block**   | Use `@content` for responsive, state variants, etc.                            | Very powerful |
| **Default args**    | Always provide sensible defaults                                               | Better DX |
| **Combine with CSS vars** | Output mixins to CSS variables when possible (dark mode, theming)             | Future-proof |
| **Layering**        | Prefer modern `@layer` + CSS nesting over heavy nesting in SCSS                | SCSS nesting can create specificity issues |
| **Documentation**   | Comment mixins with expected args & output                                     | Team scalability |

### Quick Comparison Table: Tokens vs Mixins

| Feature              | Design Tokens                          | Mixins                                      |
|----------------------|----------------------------------------|---------------------------------------------|
| Main purpose         | Store **values** (colors, sizes…)      | Store **behavior / style chunks**           |
| Reassignment         | Easy (change one place)                | Harder (logic inside)                       |
| Arguments            | No                                     | Yes (very flexible)                         |
| Output               | Variables                              | CSS rules (can include variables)           |
| Typical location     | `tokens/` folder                       | `abstracts/_mixins.scss`                    |
| Best for             | Consistency, theming                   | DRY complex patterns                        |

In your project, this combination (`@use` tokens + `@use *` mixins globally) is **excellent** for a cafe-style website — keeps colors consistent (menu items, hero, buttons) and lets you quickly create responsive layouts or card patterns without repetition.

If you share the actual content of `colors.scss` or `_mixins.scss`, I can give even more targeted feedback!

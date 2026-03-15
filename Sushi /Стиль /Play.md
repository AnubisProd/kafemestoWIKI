---
created: 2026-03-15T14:33:33+03:00
modified: 2026-03-15T14:33:33+03:00
---

# Play

## 📁 Структура SCSS (ITCSS + Tokens + Utilities)

```scss
assets/
  scss/
    main.scss
    tokens/                ← Design tokens (CSS variables)
      _colors.scss
      _spacing.scss
      _typography.scss
      _radius.scss
      _shadows.scss
      _index.scss

    abstracts/             ← Функции, миксины, брейкпоинты
      _functions.scss
      _mixins.scss
      _breakpoints.scss
      _index.scss

    base/                  ← Базовые стили
      _global.scss
      _reset.scss
      _index.scss

    layout/                ← Макеты
      _wrapper.scss
      _main.scss
      _index.scss

    components/            ← Компоненты
      _header.scss
      _footer.scss
      _button.scss
      _card.scss
      _index.scss

    utilities/             ← Автогенерируемые утилиты
      _spacing.scss
      _colors.scss
      _display.scss
      _flex.scss
      _grid.scss
      _radius.scss
      _shadow.scss
      _transition.scss
      _index.scss

    pages/                 ← Стили страниц
      _home.scss
      _about.scss
      _index.scss
      
 
````
  
### обновлённый порядок ITCSS `main.scss`


-  **Tokens** — фундамент. 
`@use "tokens/index";`
- **Abstracts** — функции, миксины, брейкпоинты. 
`@use "abstracts/index";`
- **Base** — reset, global. 
`@use "base/index";`
- **Utilities** — автогенерация классов. 
`@use "utilities/index";`
- **Layout** — структура страницы. 
`@use "layout/index";`
- **Components** — UI‑элементы. 
`@use "components/index";`
- **Pages** — локальные стили страниц. 
`@use "pages/index";`



### 📁 tokens/ 

`_colors.scss`

````scss
:root {
  --color-primary: ![](color://efefd2) #efefd2;
  --color-primary-hover: ![](color://cfbe91) #cfbe91;
  --color-primary-dark: ![](color://bfae80) #bfae80;

  --color-bg: ![](color://0a0b0a) #0a0b0a;
  --color-surface: #111;
  --color-surface-hover: ![](color://1a1a1a) #1a1a1a;

  --color-text: ![](color://ffffff) #ffffff;
  --color-text-muted: ![](color://818181) #818181;
  --color-text-inverse: ![](color://0a0b0a) #0a0b0a;

  --color-border: ![](color://333330) #333330;
  --color-border-hover: ![](color://4a4a4a) #4a4a4a;
}
````

`_spacing.scss`

````scss
:root {
  --space-xs: .5rem;
  --space-sm: 1rem;
  --space-md: 1.5rem;
  --space-lg: 2.5rem;
  --space-xl: 4rem;
}
````

`_typography.scss`

````scss
:root {
  --font-heading: "Forum-Regular", serif;
  --font-sans: "Satoshi-Regular", system-ui, sans-serif;

  --fs-root: 16px;
  --fs-h1: clamp(2.5rem, 5vw + 1rem, 3.5rem);
  --fs-h2: clamp(2rem, 4vw + .5rem, 3rem);
  --fs-h3: clamp(1.5rem, 3vw + .5rem, 2rem);
  --fs-base: 1rem;

  --lh-heading: 1.1;
  --lh-body: 1.6;
}
````

`_radius.scss`

````scss
:root {
  --radius-sm: 6px;
  --radius-md: 12px;
  --radius-full: 999px;
}
````

`_shadows.scss`

````scss
:root {
  --shadow-sm: 0 2px 6px rgba(0,0,0,.15);
  --shadow-md: 0 4px 12px rgba(0,0,0,.2);
}
````

---

📁 abstracts/

_functions.scss

`scss
@function rem($px) {
  @return calc($px / 16) * 1rem;
}

@function color($name) {
  @return var(--color-#{$name});
}
`

_mixins.scss

`scss
@mixin heading($level) {
  font-family: var(--font-heading);
  line-height: var(--lh-heading);
  font-size: var(--fs-h#{$level});
}

@mixin focus-ring {
  &:focus-visible {
    outline: 2px solid var(--color-primary);
    outline-offset: 2px;
  }
}

@mixin hover-lift($y: -3px) {
  transition: transform var(--transition), box-shadow var(--transition);
  &:hover {
    transform: translateY($y);
    box-shadow: var(--shadow-md);
  }
}

@mixin container {
  width: 100%;
  max-width: 1440px;
  margin-inline: auto;
  padding-inline: var(--space-sm);
}
`

_breakpoints.scss

`scss
$breakpoints: (
  sm: 640px,
  md: 768px,
  lg: 1024px,
  xl: 1280px
);

@mixin mq($key, $type: min) {
  @media (#{$type}-width: map-get($breakpoints, $key)) {
    @content;
  }
}
`

---

📁 utilities/ (обновлено + генерация цветов)

_colors.scss (новый файл)

`scss
$colors: (
  primary: var(--color-primary),
  primary-hover: var(--color-primary-hover),
  primary-dark: var(--color-primary-dark),

  bg: var(--color-bg),
  surface: var(--color-surface),
  surface-hover: var(--color-surface-hover),

  text: var(--color-text),
  text-muted: var(--color-text-muted),
  text-inverse: var(--color-text-inverse),

  border: var(--color-border),
  border-hover: var(--color-border-hover)
);

@each $name, $value in $colors {
  .bg-#{$name} { background-color: #{$value}; }
  .text-#{$name} { color: #{$value}; }
  .border-#{$name} { border-color: #{$value}; }
}
`

_spacing.scss (автогенерация)

`scss
$spaces: (
  xs: var(--space-xs),
  sm: var(--space-sm),
  md: var(--space-md),
  lg: var(--space-lg),
  xl: var(--space-xl)
);

@each $key, $value in $spaces {
  .m-#{$key} { margin: $value; }
  .p-#{$key} { padding: $value; }
  .gap-#{$key} { gap: $value; }
}
`

_display.scss

`scss
.flex { display: flex; }
.grid { display: grid; }
.block { display: block; }
.hidden { display: none; }
`

_flex.scss

`scss
.flex-center { display: flex; align-items: center; justify-content: center; }
.flex-between { display: flex; justify-content: space-between; align-items: center; }
.flex-col { display: flex; flex-direction: column; }
`

_grid.scss

`scss
.grid-2 { display: grid; grid-template-columns: repeat(2, 1fr); }
.grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); }
.grid-auto { display: grid; grid-auto-flow: column; gap: var(--space-md); }
`

_radius.scss

`scss
$radius: (
  sm: var(--radius-sm),
  md: var(--radius-md),
  full: var(--radius-full)
);

@each $key, $value in $radius {
  .radius-#{$key} { border-radius: $value; }
}
`

_shadow.scss

`scss
$shadows: (
  sm: var(--shadow-sm),
  md: var(--shadow-md)
);

@each $key, $value in $shadows {
  .shadow-#{$key} { box-shadow: $value; }
}
`

_transition.scss

`scss
.transition { transition: var(--transition); }
.transition-fast { transition: .15s ease; }
`

---

📁 base/

_global.scss

`scss
html {
  font-size: var(--fs-root);
  scroll-behavior: smooth;
}

body {
  background: var(--color-bg);
  color: var(--color-text);
  font-family: var(--font-sans);
  line-height: var(--lh-body);
}
`

_reset.scss

`scss
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
`

---

📁 layout/

_wrapper.scss

`scss
.wrapper {
  @include container;
}
`

_main.scss

`scss
main {
  flex-grow: 1;
  padding: var(--space-lg) 0;
}
`

---

📁 components/

_header.scss

`scss
.header {
  @include container;
  padding: var(--space-md) 0;

  display: flex;
  justify-content: space-between;
  align-items: center;

  .logo { font-size: var(--fs-h3); }
  nav a { margin-left: var(--space-md); }
}
`

_footer.scss

`scss
.footer {
  padding: var(--space-md) 0;
  text-align: center;
  color: var(--color-text-muted);
}
`

---

📁 pages/

_home.scss

`scss
.page-home .hero {
  text-align: center;
  padding: var(--space-xl) 0;
}
`

_about.scss

`scss
.page-about {
  max-width: 800px;
  margin-inline: auto;
  padding: var(--space-lg) 0;
}
`

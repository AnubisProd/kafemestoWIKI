---
created: 2026-03-15T13:48:57+03:00
modified: 2026-03-15T13:52:41+03:00
---

# README—SCSS‑архитектура проекта

# SCSS Aрхитектура проекта– README📘



Структура оптимизирована для Nuxt, Vite, Tailwind‑совместимости и масштабируемых дизайн‑систем.

 

Структура директорий

 
assets/
  scss/
    main.scss

    tokens/                ← Design tokens (CSS custom properties)
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

    layout/                ← Макеты и структура страницы
      _wrapper.scss
      _main.scss
      _index.scss

    components/            ← UI‑компоненты
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

    pages/                 ← Локальные стили страниц
      _home.scss
      _about.scss
      _index.scss
 

 

Порядок подключения (ITCSS)

main.scss подключает слои строго в порядке от абстрактного к конкретному:

`scss
/ 1. Tokens — фундамент /
@use "tokens/index";

/ 2. Abstracts — функции, миксины, брейкпоинты /
@use "abstracts/index";

/ 3. Base — reset, global /
@use "base/index";

/ 4. Utilities — автогенерация классов /
@use "utilities/index";

/ 5. Layout — структура страницы /
@use "layout/index";

/ 6. Components — UI‑элементы /
@use "components/index";

/ 7. Pages — локальные стили страниц /
@use "pages/index";
`

 

Принципы архитектуры

Design Tokens
Все визуальные значения (цвета, spacing, типографика, радиусы, тени) хранятся в tokens/ и доступны через CSS‑переменные.

Abstracts
Содержат только логику: функции, миксины, брейкпоинты.
Никаких стилей.

Base
Глобальные правила: reset, html, body, типографика.

Utilities
Автоматически генерируемые классы:

m-sm, p-lg, gap-md
bg-primary, text-muted, border-surface
flex-center, grid-3
radius-md, shadow-sm
transition, transition-fast

Layout
Макеты: wrapper, main, сетки.

Components
Стили UI‑компонентов: кнопки, карточки, хедер, футер.

Pages
Локальные стили страниц.

 

Правила разработки

Никаких “сырых” значений — только токены.
Никаких медиа‑запросов вручную — только @include mq(md).
Компоненты не переопределяют токены, только используют.
Утилиты генерируются автоматически, не пишутся вручную.
Страницы содержат только локальные стили, не глобальные.
Порядок ITCSS обязателен — иначе каскад ломается.

 

🧩 Диаграмма ITCSS‑слоёв

 ▲ │ 7. Pages (локальные стили страниц) │ │ 6. Components (UI‑компоненты) │ │ 5. Layout (структура страницы) │ │ 4. Utilities (автогенерируемые классы) │ │ 3. Base (reset, global) │ │ 2. Abstracts (mixins, functions, breakpoints) │ │ 1. Tokens (design tokens — фундамент) │ └───────────────────────────────────────────────▶ Специфичность ↑ 

Чем выше слой — тем более конкретные стили.
Чем ниже слой — тем больше абстракции и переиспользуемости.

 

🎨 Визуальная карта токенов

Цвета

 
 
 
 
 
 
 
 
 
 
 
 

 

Типографика

 
 
 
 
 
 
 
 
 

 

Spacing

 
 
 
 
 
 

 

Радиусы

 
 
 
 

 

Тени

 
 
 

 

Если хочешь, могу собрать:

PDF‑версию документации
Figma‑страницу с токенами
Готовый шаблон для Storybook

Что из этого тебе пригодится?

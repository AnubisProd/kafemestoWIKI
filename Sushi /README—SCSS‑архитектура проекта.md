# README — SCSS‑архитектура проекта

## SCSS архитектура проекта

> 📘 Документ описывает структуру SCSS‑слоя проекта, основанную на принципах **ITCSS**, **Design Tokens**, атомарных миксинах и автоматической генерации утилит.
>
> Структура оптимизирована для **Nuxt**, **Vite**, **Tailwind**‑совместимости и масштабируемых дизайн‑систем.

---

## 📁 Структура директорий

```scss
assets/
  scss/
    main.scss

    tokens/                    ← Design tokens (CSS custom properties)
      _colors.scss
      _spacing.scss
      _typography.scss
      _radius.scss
      _shadows.scss
      _index.scss

    abstracts/                 ← Функции, миксины, брейкпоинты
      _functions.scss
      _mixins.scss
      _breakpoints.scss
      _index.scss

    base/                      ← Базовые стили
      _global.scss
      _reset.scss
      _index.scss

    layout/                    ← Макеты и структура страницы
      _wrapper.scss
      _main.scss
      _index.scss

    components/                ← UI‑компоненты
      _header.scss
      _footer.scss
      _button.scss
      _card.scss
      _index.scss

    utilities/                 ← Автогенерируемые утилиты
      _spacing.scss
      _colors.scss
      _display.scss
      _flex.scss
      _grid.scss
      _radius.scss
      _shadow.scss
      _transition.scss
      _index.scss

    pages/                     ← Локальные стили страниц
      _home.scss
      _about.scss
      _index.scss
```

---

## 🔄 Порядок подключения (ITCSS)

`main.scss` подключает слои **строго в порядке** от абстрактного к конкретному:

```scss
/* 1. Tokens — фундамент */
@use "tokens/index";

/* 2. Abstracts — функции, миксины, брейкпоинты */
@use "abstracts/index";

/* 3. Base — reset, global */
@use "base/index";

/* 4. Utilities — автогенерация классов */
@use "utilities/index";

/* 5. Layout — структура страницы */
@use "layout/index";

/* 6. Components — UI‑элементы */
@use "components/index";

/* 7. Pages — локальные стили страниц */
@use "pages/index";
```

---

## 📋 Принципы архитектуры

### **Design Tokens**
Все визуальные значения (цвета, spacing, типографика, радиусы, тени) хранятся в `tokens/` и доступны через CSS‑переменные.

### **Abstracts**
- Содержат только логику: функции, миксины, брейкпоинты
- **Никаких стилей!**

### **Base**
- Глобальные правила: reset, html, body, типографика

### **Utilities**
Автоматически генерируемые классы:
- `m-sm`, `p-lg`, `gap-md` (spacing)
- `bg-primary`, `text-muted`, `border-surface` (colors)
- `flex-center`, `grid-3` (layout)
- `radius-md`, `shadow-sm` (effects)
- `transition`, `transition-fast` (animations)

### **Layout**
- Макеты: wrapper, main, сетки

### **Components**
- Стили UI‑компонентов: кнопки, карточки, хедер, футер

### **Pages**
- Локальные стили страниц

---

## ⚙️ Правила разработки

✅ **ДЕЛАЙ:**
- Только токены — никаких "сырых" значений
- Медиа‑запросы через `@include mq(md)`
- Компоненты используют токены без переопределения
- Утилиты генерируются автоматически

❌ **НЕ ДЕЛАЙ:**
- Хардкод цветов и размеров
- Ручные медиа‑запросы
- Компоненты, переопределяющие токены
- Ручное написание утилит
- Глобальные стили в Pages
- Нарушение порядка ITCSS

---

## 🧩 Диаграмма ITCSS‑слоёв

```
      ▲
      │ 7. Pages (локальные стили страниц)
      │ 6. Components (UI‑компоненты)
      │ 5. Layout (структура страницы)
      │ 4. Utilities (автогенерация классов)
      │ 3. Base (reset, global styles)
      │ 2. Abstracts (функции, миксины)
      │ 1. Tokens (Design Tokens — CSS переменные)
      │
      └─────────────────────────────────
     
⬆️ Специфичность и конкретность
⬇️ Переиспользуемость и абстракция
```

**Правило:** Чем выше слой — тем конкретнее стили.

---

## 🎨 Визуальная карта токенов

### Цвета
| Название | Применение |
|----------|-----------|
| `primary` | Основной бренд-цвет |
| `secondary` | Акценты, альтернатива |
| `success` | Успешные действия |
| `warning` | Предупреждения |
| `error` | Ошибки |
| `surface` | Фоны, карточки |
| `muted` | Второстепенный текст |

### Типографика
| Размер | Применение |
|--------|-----------|
| `h1` - `h6` | Заголовки |
| `body-lg` | Основной текст |
| `body-sm` | Вспомогательный текст |
| `caption` | Подписи, мелкий текст |

### Spacing
- `xs`, `sm`, `md`, `lg`, `xl`, `2xl`

### Радиусы
- `none`, `sm`, `md`, `lg`, `full`

### Тени
- `sm`, `md`, `lg`

---

## 📦 Что дальше?

Я могу подготовить:

- 📄 **PDF‑версию** полной документации
- 🎨 **Figma‑страницу** с актуальными токенами
- 🧩 **Готовый шаблон** для Storybook
- 🔧 **SCSS‑файлы** для быстрого старта

**Что из этого тебе пригодится?**

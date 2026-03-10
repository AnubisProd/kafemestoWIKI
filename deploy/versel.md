# Деплой проекта на **Vercel**. 

> Vercel — один из самых простых и быстрых вариантов для Nuxt 3:
часто работает с **нулевой конфигурацией**,
особенно если у тебя SSR/hybrid-режим (по умолчанию в Nuxt 3).

---

<picture>
 <source media="(prefers-color-scheme: dark)" srcset="YOUR-DARKMODE-IMAGE">
 <source media="(prefers-color-scheme: light)" srcset="YOUR-LIGHTMODE-IMAGE">
 <img alt="YOUR-ALT-TEXT" src="YOUR-DEFAULT-IMAGE">
</picture>


<div style='width: 300px', margin: 0 auto>## `#0969DA` Pекомендуемый способ (zero-config / почти zero-config)</div>


1. **Подготовь репозиторий**  
   - Убедись, что твой проект лежит на **GitHub** (или GitLab/Bitbucket).  
   - Твой `package.json` должен иметь скрипты:
     ```json
     "scripts": {
       "dev": "nuxt dev",
       "build": "nuxt build",
       "generate": "nuxt generate",
       "preview": "nuxt preview"
     }
     ```
     (у тебя они уже есть — стандартный Nuxt starter)  
   - Сделай `git push` в main-ветку (или ту, которую хочешь деплоить).

2. **Зайди в Vercel**  
   - Перейди на https://vercel.com  
   - Войди через GitHub (рекомендую — тогда автоматом увидит репозитории).  
   - Нажми **Add New → Project**.

3. **Импортируй репозиторий**  
   - Выбери свой **kafemestoyalta** из списка.  
   - Vercel автоматически определит, что это **Nuxt** (обычно пишет «Nuxt» или «Nuxt 3»).  
   - Если не определил — вручную выбери **Framework Preset → Nuxt.js**.

4. **Настройки проекта (обычно ничего менять не надо)**  
   Vercel сам применяет правильные дефолты для Nuxt 3:

   | Поле              | Рекомендуемое значение                  | Почему / комментарий |
   |-------------------|-----------------------------------------|------------------------|
   | Framework Preset  | Nuxt.js (авто)                          | — |
   | Build Command     | `nuxt build` (или пусто — Vercel сам поставит) | Для SSR / hybrid |
   | Output Directory  | `.vercel/output` или `.output` (авто)   | Nitro генерирует сюда |
   | Install Command   | `pnpm install` / `npm install` / etc.   | Vercel видит package manager из lock-файла |
   | Root Directory    | `/` (по умолчанию)                      | Если проект не в подпапке |

   - **Environment Variables** — добавь здесь, если есть (например, API-ключи, NUXT_PUBLIC_… переменные).  
     Формат: `KEY=value` (можно импортировать из .env).

5. **Нажми Deploy**  
   - Через 30–90 секунд получишь ссылку вида:  
     `https://kafemestoyalta-abc123.vercel.app`  
   - Каждый новый `git push` в main → автоматический деплой.

### Если хочешь полностью статический сайт (SSG / nuxt generate)

- В **Build Command** укажи:  
  `nuxt generate`  
- Output Directory: `dist` (или `.output/public` — зависит от версии Nuxt)  
- Vercel будет раздавать статические файлы через CDN → очень быстро и дешево.

Но для твоего кафе-сайта (с меню, about, возможно позже форма/карта) **SSR или hybrid** обычно лучше — оставь `nuxt build`.

### Полезные улучшения в nuxt.config.ts (добавь, если ещё нет)

```ts
export default defineNuxtConfig({
  // ... твои текущие настройки

  nitro: {
    // preset: 'vercel'   ← можно добавить, но в 2026 часто не обязательно (авто)
  },

  // Для SEO и красивых ссылок
  app: {
    head: {
      title: 'Кафе «Место» Ялта',
      meta: [
        { name: 'description', content: 'Уютное кафе в центре Ялты. Свежая еда, вид на море.' }
      ]
    }
  },

  // Пример routeRules — полезно для ISR или кеширования
  routeRules: {
    // Главная и меню — статически с регенерацией каждые 10 минут
    '/': { prerender: true, isr: 600 },
    '/menu': { prerender: true, isr: 600 },
    // API-роуты (если добавишь) — SSR всегда
    '/api/**': { cache: { maxAge: 0 } }
  }
})
```

ISR (Incremental Static Regeneration) — отличная штука: страницы генерируются статически, но обновляются в фоне при трафике.

### Частые проблемы и как их фиксить (2025–2026)

- **Деплой висит в "pending" > 10–45 мин** → бывает с большими зависимостями или Nuxt 4.x / Nitro 2.1x.  
  Решение: попробуй перезапустить деплой или уменьшить node_modules (удали node_modules и lock, потом pnpm install заново).

- **500 ошибка или fetch возвращает HTML вместо JSON** → обычно API-роуты (/server/api) не работают.  
  Проверь: пути начинаются с `/api/…`, используй `event.req` правильно.

- **Сайт не видит public/ изображения после деплоя** → убедись, что пути относительные (`/images/hero.jpg`) или используй `~/assets/…` + @nuxt/image.

- **Переменные окружения не подхватываются** → добавляй с префиксом `NUXT_PUBLIC_` для клиент-сайда, без — только сервер.

### Следующие шаги после первого деплоя

- Привяжи **свой домен** (kafemestoyalta.ru / .com) → в настройках Vercel → Domains.
- Включи **Automatic Redeploys** на каждые push.
- Добавь **Preview Deployments** — каждый PR будет иметь свою ссылку для теста.
- Посмотри логи в Vercel Dashboard → Functions / Build Logs.

Если после деплоя что-то сломается — пришли ссылку на Vercel-проект или логи ошибки, помогу разобрать.

Удачного деплоя! Через 5–10 минут твой сайт с Ялтинским кафе уже может быть в интернете. 🚀

---
created: 2026-03-15T14:07:03+03:00
modified: 2026-03-15T14:18:26+03:00
---

# Конфигурация Nuxt

`**Nuxt.config.ts**`
````typescript

// nuxt.config.ts
export default defineNuxtConfig({
  devtools: { enabled: true },
  
  // Модули
  modules: [
    '@nuxt/icon',
    '@nuxt/fonts',
    '@vueuse/motion/nuxt',
    '@pinia/nuxt',
  ],
  
  // CSS
  css: [
    '~/assets/scss/global.scss'
  ],
  
  // SCSS глобальные переменные
  vite: {
    css: {
      preprocessorOptions: {
        scss: {
          additionalData: `
            @use "~/assets/scss/_variables.scss" as *;
            @use "~/assets/scss/_mixins.scss" as *;
          `
        }
      }
    }
  },
  
  // Шрифты
  fonts: {
    families: [
      { name: 'Inter', provider: 'google' },
      { name: 'Playfair Display', provider: 'google' }
    ]
  },
  
  // Конфигурация приложения
  app: {
    head: {
      title: 'foodbar - Healthy Food Delivery',
      meta: [
        { charset: 'utf-8' },
        { name: 'viewport', content: 'width=device-width, initial-scale=1' },
        { name: 'description', content: 'Fresh and healthy food delivery' }
      ]
    }
  }
})

````

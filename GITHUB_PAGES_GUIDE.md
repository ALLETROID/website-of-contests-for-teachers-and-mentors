# 🌐 Загрузка на GitHub и публикация

## ⚠️ Важная информация

GitHub Pages поддерживает **только статические сайты** (HTML, CSS, JavaScript). 

Ваш проект состоит из:
- ✅ **Frontend** - можно разместить на GitHub Pages
- ❌ **Backend + База данных** - нужен другой хостинг

## 📋 Варианты размещения

### Вариант 1: Только демо frontend (GitHub Pages)
- Frontend на GitHub Pages
- Backend нужно будет запускать локально
- Подходит для демонстрации интерфейса

### Вариант 2: Полноценное приложение
- Frontend на GitHub Pages
- Backend на бесплатном хостинге (Render.com, Railway.app, Heroku)
- База данных PostgreSQL на том же хостинге

### Вариант 3: Все на одном хостинге
- Frontend + Backend вместе на Render.com/Railway/Vercel
- База данных там же

---

## 🚀 Вариант 1: GitHub Pages (только frontend)

### Шаг 1: Создайте репозиторий на GitHub

1. Перейдите на https://github.com
2. Нажмите **"New repository"** (зеленая кнопка)
3. Заполните:
   - **Repository name:** `contest-portfolio-platform`
   - **Description:** `Платформа для конкурсов с портфолио`
   - **Public** (чтобы Pages работал бесплатно)
   - ❌ НЕ добавляйте README, .gitignore, license
4. Нажмите **"Create repository"**

### Шаг 2: Инициализируйте Git локально

Откройте терминал в папке проекта:

```bash
cd C:\Users\user\Hacaton\contest-portfolio-platform

# Инициализация Git
git init

# Добавление всех файлов
git add .

# Первый коммит
git commit -m "Initial commit: Contest portfolio platform"

# Связывание с GitHub (замените YOUR-USERNAME на ваш username)
git remote add origin https://github.com/YOUR-USERNAME/contest-portfolio-platform.git

# Отправка на GitHub
git branch -M main
git push -u origin main
```

### Шаг 3: Настройка GitHub Pages для frontend

#### 3.1 Обновите vite.config.ts

Откройте `frontend/vite.config.ts` и измените:

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  base: '/contest-portfolio-platform/', // Замените на название вашего репозитория
  server: {
    port: 5173,
    proxy: {
      '/api': {
        target: 'http://localhost:3000', // Для локальной разработки
        changeOrigin: true,
      },
    },
  },
  build: {
    outDir: 'dist',
  },
});
```

#### 3.2 Создайте workflow для автодеплоя

Создайте файл `.github/workflows/deploy-frontend.yml`:

```yaml
name: Deploy Frontend to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
      
      - name: Install dependencies
        working-directory: ./frontend
        run: npm ci
      
      - name: Build
        working-directory: ./frontend
        run: npm run build
      
      - name: Setup Pages
        uses: actions/configure-pages@v4
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: './frontend/dist'
      
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

#### 3.3 Включите GitHub Pages

1. Перейдите в репозиторий на GitHub
2. **Settings** → **Pages**
3. В разделе **Source** выберите:
   - **Source:** GitHub Actions
4. Сохраните

#### 3.4 Отправьте изменения

```bash
git add .
git commit -m "Configure GitHub Pages deployment"
git push
```

### Шаг 4: Проверьте деплой

1. Перейдите в **Actions** в вашем репозитории
2. Дождитесь завершения workflow (зеленая галочка)
3. Откройте сайт: `https://YOUR-USERNAME.github.io/contest-portfolio-platform/`

⚠️ **Важно:** Сайт будет работать БЕЗ backend! Для полной работы нужен Вариант 2.

---

## 🌟 Вариант 2: Frontend (GitHub Pages) + Backend (Render.com)

### Для Backend - используйте бесплатный Render.com

#### Шаг 1: Подготовка backend

Создайте файл `backend/render.yaml`:

```yaml
services:
  - type: web
    name: contest-backend
    env: node
    buildCommand: npm install && npm run prisma:generate && npm run build
    startCommand: npm start
    envVars:
      - key: DATABASE_URL
        fromDatabase:
          name: contest-db
          property: connectionString
      - key: JWT_SECRET
        generateValue: true
      - key: MAX_STORAGE_GB
        value: 5
      - key: NODE_ENV
        value: production

databases:
  - name: contest-db
    databaseName: contest_platform
    user: contest_user
```

#### Шаг 2: Регистрация на Render.com

1. Перейдите на https://render.com
2. Зарегистрируйтесь через GitHub
3. Нажмите **"New +"** → **"Web Service"**
4. Выберите ваш репозиторий
5. Настройки:
   - **Name:** contest-backend
   - **Root Directory:** backend
   - **Environment:** Node
   - **Build Command:** `npm install && npm run prisma:generate && npm run build`
   - **Start Command:** `npm start`
6. Добавьте переменные окружения:
   - `JWT_SECRET` → случайная строка
   - `MAX_STORAGE_GB` → 5
7. Создайте PostgreSQL базу:
   - **New +** → **PostgreSQL**
   - Скопируйте **Internal Database URL**
   - Добавьте как `DATABASE_URL` в Web Service
8. Нажмите **"Create Web Service"**

#### Шаг 3: Обновите frontend для работы с Render

Измените `frontend/src/api/axios.ts`:

```typescript
import axios from 'axios';
import { useAuthStore } from '../store/authStore';

const api = axios.create({
  baseURL: import.meta.env.PROD 
    ? 'https://your-app-name.onrender.com/api'  // Замените на ваш URL Render
    : '/api', // Для локальной разработки
});

api.interceptors.request.use((config) => {
  const token = useAuthStore.getState().token;
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      useAuthStore.getState().logout();
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

export default api;
```

#### Шаг 4: Обновите и задеплойте

```bash
git add .
git commit -m "Add Render.com deployment config"
git push
```

После деплоя на Render:
1. Скопируйте URL вашего backend (например: `https://contest-backend.onrender.com`)
2. Замените в `frontend/src/api/axios.ts`
3. Закоммитьте и запушьте снова

Теперь ваш сайт на GitHub Pages будет работать с backend на Render! 🎉

---

## 🔥 Вариант 3: Все на Vercel (рекомендуется)

Vercel поддерживает и frontend, и backend.

### Шаг 1: Установите Vercel CLI

```bash
npm install -g vercel
```

### Шаг 2: Создайте vercel.json в корне

```json
{
  "version": 2,
  "builds": [
    {
      "src": "frontend/package.json",
      "use": "@vercel/static-build",
      "config": {
        "distDir": "dist"
      }
    },
    {
      "src": "backend/src/server.ts",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/api/(.*)",
      "dest": "/backend/src/server.ts"
    },
    {
      "src": "/(.*)",
      "dest": "/frontend/$1"
    }
  ]
}
```

### Шаг 3: Деплой

```bash
cd C:\Users\user\Hacaton\contest-portfolio-platform
vercel
```

Следуйте инструкциям:
- **Set up and deploy?** Yes
- **Which scope?** Ваш аккаунт
- **Link to existing project?** No
- **What's your project's name?** contest-platform
- **In which directory is your code located?** ./

### Шаг 4: Настройте базу данных

На Vercel нужна внешняя БД:

**Вариант A: Neon.tech (PostgreSQL)**
1. Зарегистрируйтесь на https://neon.tech
2. Создайте проект
3. Скопируйте Connection String
4. В Vercel Dashboard → Settings → Environment Variables
5. Добавьте `DATABASE_URL` со строкой подключения

**Вариант B: Supabase (PostgreSQL)**
1. Зарегистрируйтесь на https://supabase.com
2. Создайте проект
3. Database → Settings → Connection string
4. Добавьте в Vercel Environment Variables

### Шаг 5: Финальный деплой

```bash
vercel --prod
```

Готово! Ваш сайт доступен по адресу типа: `https://contest-platform.vercel.app`

---

## 📝 Команды Git для работы

```bash
# Проверка статуса
git status

# Добавление изменений
git add .

# Коммит
git commit -m "Описание изменений"

# Отправка на GitHub
git push

# Создание новой ветки
git checkout -b feature-name

# Переключение веток
git checkout main

# Обновление с GitHub
git pull
```

---

## 🎯 Рекомендация

**Для хакатона рекомендую Вариант 3 (Vercel)**:
- ✅ Бесплатно
- ✅ Frontend + Backend вместе
- ✅ Автоматический деплой при push
- ✅ HTTPS из коробки
- ✅ Быстрая настройка

**Для демо интерфейса - Вариант 1 (GitHub Pages)**:
- ✅ Просто показать UI
- ❌ Backend работает только локально

---

## 🆘 Проблемы

### "Failed to push"
```bash
git pull --rebase
git push
```

### "Permission denied"
Настройте SSH ключ: https://docs.github.com/en/authentication/connecting-to-github-with-ssh

### "Pages build failed"
Проверьте логи в Actions → Failed workflow → Детали

---

Нужна помощь с конкретным вариантом? Спрашивайте!

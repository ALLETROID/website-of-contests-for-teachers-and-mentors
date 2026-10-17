# 🆓 Бесплатный хостинг - ПОЛНАЯ ИНСТРУКЦИЯ

## 🎯 Что получите

После выполнения инструкции:
- ✅ **Полностью работающий сайт** в интернете
- ✅ Вход, регистрация, загрузка файлов - ВСЁ работает
- ✅ База данных PostgreSQL
- ✅ Постоянная ссылка для показа

## 🌟 Лучший вариант: Render.com

**Почему Render.com?**
- ✅ Полностью бесплатно
- ✅ Frontend + Backend + База данных на одном месте
- ✅ Автоматический деплой из GitHub
- ✅ HTTPS из коробки
- ✅ Простая настройка

---

## 📋 ЧТО КУДА ЗАГРУЖАТЬ

```
GitHub → Ваш код
   ↓
Render.com → Читает код с GitHub
   ↓
   ├── PostgreSQL Database (база данных)
   ├── Backend (API сервер)
   └── Frontend (сайт)
   ↓
Готовая ссылка: https://ваш-сайт.onrender.com
```

---

## 🚀 ПОШАГОВАЯ ИНСТРУКЦИЯ

### ШАГ 1: Загрузите код на GitHub (10 минут)

#### 1.1. Создайте репозиторий

1. Откройте https://github.com/new
2. **Repository name:** `contest-platform`
3. **Public** ✅
4. **Create repository**

#### 1.2. Загрузите код

Откройте PowerShell/Terminal:

```bash
cd C:\Users\user\Hacaton\contest-portfolio-platform

git init
git add .
git commit -m "Initial commit"

# Замените YOUR-USERNAME на ваш GitHub username!
git remote add origin https://github.com/YOUR-USERNAME/contest-platform.git
git branch -M main
git push -u origin main
```

Если попросит логин/пароль:
- Username: ваш GitHub username
- Password: создайте Personal Access Token: https://github.com/settings/tokens/new (права: repo)

✅ **Проверка:** Обновите страницу репозитория - должны появиться файлы

---

### ШАГ 2: Подготовьте backend для Render (5 минут)

#### 2.1. Обновите package.json

Откройте `backend/package.json` и добавьте в `scripts`:

```json
"scripts": {
  "dev": "tsx watch src/server.ts",
  "build": "tsc",
  "start": "node dist/server.js",
  "prisma:generate": "prisma generate",
  "prisma:migrate": "prisma migrate deploy",
  "prisma:seed": "tsx prisma/seed.ts",
  "postinstall": "prisma generate"
}
```

Должно получиться примерно так:

```json
{
  "name": "contest-portfolio-backend",
  "version": "1.0.0",
  "scripts": {
    "dev": "tsx watch src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js",
    "prisma:generate": "prisma generate",
    "prisma:migrate": "prisma migrate deploy",
    "prisma:seed": "tsx prisma/seed.ts",
    "postinstall": "prisma generate"
  },
  ...
}
```

#### 2.2. Закоммитьте изменения

```bash
git add backend/package.json
git commit -m "Prepare for Render deployment"
git push
```

---

### ШАГ 3: Создайте базу данных на Render (3 минуты)

#### 3.1. Регистрация

1. Откройте https://render.com
2. Нажмите **"Get Started for Free"**
3. Зарегистрируйтесь через GitHub (Sign up with GitHub)
4. Разрешите доступ к репозиториям

#### 3.2. Создайте базу данных

1. В Dashboard нажмите **"New +"**
2. Выберите **"PostgreSQL"**
3. Заполните:
   - **Name:** `contest-db`
   - **Database:** `contest_platform`
   - **User:** `contest_user`
   - **Region:** выберите ближайший (например, Frankfurt)
   - **Instance Type:** ✅ **Free**
4. Нажмите **"Create Database"**

⏳ Подождите 1-2 минуты пока создастся

#### 3.3. Скопируйте строку подключения

1. Откройте созданную базу данных
2. Найдите раздел **"Connections"**
3. Скопируйте **"Internal Database URL"**

Выглядит примерно так:
```
postgresql://contest_user:password@dpg-xxx.oregon-postgres.render.com/contest_platform
```

🔐 **ВАЖНО:** Сохраните эту строку - она понадобится!

---

### ШАГ 4: Разверните Backend на Render (5 минут)

#### 4.1. Создайте Web Service

1. Нажмите **"New +"**
2. Выберите **"Web Service"**
3. Нажмите **"Build and deploy from a Git repository"**
4. Нажмите **"Next"**

#### 4.2. Подключите GitHub

1. Выберите ваш репозиторий `contest-platform`
2. Нажмите **"Connect"**

#### 4.3. Настройте Backend

Заполните поля:

**Basic Settings:**
- **Name:** `contest-backend` (или любое другое)
- **Region:** тот же что и база данных
- **Branch:** `main`
- **Root Directory:** `backend`
- **Runtime:** `Node`
- **Build Command:** 
  ```
  npm install && npm run build && npx prisma migrate deploy && npx prisma db seed
  ```
- **Start Command:**
  ```
  npm start
  ```

**Instance Type:**
- ✅ **Free**

#### 4.4. Добавьте переменные окружения

Прокрутите вниз до **"Environment Variables"**

Нажмите **"Add Environment Variable"** и добавьте:

**1. DATABASE_URL**
- Key: `DATABASE_URL`
- Value: вставьте Internal Database URL (из шага 3.3)

**2. JWT_SECRET**
- Key: `JWT_SECRET`
- Value: `super-secret-jwt-key-12345-change-in-production`

**3. PORT**
- Key: `PORT`
- Value: `10000`

**4. MAX_STORAGE_GB**
- Key: `MAX_STORAGE_GB`
- Value: `5`

**5. NODE_ENV**
- Key: `NODE_ENV`
- Value: `production`

#### 4.5. Создайте сервис

1. Нажмите **"Create Web Service"**
2. ⏳ Подождите 5-10 минут (первый деплой долгий)
3. Дождитесь зеленого статуса **"Live"**

#### 4.6. Скопируйте URL Backend

В верхней части страницы будет URL типа:
```
https://contest-backend.onrender.com
```

🔐 **ВАЖНО:** Сохраните этот URL!

---

### ШАГ 5: Настройте Frontend для работы с Backend (3 минуты)

#### 5.1. Обновите axios.ts

Откройте файл `frontend/src/api/axios.ts` и замените его содержимое:

```typescript
import axios from 'axios';
import { useAuthStore } from '../store/authStore';

const api = axios.create({
  // ЗАМЕНИТЕ на ваш backend URL с /api в конце!
  baseURL: 'https://contest-backend.onrender.com/api',
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

⚠️ **ВАЖНО:** Замените `https://contest-backend.onrender.com` на ваш URL из шага 4.6!

#### 5.2. Обновите vite.config.ts

Откройте `frontend/vite.config.ts` и замените:

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  server: {
    port: 5173,
  },
  build: {
    outDir: 'dist',
  },
});
```

(Убрали `base` и `proxy` - они не нужны для production)

#### 5.3. Закоммитьте изменения

```bash
git add .
git commit -m "Configure frontend for Render backend"
git push
```

---

### ШАГ 6: Разверните Frontend на Render (5 минут)

#### 6.1. Создайте Static Site

1. В Render Dashboard нажмите **"New +"**
2. Выберите **"Static Site"**
3. Выберите ваш репозиторий
4. Нажмите **"Connect"**

#### 6.2. Настройте Frontend

**Basic Settings:**
- **Name:** `contest-frontend`
- **Branch:** `main`
- **Root Directory:** `frontend`
- **Build Command:**
  ```
  npm install && npm run build
  ```
- **Publish Directory:**
  ```
  dist
  ```

#### 6.3. Создайте сайт

1. Нажмите **"Create Static Site"**
2. ⏳ Подождите 3-5 минут
3. Дождитесь зеленого статуса

#### 6.4. Получите ссылку на сайт

В верхней части будет ссылка типа:
```
https://contest-frontend.onrender.com
```

🎉 **ЭТО ВАША ССЫЛКА НА ГОТОВЫЙ САЙТ!**

---

### ШАГ 7: Настройте CORS на Backend (2 минуты)

Нужно разрешить frontend обращаться к backend.

#### 7.1. Обновите server.ts

Откройте `backend/src/server.ts` и измените строку с `app.use(cors())`:

```typescript
// ЗАМЕНИТЕ на ваш frontend URL!
app.use(cors({
  origin: 'https://contest-frontend.onrender.com',
  credentials: true,
}));
```

Или разрешите все (менее безопасно, но проще):

```typescript
app.use(cors({
  origin: '*',
}));
```

#### 7.2. Закоммитьте и задеплойте

```bash
git add backend/src/server.ts
git commit -m "Update CORS settings"
git push
```

⏳ Render автоматически передеплоит backend (2-3 минуты)

---

## ✅ ГОТОВО! Проверьте работу

### 1. Откройте ваш сайт

```
https://contest-frontend.onrender.com
```

### 2. Зарегистрируйтесь

Создайте новый аккаунт или войдите с предустановленным:
- Логин: `Damir`
- Пароль: `289950`

### 3. Проверьте функционал

- ✅ Вход работает?
- ✅ Конкурсы отображаются?
- ✅ Файлы загружаются?
- ✅ Регистрация работает?

🎉 **Если всё работает - поздравляю!**

---

## 📱 ПОДЕЛИТЬСЯ САЙТОМ

Отправьте ссылку:

```
https://contest-frontend.onrender.com
```

**Предустановленные аккаунты для входа:**
- Damir / 289950 (Создатель)
- Nadejda / 456894 (Оператор)
- Timur / 274856 (Оператор)
- Anna / 095467 (Оператор)

---

## ⚠️ Важные моменты

### Бесплатный план Render

**Ограничения:**
- Backend "засыпает" после 15 минут неактивности
- Первый запрос после сна - долгий (30-60 секунд)
- Потом работает нормально
- 750 часов в месяц (достаточно для хакатона)

**Решение "сна":**
- Используйте сервис cron-job.org для пинга раз в 10 минут
- Или платный план ($7/мес) без сна

### Файлы

**Важно:**
- На бесплатном плане файлы удаляются при перезапуске!
- Это ОК для демо/хакатона
- Для production нужно хранилище (AWS S3, Cloudinary)

---

## 🔄 Обновление сайта

При изменении кода:

```bash
git add .
git commit -m "Описание изменений"
git push
```

Render автоматически передеплоит за 2-3 минуты!

---

## ❌ Решение проблем

### Backend не запускается

1. Проверьте логи: откройте backend service → Logs
2. Частая ошибка: неверный DATABASE_URL
3. Проверьте Environment Variables

### Frontend белый экран

1. Проверьте что `baseURL` в `axios.ts` правильный
2. Откройте консоль браузера (F12) - смотрите ошибки
3. Проверьте CORS настройки в backend

### "Failed to fetch" или CORS error

Backend не разрешает запросы с frontend:
1. Обновите `cors()` в `server.ts` (шаг 7)
2. Закоммитьте и запушьте
3. Подождите передеплоя

### База данных не подключается

1. Проверьте что DATABASE_URL правильный
2. Он должен быть **Internal Database URL**
3. Проверьте что база создалась (зеленый статус)

### Вход не работает

1. Проверьте что seed выполнился (логи backend)
2. Если нет - запустите вручную через Render Shell:
   ```bash
   npm run prisma:seed
   ```

---

## 🎯 Итоговые ссылки

После настройки у вас будет:

```
Frontend (сайт):
https://contest-frontend.onrender.com

Backend (API):
https://contest-backend.onrender.com

Database:
Internal connection
```

---

## 💰 Стоимость

**ПОЛНОСТЬЮ БЕСПЛАТНО:**
- ✅ Frontend
- ✅ Backend
- ✅ Database
- ✅ HTTPS
- ✅ Автодеплой

**Без кредитной карты!**

---

## 📊 Альтернативные варианты

Если Render не подходит:

### 1. Railway.app
- Аналогично Render
- $5 бесплатно каждый месяц
- Проще в настройке

### 2. Vercel + Supabase
- Frontend на Vercel
- Backend на Vercel Serverless Functions
- Database на Supabase (бесплатно)

### 3. Netlify + Heroku
- Frontend на Netlify
- Backend на Heroku (требует карту)

**Рекомендация:** Используйте Render - самый простой вариант!

---

## ✅ Чек-лист

- [ ] Код на GitHub
- [ ] База данных создана на Render
- [ ] Backend задеплоен на Render
- [ ] Frontend настроен на backend URL
- [ ] Frontend задеплоен на Render
- [ ] CORS настроен
- [ ] Сайт открывается
- [ ] Вход работает
- [ ] Можно показать проект!

---

**Готово! Ваш сайт в интернете! 🎉**

Вопросы? Смотрите раздел "Решение проблем" выше.

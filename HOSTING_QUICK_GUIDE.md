# ⚡ Быстрая схема размещения

## 📊 Что куда загружать

```
┌─────────────────────────────────────────────────┐
│                                                 │
│  ВАШ КОМПЬЮТЕР                                 │
│  C:\Users\user\Hacaton\contest-portfolio-...   │
│                                                 │
│  ├── backend/      (Node.js + API)             │
│  ├── frontend/     (React сайт)                │
│  └── docs/         (документация)              │
│                                                 │
└────────────────┬────────────────────────────────┘
                 │
                 │ git push
                 ↓
┌─────────────────────────────────────────────────┐
│                                                 │
│  GITHUB.COM                                     │
│  https://github.com/username/contest-platform   │
│                                                 │
│  📦 Хранит код                                  │
│                                                 │
└────────────────┬────────────────────────────────┘
                 │
                 │ автоматический деплой
                 ↓
┌─────────────────────────────────────────────────┐
│                                                 │
│  RENDER.COM (всё бесплатно!)                    │
│                                                 │
│  ┌───────────────────────────────────────┐     │
│  │  PostgreSQL Database                  │     │
│  │  База данных                          │     │
│  │  (пользователи, конкурсы, файлы)     │     │
│  └─────────────┬─────────────────────────┘     │
│                │                               │
│                │ DATABASE_URL                   │
│                ↓                               │
│  ┌───────────────────────────────────────┐     │
│  │  Backend Web Service                  │     │
│  │  https://contest-backend.onrender.com │     │
│  │  (API сервер)                         │     │
│  └─────────────┬─────────────────────────┘     │
│                │                               │
│                │ API запросы                    │
│                ↓                               │
│  ┌───────────────────────────────────────┐     │
│  │  Frontend Static Site                 │     │
│  │  https://contest-frontend.onrender... │     │
│  │  (веб-сайт)                           │     │
│  └───────────────────────────────────────┘     │
│                                                 │
└────────────────┬────────────────────────────────┘
                 │
                 │
                 ↓
      👥 ПОЛЬЗОВАТЕЛИ
      Открывают в браузере
      https://contest-frontend.onrender.com
```

---

## 🎯 7 шагов к готовому сайту

### 1️⃣ GitHub (10 мин)
```bash
cd C:\Users\user\Hacaton\contest-portfolio-platform
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/USERNAME/contest-platform.git
git push -u origin main
```

### 2️⃣ Render: Создать аккаунт (1 мин)
- Откройте https://render.com
- Sign up with GitHub

### 3️⃣ Render: База данных (3 мин)
- New + → PostgreSQL
- Name: `contest-db`
- Free tier ✅
- Скопируйте **Internal Database URL**

### 4️⃣ Render: Backend (5 мин)
- New + → Web Service
- Выберите репозиторий
- Root Directory: `backend`
- Build: `npm install && npm run build && npx prisma migrate deploy && npx prisma db seed`
- Start: `npm start`
- Environment Variables:
  - `DATABASE_URL` = (Internal URL из шага 3)
  - `JWT_SECRET` = `secret-key-12345`
  - `PORT` = `10000`
- Скопируйте URL: `https://contest-backend.onrender.com`

### 5️⃣ Локально: Настроить Frontend (3 мин)
Откройте `frontend/src/api/axios.ts`:
```typescript
const api = axios.create({
  baseURL: 'https://contest-backend.onrender.com/api', // ВАШ URL!
});
```

Откройте `frontend/vite.config.ts` - уберите `base` и `proxy`

```bash
git add .
git commit -m "Configure for Render"
git push
```

### 6️⃣ Render: Frontend (5 мин)
- New + → Static Site
- Выберите репозиторий
- Root Directory: `frontend`
- Build: `npm install && npm run build`
- Publish: `dist`
- Скопируйте URL: `https://contest-frontend.onrender.com`

### 7️⃣ Локально: Настроить CORS (2 мин)
Откройте `backend/src/server.ts`:
```typescript
app.use(cors({
  origin: 'https://contest-frontend.onrender.com', // ВАШ frontend URL!
}));
```

```bash
git add .
git commit -m "Update CORS"
git push
```

⏳ Подождите 3 минуты передеплоя

---

## ✅ Готово!

Откройте: `https://contest-frontend.onrender.com`

Войдите:
- Логин: `Damir`
- Пароль: `289950`

---

## 🔗 Полная инструкция

Смотрите **[FREE_HOSTING_GUIDE.md](FREE_HOSTING_GUIDE.md)** для детальных шагов!

---

## 📊 Что у вас будет

| Компонент | Где | URL |
|-----------|-----|-----|
| Код | GitHub | https://github.com/username/contest-platform |
| База данных | Render | Internal connection |
| Backend API | Render | https://contest-backend.onrender.com |
| Frontend сайт | Render | https://contest-frontend.onrender.com |

**Всё бесплатно! Без кредитной карты!**

---

## ⚠️ Важно

**Бесплатный план:**
- ✅ Всё работает
- ⚠️ Backend "засыпает" после 15 мин неактивности
- ⚠️ Первый запрос после сна - медленный (30-60 сек)
- ⚠️ Файлы удаляются при перезапуске
- ✅ Для демо/хакатона - идеально!

---

## 🆘 Проблемы?

**Сайт не открывается:**
→ Проверьте статус в Render Dashboard

**CORS error:**
→ Проверьте что `origin` в `server.ts` совпадает с frontend URL

**Backend не запускается:**
→ Откройте Logs в Render → смотрите ошибку

**Вход не работает:**
→ Проверьте что seed выполнился в логах

---

**Полная инструкция: [FREE_HOSTING_GUIDE.md](FREE_HOSTING_GUIDE.md)**

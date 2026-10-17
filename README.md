# 🏆 Платформа Конкурсов с Портфолио

Полнофункциональная веб-платформа для проведения конкурсов с системой портфолио участников, административной панелью и иерархией пользователей.

## ✨ Возможности

### 👥 Система пользователей с 3 уровнями доступа:

- **Создатель** - полный доступ ко всем функциям
- **Оператор** - управление конкурсами, просмотр профилей, изменение паролей
- **Обычный пользователь** - регистрация на конкурсы, загрузка портфолио

### 🎯 Функционал конкурсов:

- Каталог конкурсов с карточками
- Фильтры по направлению, возрасту, датам
- Детальная карточка конкурса
- Регистрация на конкурсы

### 📁 Облачное хранилище:

- 5 ГБ на пользователя
- Загрузка/скачивание файлов
- Публичная ссылка на портфолио
- Отслеживание использования места

### 🔐 Безопасность:

- JWT аутентификация
- Хеширование паролей (bcrypt)
- Разграничение прав доступа
- Защищенные API endpoints

## 🛠 Технологии

### Backend:
- Node.js + Express + TypeScript
- PostgreSQL + Prisma ORM
- JWT для аутентификации
- Multer для загрузки файлов

### Frontend:
- React 18 + TypeScript
- React Router v6
- Zustand (управление состоянием)
- TailwindCSS
- Axios
- Lucide Icons
- date-fns

## 🌐 Размещение на бесплатном хостинге

**Хотите разместить ПОЛНОСТЬЮ РАБОТАЮЩИЙ сайт онлайн БЕСПЛАТНО?**

📖 **[FREE_HOSTING_GUIDE.md](FREE_HOSTING_GUIDE.md)** ⭐ - Полная инструкция (30 минут)

📖 **[HOSTING_QUICK_GUIDE.md](HOSTING_QUICK_GUIDE.md)** ⚡ - Быстрая схема (7 шагов)

**Что получите:**
- ✅ Frontend + Backend + База данных
- ✅ Вход, регистрация, загрузка файлов - ВСЁ работает
- ✅ Постоянная ссылка: `https://ваш-сайт.onrender.com`
- ✅ Полностью бесплатно (без карты!)

**Рекомендация:** Render.com - всё на одном месте!

---

## 🌐 Деплой на GitHub Pages (только интерфейс)

**Хотите разместить проект онлайн?**

📖 Смотрите **[GITHUB_QUICK_START.md](GITHUB_QUICK_START.md)** - загрузка за 5 минут!

Или детальные инструкции:
- **[DEPLOY_CHECKLIST.md](DEPLOY_CHECKLIST.md)** - пошаговый чек-лист
- **[GITHUB_PAGES_GUIDE.md](GITHUB_PAGES_GUIDE.md)** - полное руководство
- **[GIT_COMMANDS_CHEATSHEET.md](GIT_COMMANDS_CHEATSHEET.md)** - шпаргалка Git

## 🚀 Установка и запуск

### Предварительные требования

- Node.js 18+
- PostgreSQL 14+
- npm или yarn

### 1. Клонирование репозитория

```bash
git clone <url-репозитория>
cd contest-portfolio-platform
```

### 2. Настройка Backend

```bash
cd backend

# Установка зависимостей
npm install

# Создание .env файла
cp .env.example .env

# Отредактируйте .env файл и укажите:
# - DATABASE_URL (строка подключения к PostgreSQL)
# - JWT_SECRET (секретный ключ для JWT)
# - PORT (порт сервера, по умолчанию 3000)
```

**Пример DATABASE_URL:**
```
DATABASE_URL="postgresql://postgres:password@localhost:5432/contest_platform?schema=public"
```

### 3. Инициализация базы данных

```bash
# Генерация Prisma Client
npm run prisma:generate

# Создание миграций базы данных
npm run prisma:migrate

# Заполнение БД начальными данными (создание 4 пользователей)
npm run prisma:seed
```

### 4. Запуск Backend

```bash
# Режим разработки (с автоперезагрузкой)
npm run dev

# Или production-сборка
npm run build
npm start
```

Backend будет доступен на `http://localhost:3000`

### 5. Настройка Frontend

Откройте новый терминал:

```bash
cd frontend

# Установка зависимостей
npm install
```

### 6. Запуск Frontend

```bash
# Режим разработки
npm run dev

# Production-сборка
npm run build
npm run preview
```

Frontend будет доступен на `http://localhost:5173`

## 📁 Структура проекта

```
contest-portfolio-platform/
├── backend/
│   ├── prisma/
│   │   ├── schema.prisma      # Схема базы данных
│   │   └── seed.ts            # Начальные данные
│   ├── src/
│   │   ├── middleware/
│   │   │   └── auth.ts        # Middleware аутентификации
│   │   ├── routes/
│   │   │   ├── auth.ts        # Авторизация
│   │   │   ├── users.ts       # Пользователи
│   │   │   ├── contests.ts    # Конкурсы
│   │   │   ├── files.ts       # Файлы
│   │   │   └── admin.ts       # Администрирование
│   │   └── server.ts          # Главный файл сервера
│   ├── uploads/               # Директория для файлов
│   └── package.json
│
└── frontend/
    ├── src/
    │   ├── api/
    │   │   └── axios.ts       # Настройка axios
    │   ├── components/
    │   │   └── Layout.tsx     # Общий Layout
    │   ├── pages/
    │   │   ├── LoginPage.tsx
    │   │   ├── RegisterPage.tsx
    │   │   ├── ContestsPage.tsx
    │   │   ├── ContestDetailPage.tsx
    │   │   ├── PortfolioPage.tsx
    │   │   ├── ProfilePage.tsx
    │   │   └── AdminPage.tsx
    │   ├── store/
    │   │   └── authStore.ts   # Zustand store
    │   ├── types/
    │   │   └── index.ts       # TypeScript типы
    │   ├── App.tsx
    │   ├── main.tsx
    │   └── index.css
    └── package.json
```

## 🔑 API Endpoints

### Публичные:
- `POST /api/auth/register` - Регистрация
- `POST /api/auth/login` - Вход
- `GET /api/contests` - Список конкурсов
- `GET /api/contests/:id` - Детали конкурса

### Защищенные (требуют авторизации):
- `GET /api/auth/me` - Текущий пользователь
- `GET /api/users/profile/:id` - Профиль пользователя
- `PUT /api/users/profile` - Обновление профиля
- `POST /api/contests/:id/register` - Регистрация на конкурс
- `GET /api/contests/my/registrations` - Мои конкурсы
- `GET /api/files` - Мои файлы
- `POST /api/files/upload` - Загрузка файла
- `DELETE /api/files/:id` - Удаление файла
- `GET /api/files/download/:id` - Скачивание файла
- `GET /api/files/portfolio/:userId` - Публичное портфолио

### Админ (Оператор/Создатель):
- `GET /api/admin/users` - Список всех пользователей
- `GET /api/admin/users/:id` - Детали пользователя
- `PUT /api/admin/users/:id/password` - Смена пароля
- `POST /api/admin/contests` - Создание конкурса
- `PUT /api/admin/contests/:id` - Обновление конкурса
- `DELETE /api/admin/contests/:id` - Удаление конкурса
- `GET /api/admin/contests/:id/registrations` - Заявки на конкурс

### Только Создатель:
- `PUT /api/admin/users/:id/role` - Изменение роли
- `GET /api/admin/stats` - Статистика

## 🎨 Интерфейс

### Главная страница
- Каталог конкурсов с фильтрами
- Карточки с изображениями
- Количество участников

### Страница конкурса
- Полное описание
- Задания
- Организатор и даты
- Кнопка регистрации

### Портфолио
- Список файлов пользователя
- Загрузка/скачивание/удаление
- Прогресс-бар хранилища
- Публичная ссылка

### Админ-панель
- Управление пользователями
- Просмотр профилей и файлов
- Создание/редактирование конкурсов
- Смена паролей и ролей

## 🔧 Разработка

### Добавление новых полей в модели

1. Отредактируйте `backend/prisma/schema.prisma`
2. Создайте миграцию: `npm run prisma:migrate`
3. Обновите TypeScript типы в frontend

### Изменение лимита хранилища

В файле `backend/.env`:
```
MAX_STORAGE_GB=10  # Увеличить до 10 ГБ
```

### Настройка CORS

В `backend/src/server.ts` измените настройки cors если нужно.

## 📝 Лицензия

MIT

## 👨‍💻 Автор

Разработано для хакатона

---

**Важно:** Не забудьте изменить `JWT_SECRET` в production!

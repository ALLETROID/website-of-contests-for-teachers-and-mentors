# 📂 Структура проекта

## Корневая директория

```
contest-portfolio-platform/
├── backend/                    # Backend приложение (Node.js + Express)
├── frontend/                   # Frontend приложение (React + TypeScript)
├── README.md                   # Основная документация
├── SETUP.md                    # Инструкция по быстрому старту
├── DEPLOY.md                   # Инструкция по деплою
├── API_DOCUMENTATION.md        # Документация API
├── PROJECT_STRUCTURE.md        # Этот файл
└── .gitignore                  # Исключения для Git
```

## Backend структура

```
backend/
├── prisma/
│   ├── schema.prisma          # Схема базы данных Prisma
│   └── seed.ts                # Начальные данные (4 пользователя)
│
├── src/
│   ├── middleware/
│   │   └── auth.ts            # JWT аутентификация middleware
│   │
│   ├── routes/
│   │   ├── auth.ts            # Регистрация, вход, текущий пользователь
│   │   ├── users.ts           # Профиль пользователя
│   │   ├── contests.ts        # Конкурсы, регистрация
│   │   ├── files.ts           # Файлы, портфолио
│   │   └── admin.ts           # Админ-панель
│   │
│   └── server.ts              # Главный файл сервера
│
├── uploads/                   # Загруженные файлы (создается автоматически)
├── package.json               # Зависимости и скрипты
├── tsconfig.json              # Конфигурация TypeScript
├── .env                       # Переменные окружения
└── .env.example               # Пример переменных окружения
```

### Backend - Основные файлы

#### `prisma/schema.prisma`
- Модели: User, File, Contest, ContestRegistration
- Перечисления: UserRole, AgeGroup, ContestCategory
- Связи между моделями

#### `src/server.ts`
- Инициализация Express
- Подключение middleware (cors, json, static files)
- Регистрация маршрутов
- Health check endpoint

#### `src/middleware/auth.ts`
- `authenticateToken` - проверка JWT токена
- `requireRole` - проверка роли пользователя
- Тип `AuthRequest` с информацией о пользователе

#### `src/routes/auth.ts`
- POST `/api/auth/register` - регистрация
- POST `/api/auth/login` - вход
- GET `/api/auth/me` - текущий пользователь

#### `src/routes/users.ts`
- GET `/api/users/profile/:id` - профиль пользователя
- PUT `/api/users/profile` - обновление профиля

#### `src/routes/contests.ts`
- GET `/api/contests` - список с фильтрами
- GET `/api/contests/:id` - детали конкурса
- POST `/api/contests/:id/register` - регистрация
- GET `/api/contests/my/registrations` - мои конкурсы

#### `src/routes/files.ts`
- GET `/api/files` - список файлов + статистика
- POST `/api/files/upload` - загрузка (multer)
- DELETE `/api/files/:id` - удаление
- GET `/api/files/download/:id` - скачивание
- GET `/api/files/portfolio/:userId` - публичное портфолио

#### `src/routes/admin.ts`
- GET `/api/admin/users` - все пользователи
- GET `/api/admin/users/:id` - детали пользователя
- PUT `/api/admin/users/:id/password` - смена пароля
- PUT `/api/admin/users/:id/role` - смена роли (только CREATOR)
- POST/PUT/DELETE `/api/admin/contests` - CRUD конкурсов
- GET `/api/admin/contests/:id/registrations` - заявки
- GET `/api/admin/stats` - статистика (только CREATOR)

## Frontend структура

```
frontend/
├── src/
│   ├── api/
│   │   └── axios.ts           # Настройка axios + interceptors
│   │
│   ├── components/
│   │   └── Layout.tsx         # Общий Layout с навигацией
│   │
│   ├── pages/
│   │   ├── LoginPage.tsx      # Страница входа
│   │   ├── RegisterPage.tsx   # Страница регистрации
│   │   ├── ContestsPage.tsx   # Каталог конкурсов с фильтрами
│   │   ├── ContestDetailPage.tsx  # Детали конкурса
│   │   ├── PortfolioPage.tsx  # Портфолио (свое/публичное)
│   │   ├── ProfilePage.tsx    # Профиль пользователя
│   │   └── AdminPage.tsx      # Админ-панель
│   │
│   ├── store/
│   │   └── authStore.ts       # Zustand store для аутентификации
│   │
│   ├── types/
│   │   └── index.ts           # TypeScript типы и интерфейсы
│   │
│   ├── utils/
│   │
│   ├── App.tsx                # Главный компонент с роутингом
│   ├── main.tsx               # Точка входа React
│   └── index.css              # Tailwind CSS
│
├── public/                    # Статические файлы
├── index.html                 # HTML шаблон
├── package.json               # Зависимости и скрипты
├── tsconfig.json              # TypeScript конфигурация
├── tsconfig.node.json         # TypeScript для Vite
├── vite.config.ts             # Vite конфигурация
├── tailwind.config.js         # Tailwind CSS конфигурация
└── postcss.config.js          # PostCSS конфигурация
```

### Frontend - Основные файлы

#### `src/App.tsx`
- React Router настройка
- Защищенные маршруты (`PrivateRoute`)
- Админ маршруты (`AdminRoute`)
- Все страницы приложения

#### `src/store/authStore.ts`
- Zustand store с persist middleware
- Состояние: user, token
- Действия: setAuth, logout

#### `src/api/axios.ts`
- Axios instance с baseURL
- Request interceptor - добавление токена
- Response interceptor - обработка 401

#### `src/types/index.ts`
- User, Contest, File, ContestRegistration
- ContestCategory, AgeGroup перечисления
- Маппинг на русские названия

#### `src/components/Layout.tsx`
- Верхняя навигация
- Меню: Конкурсы, Портфолио, Админ-панель
- Информация о пользователе
- Выход

#### Страницы

**LoginPage.tsx**
- Форма входа (логин/email + пароль)
- Обработка ошибок
- Редирект после входа

**RegisterPage.tsx**
- Форма регистрации
- Обязательные поля: логин, email, пароль
- Опциональные: ФИО

**ContestsPage.tsx**
- Каталог конкурсов (grid)
- Фильтры: направление, возраст, даты
- Карточки с изображениями
- Количество участников

**ContestDetailPage.tsx**
- Детальная информация
- Описание, задания, организатор
- Кнопка регистрации
- Даты и статистика

**PortfolioPage.tsx**
- Режим: свое портфолио / публичное
- Список файлов с превью
- Загрузка/скачивание/удаление
- Прогресс-бар хранилища
- Публичная ссылка

**ProfilePage.tsx**
- Редактирование профиля
- ФИО, email
- Информация о роли
- Список зарегистрированных конкурсов

**AdminPage.tsx**
- 3 вкладки: Пользователи, Конкурсы, Создание
- Таблица пользователей с действиями
- Просмотр деталей, смена пароля/роли
- CRUD конкурсов
- Форма создания/редактирования

## База данных (PostgreSQL)

### Таблицы

**User**
- Пользователи системы
- Роли: CREATOR, OPERATOR, USER
- ФИО опционально
- Связи: files, registrations

**File**
- Загруженные файлы
- Хранит: имя, размер, путь, MIME type
- Связь: userId

**Contest**
- Конкурсы
- Поля: название, описание, организатор, даты
- Категория и возрастная группа
- Связь: registrations

**ContestRegistration**
- Регистрации на конкурсы
- Уникальная пара userId-contestId
- Связи: user, contest

## Основные технологии

### Backend
- **Express** - веб-фреймворк
- **Prisma** - ORM для PostgreSQL
- **JWT** - аутентификация
- **bcrypt** - хеширование паролей
- **multer** - загрузка файлов
- **TypeScript** - типизация

### Frontend
- **React 18** - UI библиотека
- **React Router** - роутинг
- **Zustand** - state management
- **TailwindCSS** - стили
- **Axios** - HTTP клиент
- **date-fns** - работа с датами
- **Lucide** - иконки
- **Vite** - сборщик

## Основные концепции

### Роли пользователей

**CREATOR (Создатель)**
- Все возможности операторов
- Смена ролей пользователей
- Просмотр статистики
- Высший уровень доступа

**OPERATOR (Оператор)**
- Просмотр всех пользователей
- Просмотр профилей и портфолио
- Смена паролей пользователей
- CRUD конкурсов
- Просмотр заявок

**USER (Обычный пользователь)**
- Регистрация на конкурсы
- Загрузка портфолио
- Просмотр конкурсов
- Редактирование профиля

### Файловое хранилище

- 5 ГБ на пользователя (настраивается)
- Файлы хранятся в `backend/uploads/:userId/`
- Проверка лимита при загрузке
- Удаление файла удаляет его с диска
- Публичный доступ через ссылку

### Безопасность

- JWT токены (срок действия 7 дней)
- Пароли хешируются bcrypt (10 раундов)
- Middleware проверяет роли
- CORS настроен
- Защита от SQL инъекций (Prisma)

## Скрипты

### Backend
```bash
npm run dev           # Разработка (tsx watch)
npm run build         # Сборка TypeScript
npm start             # Production запуск
npm run prisma:generate  # Генерация Prisma Client
npm run prisma:migrate   # Миграции БД
npm run prisma:seed      # Начальные данные
```

### Frontend
```bash
npm run dev           # Dev сервер (Vite)
npm run build         # Production сборка
npm run preview       # Просмотр сборки
```

## Переменные окружения

### Backend (.env)
```
DATABASE_URL          # PostgreSQL connection string
JWT_SECRET            # Секретный ключ JWT
PORT                  # Порт сервера (default: 3000)
UPLOAD_DIR            # Директория файлов (default: ./uploads)
MAX_STORAGE_GB        # Лимит хранилища (default: 5)
```

## Полезные команды

```bash
# Просмотр схемы БД
npx prisma studio

# Сброс БД
npx prisma migrate reset

# Форматирование schema.prisma
npx prisma format

# Проверка TypeScript
npm run build --dry-run
```

---

Проект полностью готов к разработке и деплою!

# 🚀 Быстрый старт

## Шаг 1: Установка PostgreSQL

Если PostgreSQL не установлен:

### Windows:
1. Скачайте установщик с https://www.postgresql.org/download/windows/
2. Установите PostgreSQL (запомните пароль для пользователя postgres)
3. По умолчанию сервер будет работать на порту 5432

### Создание базы данных:

Откройте pgAdmin или psql и выполните:

```sql
CREATE DATABASE contest_platform;
```

## Шаг 2: Настройка Backend

1. Откройте терминал в папке проекта
2. Перейдите в backend:

```bash
cd backend
```

3. Установите зависимости:

```bash
npm install
```

4. Отредактируйте файл `.env` если нужно (уже создан):
   - Измените `DATABASE_URL` если у вас другой пароль/порт PostgreSQL
   - Измените `JWT_SECRET` на случайную строку

5. Инициализируйте базу данных:

```bash
npm run prisma:generate
npm run prisma:migrate
npm run prisma:seed
```

6. Запустите backend:

```bash
npm run dev
```

Backend запустится на http://localhost:3000

## Шаг 3: Настройка Frontend

Откройте **НОВЫЙ** терминал (не закрывая backend):

1. Перейдите в frontend:

```bash
cd frontend
```

2. Установите зависимости:

```bash
npm install
```

3. Запустите frontend:

```bash
npm run dev
```

Frontend откроется на http://localhost:5173

## Шаг 4: Первый вход

Откройте браузер и перейдите на http://localhost:5173

Войдите с одним из предустановленных аккаунтов:

**Создатель:**
- Логин: `Damir`
- Пароль: `289950`

**Операторы:**
- Логин: `Nadejda`, Пароль: `456894`
- Логин: `Timur`, Пароль: `274856`
- Логин: `Anna`, Пароль: `095467`

## Шаг 5: Создание конкурса (для операторов)

1. Войдите как оператор или создатель
2. Перейдите в **Админ-панель**
3. Вкладка **Создать конкурс**
4. Заполните все поля
5. Нажмите **Создать конкурс**

## Проверка работы

✅ Backend работает: http://localhost:3000/health (должен вернуть {"status":"OK"})
✅ Frontend работает: http://localhost:5173
✅ База данных подключена: в консоли backend не должно быть ошибок подключения

## Возможные проблемы

### Ошибка подключения к базе данных

**Ошибка:** `Can't reach database server`

**Решение:**
1. Проверьте, что PostgreSQL запущен
2. Проверьте пароль в `.env` файле
3. Убедитесь, что база данных `contest_platform` создана

### Порт уже занят

**Ошибка:** `Port 3000 is already in use`

**Решение:**
1. Закройте программу, использующую порт 3000
2. Или измените порт в `backend/.env`: `PORT=3001`

### Frontend не видит Backend

**Ошибка:** `Network Error` при логине

**Решение:**
1. Проверьте, что backend запущен и работает
2. Проверьте http://localhost:3000/health в браузере
3. Убедитесь, что в `frontend/vite.config.ts` правильный proxy

## Команды для разработки

### Backend:
```bash
npm run dev        # Запуск с автоперезагрузкой
npm run build      # Сборка для production
npm start          # Запуск production версии
```

### Frontend:
```bash
npm run dev        # Запуск dev сервера
npm run build      # Сборка для production
npm run preview    # Просмотр production сборки
```

### Database:
```bash
npm run prisma:generate   # Генерация Prisma Client
npm run prisma:migrate    # Создание миграций
npm run prisma:seed       # Заполнение данными
```

## Дальнейшие шаги

1. Зарегистрируйте нового пользователя
2. Загрузите файлы в портфолио
3. Создайте конкурсы через админ-панель
4. Зарегистрируйтесь на конкурс
5. Поделитесь ссылкой на портфолио

---

**Нужна помощь?** Проверьте README.md для подробной документации.

# ⚡ Быстрый старт за 5 минут

## Минимальные требования
- Node.js 18+
- PostgreSQL 14+

## Шаги

### 1. Установите PostgreSQL и создайте БД

```sql
CREATE DATABASE contest_platform;
```

### 2. Запустите Backend

```bash
cd backend
npm install
npm run prisma:generate
npm run prisma:migrate
npm run prisma:seed
npm run dev
```

✅ Backend работает на http://localhost:3000

### 3. Запустите Frontend (новый терминал)

```bash
cd frontend
npm install
npm run dev
```

✅ Frontend работает на http://localhost:5173

### 4. Войдите в систему

Откройте http://localhost:5173

**Создатель:**
- Логин: `Damir`
- Пароль: `289950`

**Операторы:**
- `Nadejda` / `456894`
- `Timur` / `274856`
- `Anna` / `095467`

## Что дальше?

1. **Создайте конкурс** (Админ-панель → Создать конкурс)
2. **Загрузите файлы** (Портфолио → Загрузить файл)
3. **Зарегистрируйтесь на конкурс** (Конкурсы → Выбрать → Зарегистрироваться)

## Проблемы?

Смотрите [SETUP.md](SETUP.md) для детальной инструкции

## Документация

- [README.md](README.md) - Полная документация
- [API_DOCUMENTATION.md](API_DOCUMENTATION.md) - API endpoints
- [PROJECT_STRUCTURE.md](PROJECT_STRUCTURE.md) - Структура проекта
- [DEPLOY.md](DEPLOY.md) - Деплой в production

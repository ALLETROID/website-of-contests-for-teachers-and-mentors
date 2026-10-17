# ⚡ Загрузка на GitHub за 5 минут

## 🎯 Что получите

После выполнения этих шагов:
- ✅ Ваш код будет на GitHub
- ✅ Красивое портфолио проекта
- ✅ Возможность показать демо интерфейса онлайн

## 📋 Что нужно

1. Аккаунт на GitHub (https://github.com/signup)
2. Git установлен (проверьте: `git --version`)

---

## 🚀 Шаги

### Шаг 1: Создайте репозиторий на GitHub (1 мин)

1. Откройте https://github.com/new
2. **Repository name:** `contest-portfolio-platform`
3. **Public** ✅
4. **Create repository** ← нажмите

### Шаг 2: Загрузите код (2 мин)

Откройте PowerShell/Terminal и выполните:

```bash
cd C:\Users\user\Hacaton\contest-portfolio-platform

git init
git add .
git commit -m "Contest portfolio platform"
```

**Важно! Замените YOUR-USERNAME на ваш GitHub username:**

```bash
git remote add origin https://github.com/YOUR-USERNAME/contest-portfolio-platform.git
git branch -M main
git push -u origin main
```

Если попросит логин/пароль:
- **Username:** ваш GitHub username
- **Password:** используйте Personal Access Token (не пароль от GitHub!)
  - Создайте токен: https://github.com/settings/tokens/new
  - Права: ✅ repo

### Шаг 3: Включите GitHub Pages (1 мин)

1. Откройте ваш репозиторий на GitHub
2. **Settings** → **Pages**
3. **Source:** выберите **GitHub Actions**
4. Готово!

### Шаг 4: Обновите конфигурацию (1 мин)

Файл `frontend/vite.config.ts` уже обновлен!

Если ваш репозиторий называется по-другому:
1. Откройте `frontend/vite.config.ts`
2. Найдите строку: `base: '/contest-portfolio-platform/',`
3. Замените на название вашего репозитория

Затем:

```bash
git add .
git commit -m "Configure GitHub Pages"
git push
```

### Шаг 5: Дождитесь деплоя (2-3 мин)

1. Откройте ваш репозиторий
2. Вкладка **Actions**
3. Дождитесь зеленой галочки ✅

### Шаг 6: Откройте сайт! 🎉

```
https://YOUR-USERNAME.github.io/contest-portfolio-platform/
```

Замените YOUR-USERNAME на ваш username!

---

## ⚠️ Важно!

GitHub Pages показывает **только интерфейс**:
- ✅ Страницы работают
- ✅ Навигация работает
- ❌ Вход не работает (нет backend)
- ❌ Регистрация не работает
- ❌ Загрузка файлов не работает

**Это демо-версия для показа дизайна!**

Для полной версии нужен backend на хостинге.

---

## 🔄 Обновление сайта

При изменениях:

```bash
git add .
git commit -m "Описание изменений"
git push
```

Сайт обновится автоматически через 2-3 минуты!

---

## ❌ Проблемы?

### "Permission denied"
Используйте Personal Access Token вместо пароля:
https://github.com/settings/tokens/new

### Страница 404
1. Проверьте `base` в `frontend/vite.config.ts`
2. Он должен совпадать с названием репозитория
3. Формат: `/repo-name/` (со слешами)

### Сайт без стилей
Неверный `base` в `vite.config.ts` - проверьте название репозитория

---

## 📱 Поделиться

Отправьте ссылку на сайт:

```
https://YOUR-USERNAME.github.io/contest-portfolio-platform/
```

---

## 📚 Подробные инструкции

- **DEPLOY_CHECKLIST.md** - детальный чек-лист
- **GITHUB_PAGES_GUIDE.md** - полное руководство с вариантами
- **GIT_COMMANDS_CHEATSHEET.md** - шпаргалка Git команд

---

## ✅ Готово!

Ваш проект теперь на GitHub с красивым демо!

Для полной версии с backend → **GITHUB_PAGES_GUIDE.md** (Вариант 2 или 3)

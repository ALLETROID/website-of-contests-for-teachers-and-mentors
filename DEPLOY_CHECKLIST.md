# ✅ Чек-лист деплоя на GitHub Pages

## 📋 Что нужно сделать

### Подготовка (5 минут)

- [ ] 1. У вас есть аккаунт на GitHub? Если нет → https://github.com/signup
- [ ] 2. Git установлен на компьютере? Проверьте: `git --version`
      - Если нет → https://git-scm.com/downloads

### Создание репозитория (2 минуты)

- [ ] 3. Откройте https://github.com
- [ ] 4. Нажмите зеленую кнопку **"New"** (или + → New repository)
- [ ] 5. Заполните:
  - **Repository name:** `contest-portfolio-platform` (или любое другое)
  - **Description:** `Платформа для конкурсов с портфолио`
  - **Visibility:** ✅ Public (обязательно для бесплатного Pages)
  - ❌ НЕ добавляйте README, gitignore, license
- [ ] 6. Нажмите **"Create repository"**

### Загрузка кода (3 минуты)

Откройте PowerShell/Terminal в папке проекта:

```powershell
cd C:\Users\user\Hacaton\contest-portfolio-platform
```

Выполните команды по порядку:

- [ ] 7. Инициализация Git:
```bash
git init
```

- [ ] 8. Добавление файлов:
```bash
git add .
```

- [ ] 9. Первый коммит:
```bash
git commit -m "Initial commit: Contest portfolio platform"
```

- [ ] 10. Связывание с GitHub (замените YOUR-USERNAME на ваш username):
```bash
git remote add origin https://github.com/YOUR-USERNAME/contest-portfolio-platform.git
```

Пример: `git remote add origin https://github.com/ivan123/contest-portfolio-platform.git`

- [ ] 11. Отправка на GitHub:
```bash
git branch -M main
git push -u origin main
```

⚠️ Если попросит ввести логин/пароль - используйте Personal Access Token вместо пароля:
- Создайте токен: https://github.com/settings/tokens/new
- Разрешения: ✅ repo (полный доступ)
- Используйте токен вместо пароля

### Настройка GitHub Pages (2 минуты)

- [ ] 12. Откройте ваш репозиторий на GitHub
- [ ] 13. Перейдите в **Settings** (вкладка справа)
- [ ] 14. В левом меню найдите **Pages**
- [ ] 15. В разделе **Source** выберите:
  - **Source:** ✅ GitHub Actions (не Deploy from a branch!)
- [ ] 16. Нажмите **Save** если есть кнопка

### Обновление конфигурации (3 минуты)

- [ ] 17. Откройте файл `frontend/vite.config.ts` в редакторе

- [ ] 18. Найдите строку:
```typescript
export default defineConfig({
  plugins: [react()],
```

- [ ] 19. Добавьте после `plugins: [react()],`:
```typescript
  base: '/contest-portfolio-platform/',  // Замените на название ВАШЕГО репозитория
```

Должно получиться:
```typescript
export default defineConfig({
  plugins: [react()],
  base: '/contest-portfolio-platform/',
  server: {
    ...
```

⚠️ **Важно:** Если ваш репозиторий называется по-другому, используйте его название!

- [ ] 20. Сохраните файл

- [ ] 21. Закоммитьте изменения:
```bash
git add frontend/vite.config.ts
git commit -m "Configure base path for GitHub Pages"
git push
```

### Деплой (автоматический, 2-3 минуты)

- [ ] 22. Откройте ваш репозиторий на GitHub
- [ ] 23. Перейдите во вкладку **Actions** (вверху)
- [ ] 24. Дождитесь завершения workflow
  - 🟡 Желтый = в процессе
  - ✅ Зеленый = готово
  - ❌ Красный = ошибка (см. раздел "Решение проблем")

### Проверка сайта (1 минута)

- [ ] 25. Откройте браузер
- [ ] 26. Перейдите на:
```
https://YOUR-USERNAME.github.io/contest-portfolio-platform/
```

Замените YOUR-USERNAME на ваш GitHub username!

Пример: `https://ivan123.github.io/contest-portfolio-platform/`

---

## ⚠️ Важно понимать

GitHub Pages показывает только **frontend**:
- ✅ Интерфейс будет работать
- ✅ Страницы открываются
- ❌ Вход не работает (нет backend)
- ❌ Регистрация не работает (нет backend)
- ❌ Загрузка файлов не работает (нет backend)

**Это демо-версия только интерфейса!**

Для полной работы нужен backend на отдельном хостинге (см. GITHUB_PAGES_GUIDE.md)

---

## 🔧 Решение проблем

### ❌ Actions завершился с ошибкой

**Проблема:** Красная иконка в Actions

**Решение:**
1. Нажмите на failed workflow
2. Посмотрите логи (где ошибка)
3. Частая проблема: неверный путь в `base`
4. Исправьте, закоммитьте, запушьте снова

### ❌ Страница не открывается (404)

**Проблема:** `https://username.github.io/repo-name/` показывает 404

**Решения:**
1. Проверьте что `base` в `vite.config.ts` совпадает с названием репозитория
2. Проверьте что Pages включен в Settings → Pages
3. Проверьте что репозиторий Public
4. Подождите 5-10 минут (деплой может занять время)

### ❌ git push не работает

**Проблема:** `Permission denied` или `Authentication failed`

**Решение:**
1. Создайте Personal Access Token: https://github.com/settings/tokens/new
2. Скопируйте токен
3. При `git push` используйте токен вместо пароля
4. Username: ваш GitHub username
5. Password: вставьте токен

### ❌ Сайт открывается, но стили сломаны

**Проблема:** Страница белая или без стилей

**Решение:**
1. Неверный `base` в `vite.config.ts`
2. Проверьте что путь `/contest-portfolio-platform/` совпадает с репозиторием
3. Должен быть слеш в начале и в конце: `/repo-name/`

---

## 🔄 Обновление сайта

При изменениях в коде:

```bash
# 1. Внесите изменения в файлы
# 2. Добавьте изменения
git add .

# 3. Закоммитьте
git commit -m "Описание изменений"

# 4. Отправьте на GitHub
git push
```

Сайт обновится автоматически через 2-3 минуты!

---

## 📱 Поделиться сайтом

После успешного деплоя поделитесь ссылкой:

```
https://YOUR-USERNAME.github.io/contest-portfolio-platform/
```

⚠️ Напомните, что это только демо интерфейса без backend!

---

## ✅ Готово!

После прохождения всех пунктов у вас будет:
- ✅ Код на GitHub
- ✅ Автоматический деплой при изменениях
- ✅ Публичная ссылка на сайт
- ✅ Красивое портфолио для показа

Для полной версии с backend смотрите **GITHUB_PAGES_GUIDE.md** → Вариант 2 или 3

---

**Нужна помощь?** Напишите какой пункт не получается!

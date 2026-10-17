# 🚀 Шпаргалка Git команд

## 📤 Загрузка проекта на GitHub (первый раз)

### 1. Подготовка
```bash
cd C:\Users\user\Hacaton\contest-portfolio-platform
git init
git add .
git commit -m "Initial commit"
```

### 2. Связывание с GitHub
```bash
# Замените YOUR-USERNAME и REPO-NAME
git remote add origin https://github.com/YOUR-USERNAME/REPO-NAME.git
git branch -M main
git push -u origin main
```

---

## 🔄 Ежедневная работа

### Сохранение изменений
```bash
# Посмотреть что изменилось
git status

# Добавить все изменения
git add .

# Создать коммит
git commit -m "Описание изменений"

# Отправить на GitHub
git push
```

### Быстрая команда (все сразу)
```bash
git add . && git commit -m "Update" && git push
```

---

## 📥 Получение изменений

### Скачать изменения с GitHub
```bash
git pull
```

### Если конфликт
```bash
git pull --rebase
# Разрешите конфликты в файлах
git add .
git rebase --continue
git push
```

---

## 🌿 Работа с ветками

### Создать новую ветку
```bash
git checkout -b feature-name
```

### Переключиться на другую ветку
```bash
git checkout main
git checkout feature-name
```

### Список веток
```bash
git branch
```

### Удалить ветку
```bash
git branch -d feature-name
```

---

## 🔍 Просмотр истории

### История коммитов
```bash
git log
git log --oneline  # Компактный вид
```

### Изменения в файлах
```bash
git diff
```

---

## ↩️ Отмена изменений

### Отменить изменения в файле (до add)
```bash
git checkout -- filename
```

### Отменить git add (до commit)
```bash
git reset HEAD filename
```

### Отменить последний коммит (до push)
```bash
git reset --soft HEAD~1  # Изменения остаются
git reset --hard HEAD~1  # Изменения удаляются
```

---

## 🔧 Настройка Git

### Первая настройка
```bash
git config --global user.name "Ваше Имя"
git config --global user.email "your@email.com"
```

### Сохранение пароля (Windows)
```bash
git config --global credential.helper wincred
```

---

## 🔑 GitHub Token (вместо пароля)

Если Git просит пароль:

1. Создайте токен: https://github.com/settings/tokens/new
2. Разрешения: ✅ repo
3. Скопируйте токен
4. При `git push`:
   - Username: ваш GitHub username
   - Password: вставьте токен (НЕ пароль от GitHub)

---

## 📋 Полезные команды

### Проверка remote
```bash
git remote -v
```

### Изменение remote URL
```bash
git remote set-url origin https://github.com/USER/REPO.git
```

### Клонирование репозитория
```bash
git clone https://github.com/USER/REPO.git
```

### Игнорирование изменений в файле
```bash
# Добавьте путь в .gitignore
echo "filename" >> .gitignore
```

---

## ⚡ Быстрые сценарии

### Первая загрузка на GitHub
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/USER/REPO.git
git branch -M main
git push -u origin main
```

### Обновление кода
```bash
git add .
git commit -m "Update code"
git push
```

### Синхронизация с GitHub
```bash
git pull
```

### Откат к предыдущей версии
```bash
git log --oneline  # Найдите hash коммита
git checkout <hash> -- filename
```

---

## ❌ Решение проблем

### "Permission denied"
Используйте GitHub Token вместо пароля

### "Failed to push"
```bash
git pull --rebase
git push
```

### "Merge conflict"
1. Откройте файлы с конфликтами
2. Исправьте между `<<<<<<` и `>>>>>>`
3. Сохраните файлы
4. ```bash
   git add .
   git commit -m "Resolve conflicts"
   git push
   ```

### "Remote already exists"
```bash
git remote remove origin
git remote add origin https://github.com/USER/REPO.git
```

---

## 📚 Ресурсы

- Интерактивное обучение: https://learngitbranching.js.org/
- Официальная документация: https://git-scm.com/doc
- GitHub Guides: https://guides.github.com/

---

## 💡 Советы

1. **Коммитьте часто** - лучше много маленьких коммитов, чем один большой
2. **Пишите понятные сообщения** - не "Update", а "Add user authentication"
3. **Пушьте регулярно** - не теряйте код при поломке компьютера
4. **Используйте ветки** - для новых фич создавайте отдельные ветки
5. **Проверяйте git status** - перед коммитом всегда смотрите что добавляете

---

**Распечатайте и держите под рукой! 📌**

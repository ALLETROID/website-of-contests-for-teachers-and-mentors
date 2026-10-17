# 🚀 Деплой приложения

## Production деплой

### 1. Подготовка сервера

Требования:
- Ubuntu 20.04+ / Debian 11+
- Node.js 18+
- PostgreSQL 14+
- Nginx (опционально, для reverse proxy)

### 2. Установка зависимостей на сервере

```bash
# Обновление системы
sudo apt update && sudo apt upgrade -y

# Установка Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Установка PostgreSQL
sudo apt install postgresql postgresql-contrib -y

# Установка Nginx (опционально)
sudo apt install nginx -y
```

### 3. Настройка PostgreSQL

```bash
# Вход в PostgreSQL
sudo -u postgres psql

# Создание базы данных и пользователя
CREATE DATABASE contest_platform;
CREATE USER contest_user WITH ENCRYPTED PASSWORD 'secure_password_here';
GRANT ALL PRIVILEGES ON DATABASE contest_platform TO contest_user;
\q
```

### 4. Клонирование проекта

```bash
cd /var/www
git clone <your-repo-url> contest-platform
cd contest-platform
```

### 5. Настройка Backend

```bash
cd backend

# Установка зависимостей
npm ci --production

# Создание .env файла
nano .env
```

Содержимое `.env`:
```
DATABASE_URL="postgresql://contest_user:secure_password_here@localhost:5432/contest_platform?schema=public"
JWT_SECRET="your-super-secret-jwt-key-change-this-in-production"
PORT=3000
UPLOAD_DIR="./uploads"
MAX_STORAGE_GB=5
NODE_ENV=production
```

```bash
# Генерация Prisma Client
npm run prisma:generate

# Применение миграций
npm run prisma:migrate

# Заполнение начальными данными
npm run prisma:seed

# Сборка проекта
npm run build
```

### 6. Настройка Frontend

```bash
cd ../frontend

# Установка зависимостей
npm ci

# Создание production сборки
npm run build
```

### 7. Настройка PM2 для автозапуска Backend

```bash
# Установка PM2 глобально
sudo npm install -g pm2

cd /var/www/contest-platform/backend

# Запуск приложения через PM2
pm2 start dist/server.js --name contest-backend

# Автозапуск при перезагрузке системы
pm2 startup
pm2 save
```

### 8. Настройка Nginx

Создайте конфигурацию:

```bash
sudo nano /etc/nginx/sites-available/contest-platform
```

Содержимое файла:

```nginx
server {
    listen 80;
    server_name your-domain.com;

    # Frontend
    location / {
        root /var/www/contest-platform/frontend/dist;
        try_files $uri $uri/ /index.html;
    }

    # Backend API
    location /api {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Uploaded files
    location /uploads {
        alias /var/www/contest-platform/backend/uploads;
        expires 30d;
        add_header Cache-Control "public, immutable";
    }

    client_max_body_size 100M;
}
```

Активируйте конфигурацию:

```bash
sudo ln -s /etc/nginx/sites-available/contest-platform /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

### 9. Настройка SSL с Let's Encrypt

```bash
# Установка Certbot
sudo apt install certbot python3-certbot-nginx -y

# Получение SSL сертификата
sudo certbot --nginx -d your-domain.com

# Автообновление сертификата
sudo certbot renew --dry-run
```

### 10. Настройка Firewall

```bash
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
sudo ufw enable
```

## Docker деплой (альтернативный метод)

### Backend Dockerfile

Создайте `backend/Dockerfile`:

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --production

COPY . .
RUN npx prisma generate
RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
```

### Frontend Dockerfile

Создайте `frontend/Dockerfile`:

```dockerfile
FROM node:18-alpine as build

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### docker-compose.yml

В корне проекта:

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:14-alpine
    environment:
      POSTGRES_DB: contest_platform
      POSTGRES_USER: contest_user
      POSTGRES_PASSWORD: secure_password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  backend:
    build: ./backend
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgresql://contest_user:secure_password@postgres:5432/contest_platform?schema=public
      JWT_SECRET: your-secret-key
      PORT: 3000
    depends_on:
      - postgres
    volumes:
      - ./backend/uploads:/app/uploads

  frontend:
    build: ./frontend
    ports:
      - "80:80"
    depends_on:
      - backend

volumes:
  postgres_data:
```

Запуск:

```bash
docker-compose up -d
```

## Обновление приложения

```bash
cd /var/www/contest-platform

# Получение обновлений
git pull

# Backend
cd backend
npm install
npm run prisma:generate
npm run prisma:migrate
npm run build
pm2 restart contest-backend

# Frontend
cd ../frontend
npm install
npm run build

# Перезапуск Nginx
sudo systemctl restart nginx
```

## Мониторинг

### Просмотр логов PM2

```bash
pm2 logs contest-backend
pm2 monit
```

### Просмотр логов Nginx

```bash
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log
```

## Бэкап базы данных

```bash
# Создание бэкапа
pg_dump -U contest_user contest_platform > backup_$(date +%Y%m%d).sql

# Восстановление
psql -U contest_user contest_platform < backup_20240101.sql
```

## Безопасность

1. Измените все пароли и секретные ключи
2. Используйте HTTPS (SSL)
3. Настройте firewall
4. Регулярно обновляйте систему и зависимости
5. Ограничьте доступ к PostgreSQL только с localhost
6. Используйте strong passwords для пользователей БД
7. Настройте регулярные бэкапы

## Производительность

1. Настройте кэширование Nginx
2. Оптимизируйте запросы к БД
3. Используйте CDN для статики
4. Настройте gzip сжатие
5. Мониторьте использование ресурсов

---

Для вопросов обращайтесь к документации или создайте issue.

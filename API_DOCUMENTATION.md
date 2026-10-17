# 📚 API Документация

Базовый URL: `http://localhost:3000/api`

## Аутентификация

Большинство endpoints требуют JWT токен в заголовке:

```
Authorization: Bearer <your-jwt-token>
```

---

## 🔐 Auth Routes

### Регистрация пользователя

**POST** `/auth/register`

**Body:**
```json
{
  "username": "string (required)",
  "email": "string (required)",
  "password": "string (required, min 6 chars)",
  "firstName": "string (optional)",
  "lastName": "string (optional)",
  "middleName": "string (optional)"
}
```

**Response (201):**
```json
{
  "token": "jwt-token",
  "user": {
    "id": "uuid",
    "username": "string",
    "email": "string",
    "role": "USER",
    "firstName": "string | null",
    "lastName": "string | null",
    "middleName": "string | null"
  }
}
```

### Вход

**POST** `/auth/login`

**Body:**
```json
{
  "login": "string (username or email)",
  "password": "string"
}
```

**Response (200):**
```json
{
  "token": "jwt-token",
  "user": {
    "id": "uuid",
    "username": "string",
    "email": "string",
    "role": "CREATOR | OPERATOR | USER",
    "firstName": "string | null",
    "lastName": "string | null",
    "middleName": "string | null"
  }
}
```

### Получение текущего пользователя

**GET** `/auth/me`

**Headers:** `Authorization: Bearer <token>`

**Response (200):**
```json
{
  "id": "uuid",
  "username": "string",
  "email": "string",
  "role": "CREATOR | OPERATOR | USER",
  "firstName": "string | null",
  "lastName": "string | null",
  "middleName": "string | null",
  "createdAt": "ISO date"
}
```

---

## 👤 User Routes

### Получение профиля пользователя

**GET** `/users/profile/:id`

**Headers:** `Authorization: Bearer <token>`

**Response (200):**
```json
{
  "id": "uuid",
  "username": "string",
  "email": "string",
  "role": "string",
  "firstName": "string | null",
  "lastName": "string | null",
  "middleName": "string | null",
  "createdAt": "ISO date",
  "files": [
    {
      "id": "uuid",
      "filename": "string",
      "originalName": "string",
      "mimetype": "string",
      "size": "number",
      "createdAt": "ISO date"
    }
  ]
}
```

### Обновление профиля

**PUT** `/users/profile`

**Headers:** `Authorization: Bearer <token>`

**Body:**
```json
{
  "firstName": "string (optional)",
  "lastName": "string (optional)",
  "middleName": "string (optional)",
  "email": "string (optional)"
}
```

**Response (200):**
```json
{
  "id": "uuid",
  "username": "string",
  "email": "string",
  "role": "string",
  "firstName": "string | null",
  "lastName": "string | null",
  "middleName": "string | null"
}
```

---

## 🏆 Contest Routes

### Получение списка конкурсов

**GET** `/contests`

**Query Parameters:**
- `category` (optional): ART | MUSIC | DANCE | LITERATURE | PHOTOGRAPHY | VIDEO | DESIGN | TECHNOLOGY | OTHER
- `ageGroup` (optional): UNDER_12 | AGES_12_15 | AGES_16_18 | AGES_19_25 | OVER_25
- `startDate` (optional): ISO date string
- `endDate` (optional): ISO date string

**Response (200):**
```json
[
  {
    "id": "uuid",
    "title": "string",
    "description": "string",
    "imageUrl": "string | null",
    "organizer": "string",
    "category": "enum",
    "ageGroup": "enum",
    "startDate": "ISO date",
    "endDate": "ISO date",
    "tasks": "string",
    "createdAt": "ISO date",
    "_count": {
      "registrations": "number"
    }
  }
]
```

### Получение конкурса по ID

**GET** `/contests/:id`

**Response (200):**
```json
{
  "id": "uuid",
  "title": "string",
  "description": "string",
  "imageUrl": "string | null",
  "organizer": "string",
  "category": "enum",
  "ageGroup": "enum",
  "startDate": "ISO date",
  "endDate": "ISO date",
  "tasks": "string",
  "createdAt": "ISO date",
  "_count": {
    "registrations": "number"
  }
}
```

### Регистрация на конкурс

**POST** `/contests/:id/register`

**Headers:** `Authorization: Bearer <token>`

**Response (201):**
```json
{
  "id": "uuid",
  "userId": "uuid",
  "contestId": "uuid",
  "createdAt": "ISO date",
  "contest": { ... }
}
```

### Получение своих регистраций

**GET** `/contests/my/registrations`

**Headers:** `Authorization: Bearer <token>`

**Response (200):**
```json
[
  {
    "id": "uuid",
    "userId": "uuid",
    "contestId": "uuid",
    "createdAt": "ISO date",
    "contest": { ... }
  }
]
```

---

## 📁 File Routes

### Получение списка файлов

**GET** `/files`

**Headers:** `Authorization: Bearer <token>`

**Response (200):**
```json
{
  "files": [
    {
      "id": "uuid",
      "filename": "string",
      "originalName": "string",
      "mimetype": "string",
      "size": "number",
      "createdAt": "ISO date"
    }
  ],
  "totalSize": "number (bytes)",
  "maxSize": "number (bytes)",
  "usedPercent": "number"
}
```

### Загрузка файла

**POST** `/files/upload`

**Headers:** 
- `Authorization: Bearer <token>`
- `Content-Type: multipart/form-data`

**Body:**
- `file`: File (max 100MB per file)

**Response (201):**
```json
{
  "id": "uuid",
  "filename": "string",
  "originalName": "string",
  "mimetype": "string",
  "size": "number",
  "path": "string",
  "createdAt": "ISO date"
}
```

### Удаление файла

**DELETE** `/files/:id`

**Headers:** `Authorization: Bearer <token>`

**Response (200):**
```json
{
  "message": "File deleted successfully"
}
```

### Скачивание файла

**GET** `/files/download/:id`

**Headers:** `Authorization: Bearer <token>`

**Response:** File download

### Публичное портфолио

**GET** `/files/portfolio/:userId`

**Response (200):**
```json
{
  "id": "uuid",
  "username": "string",
  "firstName": "string | null",
  "lastName": "string | null",
  "middleName": "string | null",
  "files": [ ... ]
}
```

---

## 🔧 Admin Routes

**Требуется роль:** OPERATOR или CREATOR

### Получение всех пользователей

**GET** `/admin/users`

**Headers:** `Authorization: Bearer <token>`

**Response (200):**
```json
[
  {
    "id": "uuid",
    "username": "string",
    "email": "string",
    "role": "enum",
    "firstName": "string | null",
    "lastName": "string | null",
    "middleName": "string | null",
    "createdAt": "ISO date",
    "_count": {
      "files": "number",
      "registrations": "number"
    }
  }
]
```

### Получение деталей пользователя

**GET** `/admin/users/:id`

**Headers:** `Authorization: Bearer <token>`

**Response (200):**
```json
{
  "id": "uuid",
  "username": "string",
  "email": "string",
  "role": "enum",
  "firstName": "string | null",
  "lastName": "string | null",
  "middleName": "string | null",
  "createdAt": "ISO date",
  "updatedAt": "ISO date",
  "files": [ ... ],
  "registrations": [ ... ],
  "storage": {
    "totalSize": "number",
    "maxSize": "number",
    "usedPercent": "number"
  }
}
```

### Изменение пароля пользователя

**PUT** `/admin/users/:id/password`

**Headers:** `Authorization: Bearer <token>`

**Body:**
```json
{
  "password": "string (min 6 chars)"
}
```

**Response (200):**
```json
{
  "message": "Password updated successfully"
}
```

### Изменение роли пользователя

**PUT** `/admin/users/:id/role`

**Headers:** `Authorization: Bearer <token>`

**Требуется роль:** CREATOR только

**Body:**
```json
{
  "role": "CREATOR | OPERATOR | USER"
}
```

**Response (200):**
```json
{
  "id": "uuid",
  "username": "string",
  "email": "string",
  "role": "enum",
  "firstName": "string | null",
  "lastName": "string | null",
  "middleName": "string | null"
}
```

### Создание конкурса

**POST** `/admin/contests`

**Headers:** `Authorization: Bearer <token>`

**Body:**
```json
{
  "title": "string (required)",
  "description": "string (required)",
  "imageUrl": "string (optional)",
  "organizer": "string (required)",
  "category": "enum (required)",
  "ageGroup": "enum (required)",
  "startDate": "ISO date (required)",
  "endDate": "ISO date (required)",
  "tasks": "string (required)"
}
```

**Response (201):**
```json
{
  "id": "uuid",
  "title": "string",
  ...
}
```

### Обновление конкурса

**PUT** `/admin/contests/:id`

**Headers:** `Authorization: Bearer <token>`

**Body:** Любые поля из создания конкурса (все опциональны)

**Response (200):**
```json
{
  "id": "uuid",
  ...
}
```

### Удаление конкурса

**DELETE** `/admin/contests/:id`

**Headers:** `Authorization: Bearer <token>`

**Response (200):**
```json
{
  "message": "Contest deleted successfully"
}
```

### Получение заявок на конкурс

**GET** `/admin/contests/:id/registrations`

**Headers:** `Authorization: Bearer <token>`

**Response (200):**
```json
[
  {
    "id": "uuid",
    "userId": "uuid",
    "contestId": "uuid",
    "createdAt": "ISO date",
    "user": {
      "id": "uuid",
      "username": "string",
      "email": "string",
      "firstName": "string | null",
      "lastName": "string | null",
      "middleName": "string | null",
      "files": [ ... ]
    }
  }
]
```

### Статистика

**GET** `/admin/stats`

**Headers:** `Authorization: Bearer <token>`

**Требуется роль:** CREATOR только

**Response (200):**
```json
{
  "users": "number",
  "contests": "number",
  "registrations": "number",
  "files": "number",
  "usersByRole": [
    {
      "role": "enum",
      "_count": "number"
    }
  ]
}
```

---

## ❌ Коды ошибок

- **400** - Bad Request (неверные данные)
- **401** - Unauthorized (не авторизован)
- **403** - Forbidden (недостаточно прав)
- **404** - Not Found (ресурс не найден)
- **500** - Internal Server Error (ошибка сервера)

**Формат ошибки:**
```json
{
  "error": "Error message description"
}
```

---

## 📝 Примеры использования

### JavaScript (Axios)

```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: 'http://localhost:3000/api',
});

// Вход
const { data } = await api.post('/auth/login', {
  login: 'Damir',
  password: '289950',
});

// Установка токена
api.defaults.headers.common['Authorization'] = `Bearer ${data.token}`;

// Получение конкурсов
const contests = await api.get('/contests');

// Загрузка файла
const formData = new FormData();
formData.append('file', fileInput.files[0]);
await api.post('/files/upload', formData);
```

### cURL

```bash
# Вход
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"login": "Damir", "password": "289950"}'

# Получение конкурсов с токеном
curl -X GET http://localhost:3000/api/contests \
  -H "Authorization: Bearer <your-token>"

# Загрузка файла
curl -X POST http://localhost:3000/api/files/upload \
  -H "Authorization: Bearer <your-token>" \
  -F "file=@/path/to/file.pdf"
```

---

Для вопросов обращайтесь к основной документации в README.md

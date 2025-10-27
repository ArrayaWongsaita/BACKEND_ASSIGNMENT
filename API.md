# 🏥 Health Tracker API Documentation

## 📑 API Endpoints Summary

| Method             | Endpoint                     | Description                       | Access |
| ------------------ | ---------------------------- | --------------------------------- | ------ |
| **Authentication** |                              |                                   |        |
| POST               | `/auth/register/doctor`      | ลงทะเบียนแพทย์                    | Public |
| POST               | `/auth/register/user`        | ลงทะเบียนผู้ป่วย                  | Public |
| POST               | `/auth/login/doctor`         | เข้าสู่ระบบแพทย์                  | Public |
| POST               | `/auth/login/user`           | เข้าสู่ระบบผู้ป่วย                | Public |
| **Users**          |                              |                                   |        |
| GET                | `/users/me`                  | ดูข้อมูลตัวเอง                    | User   |
| PUT                | `/users/me`                  | แก้ไขข้อมูลตัวเอง                 | User   |
| **Doctor**         |                              |                                   |        |
| GET                | `/doctors/me`                | ดูข้อมูลตัวเอง                    | Doctor |
| PUT                | `/doctors/me`                | แก้ไขข้อมูลตัวเอง                 | Doctor |
| **Health Records** |                              |                                   |        |
| POST               | `/health-records`            | สร้างบันทึกสุขภาพ                 | User   |
| GET                | `/health-records`            | ดูบันทึกสุขภาพทั้งหมด             | User   |
| GET                | `/health-records/:id`        | ดูบันทึกสุขภาพเฉพาะ               | User   |
| PUT                | `/health-records/:id`        | แก้ไขบันทึกสุขภาพ                 | User   |
| DELETE             | `/health-records/:id`        | ลบบันทึกสุขภาพ                    | User   |
| **Doctor Notes**   |                              |                                   |        |
| POST               | `/doctor-notes`              | สร้างบันทึกให้ผู้ป่วย             | Doctor |
| GET                | `/doctor-notes/my-notes`     | ดูบันทึกที่เขียนทั้งหมด           | Doctor |
| GET                | `/doctor-notes/user/:userId` | ดูบันทึกที่เขียนให้ผู้ป่วยคนหนึ่ง | Doctor |
| GET                | `/doctor-notes/received`     | ดูบันทึกที่ได้รับจากหมอ           | User   |
| PUT                | `/doctor-notes/:id`          | แก้ไขบันทึก                       | Doctor |
| DELETE             | `/doctor-notes/:id`          | ลบบันทึก                          | Doctor |

## 🔐 Authentication

### Register Doctor

```http
POST /auth/register/doctor
```

**Request Body**

```json
{
  "username": "string",
  "password": "string",
  "specialization": "string"
}
```

### Register User

```http
POST /auth/register/user
```

**Request Body**

```json
{
  "username": "string",
  "password": "string"
}
```

### Login Doctor

```http
POST /auth/login/doctor
```

**Request Body**

```json
{
  "username": "string",
  "password": "string"
}
```

**Response**

```json
{
  "success": true,
  "token": "string",
  "doctor": {
    "id": "number",
    "username": "string",
    "specialization": "string"
  }
}
```

### Login User

```http
POST /auth/login/user
```

**Request Body**

```json
{
  "username": "string",
  "password": "string"
}
```

**Response**

```json
{
  "success": true,
  "token": "string",
  "user": {
    "id": "number",
    "username": "string"
  }
}
```

## 🔐 Authentication Implementation Options

### การแยก Login Endpoints

ระบบใช้ **Separate Login Endpoints** สำหรับ Doctor และ User (`/auth/login/doctor` และ `/auth/login/user`)

### 💡 Implementation Options

สามารถ implement authentication ได้หลายวิธี เช่น:

#### วิธีที่ 1: แยก Secret Key

```env
JWT_SECRET_DOCTOR=doctor-secret-key-here
JWT_SECRET_USER=user-secret-key-here
```

#### วิธีที่ 2: เพิ่ม Role ใน JWT Payload

```json
{
  "userId": "number",
  "username": "string",
  "role": "doctor" | "user",
  "iat": "timestamp",
  "exp": "timestamp"
}
```

#### วิธีที่ 3: แยก Database Tables

- Doctor login → ค้นหาใน `Doctor` table
- User login → ค้นหาใน `User` table

#### วิธีที่ 4: เพิ่ม Additional Claims

```json
// Doctor Token
{
  "userId": 1,
  "role": "doctor",
  "specialization": "string",
  "permissions": ["read_notes", "write_notes"]
}

// User Token
{
  "userId": 15,
  "role": "user",
  "permissions": ["read_own_records", "write_own_records"]
}
```

## �👤 User Management

### Get User Profile

```http
GET /users/me
```

**Response**

```json
{
  "id": "number",
  "username": "string"
}
```

### Update User Profile

```http
PUT /users/me
```

**Request Body**

```json
{
  "username": "string",
  "password": "string"
}
```

## 👨‍⚕️ Doctor Management

### Get Doctor Profile

```http
GET /doctors/me
```

**Response**

```json
{
  "id": "number",
  "username": "string",
  "specialization": "string"
}
```

## 📊 Health Records

### Create Health Record

```http
POST /health-records
```

**Request Body**

```json
{
  "type": "string",
  "value": "string"
}
```

### Get User's Health Records

```http
GET /health-records
```

**Query Parameters**

- `type`: กรองตามประเภท
- `from`: วันที่เริ่มต้น (YYYY-MM-DD)
- `to`: วันที่สิ้นสุด (YYYY-MM-DD)

## 📝 Doctor Notes

### Create Note

```http
POST /doctor-notes
```

**Request Body**

```json
{
  "userId": "number",
  "note": "string"
}
```

### Get Notes (Doctor)

```http
GET /doctor-notes/my-notes
```

### Get Notes For Patient (Doctor)

```http
GET /doctor-notes/user/:userId
```

### Get Received Notes (User)

```http
GET /doctor-notes/received
```

## ⚠️ Error Responses

### 401 Unauthorized

```json
{
  "error": "Authentication required"
}
```

### 403 Forbidden

```json
{
  "error": "You don't have permission to perform this action"
}
```

### 404 Not Found

```json
{
  "error": "Resource not found"
}
```

## 📝 Notes

1. ทุก protected route ต้องส่ง token ใน header:
   ```
   Authorization: Bearer <token>
   ```
2. Error responses จะมี HTTP status code ที่เหมาะสม

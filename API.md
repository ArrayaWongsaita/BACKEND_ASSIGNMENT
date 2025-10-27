# 🏥 Health Tracker API Documentation

## 📑 API Endpoints Summary

| No. | Method             | Endpoint                     | Description                       | Access | Points |
| --- | ------------------ | ---------------------------- | --------------------------------- | ------ | ------ |
|     | **Authentication** |                              |                                   |        |        |
| 1   | POST               | `/auth/register/doctor`      | ลงทะเบียนแพทย์                    | Public | 10     |
| 2   | POST               | `/auth/register/user`        | ลงทะเบียนผู้ป่วย                  | Public | 10     |
| 3   | POST               | `/auth/login/doctor`         | เข้าสู่ระบบแพทย์                  | Public | 10     |
| 4   | POST               | `/auth/login/user`           | เข้าสู่ระบบผู้ป่วย                | Public | 10     |
|     | **User & Doctor**  |                              |                                   |        |        |
| 5   | GET                | `/users/me`                  | ดูข้อมูลตัวเอง                    | User   | 10     |
| 6   | PUT                | `/users/me`                  | แก้ไขข้อมูลตัวเอง                 | User   | 10     |
| 7   | GET                | `/doctors/me`                | ดูข้อมูลตัวเอง                    | Doctor | 10     |
| 8   | PUT                | `/doctors/me`                | แก้ไขข้อมูลตัวเอง                 | Doctor | 10     |
|     | **Health Records** |                              |                                   |        |        |
| 9   | POST               | `/health-records`            | สร้างบันทึกสุขภาพ                 | User   | 10     |
| 10  | GET                | `/health-records`            | ดูบันทึกสุขภาพทั้งหมด             | User   | 10     |
| 11  | GET                | `/health-records/:id`        | ดูบันทึกสุขภาพเฉพาะ               | User   | 10     |
| 12  | PUT                | `/health-records/:id`        | แก้ไขบันทึกสุขภาพ                 | User   | 10     |
| 13  | DELETE             | `/health-records/:id`        | ลบบันทึกสุขภาพ                    | User   | 10     |
|     | **Doctor Notes**   |                              |                                   |        |        |
| 14  | POST               | `/doctor-notes`              | สร้างบันทึกให้ผู้ป่วย             | Doctor | 10     |
| 15  | GET                | `/doctor-notes/my-notes`     | ดูบันทึกที่เขียนทั้งหมด           | Doctor | 10     |
| 16  | GET                | `/doctor-notes/user/:userId` | ดูบันทึกที่เขียนให้ผู้ป่วยคนหนึ่ง | Doctor | 10     |
| 17  | GET                | `/doctor-notes/received`     | ดูบันทึกที่ได้รับจากหมอ           | User   | 10     |
| 18  | PUT                | `/doctor-notes/:id`          | แก้ไขบันทึก                       | Doctor | 10     |
| 19  | DELETE             | `/doctor-notes/:id`          | ลบบันทึก                          | Doctor | 10     |

**รวม 19 endpoints = 190 คะแนน**

## 🔐 Authentication

### 1. Register Doctor

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

### 2. Register User

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

### 3. Login Doctor

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

### 4. Login User

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

### 5. Get User Profile

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

### 6. Update User Profile

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

### 7. Get Doctor Profile

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

### 8. Update Doctor Profile

```http
PUT /doctors/me
```

**Request Body**

```json
{
  "username": "string",
  "password": "string",
  "specialization": "string"
}
```

## 📊 Health Records

### 9. Create Health Record

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

### 10. Get User's Health Records

```http
GET /health-records
```

**Query Parameters**

- `type`: กรองตามประเภท
- `from`: วันที่เริ่มต้น (YYYY-MM-DD)
- `to`: วันที่สิ้นสุด (YYYY-MM-DD)

### 11. Get Health Record by ID

```http
GET /health-records/:id
```

**Response**

```json
{
  "id": "number",
  "userId": "number",
  "type": "string",
  "value": "string",
  "date": "datetime"
}
```

### 12. Update Health Record

```http
PUT /health-records/:id
```

**Request Body**

```json
{
  "type": "string",
  "value": "string"
}
```

### 13. Delete Health Record

```http
DELETE /health-records/:id
```

**Response**

```json
{
  "success": true,
  "message": "Health record deleted successfully"
}
```

## 📝 Doctor Notes

### 14. Create Note

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

### 15. Get Notes (Doctor)

```http
GET /doctor-notes/my-notes
```

### 16. Get Notes For Patient (Doctor)

```http
GET /doctor-notes/user/:userId
```

### 17. Get Received Notes (User)

```http
GET /doctor-notes/received
```

### 18. Update Doctor Note

```http
PUT /doctor-notes/:id
```

**Request Body**

```json
{
  "note": "string"
}
```

**Response**

```json
{
  "id": "number",
  "doctorId": "number",
  "userId": "number",
  "note": "string",
  "createdAt": "datetime"
}
```

### 19. Delete Doctor Note

```http
DELETE /doctor-notes/:id
```

**Response**

```json
{
  "success": true,
  "message": "Doctor note deleted successfully"
}
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

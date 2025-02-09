# 📝 เกณฑ์การให้คะแนน Health Tracker API

## 🎯 คะแนนรวม 150 คะแนน

### 1. การออกแบบและจัดการโครงสร้างโปรเจกต์ (20 คะแนน)

- [ ] จัดโครงสร้างโฟลเดอร์ถูกต้อง (5 คะแนน)
  ```
  📁 src/
    📁 config/
    📁 controllers/
    📁 middlewares/
    📁 routes/
    📁 utils/
    📄 app.js
  ```
- [ ] แยก routes และ controllers (5 คะแนน)
- [ ] มี Error Handling (5 คะแนน)
- [ ] ใช้ Environment Variables (5 คะแนน)

### 2. การออกแบบและจัดการ Database (20 คะแนน)

- [ ] สร้าง Models ครบถ้วน (10 คะแนน)
  - Doctor Model
  - User Model
  - HealthRecord Model
  - DoctorNote Model
- [ ] กำหนด Relations ถูกต้อง (5 คะแนน)
- [ ] ใช้ Data Types เหมาะสม (5 คะแนน)

### 3. การพัฒนา API Endpoints (100 คะแนน)

แต่ละ endpoint มีคะแนน 10 คะแนน:

#### Authentication (30 คะแนน)

- [ ] POST /auth/register/doctor (10 คะแนน)
- [ ] POST /auth/register/user (10 คะแนน)
- [ ] POST /auth/login (10 คะแนน)

#### User & Health Records (40 คะแนน)

- [ ] GET /users/me (10 คะแนน)
- [ ] POST /health-records (10 คะแนน)
- [ ] GET /health-records (10 คะแนน)
- [ ] GET /health-records/:id (10 คะแนน)

#### Doctor Notes (30 คะแนน)

- [ ] POST /doctor-notes (10 คะแนน)
- [ ] GET /doctor-notes/my-notes (10 คะแนน)
- [ ] GET /doctor-notes/received (10 คะแนน)

### 4. Authentication & Security (10 คะแนน)

- [ ] เข้ารหัสรหัสผ่านด้วย bcryptjs (3 คะแนน)
- [ ] สร้างและตรวจสอบ JWT token (4 คะแนน)
- [ ] ตรวจสอบสิทธิ์ผู้ใช้ (3 คะแนน)

## 🚫 หัก/ตัดคะแนน

- ไม่มี Error Handling (-10 คะแนน)
- ไม่ Validate Input (-5 คะแนน)
- โค้ดไม่เป็นระเบียบ (-5 คะแนน)
- ไม่ใช้ Environment Variables (-5 คะแนน)

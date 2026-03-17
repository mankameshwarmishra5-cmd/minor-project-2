# 🎓 SOET Resource Portal

<div align="center">

![KR Mangalam University](https://img.shields.io/badge/KR%20Mangalam%20University-SOET-003399?style=for-the-badge&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-18+-339933?style=for-the-badge&logo=node.js&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Express.js](https://img.shields.io/badge/Express.js-4.x-000000?style=for-the-badge&logo=express&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white)

**A secure, role-based academic resource management portal for the School of Engineering & Technology.**

[Features](#-features) · [Tech Stack](#-tech-stack) · [Getting Started](#-getting-started) · [Project Structure](#-project-structure) · [Database](#-database) · [API Routes](#-api-routes) · [Team](#-team)

</div>

---

## 📌 About The Project

The **SOET Resource Portal** is a full-stack web application built for **KR Mangalam University's School of Engineering & Technology**. It provides a single, authenticated platform where:

- **Students** access semester-wise study materials and previous year question papers (PYQs) for their enrolled subjects
- **Faculty** upload and manage unit-wise study materials and PYQs for their assigned subjects
- **Admins** oversee all uploads, manage content, and have full visibility across all courses

The portal was built as a prototype using dummy data (1,200+ students, 17 faculty, 70+ subjects) since official university database access was not available.

---

## ✨ Features

### 👨‍🎓 Student Features
- **Microsoft Outlook Login** via Azure AD OAuth2 — only `@krmu.edu.in` accounts allowed
- **Auto Semester Calculation** — current semester computed from enrollment year and current date
- **Personalised Dashboard** — profile card, degree progress bar, subject cards, quick stats
- **Study Materials** — unit-wise organised PDFs (Unit 1–6) per subject
- **Previous Year Papers** — filterable by year, semester type (odd/even), and exam type (Mid-Term/End-Term)
- **Subject Search** — real-time search/filter on dashboard subjects
- **Attendance & Exam Schedule** placeholders (ready for ERP integration)

### 👨‍🏫 Faculty Features
- **Manual Login** with email and bcrypt-hashed password
- **Upload Study Materials** — drag-and-drop PDF upload, unit picker, subject search
- **Upload PYQs** — exam type picker (Mid-Term / End-Term), year, and semester type
- **Subject Authorization** — faculty can only upload for their mapped subjects
- **Recent Uploads** — clickable PDFs with timestamps in dashboard
- **Duplicate PYQ Prevention** — same subject + year + exam type blocked
- **Delete Own Uploads** — with file removal from disk

### 🛡️ Admin Features
- All faculty powers plus full system oversight
- View ALL uploads across ALL subjects and ALL faculty
- Delete any upload (not just own)
- Admin Panel with subject-code filter
- Total students stat card

### 🔒 Security Features
- **Rate Limiting** — 10 login attempts per 15 minutes (brute force protection)
- **JWT in httpOnly Cookie** — JavaScript cannot access the token
- **XSS Protection** — `xss-clean` sanitizes all inputs
- **NoSQL Injection Protection** — `express-mongo-sanitize`
- **Security Headers** — Helmet.js (CSP, HSTS, X-Frame-Options, etc.)
- **Secure Logout** — Cookie cleared + `no-cache` headers (back button blocked)
- **File Validation** — PDF MIME type + extension check + 20MB size limit
- **Password Strength** — enforced on password change (uppercase + lowercase + number + special char)

---

## 🛠 Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Runtime | Node.js 18+ | Server-side JavaScript |
| Framework | Express.js 4.x | Routing and middleware |
| Database | MongoDB + Mongoose | Data storage and schema modeling |
| Templating | EJS | Server-rendered dynamic HTML |
| Frontend | Bootstrap 5.3 | Responsive UI components |
| Auth (Students) | Microsoft Azure AD OAuth2 | Outlook login via Microsoft Graph API |
| Auth (Faculty) | JWT + bcrypt.js | Token-based sessions + password hashing |
| File Upload | Multer | PDF handling with type/size validation |
| Security | Helmet, xss-clean, express-mongo-sanitize, express-rate-limit | Multi-layer protection |
| UI Extras | Select2, Bootstrap Icons | Searchable dropdowns + icon set |
| Compression | compression | Gzip responses |
| Unique IDs | uuid | UUID-named uploaded files |

---

## 🚀 Getting Started

### Prerequisites

Make sure the following are installed on your machine:

- [Node.js](https://nodejs.org/) v18 or higher
- [MongoDB](https://www.mongodb.com/try/download/community) (Community Server)
- [Git](https://git-scm.com/)

Check your versions:
```bash
node --version    # should be v18+
npm --version     # should be 8+
mongod --version  # should be v6+
```

### Installation

**1. Clone the repository**
```bash
git clone https://github.com/YourUsername/soet-portal.git
cd soet-portal
```

**2. Install dependencies**
```bash
npm install
```

**3. Set up environment variables**

Create a `.env` file in the root directory:
```env
PORT=3000
NODE_ENV=development
MONGO_URI=mongodb://localhost:27017/soet_portal
JWT_SECRET=soet_kr_mangalam_super_secret_2024_!@#
JWT_EXPIRES_IN=7d

# Microsoft Azure AD (for student Outlook login)
AZURE_CLIENT_ID=your_azure_client_id
AZURE_CLIENT_SECRET=your_azure_client_secret
AZURE_TENANT_ID=your_azure_tenant_id
AZURE_REDIRECT_URI=http://localhost:3000/auth/azure/callback
AZURE_SCOPE=openid profile email User.Read
```

> See [Azure Setup](#azure-ad-setup) below to get your Azure credentials.

**4. Start MongoDB**
```bash
# Windows
mongod

# macOS/Linux
sudo systemctl start mongod
```

**5. Seed the database**
```bash
npm run seed
```

Expected output:
```
✅ MongoDB Connected
📚 Seeding courses...     ✅ 6 courses inserted
📖 Seeding subjects...    ✅ 70+ subjects inserted
👨‍🏫 Seeding faculty...     ✅ 17 faculty inserted
🎓 Seeding students...    ✅ 1201 students inserted
🔗 Seeding mappings...    ✅ 200+ mappings inserted

🎉 SOET Portal Database Seeded!
🔐 Faculty login password: Faculty@123
```

**6. Start the development server**
```bash
npm run dev
```

Open your browser at **http://localhost:3000**

---

## 🔑 Demo Login Credentials

### Faculty / Admin Accounts
All faculty accounts use the password: `Faculty@123`

| Name | Email | Role |
|------|-------|------|
| Dr. Rajesh Sharma | dr.rajesh.sharma@krmu.edu.in | **Admin** |
| Dr. Priya Verma | dr.priya.verma@krmu.edu.in | Faculty |
| Dr. Anil Kumar | dr.anil.kumar@krmu.edu.in | Faculty |
| Dr. Kavita Singh | dr.kavita.singh@krmu.edu.in | Faculty |
| Ms. Ananya Mishra | ms.ananya.mishra@krmu.edu.in | Faculty |
| Mr. Vikram Chauhan | mr.vikram.chauhan@krmu.edu.in | Faculty |

### Student Login
Students must use **Microsoft Outlook** (`@krmu.edu.in`) accounts. The seeded student email follows the format:
```
firstname.lastnameSEQ@krmu.edu.in
```
Example: `2401730232@krmu.edu.in` (your roll number as email, Semester 4, CSE-AIML)

---

## 🏗 Project Structure

```
soet-portal/
│
├── config/
│   └── db.js                    # MongoDB connection
│
├── middleware/
│   ├── auth.js                  # JWT verify + no-cache headers
│   └── roleCheck.js             # studentOnly / facultyOnly / adminOnly
│
├── models/
│   ├── Student.js               # Student schema
│   ├── Faculty.js               # Faculty schema (with bcrypt password)
│   ├── Course.js                # Course/branch schema
│   ├── Subject.js               # Subject schema (code, semester, credits, type)
│   ├── SubjectFacultyMap.js     # Faculty-subject assignment mapping
│   ├── StudyMaterial.js         # Uploaded material metadata
│   └── PYQ.js                   # Previous year paper metadata
│
├── routes/
│   ├── auth.js                  # /auth/* (landing, login, Azure callback, logout)
│   ├── student.js               # /student/* (dashboard, materials, pyqs)
│   └── faculty.js               # /faculty/* (dashboard, upload, delete, admin, password)
│
├── views/
│   ├── partials/
│   │   ├── studentNavbar.ejs    # Navbar for student pages
│   │   └── facultyNavbar.ejs    # Navbar for faculty pages
│   ├── auth/
│   │   ├── landing.ejs          # Login selection page
│   │   └── faculty-login.ejs   # Faculty manual login form
│   ├── student/
│   │   ├── dashboard.ejs        # Student dashboard
│   │   ├── materials.ejs        # Unit-wise materials page
│   │   └── pyqs.ejs             # PYQ filter and list page
│   ├── faculty/
│   │   ├── dashboard.ejs        # Faculty/admin dashboard
│   │   ├── upload-material.ejs  # Material upload form
│   │   ├── upload-pyq.ejs       # PYQ upload form
│   │   ├── subjects.ejs         # Faculty subject list
│   │   ├── admin-uploads.ejs    # Admin panel (all uploads)
│   │   └── change-password.ejs  # Password change form
│   ├── 404.ejs                  # Custom 404 page
│   └── error.ejs                # Custom error page
│
├── public/
│   ├── css/
│   │   └── main.css             # Global styles (toasts, spinner, session warning)
│   └── js/
│       └── main.js              # Toast system, loader, session manager, file validator
│
├── uploads/
│   ├── materials/               # Uploaded study material PDFs
│   └── pyqs/                    # Uploaded PYQ PDFs
│
├── seeders/
│   └── seed.js                  # Database seeder (1200+ students, faculty, subjects)
│
├── utils/
│   ├── semesterHelper.js        # Semester calculation logic
│   └── uploadHelper.js          # Multer config for materials and PYQs
│
├── app.js                       # Main Express application entry point
├── .env                         # Environment variables (not committed)
├── .gitignore                   # node_modules, .env, uploads excluded
├── Procfile                     # Railway deployment config
└── package.json
```

---

## 🗄 Database

### Collections Overview

| Collection | Documents | Description |
|-----------|-----------|-------------|
| `students` | 1,200+ | Student profiles with roll numbers and course info |
| `faculty` | 17 | Faculty accounts with hashed passwords and specializations |
| `courses` | 6 | B.Tech specializations (CSE, AIML, DS, FSD, CC, UIUX) |
| `subjects` | 70+ | Subjects mapped to courses and semesters |
| `subjectfacultymaps` | 200+ | Faculty-to-subject assignments (theory + lab) |
| `studymaterials` | dynamic | Uploaded material PDFs with metadata |
| `pyqs` | dynamic | Uploaded PYQ PDFs with year and exam type |

### Courses & Course Codes

| Course | Code | Batch Sizes (per year) |
|--------|------|----------------------|
| CSE Core | 0171 | 180 students |
| CSE — AI & Machine Learning | 0173 | 120 students |
| CSE — Data Science | 0175 | 90 students |
| CSE — Full Stack Development | 0177 | 90 students |
| CSE — Cloud Computing | 0179 | 60 students |
| CSE — UI/UX | 0181 | 60 students |

### Roll Number Format
```
2401730232
│├┤├──┤├──┤
│  │    └── 0232  →  Roll number 232
│  └──────  0173  →  Course code (CSE-AIML)
└─────────  24    →  Enrollment year 2024
```

### Semester Calculation Logic
```
University semester schedule:
  Jan – Jun  =  Even semester (2, 4, 6, 8)
  Jul – Nov  =  Odd semester  (1, 3, 5, 7)

Formula:
  yearsDiff = currentYear - enrollmentYear
  if (Jan–Jun): semester = yearsDiff × 2
  if (Jul–Nov): semester = yearsDiff × 2 + 1

Example:
  Enrolled 2024, current date Feb 2026
  yearsDiff = 2, even half → semester = 2 × 2 = 4 ✓
```

---

## 🛣 API Routes

### Auth Routes (`/auth`)
| Method | Route | Description | Access |
|--------|-------|-------------|--------|
| GET | `/auth/landing` | Login selection page | Public |
| GET | `/auth/faculty-login` | Faculty login form | Public |
| POST | `/auth/login` | Faculty manual login | Public |
| GET | `/auth/azure` | Redirect to Microsoft login | Public |
| GET | `/auth/azure/callback` | Handle Azure OAuth2 callback | Public |
| GET | `/auth/logout` | Clear cookie + MS logout | Authenticated |

### Student Routes (`/student`)
| Method | Route | Description | Access |
|--------|-------|-------------|--------|
| GET | `/student/dashboard` | Student dashboard | Student only |
| GET | `/student/subjects/:code/materials` | Unit-wise materials | Student only |
| GET | `/student/subjects/:code/pyqs` | PYQs with filters | Student only |

### Faculty Routes (`/faculty`)
| Method | Route | Description | Access |
|--------|-------|-------------|--------|
| GET | `/faculty/dashboard` | Faculty/admin dashboard | Faculty + Admin |
| GET | `/faculty/upload/material` | Material upload form | Faculty + Admin |
| POST | `/faculty/material/upload` | Process material upload | Faculty + Admin |
| GET | `/faculty/upload/pyq` | PYQ upload form | Faculty + Admin |
| POST | `/faculty/pyq/upload` | Process PYQ upload | Faculty + Admin |
| GET | `/faculty/subjects` | Faculty subject list | Faculty + Admin |
| POST | `/faculty/material/delete/:id` | Delete a material | Faculty (own) / Admin (any) |
| POST | `/faculty/pyq/delete/:id` | Delete a PYQ | Faculty (own) / Admin (any) |
| GET | `/faculty/admin/all-uploads` | Admin panel | Admin only |
| GET | `/faculty/change-password` | Password change form | Faculty + Admin |
| POST | `/faculty/change-password` | Process password change | Faculty + Admin |

---

## ☁️ Azure AD Setup

To enable student Microsoft Outlook login:

1. Go to [portal.azure.com](https://portal.azure.com) → **App registrations** → **New registration**
2. Name: `SOET Resource Portal`
3. Supported account types: **Accounts in any organizational directory (Multitenant)**
4. Redirect URI: `Web` → `http://localhost:3000/auth/azure/callback`
5. After registration, copy:
   - **Application (client) ID** → `AZURE_CLIENT_ID`
   - **Directory (tenant) ID** → `AZURE_TENANT_ID`
6. Go to **Certificates & secrets** → **New client secret** → Copy value → `AZURE_CLIENT_SECRET`
7. Go to **API permissions** → Add `openid`, `profile`, `email`, `User.Read`
8. Go to **Authentication** → Enable **Access tokens** and **ID tokens**

---

## 🌐 Deployment

This project is configured for deployment on [Railway](https://railway.app) with [MongoDB Atlas](https://cloud.mongodb.com).

### Quick Deploy Steps

1. **MongoDB Atlas** — Create free M0 cluster, get connection string
2. **GitHub** — Push code to private repository
3. **Railway** — Connect GitHub repo, add environment variables
4. **Azure** — Add production redirect URI to Azure app registration

### Environment Variables for Production
```env
PORT=3000
NODE_ENV=production
MONGO_URI=mongodb+srv://user:password@cluster.mongodb.net/soet_portal
JWT_SECRET=your_strong_secret_here
JWT_EXPIRES_IN=7d
AZURE_CLIENT_ID=your_client_id
AZURE_CLIENT_SECRET=your_client_secret
AZURE_TENANT_ID=your_tenant_id
AZURE_REDIRECT_URI=https://your-app.railway.app/auth/azure/callback
```

### Available Scripts
```bash
npm run dev      # Start with nodemon (auto-restart on changes)
npm start        # Start production server
npm run seed     # Seed database with dummy data
```

---

## 🔮 Future Roadmap

### Phase A — High Priority
- [ ] 🔔 Notification system (in-app + email alerts for new uploads)
- [ ] 📢 Announcement board (course-specific and college-wide notices)
- [ ] 📄 Syllabus viewer (PDF.js in-browser viewer per subject)
- [ ] 📅 Timetable management (weekly schedule per section)
- [ ] 🤖 AI study assistant (Google Gemini free API per subject)

### Phase B — Medium Priority
- [ ] 📝 Assignment submission with deadlines and faculty grading
- [ ] 💬 Subject-wise discussion forum (Q&A with faculty moderation)
- [ ] 🔖 Bookmarks (save materials and PYQs)
- [ ] 🧪 AI mock test generator from uploaded notes
- [ ] 📊 Faculty analytics (download counts, engagement metrics)

### Phase C — Nice to Have
- [ ] 🌙 Dark mode toggle
- [ ] 📱 PWA — installable on mobile (Add to Home Screen)
- [ ] ⭐ Student ratings on materials
- [ ] 📦 Bulk PDF upload for faculty
- [ ] 🔍 Semantic search across all resources (AI-powered)

---

## 👥 Team

| Member | Role |
|--------|------|
| [Zaid] | Backend Lead & Database Architect |
| [Aamir, Mankameshwara] | Authentication & Security Engineer |
| [Tushar] | Student Portal Developer |
| [Devi] | Faculty Portal & Upload System |
| [Hitesh] | Frontend & UX Designer |

**Institution:** KR Mangalam University, Gurugram, Haryana
**Department:** School of Engineering & Technology (SOET)
**Course:** B.Tech CSE — AI & Machine Learning
**Batch:** 2024–2028 | Semester IV | Academic Year 2025-26

---

## 📄 License

This project is built for academic purposes at KR Mangalam University. All rights reserved.

---

<div align="center">

Built with ❤️ for KR Mangalam University · SOET · 2026

</div>
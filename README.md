# 🚀 Job Portal Management System

A full-stack Job Portal Management System with three user roles (Seeker, Employer, Admin), JWT authentication, resume upload, email notifications, and a modern responsive UI.

---

## 📋 Prerequisites

Before running this project, install the following:

1. **Node.js** (v18+) — https://nodejs.org/
2. **MongoDB** (v6+) — https://www.mongodb.com/try/download/community
   - Or use **MongoDB Atlas** (free cloud) — https://www.mongodb.com/cloud/atlas

---

## ⚡ Quick Start

### Step 1 — Install MongoDB (if not installed)

**Windows:**
1. Download MongoDB Community Server from https://www.mongodb.com/try/download/community
2. Run the installer with default settings (installs as a Windows service)
3. MongoDB will now auto-start on port 27017

**Or use MongoDB Atlas (Cloud):**
1. Create a free account at https://cloud.mongodb.com
2. Create a cluster → Get connection string
3. Replace `MONGO_URI` in `backend/.env` with your Atlas URI

### Step 2 — Configure Environment

Edit `backend/.env`:
```env
PORT=5000
MONGO_URI=mongodb://localhost:27017/jobportal
JWT_SECRET=jobportal_super_secret_key_2024_xyz
JWT_EXPIRE=7d

# Email (optional — for application status notifications)
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your@gmail.com
EMAIL_PASS=your_app_password
```

### Step 3 — Install Backend Dependencies

```bash
cd backend
npm install
```

### Step 4 — Seed Sample Data

```bash
cd backend
npm run seed
```

This creates:
- 1 Admin user
- 2 Employer users with company profiles
- 5 Job Seeker users
- 10 Job listings
- 8 Sample applications

### Step 5 — Start the Backend Server

```bash
cd backend
npm start
```

Server runs at: **http://localhost:5000**  
Health check: **http://localhost:5000/api/health**

### Step 6 — Serve the Frontend

Open the `frontend/` folder with a local server. Options:

**Option A: VS Code Live Server**
- Install the "Live Server" extension in VS Code
- Right-click `frontend/index.html` → Open with Live Server (typically runs on port 5500)

**Option B: Python HTTP Server**
```bash
cd frontend
python -m http.server 3000
```
Open: http://localhost:3000

**Option C: npx serve**
```bash
npx serve frontend -p 3000
```

---

## 🔑 Test Credentials

| Role     | Email                      | Password     |
|----------|---------------------------|--------------|
| Admin    | admin@jobportal.com       | admin123     |
| Employer | sarah@techcorp.com        | employer123  |
| Employer | michael@innovatelab.com   | employer123  |
| Seeker   | alice@gmail.com           | seeker123    |
| Seeker   | bob@gmail.com             | seeker123    |
| Seeker   | carol@gmail.com           | seeker123    |

> **Tip:** The login page has clickable credential quick-fill buttons!

---

## 📁 Project Structure

```
job portal management system/
├── backend/
│   ├── server.js            # Express app entry point
│   ├── seed.js              # Database seeder
│   ├── .env                 # Environment variables
│   ├── package.json
│   ├── config/
│   │   ├── db.js            # MongoDB connection
│   │   └── email.js         # Nodemailer setup
│   ├── models/
│   │   ├── User.js          # User schema
│   │   ├── Company.js       # Company schema
│   │   ├── Job.js           # Job schema
│   │   └── Application.js   # Application schema
│   ├── routes/
│   │   ├── auth.js          # Register/Login/Me
│   │   ├── users.js         # Profile management
│   │   ├── companies.js     # Company CRUD
│   │   ├── jobs.js          # Jobs CRUD + search
│   │   ├── applications.js  # Job applications
│   │   └── admin.js         # Admin endpoints
│   ├── middleware/
│   │   ├── auth.js          # JWT verification
│   │   └── upload.js        # Multer file upload
│   └── uploads/             # Uploaded resumes
└── frontend/
    ├── index.html           # Home page
    ├── login.html           # Login page
    ├── register.html        # Register page
    ├── jobs.html            # Job listings
    ├── job-detail.html      # Job details + apply
    ├── seeker-dashboard.html
    ├── employer-dashboard.html
    ├── admin-dashboard.html
    ├── css/styles.css       # Full design system
    └── js/
        ├── api.js           # Fetch wrapper + utilities
        ├── auth.js          # Login/Register logic
        ├── jobs.js          # Jobs listing + detail
        ├── seeker.js        # Seeker dashboard
        ├── employer.js      # Employer dashboard
        └── admin.js         # Admin dashboard
```

---

## 🌐 Pages

| Page | URL | Description |
|------|-----|-------------|
| Home | `/index.html` | Landing page with featured jobs |
| Jobs | `/jobs.html` | Search & filter all jobs |
| Job Detail | `/job-detail.html?id=...` | Full job info + apply |
| Login | `/login.html` | Sign in with quick-fill test credentials |
| Register | `/register.html` | Create seeker or employer account |
| Seeker Dashboard | `/seeker-dashboard.html` | Profile, skills, resume, applications |
| Employer Dashboard | `/employer-dashboard.html` | Company, job postings, applicants |
| Admin Dashboard | `/admin-dashboard.html` | Analytics, user/job management |

---

## 🔌 API Documentation

### Authentication

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | `/api/auth/register` | Register user | Public |
| POST | `/api/auth/login` | Login user | Public |
| GET | `/api/auth/me` | Get current user | Bearer |

### Jobs

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/api/jobs` | List jobs (search, filter, paginate) | Public |
| GET | `/api/jobs/:id` | Get job detail | Public |
| POST | `/api/jobs` | Create job | Employer |
| PUT | `/api/jobs/:id` | Update job | Employer |
| DELETE | `/api/jobs/:id` | Delete job | Employer |
| GET | `/api/jobs/employer/mine` | My jobs | Employer |

**Query params for GET /api/jobs:**
- `search` — full-text search
- `category` — Technology, Marketing, etc.
- `location` — partial location match
- `type` — full-time, part-time, remote, contract, internship
- `experience` — Entry Level, 1-2 years, etc.
- `page` — page number (default: 1)
- `limit` — results per page (default: 9)

### Applications

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | `/api/applications` | Apply for job (multipart) | Seeker |
| GET | `/api/applications/mine` | My applications | Seeker |
| GET | `/api/applications/check/:jobId` | Check if applied | Seeker |
| GET | `/api/applications/job/:jobId` | Job's applicants | Employer |
| PUT | `/api/applications/:id/status` | Update status | Employer |

### Admin

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/api/admin/stats` | Dashboard analytics | Admin |
| GET | `/api/admin/users` | All users | Admin |
| PUT | `/api/admin/users/:id/toggle` | Activate/deactivate | Admin |
| DELETE | `/api/admin/users/:id` | Delete user | Admin |
| GET | `/api/admin/jobs` | All jobs | Admin |
| PUT | `/api/admin/jobs/:id/toggle` | Toggle job active | Admin |
| DELETE | `/api/admin/jobs/:id` | Delete job | Admin |

---

## 🛡️ Security Features

- **JWT Authentication** — Stateless token-based auth
- **bcrypt Password Hashing** — 12 salt rounds
- **Role-based Access Control** — seeker/employer/admin guards
- **Input Validation** — express-validator on all inputs
- **File Type Validation** — Only PDF, DOC, DOCX accepted
- **Duplicate Application Prevention** — MongoDB unique index
- **Account Deactivation** — Admin can block users

---

## 📧 Email Setup (Optional)

For Gmail:
1. Enable 2-factor authentication on your Gmail
2. Generate an App Password: Google Account → Security → App Passwords
3. Add to `.env`:
   ```
   EMAIL_USER=you@gmail.com
   EMAIL_PASS=your-16-char-app-password
   ```

Emails sent when employer accepts/rejects an application.

---

## 🎨 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Backend | Node.js, Express.js |
| Database | MongoDB with Mongoose ODM |
| Auth | JWT + bcryptjs |
| File Upload | Multer |
| Email | Nodemailer |
| Validation | express-validator |
| Font | Google Fonts — Inter |

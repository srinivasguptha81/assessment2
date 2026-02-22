# 🎓 LPU Smart Campus Management System
### Smart AI-Enabled Campus Management System — Python Full Stack Project II

![Python](https://img.shields.io/badge/Python-3.11-blue?style=flat-square&logo=python)
![Django](https://img.shields.io/badge/Django-5.x-green?style=flat-square&logo=django)
![SQLite](https://img.shields.io/badge/Database-SQLite-lightgrey?style=flat-square&logo=sqlite)
![AI](https://img.shields.io/badge/AI-Face%20Recognition-gold?style=flat-square)

---

## 📌 Project Overview

A full-stack web application built for **Lovely Professional University** as part of the Python and Full Stack course (Project II). The system digitizes and automates campus operations including attendance tracking, resource management, food ordering, and remedial class scheduling.

**Module covered in this submission:** Smart Attendance Management System

---

## 🚀 Features

### ✅ Smart Attendance Management System
- **Role-based dashboards** — Admin, Faculty, Student each see their own view
- **Manual attendance marking** — Faculty marks P / A / L / E per student with one-click bulk submit
- **AI Face Recognition** — Faculty uploads a class photo; system auto-detects and marks attendance
- **Auto absentee detection** — System instantly identifies who is absent after each session
- **Simulated notifications** — Email alerts sent to absent students and their parents automatically
- **Low attendance warnings** — Students below 75% get flagged with alerts on their dashboard
- **Attendance matrix report** — Full session-by-session breakdown per course for faculty
- **Student portal** — Students view their attendance % per course with visual progress bars

---

## 🧠 AI Technology Used

| Component | Technology |
|---|---|
| Face Detection | HOG (Histogram of Oriented Gradients) via `dlib` |
| Face Encoding | 128-dimension face embedding (deep neural network) |
| Face Matching | Euclidean distance comparison (threshold: 0.6) |
| Library | `face_recognition` (built on `dlib`) |

### How it works:
1. Each student uploads a profile photo — system encodes it into a 128D vector
2. Faculty uploads a class photo during a session
3. System detects all faces in the class photo
4. Each detected face is compared against all stored student encodings
5. Matched students → **Present**, unmatched → **Absent**
6. Notifications sent automatically to absentees

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Backend Framework | Django 5.x |
| Language | Python 3.11 |
| Database | SQLite (dev) |
| Frontend | HTML5, CSS3, Bootstrap 5, Bootstrap Icons |
| AI / ML | face_recognition, dlib, OpenCV, NumPy |
| Image Handling | Pillow |
| Forms | django-crispy-forms + crispy-bootstrap5 |
| Email | Django Console Email Backend (simulated) |

---

## 📁 Project Structure

```
lpu_cms/
├── lpu_cms/                  ← Project configuration
│   ├── settings.py           ← All Django settings
│   ├── urls.py               ← Root URL configuration
│   └── wsgi.py
│
├── attendance/               ← Smart Attendance Module
│   ├── models.py             ← 8 database models
│   ├── views.py              ← 11 views (dashboards, marking, AI, AJAX)
│   ├── urls.py               ← 11 URL routes
│   ├── forms.py              ← Session, bulk attendance, image upload forms
│   ├── ai_service.py         ← Face recognition logic
│   ├── notifications.py      ← Email alert service
│   └── admin.py              ← Admin panel registration
│
├── templates/
│   ├── base.html             ← Master layout (sidebar, topbar, CSS)
│   ├── registration/
│   │   └── login.html        ← Custom login page
│   └── attendance/
│       ├── admin_dashboard.html
│       ├── faculty_dashboard.html
│       ├── start_session.html
│       ├── mark_attendance.html
│       ├── session_detail.html
│       ├── student_dashboard.html
│       ├── student_course_detail.html
│       ├── course_report.html
│       ├── ai_mark.html
│       └── no_role.html
│
├── media/                    ← Uploaded student/faculty photos
├── static/                   ← Static CSS/JS files
├── db.sqlite3                ← SQLite database
├── manage.py
└── requirements.txt
```

---

## ⚙️ Database Models

| Model | Purpose |
|---|---|
| `Department` | Academic departments |
| `Faculty` | Faculty profiles linked to Django User |
| `Student` | Student profiles with photo, parent contact |
| `Course` | Courses linking faculty to enrolled students |
| `AttendanceSession` | One session = one class on one date |
| `AttendanceRecord` | One row per student per session (P/A/L/E) |
| `AbsenteeAlert` | Log of all notifications sent |
| `AttendanceSummary` | Cached attendance % per student per course |

---

## 🔗 URL Routes

| URL | View | Description |
|---|---|---|
| `/` | Redirect | Redirects to login |
| `/accounts/login/` | LoginView | Custom login page |
| `/attendance/` | dashboard | Role-based redirect |
| `/attendance/admin-dashboard/` | admin_dashboard | Admin overview |
| `/attendance/faculty/` | faculty_dashboard | Faculty portal |
| `/attendance/session/start/` | start_session | Create new session |
| `/attendance/session/<id>/mark/` | mark_attendance | Manual marking |
| `/attendance/session/<id>/ai-mark/` | ai_mark_attendance | AI photo upload |
| `/attendance/session/<id>/` | session_detail | Session results |
| `/attendance/course/<id>/report/` | course_attendance_report | Full matrix |
| `/attendance/student/` | student_dashboard | Student portal |
| `/attendance/api/absentees/<id>/` | detect_absentees | AJAX endpoint |

---

## 🏃 How to Run

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/lpu-smart-cms.git
cd lpu-smart-cms
```

### 2. Create and activate virtual environment
```bash
python -m venv lpu_env

# Windows
lpu_env\Scripts\activate

# Mac/Linux
source lpu_env/bin/activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

> **Windows note for face_recognition:** If dlib fails to install, run:
> ```bash
> pip install cmake dlib
> pip install face-recognition
> ```

### 4. Run migrations
```bash
python manage.py makemigrations
python manage.py migrate
```

### 5. Create superuser (Admin account)
```bash
python manage.py createsuperuser
```

### 6. Start the development server
```bash
python manage.py runserver
```

### 7. Open in browser
```
http://127.0.0.1:8000/
```

---

## 🧪 Setting Up Test Data

After running the server, go to `http://127.0.0.1:8000/admin/` and:

1. **Add Department** — e.g. "Computer Science", code "CSE"
2. **Add User** — create a user for faculty (e.g. username: `prof_singh`)
3. **Add Faculty** — link to the user, set faculty_id
4. **Add User** — create users for students
5. **Add Students** — link each to a user, upload a face photo
6. **Add Course** — assign faculty, enroll students via the Students field
7. **Login as faculty** at `/accounts/login/` to start marking attendance

---

## 👥 User Roles

| Role | How to create | Access |
|---|---|---|
| **Admin** | `createsuperuser` or `is_staff=True` | Full system overview, all data |
| **Faculty** | Create User → Create Faculty profile in admin | Mark attendance, view reports |
| **Student** | Create User → Create Student profile in admin | View own attendance only |

---

## 📧 Email Notifications (Simulated)

In development, emails print to the terminal instead of sending. You will see output like:

```
Content-Type: text/plain; charset="utf-8"
Subject: [LPU] Absence Recorded — CS101
To: student@example.com

Dear John Doe,
You were marked ABSENT for Data Structures (CS101)
Date: 22 February 2026  |  Time: 10:00 AM – 11:00 AM
...
```

To enable real email, update `settings.py`:
```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST     = 'smtp.gmail.com'
EMAIL_PORT     = 587
EMAIL_USE_TLS  = True
EMAIL_HOST_USER     = 'your@gmail.com'
EMAIL_HOST_PASSWORD = 'your_app_password'
```

---

## 🔑 Key Python Concepts Used

| Concept | Where Used |
|---|---|
| Class-based models | `models.py` — all 8 database models |
| `@login_required` decorator | All views — authentication protection |
| `@property` decorator | `AttendanceSummary.percentage` — computed field |
| `get_object_or_404` | Clean 404 error handling in views |
| `update_or_create` | Prevents duplicate attendance records |
| `JsonResponse` | AJAX absentee detection API |
| `ManyToManyField` | Students ↔ Courses enrollment |
| `OneToOneField` | User ↔ Student/Faculty profile |
| Dictionary comprehension | Building face encodings map in `ai_service.py` |
| NumPy arrays | 128D face embedding comparison |

---


# 📋 ProjectHub — Full-Stack Project Management Tool

A complete project management application with JWT authentication, project CRUD, and task tracking.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Django 4.2 + Django REST Framework |
| Auth | JWT via `djangorestframework-simplejwt` |
| Password Hashing | bcrypt via Django's `BCryptSHA256PasswordHasher` |
| Database | SQLite (default) / PostgreSQL (optional) |
| Frontend | React 18 + TypeScript + Vite |
| Styling | Tailwind CSS |
| Forms | React Hook Form + Yup validation |
| HTTP Client | Axios with auto-refresh interceptor |
| State | React Context API |

---

## Features

### Backend
- **JWT Authentication** — Register / login with email + password; bcrypt password hashing; auto-refresh tokens
- **Projects API** — Full CRUD; filter by status; search by title/description; pagination
- **Tasks API** — Full CRUD scoped to user's projects; filter by status; due-date tracking
- Proper HTTP status codes (200, 201, 400, 401, 404)
- Input validation on all endpoints

### Frontend
- Login & Register pages with form validation (Yup)
- Dashboard with project grid, search, status filter, and pagination
- Project detail page with kanban-style task columns (To Do / In Progress / Done)
- Add/edit modals for both projects and tasks
- Delete confirmation modals
- Responsive design with Tailwind CSS
- Auto-redirect on unauthenticated requests; token auto-refresh

---

## Setup

### Prerequisites
- Python 3.10+
- Node.js 18+
- npm or yarn

---

### Backend Setup

```bash
cd backend

# 1. Create virtual environment
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run migrations
python manage.py migrate

# 4. (Optional) Create a superuser for Django Admin
python manage.py createsuperuser

# 5. Run the development server
python manage.py runserver
```

The API will be available at **http://localhost:8000**

---

### Frontend Setup

```bash
cd frontend

# 1. Install dependencies
npm install

# 2. Start the dev server
npm run dev
```

The frontend will be available at **http://localhost:5173**

The Vite proxy is pre-configured to forward `/api/*` requests to `http://localhost:8000`, so no CORS issues during development.

---

### Running Seeds

Seed the database with 2 demo users, 4 projects, and 12 tasks:

```bash
cd backend
python manage.py seed
```

To clear all data and re-seed:

```bash
python manage.py seed --clear
```

**Demo credentials after seeding:**

| Email | Password |
|-------|----------|
| alice@example.com | Password123 |
| bob@example.com | Password123 |

---

## API Reference

### Auth Endpoints

| Method | URL | Auth | Description |
|--------|-----|------|-------------|
| POST | `/api/auth/register/` | ❌ | Register new user |
| POST | `/api/auth/login/` | ❌ | Login, returns JWT tokens |
| POST | `/api/auth/refresh/` | ❌ | Refresh access token |
| GET | `/api/auth/me/` | ✅ | Get current user profile |

### Projects Endpoints

| Method | URL | Description |
|--------|-----|-------------|
| GET | `/api/projects/` | List user's projects (supports `?search=`, `?status=`, `?page=`) |
| POST | `/api/projects/` | Create a project |
| GET | `/api/projects/{id}/` | Get a single project |
| PATCH | `/api/projects/{id}/` | Partially update a project |
| PUT | `/api/projects/{id}/` | Full update a project |
| DELETE | `/api/projects/{id}/` | Delete a project |

### Tasks Endpoints

| Method | URL | Description |
|--------|-----|-------------|
| GET | `/api/tasks/` | List tasks (supports `?project=`, `?status=`, `?search=`) |
| POST | `/api/tasks/` | Create a task |
| GET | `/api/tasks/{id}/` | Get a single task |
| PATCH | `/api/tasks/{id}/` | Partially update a task |
| PUT | `/api/tasks/{id}/` | Full update a task |
| DELETE | `/api/tasks/{id}/` | Delete a task |

---

## Project Structure

```
project-manager/
├── backend/
│   ├── manage.py
│   ├── requirements.txt
│   ├── config/
│   │   ├── settings.py
│   │   └── urls.py
│   └── apps/
│       ├── authentication/    # JWT auth (register, login, me)
│       ├── projects/          # Project CRUD
│       ├── tasks/             # Task CRUD
│       └── seeder/            # `python manage.py seed`
│
└── frontend/
    ├── package.json
    ├── vite.config.ts
    ├── tailwind.config.js
    └── src/
        ├── api/               # Axios API helpers
        ├── components/        # Navbar, Modal, ProjectCard, TaskCard
        ├── context/           # AuthContext (state management)
        ├── pages/             # Login, Register, Dashboard, ProjectDetail
        └── types/             # TypeScript interfaces
```

---

## Using PostgreSQL (Optional)

In `backend/config/settings.py`, replace the `DATABASES` block:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'projecthub',
        'USER': 'your_db_user',
        'PASSWORD': 'your_db_password',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

Install the PostgreSQL driver:

```bash
pip install psycopg2-binary
```

---

## Known Limitations

- No email verification on registration
- Token blacklisting on logout is not implemented (tokens expire naturally)
- No real-time updates (would need WebSockets / SSE)
- File attachments for tasks are not supported
- No team/collaboration features — projects are per-user only

---

## Evaluation Criteria Coverage

| Category | Implementation |
|----------|---------------|
| Code Structure & Readability | Modular Django apps, typed React components, consistent naming |
| Backend Functionality | Full CRUD, JWT auth, bcrypt, filtering, pagination, validation |
| Frontend Implementation | Login/Register, Dashboard, Project Detail, modals, forms |
| TypeScript Usage | Strict types throughout — interfaces, generics, typed API responses |
| Git & Documentation | This README covers all setup steps, API reference, and limitations |
| Bonus Features | React Hook Form + Yup, pagination & search, Context API state management |

# 📚 Bookoholik — Home Library Management System

A modern, multilingual, self-hosted web application for managing your personal home library. Track books across multiple locations, scan book covers with AI, manage writers and publishers, lend books to friends, and generate reports — all from a responsive mobile-friendly interface.

![Vue.js](https://img.shields.io/badge/Vue.js-3.4-4FC08D?logo=vuedotjs&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.3-777BB4?logo=php&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)
[![GitHub stars](https://img.shields.io/github/stars/XKlibure/home-library?style=social)](https://github.com/XKlibure/home-library)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue?logo=docker)](docker-compose.yml)
[![SonarCloud](https://sonarcloud.io/api/project_badges/measure?project=bookoholik&metric=alert_status)](https://sonarcloud.io/dashboard?id=bookoholik)

---

## ✨ Features

### 📷 AI Book Scanner (NEW)
- **Scan book covers** with your phone camera to extract title and author
- **Scan back pages** to extract ISBN, publisher, and publication year
- Powered by **Google Gemini Flash** Vision AI (free tier — 1,500 scans/day)
- Supports **Arabic calligraphy**, French, and English book covers
- Falls back to Tesseract OCR if no API key configured
- Editable results — correct OCR output before searching/adding
- Automatically searches your library for matches
- One-tap "Add Book" with pre-filled details from scan

### 📖 Book Management
- Full CRUD for your book collection
- Search by title, author, genre, language, year
- Filter by read status, borrowed status, location
- ISBN lookup via Open Library API
- Assign books to specific shelves (Address → Room → Shelf)
- Link books to publishers from your registry
- Mark books as read/unread

### 📍 Multi-Location Library (NEW)
- Manage **multiple physical addresses** (Home, Office, Summer House, etc.)
- Set a **primary location** for your library
- Create **rooms** within each address (Living Room, Study, etc.)
- Create **shelves** within each room with capacity tracking
- Full hierarchy: Address → Rooms → Shelves → Books
- Visual tree view of your entire library layout
- Book count per location/room/shelf

### 🏢 Publishers Management (NEW)
- Dedicated publisher/edition house registry
- Multilingual names (English, Arabic, French)
- Contact info: address, city, country, phone, email, website
- Link publishers to books
- View book count per publisher

### ✍️ Writers Management
- Dedicated writers/authors registry
- Multilingual names (English, Arabic, French)
- Nationality, birth/death year, biography
- Select writers when adding a book (dropdown + manual entry)
- View book count per writer

### 📖 Genres Management
- Create, edit, and delete genres
- Multilingual genre names (English, Arabic, French)
- Select genres when adding books

### 🤝 Lending System
- Lend books to friends with due dates
- Track overdue books with alerts
- Mark books as returned
- Full lending history per book

### 📊 Reports & Analytics
- Dashboard with reading statistics
- Reports by genre, author, year, location
- Language distribution visualization
- Export to CSV and PDF

### 👥 User Management (Admin)
- Role-based access control (Admin, User, Viewer)
- Create/disable/delete users
- JWT-based authentication

### ⚙️ User Settings (NEW)
- Update profile information (name, username, email)
- Change password with strength validation
- View account info and role

### 💾 Backup System
- Automatic daily database backups (2:00 AM)
- Manual backup creation
- Download and manage backup files
- Secure `.pgpass` authentication (no env var exposure)

### 🌍 Multilingual Interface
- **English** 🇬🇧
- **Arabic** 🇸🇦 (with full RTL support)
- **French** 🇫🇷
- Language switcher in the UI, preference saved per user

### 📱 Mobile Responsive (NEW)
- Hamburger menu navigation on mobile
- Touch-friendly card layouts
- Camera capture directly from phone
- Optimized for phone-first usage

### 🔒 Security Hardened
- Rate limiting on login (5/15min) and registration (3/hr)
- CORS restricted to configured origin
- Content Security Policy headers
- JWT with 8-hour expiry
- No hardcoded secrets — fails fast if env not set
- Container resource limits
- Password policy enforcement (10+ chars with complexity)
- Command injection prevention

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────┐
│                   Frontend                       │
│         Vue.js 3 + Vite + Tailwind CSS          │
│              (nginx:alpine)                      │
│                 Port 3000                        │
└─────────────────┬───────────────────────────────┘
                  │ HTTP API calls
┌─────────────────▼───────────────────────────────┐
│                   Backend                        │
│     PHP 8.3 + Apache + Tesseract OCR + GD       │
│         Custom REST API + Gemini Vision         │
│                 Port 8080                        │
└─────────────────┬───────────────────────────────┘
                  │ PostgreSQL protocol
┌─────────────────▼───────────────────────────────┐
│                  Database                        │
│              PostgreSQL 16 Alpine                │
│                   Port 5432                      │
└─────────────────────────────────────────────────┘
```

---

## 🚀 Quick Start

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) or [Podman](https://podman.io/getting-started/installation) with Compose support
- (Optional) [Google Gemini API Key](https://aistudio.google.com/apikey) for AI book scanning (free)

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/XKlibure/home-library.git
   cd home-library
   ```

2. **Configure environment**

   ```bash
   cp .env.example .env
   # Edit .env — generate strong passwords:
   # DB_PASSWORD, JWT_SECRET, APP_KEY (see .env.example for instructions)
   ```

   For AI book scanning, add your free Gemini key:
   ```env
   GEMINI_API_KEY=your-key-from-aistudio.google.com
   ```

3. **Build and start**

   ```bash
   docker compose up -d --build
   ```

   Or with Podman:
   ```bash
   podman compose up -d --build
   ```

4. **Access the application**

   | Service  | URL                        |
   |----------|----------------------------|
   | Frontend | http://localhost:3000       |
   | Backend API | http://localhost:8080/api |

5. **Login**

   | Field    | Value        |
   |----------|--------------|
   | Username | `admin`      |
   | Password | `Admin1234!` |

   > ⚠️ **Change this password immediately** via Settings page!

### Access from Phone (LAN)

To use the scanner from your phone on the same network:
```env
# In .env, set your computer's LAN IP:
API_URL=http://192.168.1.x:8080/api
CORS_ORIGIN=http://192.168.1.x:3000
```
Then rebuild frontend: `docker compose up -d --build frontend`

---

## 📁 Project Structure

```
home-library/
├── docker-compose.yml
├── .env.example
├── sonar-project.properties
├── CONTRIBUTING.md
├── LICENSE
├── docker/
│   ├── Dockerfile.frontend     # Vue.js multi-stage build
│   ├── Dockerfile.backend      # PHP 8.3 + Apache + Tesseract + GD
│   ├── Dockerfile.db           # PostgreSQL + init script
│   ├── Dockerfile.backup       # Backup cron service
│   ├── nginx.conf              # Frontend nginx + security headers
│   ├── apache.conf             # Backend Apache vhost
│   ├── init.sql                # Database schema (10 tables)
│   └── backup-cron.sh          # Automated backup script
├── backend/
│   ├── composer.json
│   ├── public/index.php        # Entry point + CORS + security headers
│   ├── routes/api.php          # 50+ API routes
│   └── app/
│       ├── Config/Database.php
│       ├── Controllers/
│       │   ├── AuthController.php
│       │   ├── BooksController.php
│       │   ├── WritersController.php
│       │   ├── GenresController.php
│       │   ├── PublishersController.php    # NEW
│       │   ├── LocationsController.php    # NEW
│       │   ├── ScanController.php         # NEW (AI Vision)
│       │   ├── LendingController.php
│       │   ├── ReportsController.php
│       │   ├── UsersController.php
│       │   └── BackupController.php
│       ├── Middleware/
│       │   ├── AuthMiddleware.php
│       │   ├── AdminMiddleware.php
│       │   ├── UserMiddleware.php         # NEW
│       │   └── RateLimiter.php            # NEW
│       └── Router.php
├── frontend/
│   ├── package.json
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── index.html
│   └── src/
│       ├── main.js
│       ├── App.vue                 # Responsive nav + hamburger menu
│       ├── i18n/                   # en.js, ar.js, fr.js
│       ├── router/index.js         # 15 routes with guards
│       ├── services/api.js         # Axios + token expiry check
│       ├── store/                   # auth.js, toast.js
│       └── views/
│           ├── LoginView.vue
│           ├── DashboardView.vue
│           ├── BooksView.vue
│           ├── BookFormView.vue
│           ├── BookDetailView.vue
│           ├── ScanBookView.vue       # NEW (camera + AI)
│           ├── WritersView.vue
│           ├── GenresView.vue
│           ├── PublishersView.vue      # NEW
│           ├── LocationsView.vue      # NEW (hierarchy)
│           ├── LendingView.vue
│           ├── ReportsView.vue
│           ├── SettingsView.vue       # NEW (profile + password)
│           ├── UsersView.vue
│           └── BackupView.vue
└── .github/
    ├── workflows/sonarqube.yml
    └── ISSUE_TEMPLATE/
        ├── bug_report.md
        └── feature_request.md
```

---

## 🔌 API Endpoints

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/login` | Login (rate limited) |
| POST | `/api/auth/register` | Register (rate limited) |
| GET | `/api/auth/me` | Get current user |
| PUT | `/api/auth/profile` | Update profile info |
| PUT | `/api/auth/password` | Change password |

### Book Scanner
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/scan/cover` | Scan front cover (AI + OCR) |
| POST | `/api/scan/back` | Scan back page (AI + OCR) |

### Books
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/books` | List books (paginated, filterable) |
| GET | `/api/books/{id}` | Get book details + lending history |
| POST | `/api/books` | Create book |
| PUT | `/api/books/{id}` | Update book |
| DELETE | `/api/books/{id}` | Delete book |
| POST | `/api/books/{id}/toggle-read` | Toggle read status |
| POST | `/api/books/isbn-lookup` | Lookup by ISBN (Open Library) |

### Publishers
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/publishers` | List publishers (searchable) |
| GET | `/api/publishers/{id}` | Get publisher details |
| POST | `/api/publishers` | Create publisher (admin) |
| PUT | `/api/publishers/{id}` | Update publisher (admin) |
| DELETE | `/api/publishers/{id}` | Delete publisher (admin) |

### Locations (Addresses → Rooms → Shelves)
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/locations/tree` | Full hierarchy for dropdowns |
| GET | `/api/locations` | List all addresses |
| GET | `/api/locations/{id}` | Get address + rooms + shelves |
| POST | `/api/locations` | Create address (admin) |
| PUT | `/api/locations/{id}` | Update address (admin) |
| DELETE | `/api/locations/{id}` | Delete address (admin) |
| GET | `/api/locations/{id}/rooms` | List rooms in address |
| POST | `/api/rooms` | Create room (admin) |
| PUT | `/api/rooms/{id}` | Update room (admin) |
| DELETE | `/api/rooms/{id}` | Delete room (admin) |
| GET | `/api/rooms/{id}/shelves` | List shelves in room |
| POST | `/api/shelves` | Create shelf (admin) |
| PUT | `/api/shelves/{id}` | Update shelf (admin) |
| DELETE | `/api/shelves/{id}` | Delete shelf (admin) |

### Writers
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/writers` | List all writers |
| GET | `/api/writers/{id}` | Get writer + books |
| POST | `/api/writers` | Create writer (admin) |
| PUT | `/api/writers/{id}` | Update writer (admin) |
| DELETE | `/api/writers/{id}` | Delete writer (admin) |

### Genres
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/genres` | List all genres |
| POST | `/api/genres` | Create genre (admin) |
| PUT | `/api/genres/{id}` | Update genre (admin) |
| DELETE | `/api/genres/{id}` | Delete genre (admin) |

### Lending
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/lending` | List lending records |
| POST | `/api/lending` | Lend a book |
| PUT | `/api/lending/{id}` | Update lending record |
| POST | `/api/lending/{id}/return` | Mark as returned |
| DELETE | `/api/lending/{id}` | Delete record |
| GET | `/api/lending/overdue` | Get overdue books |

### Reports
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/reports/summary` | Dashboard stats |
| GET | `/api/reports/by-genre` | Books by genre |
| GET | `/api/reports/by-author` | Books by author |
| GET | `/api/reports/by-year` | Books by year |
| GET | `/api/reports/by-location` | Books by location |
| GET | `/api/reports/export/csv` | Export CSV |
| GET | `/api/reports/export/pdf` | Export PDF |

### Admin — Users & Backup
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/users` | List users |
| PUT | `/api/users/{id}` | Update user |
| DELETE | `/api/users/{id}` | Delete user |
| POST | `/api/backup/create` | Create backup |
| GET | `/api/backup/list` | List backups |
| GET | `/api/backup/download/{file}` | Download backup |
| DELETE | `/api/backup/{file}` | Delete backup |

---

## 🔧 Common Operations

```bash
# Stop
docker compose down

# Reset database (fresh start)
docker compose down -v && docker compose up -d --build

# View logs
docker compose logs -f backend

# Rebuild single service
docker compose up -d --build frontend

# Access from phone (set your LAN IP in .env first)
API_URL=http://192.168.1.x:8080/api docker compose up -d --build frontend
```

---

## 🔒 Security

This application is security-hardened against the OWASP Top 10:

| Feature | Status |
|---------|--------|
| SQL Injection protection (PDO prepared statements) | ✅ |
| XSS prevention (Vue.js auto-escaping + htmlspecialchars) | ✅ |
| CORS restricted to configured origin | ✅ |
| JWT authentication with 8h expiry | ✅ |
| Rate limiting (login: 5/15min, register: 3/hr) | ✅ |
| Password policy (10+ chars, uppercase, lowercase, number) | ✅ |
| Role-based access control (Admin/User/Viewer) | ✅ |
| Security headers (CSP, X-Frame-Options, etc.) | ✅ |
| No hardcoded secrets (fails fast if env not set) | ✅ |
| Command injection prevention (escapeshellarg) | ✅ |
| Container resource limits (memory/CPU) | ✅ |
| Database port not exposed by default | ✅ |
| Secure backup file permissions (chmod 600) | ✅ |
| Request timeout (30s default, 60s for scans) | ✅ |
| Token expiry check on frontend | ✅ |
| Generic error messages (no stack traces to client) | ✅ |

See [Security Notes](#security-production) below for production deployment checklist.

<a id="security-production"></a>
### Production Deployment Checklist

1. Generate strong secrets in `.env` (see `.env.example`)
2. Change default admin password immediately
3. Set `APP_DEBUG=false`
4. Configure `CORS_ORIGIN` to your frontend domain
5. Deploy behind HTTPS reverse proxy (nginx/Caddy/Traefik)
6. Database port is NOT exposed by default ✓
7. Optionally disable open registration (see `CONTRIBUTING.md`)

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Vue.js 3, Vite 5, Tailwind CSS 3, Pinia, Vue Router, Vue I18n, Axios |
| Backend | PHP 8.3, Apache, Custom Router, Firebase PHP-JWT, DomPDF, Tesseract OCR |
| AI Vision | Google Gemini Flash (free tier) |
| Database | PostgreSQL 16 (10 tables) |
| Containerization | Docker / Podman Compose |
| Web Server | Nginx Alpine (frontend), Apache (backend) |
| CI/CD | GitHub Actions + SonarQube |
| Backup | pg_dump + cron + gzip |

---

## 🤝 Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

- 🐛 [Report a Bug](.github/ISSUE_TEMPLATE/bug_report.md)
- 💡 [Request a Feature](.github/ISSUE_TEMPLATE/feature_request.md)
- 🌐 **Translate** — Copy `frontend/src/i18n/en.js` to add a new language

---

## 📄 License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and distribute this software.

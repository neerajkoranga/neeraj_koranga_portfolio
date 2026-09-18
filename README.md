# Neeraj — Full-Stack Personal Portfolio & Alpine Showcase

> A modern, high-performance personal portfolio website celebrating the craft of **Full-Stack Software Engineering**, **Himalayan & Alpine Photography**, **High-Altitude Expeditions**, and **Nature Exploration**.

---

## 🌟 Overview & Dual-Mode Architecture

The application is built on a clean full-stack decoupled architecture designed to serve two completely distinct audiences:

1. **Public Visitor Experience** (`/`, `/projects`, `/photography`, `/travel`, `/blog`, `/skills`, `/contact`):
   - Fast, aesthetic, responsive dark-themed presentation.
   - Strict read-only access: visitors only see published content (`published = true`).
   - Interactive Himalayan photography masonry gallery with a **fullscreen lightbox and EXIF metadata inspector** (camera, lens, aperture, shutter speed, ISO, focal length).
   - High-altitude expedition journals with route altitudes, difficulties, and field galleries.
   - Comprehensive engineering project case studies with architectural highlights and live/source links.
   - Skills matrix categorized by Frontend, Backend, Cloud/DevOps, Database, and Alpine/Photography.
   - Contact form with validation and backend persistence.

2. **Secured Admin Portal** (`/admin/login`, `/admin/dashboard`, `/admin/*`):
   - Protected by **Spring Security 6 stateless JWT** and **Angular functional auth guards**.
   - Live Command Center with metrics: total projects, photos, albums, expeditions, articles, skills, and unread contact messages.
   - Complete publishing workflow (Draft vs Published status toggles with single clicks).
   - Local/S3-ready file upload manager with MIME validation, file size constraints, and path traversal protection.
   - Interactive content editors for Projects, Photo Vault, Albums, Expeditions, Articles, Skills, and Contact Messages with reply tracking.
   - One-click demo credential filler on the login screen for testing convenience.

---

## 🛠️ Technology Stack

### Frontend
- **Framework**: Angular 21 (Standalone Components, Signals, Functional Guards & Interceptors)
- **Language**: TypeScript 5.9
- **Styling**: SCSS Design System (Dark Obsidian `#090D16`, Alpine Slate, Frost Cyan `#38BDF8`, Ember Amber `#F59E0B`)
- **Animation**: GSAP (GreenSock) & AOS (Animate on Scroll)
- **Typography**: Google Fonts (*Plus Jakarta Sans*, *Inter*, *JetBrains Mono*)
- **Icons**: Bootstrap Icons
- **Testing**: Vitest & jsdom

### Backend
- **Framework**: Spring Boot 3.3.4 (Java 22)
- **Security**: Spring Security 6 (Stateless JWT via `io.jsonwebtoken:jjwt:0.12.5`, BCrypt password hashing)
- **Persistence**: Spring Data JPA & Hibernate with HikariCP connection pooling
- **Validation**: Jakarta Bean Validation (`@NotBlank`, `@Size`, `@Email`, etc.)
- **Profiles**:
  - `dev`: In-memory H2 database running in PostgreSQL compatibility dialect for zero-setup instant local run.
  - `postgres`: Full PostgreSQL production setup.
- **Testing**: JUnit 5, Mockito, Spring Security Test

### Database & DevOps
- **Relational DB**: PostgreSQL 16 (or H2 in dev mode)
- **Containerization**: Docker & Docker Compose (Multi-stage builds with Eclipse Temurin JRE 22 and Nginx Alpine)
- **Reverse Proxy**: Nginx Alpine with gzip compression and client-side SPA routing

---

## 🚀 Quick Start Guide

### Method 1: Instant Local Development (Zero Database Setup Required)

The backend is configured with a default `dev` profile that spins up an in-memory H2 database in PostgreSQL mode and pre-seeds all flagship projects, photo albums, expeditions, blogs, skills, and the admin account automatically.

#### 1. Start the Backend (Port 8080)
Ensure Java 22 (or 21+) and Maven are installed:
```bash
cd backend
mvn spring-boot:run
```
*The Spring Boot server will start at `http://localhost:8080` with mock seed data and file uploads enabled.*

#### 2. Start the Frontend (Port 4200)
Ensure Node.js 20+ is installed:
```bash
cd frontend
npm install
npm start
```
*The Angular development server starts at `http://localhost:4200` and automatically proxies `/api` and `/uploads` to `http://localhost:8080`.*

---

### Method 2: Docker Compose Deployment (Production Ready)

To build and run the entire stack (PostgreSQL + Spring Boot + Angular Nginx) with a single command:

```bash
docker-compose up --build
```

#### Running Services:
| Service | Container Name | Port | Description |
|---|---|---|---|
| **Frontend** | `portfolio-frontend` | `http://localhost:80` | Nginx Alpine serving Angular SPA & proxying API |
| **Backend** | `portfolio-backend` | `http://localhost:8080` | Spring Boot REST API & Image Upload handler |
| **Database** | `portfolio-postgres` | `localhost:5432` | PostgreSQL 16 database |

---

## 🔑 Default Admin Credentials

An administrator account is automatically provisioned during startup:

- **Login URL**: [http://localhost:4200/admin/login](http://localhost:4200/admin/login) (or `/admin/login` on Docker port 80)
- **Email**: `admin@portfolio.com`
- **Password**: `admin123`
- *Tip: Click the **"Fill Demo Credentials"** button on the login screen to auto-populate.*

---

## 📡 REST API Reference

### Public Endpoints (`/api/public/**`)
All public endpoints are read-only and return only published records.

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/public/projects` | List all published projects (supports `featured=true`) |
| `GET` | `/api/public/projects/{slug}` | Get single project details by URL slug |
| `GET` | `/api/public/albums` | List published photography albums with photo counts |
| `GET` | `/api/public/albums/{slug}` | Get album details and associated photos |
| `GET` | `/api/public/photos` | List all published photos across albums |
| `GET` | `/api/public/travel` | List high-altitude expeditions & field notes |
| `GET` | `/api/public/travel/{slug}` | Get expedition journal with route specs and gallery |
| `GET` | `/api/public/blog` | List published articles & field guides |
| `GET` | `/api/public/blog/{slug}` | Get blog post by slug |
| `GET` | `/api/public/skills` | List skills grouped by category |
| `GET` | `/api/public/search?q={query}` | Global full-text search across projects, photos, travel, and blogs |
| `POST` | `/api/public/contact` | Submit contact form message |

### Admin Endpoints (`/api/admin/**`)
All admin endpoints require an `Authorization: Bearer <jwt_token>` header.

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/admin/dashboard/stats` | Summary statistics (counts, unread messages, publish ratios) |
| `GET/POST` | `/api/admin/projects` | List all projects (including drafts) / Create project |
| `PUT/DELETE`| `/api/admin/projects/{id}` | Update project / Delete project |
| `PATCH` | `/api/admin/projects/{id}/publish` | Toggle project publication status |
| `GET/POST` | `/api/admin/albums` | List all albums / Create album |
| `PUT/DELETE`| `/api/admin/albums/{id}` | Update album / Delete album |
| `PATCH` | `/api/admin/albums/{id}/publish` | Toggle album publication status |
| `GET/POST` | `/api/admin/photos` | List all photos / Create photo entry |
| `PUT/DELETE`| `/api/admin/photos/{id}` | Update photo / Delete photo |
| `PATCH` | `/api/admin/photos/{id}/publish` | Toggle photo publication status |
| `GET/POST` | `/api/admin/travel` | List expeditions / Create expedition journal |
| `PUT/DELETE`| `/api/admin/travel/{id}` | Update expedition / Delete expedition |
| `PATCH` | `/api/admin/travel/{id}/publish` | Toggle expedition publication status |
| `GET/POST` | `/api/admin/blogs` | List articles / Create article |
| `PUT/DELETE`| `/api/admin/blogs/{id}` | Update article / Delete article |
| `PATCH` | `/api/admin/blogs/{id}/publish` | Toggle article publication status |
| `GET/POST` | `/api/admin/skills` | List skills / Create skill |
| `PUT/DELETE`| `/api/admin/skills/{id}` | Update skill / Delete skill |
| `GET` | `/api/admin/messages` | List received contact messages |
| `PATCH` | `/api/admin/messages/{id}/read` | Mark message as read / unread |
| `PATCH` | `/api/admin/messages/{id}/replied` | Mark message as replied |
| `DELETE` | `/api/admin/messages/{id}` | Delete message |
| `POST` | `/api/admin/uploads` | Upload image file (returns URL and dimensions) |

### Authentication Endpoints (`/api/auth/**`)

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/auth/login` | Authenticate with email/password; returns JWT token |
| `GET` | `/api/auth/me` | Fetch currently logged-in user profile |

---

## 🛡️ Security Best Practices

1. **Stateless JWT Tokens**: Tokens expire after 24 hours (configurable in `application.properties`). No server session state is stored.
2. **Password Cryptography**: Passwords are encrypted using Spring Security's `BCryptPasswordEncoder` with a cost factor of 12.
3. **Upload File Validation**:
   - Permitted MIME types: `image/jpeg`, `image/png`, `image/webp`, `image/gif`.
   - Max file size: 10 MB per file, 30 MB per request.
   - Unique filenames: Generated with UUID timestamps to prevent collisions.
   - Path-traversal protection: Validated against directory escape sequences (`..`).
4. **CORS & CSRF**:
   - Cross-Origin Resource Sharing is strictly configured to permit requests from Angular client origins.
   - CSRF is disabled safely for stateless REST APIs utilizing Bearer token authentication.

---

## 🧪 Verification & Testing

### Backend Unit & Integration Tests
Run JUnit 5 and Spring Security tests:
```bash
cd backend
mvn clean test
```
*Results: 6 test suites passed with 0 failures.*

### Frontend Tests & Production Build
Run Angular Vitest unit tests:
```bash
cd frontend
npx ng test --watch=false
```
*Results: 1 test passed in headless environment.*

Run Angular production bundle build:
```bash
cd frontend
npm run build
```
*Results: Application bundle successfully generated in `dist/frontend`.*

---

## 🎨 Design Philosophy

- **Dark Obsidian Aesthetics**: Evoking Himalayan night skies and high-altitude ridges with deep obsidian backgrounds (`#090D16`), frosted glass cards (`rgba(15, 23, 42, 0.7)`), and subtle ambient gradient orbs.
- **Accents**:
  - `Frost Cyan` (`#38BDF8`): Reflecting alpine glaciers, code syntax, and technology.
  - `Ember Amber` (`#F59E0B`): Reflecting golden hour summits, campfires, and action buttons.
  - `Alpine Emerald` (`#10B981`): Reflecting mountain valleys and success indicators.
- **Micro-interactions**: Smooth GSAP typing effects, subtle card hovers, image zoom transitions, and full accessibility support via `@media (prefers-reduced-motion)`.

---

## 📄 License
This project is open-source under the [MIT License](LICENSE).

# PrepSpace — Technical Interview Preparation & Placement Tracker SaaS

**PrepSpace** is a full-stack interview preparation, technical learning, and placement tracking web application. It combines a zero-build Vanilla ES6+ Single-Page Application (with PWA offline support and local fallback capabilities) with a Spring Boot REST API backend backed by MySQL 8 (or H2 in-memory mode for local development).

---

## 🛠 Tech Stack

### Backend (`backend/`)
- **Java & Spring Boot 3** (`InterviewTrackerApplication.java`)
- **Spring Security 6** (Stateless HMAC-SHA256 JWT authentication, BCrypt password hashing, account lockout via `LoginAttemptService`, custom `SecurityHeadersFilter`)
- **Spring Data JPA & Hibernate** (48 JPA entities, 49 repositories, 18 service interfaces/implementations, 29 REST controllers)
- **Database**: MySQL 8 (default `application.properties`) and H2 in-memory MySQL-compatibility mode (`application-h2.properties`)
- **AI Integration**: Google Gemini API (`gemini-flash-latest` via `java.net.http.HttpClient` in `AiServiceImpl`)
- **Payments**: Razorpay Java SDK (`com.razorpay`) & Cashfree Payment Gateway REST API with HMAC-SHA256 webhook signature verification (`PaymentController`)
- **Reporting & Exports**: Apache POI (`XSSFWorkbook` for Excel) & iText 7 (`PdfDocument` / `PdfWriter` for PDF)

### Frontend (`frontend/`)
- **Core SPA**: HTML5, CSS3 (`assets/css/index.css`), Bootstrap 5.3.2, Vanilla ES6+ JavaScript with hash-based routing (`assets/js/app.js`)
- **PWA & Offline Support**: `manifest.json` & service worker `sw.js` (app shell `v6.1.0`) plus client-side stale-while-revalidate caching and local fallback auth/data
- **Serverless Email Dispatch**: Vercel serverless function `frontend/api/send-otp.js` powered by the Resend API (`RESEND_API_KEY`) for email OTPs and admin broadcasts
- **Static Pages & Assets**: Standalone `about/`, `privacy/`, `terms/` pages, `assets/data/questions.json`, and downloadable cybersecurity PDFs under `frontend/downloads/cybersecurity/`

---

## ✨ Key Modules & Features

- **Authentication & Security**: Email/password registration and login, Google SSO endpoint, 2FA/MFA verification, account lockout protection, device session tracking, and password change flows.
- **Candidate Dashboard & Telemetry**: Study hours, problem difficulty breakdown, platform solve metrics, readiness score, and weekly activity tracking.
- **Technical Library & Digital Reader**: Structured books and chapters with free previews, Pro subscription gating (`HTTP 403`), reading progress tracking, and bookmarks (`technical-library-data.js` + `LibraryController`).
- **Coding Practice & DSA Roadmap**: Curated problem bank (`questions-data.js`, `questions.json`, `dsa-notes-data.js`), status tracking (solved, bookmarked, favorite, revision), and 8-stage DSA roadmap (`CodingPracticeController`, `QuestionController`).
- **Aptitude Triad & Mock Assessments**: Quantitative, logical, and verbal aptitude modules (`aptitude-curriculum.js`, `AptitudeController`), timed mock exams (`MockTestController`), and mock interviews (`MockInterviewController`, `interview-suite.js`).
- **AI Suite (`ai-ecosystem.js` & `AiController`)**: AI Mentor, AI Doubt Solver, AI Mock Interviewer, weak-topic diagnostics, and personalized study plans powered by Google Gemini (with deterministic fallbacks when `GEMINI_API_KEY` is unset).
- **Study Planner, Notes & Flashcards**: Target company study plans with Pomodoro timer, calendar events, folder-organized notes, rich notes, and spaced-repetition flashcards (`StudyPlanController`, `NotesController`, `CalendarController`).
- **Placement Kanban & Career Hub**: Job application pipeline tracking (`JobApplicationController`), career roadmaps, company-specific questions, readiness diagnostics, and Career Pulse articles (`CareerController`, `PulseController`, `career-pulse.js`).
- **Gamification, Community & Referrals**: Daily missions, daily challenges, XP/coin rewards, leaderboard, reward redemptions, study rooms, interview experiences, discussion posts (`engagement-gamification.js`, `community-social.js`), and referral/withdrawal tracking (`ReferralController`).
- **Courses, Certificates & Reports**: Course enrollment, lesson progress, verifiable certificates (`CourseController`), and Excel/PDF progress exports (`ReportController`).
- **Admin Command Center**: User management, role updates, system settings, library Pro-gating controls, audit logs, and broadcast messaging (`AdminController`, `AdminLibraryController`).

---

## 📁 Directory Structure

```text
.
├── backend/
│   └── src/
│       ├── main/
│       │   ├── java/com/interviewtracker/
│       │   │   ├── InterviewTrackerApplication.java
│       │   │   ├── config/          # QuestionDataLoader, SecurityConfig, SecurityHeadersFilter
│       │   │   ├── controller/      # 29 REST controllers
│       │   │   ├── dto/             # 14 request/response DTOs
│       │   │   ├── entity/          # 48 JPA entities
│       │   │   ├── exception/       # Custom exceptions & GlobalExceptionHandler
│       │   │   ├── jwt/             # JwtAuthenticationFilter, JwtTokenProvider
│       │   │   ├── mapper/          # DtoMapper
│       │   │   ├── repository/      # 49 Spring Data JPA repositories
│       │   │   ├── security/        # CustomUserDetailsService, LoginAttemptService
│       │   │   └── service/         # 18 service interfaces & impl/ implementations
│       │   └── resources/
│       │       ├── application.properties      # Default MySQL config (port 8085)
│       │       ├── application-h2.properties   # Local H2 in-memory config (port 8085)
│       │       ├── questions.json              # Seeded coding/interview questions
│       │       └── static/index.html
│       └── test/
│           └── java/com/interviewtracker/service/
│               ├── AuthServiceTests.java       # 7 unit tests
│               ├── LibraryServiceTests.java    # 6 unit tests
│               └── QuestionServiceTests.java   # 3 unit tests
├── database/
│   ├── schema.sql                   # H2 / MySQL-compatible relational schema
│   └── data.sql                     # Idempotent seed data (users, badges, initial data)
├── docs/
│   ├── 01-product/                  # PRD (01_PRD.md)
│   ├── 02-requirements/             # TRD (02_TRD.md)
│   ├── 03-ui-ux/                    # UI/UX Design System (03_UI_UX_DESIGN.md)
│   ├── 04-database/                 # Database Schema & ERDs (04_DATABASE_SCHEMA.md)
│   ├── 05-architecture/             # System Architecture (05_SYSTEM_ARCHITECTURE.md)
│   ├── 06-api/                      # API Specification (06_API_SPECIFICATION.md)
│   ├── 07-security/                 # Security & Threat Model (07_SECURITY_REQUIREMENTS.md)
│   ├── 08-testing/                  # Test Strategy (08_TEST_STRATEGY.md)
│   ├── 09-traceability/             # Traceability Matrix (09_TRACEABILITY_MATRIX.md)
│   ├── 10-decisions/                # Architecture Decision Records (10_ARCHITECTURE_DECISIONS.md)
│   ├── API-Documentation.md         # REST API quick reference
│   └── deployment-guide.md          # Vercel + Render + Cloud MySQL deployment manual
└── frontend/
    ├── index.html                   # Main SPA shell (loads 13 JS modules & PWA SW)
    ├── manifest.json                # PWA manifest
    ├── sw.js                        # Service worker (precache + offline fallback)
    ├── about/                       # Static About page
    ├── privacy/                     # Static Privacy Policy page
    ├── terms/                       # Static Terms of Service page
    ├── api/
    │   └── send-otp.js              # Vercel serverless function (Resend email OTP/broadcasts)
    ├── assets/
    │   ├── css/index.css            # Design tokens, dark/light themes, glassmorphism
    │   ├── data/questions.json      # Client-side question dataset
    │   └── js/                      # app.js, components.js, ai-ecosystem.js, etc. (13 files)
    └── downloads/cybersecurity/     # Downloadable cybersecurity PDF guides
```

> **Note on build files**: `pom.xml`, `Dockerfile`, and `docker-compose.yml` are not currently present in this workspace. See [Backend Setup](#2-backend-spring-boot-api--port-8085) below before compiling or containerizing the backend.

---

## 🌐 Backend API Route Groups

The backend exposes REST endpoints on port `8085` (local: `http://localhost:8085/api`, production: `https://api.stream-in.app/api`):

| Controller | Base Mapping | Purpose |
| :--- | :--- | :--- |
| `AuthController` | `/api/auth`, `/api/v1/auth` | Register, login, Google SSO, MFA/OTP auth |
| `UserController` | `/api/users` | Profile, password updates, user metadata |
| `DashboardController` | `/api/dashboard` | Aggregated dashboard stats & metrics |
| `LibraryController` | `/api/v1/library`, `/api/library` | Books, gated chapters, progress, bookmarks |
| `CodingPracticeController` | `/api/v1` | `/questions/**`, `/dsa/roadmap` |
| `QuestionController` | `/api/questions` | Question bank, bookmarks, notes |
| `AptitudeController` | `/api/v1/aptitude` | Quant, logical, and verbal practice |
| `CourseController` | `/api/v1` | `/courses/**`, `/certificates/**` |
| `MockTestController` | `/api/v1/mocktests` | Timed MCQ mock exams |
| `MockInterviewController` | `/api/mock` | Mock interview scheduling & feedback |
| `AiController` | `/api/v1/ai` | Weak topics, study plans, AI mentor/interviewer |
| `StudyPlanController` | `/api/studyplans` | Study plans & milestones |
| `NotesController` | `/api/v1` | `/notes/**`, `/flashcards/**` |
| `CalendarController` | `/api/calendar` | Interview & study calendar events |
| `JobApplicationController` | `/api/applications` | Placement Kanban applications |
| `ProgressController` | `/api/progress` | Study & topic progress logs |
| `ReportController` | `/api/reports` | Excel & PDF progress exports |
| `DailyMissionController` | `/api/v1/missions` | Daily study missions |
| `DailyChallengeController` | `/api/v1/challenges` | Daily coding/aptitude challenges |
| `GamificationController` | `/api/v1/gamification` | XP, coins, badges, leaderboards, rewards |
| `CommunitySocialController` | `/api/v1/social` | Study rooms, experiences, discussions |
| `CareerController` | `/api/v1/career` | Career goals, roadmaps, readiness, company prep |
| `PulseController` | `/api/v1/pulse` | Tech articles & quick-learn bites |
| `NotificationController` | `/api/v1/notifications` | In-app notifications |
| `ReferralController` | `/api/referrals` | Referral codes, rewards & withdrawals |
| `PaymentController` | `/api/payments` | Razorpay & Cashfree orders, verification & webhooks |
| `SettingsController` | `/api/v1/settings` | User preferences & configuration |
| `AdminController` | `/api/admin` | Platform KPIs, user/role/question admin |
| `AdminLibraryController` | `/api/admin/library` | Book/chapter management & Pro toggles |

See [docs/API-Documentation.md](docs/API-Documentation.md) and [docs/06-api/06_API_SPECIFICATION.md](docs/06-api/06_API_SPECIFICATION.md) for payload details.

---

## ⚙️ Environment Variables

### Backend (`application.properties`)
| Variable | Default / Fallback | Purpose |
| :--- | :--- | :--- |
| `SPRING_DATASOURCE_URL` | `jdbc:mysql://localhost:3306/interview_tracker?...` | JDBC connection URL |
| `SPRING_DATASOURCE_USERNAME` | `root` | Database username |
| `SPRING_DATASOURCE_PASSWORD` | *(empty)* | Database password |
| `DB_DRIVER` | `com.mysql.cj.jdbc.Driver` | JDBC driver class |
| `DB_DIALECT` | `org.hibernate.dialect.MySQLDialect` | Hibernate dialect |
| `SPRING_JPA_HIBERNATE_DDL_AUTO` | `update` | Hibernate DDL strategy |
| `SPRING_SQL_INIT_MODE` | `never` | SQL init script mode |
| `GEMINI_API_KEY` | *(empty)* | Google Gemini API key (`AiServiceImpl`) |
| `RAZORPAY_KEY_ID` / `RAZORPAY_KEY_SECRET` | Test fallback in `PaymentController` | Razorpay credentials |
| `RAZORPAY_WEBHOOK_SECRET` | *(unset)* | Razorpay webhook HMAC verification |
| `CASHFREE_APP_ID` / `CASHFREE_SECRET_KEY` / `CASHFREE_ENV` | *(unset)* | Cashfree Payment Gateway credentials |

### Frontend Serverless Function (`frontend/api/send-otp.js`)
| Variable | Purpose |
| :--- | :--- |
| `RESEND_API_KEY` | Required by Vercel `/api/send-otp` function to dispatch OTP and admin emails via Resend |

---

## 🚀 Running Locally

### 1. Frontend SPA (Standalone)
The frontend has no build step and runs directly from any static HTTP server. Serving `frontend/` as the web root ensures root-relative paths (`/assets/...`, `/manifest.json`, `/sw.js`) resolve properly:

```powershell
python -m http.server 3000 --directory frontend
```

Then open **http://localhost:3000**.

- On `localhost` / `127.0.0.1`, `app.js` automatically targets `http://localhost:8085/api` (unless the page itself is served on port `8085`, in which case it uses `/api`).
- If the backend is not running, the frontend automatically falls back to local authentication (`localStorage`) and bundled curriculum/question datasets so you can explore the UI offline.

### 2. Backend (Spring Boot API — Port `8085`)
The Java source tree in `backend/src/` is configured for **Spring Boot 3** on port **`8085`**:
- **MySQL 8 mode** (`application.properties`): expects database `interview_tracker` (schema & seed SQL are in `database/schema.sql` and `database/data.sql`).
- **H2 in-memory mode** (`application-h2.properties`): launch with `--spring.profiles.active=h2` (`jdbc:h2:mem:interview_tracker;DB_CLOSE_DELAY=-1;MODE=MySQL`).

> **Current workspace status**: Because `pom.xml` is not currently checked into `backend/` or the repository root, add a Maven `pom.xml` (or Gradle build file) declaring Spring Boot Web, Security, Data JPA, Validation, MySQL/H2 drivers, `jjwt`, Lombok, Apache POI, iText 7, `org.json`, and `razorpay-java` before running `mvn spring-boot:run` or `mvn test`.

### Seeded Database Accounts (`database/data.sql`)
- **Admin**: `admin@tracker.com` (`ADMIN`)
- **Student**: `nagesh@tracker.com` (`STUDENT`)

---

## 🧪 Testing

The backend includes 16 JUnit 5 + Mockito unit tests under `backend/src/test/java/com/interviewtracker/service/`:
- `AuthServiceTests.java` (7 tests)
- `LibraryServiceTests.java` (6 tests)
- `QuestionServiceTests.java` (3 tests)

Once `pom.xml` and Maven are configured, run:
```bash
mvn test
```

---

## 📚 Documentation Index

- [Product Requirements Document (PRD)](docs/01-product/01_PRD.md)
- [Technical Requirements Document (TRD)](docs/02-requirements/02_TRD.md)
- [UI/UX Design System](docs/03-ui-ux/03_UI_UX_DESIGN.md)
- [Database Architecture & Schema](docs/04-database/04_DATABASE_SCHEMA.md)
- [System Architecture Specification](docs/05-architecture/05_SYSTEM_ARCHITECTURE.md)
- [REST API Specification](docs/06-api/06_API_SPECIFICATION.md) & [API Quick Reference](docs/API-Documentation.md)
- [Security Architecture & Threat Model](docs/07-security/07_SECURITY_REQUIREMENTS.md)
- [QA & Test Strategy](docs/08-testing/08_TEST_STRATEGY.md)
- [Requirements Traceability Matrix](docs/09-traceability/09_TRACEABILITY_MATRIX.md)
- [Architecture Decision Records (ADRs)](docs/10-decisions/10_ARCHITECTURE_DECISIONS.md)
- [Production Deployment Guide](docs/deployment-guide.md)

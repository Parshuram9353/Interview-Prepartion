# PrepSpace REST API Documentation

This catalog outlines the REST endpoints exposed by the Spring Boot backend server (and the Vercel serverless email endpoint). For full contract details, see [`docs/06-api/06_API_SPECIFICATION.md`](./06-api/06_API_SPECIFICATION.md).

- **Local Backend Base URL**: `http://localhost:8085/api`
- **Production Backend Base URL**: `https://api.stream-in.app/api`
- **Frontend Serverless Email Endpoint**: `POST /api/send-otp` (`frontend/api/send-otp.js`)

## 📋 Complete Controller Index (29 Controllers)

| Controller | Base Path(s) | Responsibility |
| :--- | :--- | :--- |
| `AuthController` | `/api/auth`, `/api/v1/auth` | Registration, login, Google SSO, MFA verification |
| `UserController` | `/api/users` | Profile read/update, password changes |
| `DashboardController` | `/api/dashboard` | Aggregated candidate telemetry & charts |
| `LibraryController` | `/api/v1/library`, `/api/library` | Books catalog, Pro-gated chapters, progress, bookmarks |
| `CodingPracticeController` | `/api/v1` | `/questions/**`, `/dsa/roadmap` |
| `QuestionController` | `/api/questions` | Question list, bookmarks, personal notes |
| `AptitudeController` | `/api/v1/aptitude` | Quantitative, logical, and verbal aptitude modules |
| `CourseController` | `/api/v1` | `/courses/**`, `/certificates/**` |
| `MockTestController` | `/api/v1/mocktests` | Timed MCQ mock exams |
| `MockInterviewController` | `/api/mock` | Mock interview scheduling & feedback |
| `AiController` | `/api/v1/ai` | Weak topics, study plans, AI mentor & interviewer |
| `StudyPlanController` | `/api/studyplans` | Target company study plans |
| `NotesController` | `/api/v1` | `/notes/**`, `/notes/folders`, `/flashcards/**` |
| `CalendarController` | `/api/calendar` | Interview & study schedule events |
| `JobApplicationController` | `/api/applications` | Placement Kanban pipeline CRUD & status moves |
| `ProgressController` | `/api/progress` | Topic & study progress tracking |
| `ReportController` | `/api/reports` | Excel (Apache POI) & PDF (iText 7) exports |
| `DailyMissionController` | `/api/v1/missions` | Daily candidate missions |
| `DailyChallengeController` | `/api/v1/challenges` | Daily challenges & attempts |
| `GamificationController` | `/api/v1/gamification` | XP, coins, badges, leaderboards, rewards |
| `CommunitySocialController` | `/api/v1/social` | Study rooms, interview experiences, discussions |
| `CareerController` | `/api/v1/career` | Career goals, roadmaps, readiness, company prep |
| `PulseController` | `/api/v1/pulse` | Career Pulse articles & quick-learn bites |
| `NotificationController` | `/api/v1/notifications` | Candidate notification center |
| `ReferralController` | `/api/referrals` | Referral codes, rewards & withdrawals |
| `PaymentController` | `/api/payments` | Razorpay (`/order`, `/verify`, `/webhook`) & Cashfree (`/cashfree/order`, `/cashfree/verify`, `/cashfree/webhook`) |
| `SettingsController` | `/api/v1/settings` | User preferences & 2FA settings |
| `AdminController` | `/api/admin` | Platform stats, user management, questions & broadcasts |
| `AdminLibraryController` | `/api/admin/library` | Library inventory & Pro-gating administration |

---

## 🔑 Authentication Endpoints (`/api/auth`)

### 1. Register Account
* **Endpoint**: `/api/auth/register`
* **Method**: `POST`
* **Security**: `Public` (ALL)
* **Request Payload**:
```json
{
  "name": "Nagesh Methre",
  "email": "nagesh@tracker.com",
  "password": "password123"
}
```
* **Response**: `201 Created`
```json
{
  "id": 2,
  "name": "Nagesh Methre",
  "email": "nagesh@tracker.com",
  "role": "USER"
}
```

### 2. Login User
* **Endpoint**: `/api/auth/login`
* **Method**: `POST`
* **Security**: `Public` (ALL)
* **Request Payload**:
```json
{
  "email": "nagesh@tracker.com",
  "password": "password123"
}
```
* **Response**: `200 OK`
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "email": "nagesh@tracker.com",
  "name": "Nagesh Methre",
  "role": "USER"
}
```

---

## 👤 User Profile Endpoints (`/api/users`)

### 1. Get Profile Details
* **Endpoint**: `/api/users/profile`
* **Method**: `GET`
* **Security**: `Authenticated` (USER, ADMIN)
* **Response**: `200 OK`
```json
{
  "id": 2,
  "name": "Nagesh Methre",
  "email": "nagesh@tracker.com",
  "role": "USER"
}
```

### 2. Update Profile Name
* **Endpoint**: `/api/users/profile`
* **Method**: `PUT`
* **Security**: `Authenticated`
* **Request Payload**:
```json
{
  "name": "Nagesh Methre Edited"
}
```
* **Response**: `200 OK` (returns updated user details)

### 3. Change Password
* **Endpoint**: `/api/users/change-password`
* **Method**: `PUT`
* **Security**: `Authenticated`
* **Request Payload**:
```json
{
  "oldPassword": "password123",
  "newPassword": "newPassword456"
}
```
* **Response**: `200 OK`
```json
{
  "message": "Password changed successfully"
}
```

---

## 📊 Dashboard Statistics (`/api/dashboard`)

### 1. Fetch Aggregated Metrics
* **Endpoint**: `/api/dashboard/stats`
* **Method**: `GET`
* **Security**: `Authenticated`
* **Response**: `200 OK`
```json
{
  "totalStudyHours": 6.3,
  "completedTopics": 3,
  "upcomingInterviewsCount": 0,
  "applicationsCount": 5,
  "statusCounts": {
    "APPLIED": 1,
    "PHONE_SCREEN": 1,
    "INTERVIEW_SCHEDULED": 1,
    "OFFER": 1,
    "REJECTED": 1
  },
  "weeklyStudyTime": {
    "2026-07-03": 0,
    "2026-07-04": 0,
    "2026-07-05": 45,
    "2026-07-06": 60,
    "2026-07-07": 90,
    "2026-07-08": 120,
    "2026-07-09": 60
  },
  "difficultyStats": {
    "EASY": 1,
    "MEDIUM": 2,
    "HARD": 0
  },
  "codingPlatformsSolved": {
    "LeetCode": 33,
    "CodeChef": 13,
    "Codeforces": 6
  },
  "streak": 5,
  "readinessScore": 82,
  "xpPoints": 940
}
```

---

## ❓ Question Bank Endpoints (`/api/questions`)

### 1. List / Filter Questions
* **Endpoint**: `/api/questions?company=Meta&category=Data Structures&difficulty=EASY`
* **Method**: `GET`
* **Security**: `Authenticated`
* **Response**: `200 OK`
```json
[
  {
    "id": 6,
    "title": "Valid Parentheses",
    "company": "Meta",
    "category": "Data Structures",
    "difficulty": "EASY",
    "question": "Given a string s, determine if valid...",
    "answer": "Use a Stack...",
    "tags": "Stack,String",
    "bookmarked": false,
    "noteContent": null
  }
]
```

### 2. Toggle Bookmark
* **Endpoint**: `/api/questions/{id}/bookmark`
* **Method**: `POST`
* **Security**: `Authenticated`
* **Response**: `200 OK`

### 3. Add/Edit Personal Note
* **Endpoint**: `/api/questions/{id}/note`
* **Method**: `POST`
* **Security**: `Authenticated`
* **Request Payload**:
```json
{
  "note": "Optimized space to O(N) by checking length first."
}
```
* **Response**: `200 OK`

---

## 💼 Job Tracker Endpoints (`/api/applications`)

### 1. Create Job Application
* **Endpoint**: `/api/applications`
* **Method**: `POST`
* **Security**: `Authenticated`
* **Request Payload**:
```json
{
  "company": "Meta",
  "role": "Software Engineer II",
  "appliedDate": "2026-07-09",
  "status": "APPLIED"
}
```
* **Response**: `201 Created` (returns created Application DTO)

### 2. Update Kanban Column Status
* **Endpoint**: `/api/applications/{id}/status`
* **Method**: `PUT`
* **Security**: `Authenticated`
* **Request Payload**:
```json
{
  "status": "INTERVIEW_SCHEDULED"
}
```
* **Response**: `200 OK`

---

## 🛡 Administrator Actions (`/api/admin`)

### 1. View Registered Users List
* **Endpoint**: `/api/admin/users`
* **Method**: `GET`
* **Security**: `Role: ADMIN`
* **Response**: `200 OK` (list of registered users details)

### 2. Revoke User Access
* **Endpoint**: `/api/admin/users/{id}`
* **Method**: `DELETE`
* **Security**: `Role: ADMIN`
* **Response**: `200 OK`

### 3. Publish New Official Question
* **Endpoint**: `/api/admin/questions`
* **Method**: `POST`
* **Security**: `Role: ADMIN`
* **Request Payload**:
```json
{
  "title": "Reverse Linked List",
  "company": "Amazon",
  "category": "Data Structures",
  "difficulty": "EASY",
  "question": "Reverse a singly linked list.",
  "answer": "Use three pointers (prev, curr, next).",
  "tags": "Linked List"
}
```
* **Response**: `201 Created`

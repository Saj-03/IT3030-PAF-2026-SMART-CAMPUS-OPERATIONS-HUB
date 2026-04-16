# Smart Campus Operations Hub 🏫✨

A production-inspired university facility management system built for the **IT3030 PAF Assignment (2026, Semester 1)**. This platform features a modern Dark Gradient design with glassmorphism, real-time notifications, and advanced resource tracking.

---

## 👥 Team & Module Contributions

> **Individual contribution is assessed at the viva — each member owns the endpoints listed below.**

| Member | IT Number | Modules Owned | Key Endpoints |
|--------|-----------|---------------|---------------|
| Sulakshana | IT23249752 | **Module A** — Facilities & Assets Catalogue | `GET/POST/PUT/DELETE /api/resources`, `GET /api/resources/{id}/image` |
| Kiyara | IT2345599 | **Module B** — Booking Management | `GET/POST /api/bookings`, `GET /api/bookings/{id}`, `PUT /api/bookings/{id}/status`, `DELETE /api/bookings/{id}/cancel` |
| Sajalee | IT234576869 | **Module C** — Incident Ticketing, **Module D** — Notifications, **Module E** — Auth & Users | `GET/POST /api/tickets`, `PUT /api/tickets/{id}/status`, `GET/PUT /api/notifications/...`, `GET/PUT/DELETE /api/auth/users` |

---

## 🌟 Key Highlights

- **Premium Aesthetics**: Custom-built with a deep-sea dark theme, radial gradients, and fluid micro-animations.
- **Dynamic Catalogue**: Switch between Horizontal List and Modern Grid views with real-time search and filtering.
- **Robust Validation**: Multi-layer JSR-303 (Backend) and Smart Logic (Frontend) validation for all inputs.
- **Role-Based Access**: Specialized dashboards for Students/Staff (`USER`), Admins (`ADMIN`), and Technicians (`TECHNICIAN`).
- **Conflict Detection**: Booking service prevents overlapping approved reservations at the database query level.
- **Unit Tested**: 20+ Mockito-based unit tests across ResourceService, BookingService, and TicketService.

---

## 🛠️ Modules

### Module A: Facilities & Assets Catalogue (Sulakshana)
- View resources in **List** or **Grid** mode.
- Real-time search by name, location, or type.
- Admin: Create/edit/delete resources with optional image upload.
- Status tracking: `ACTIVE`, `OUT_OF_SERVICE`.

### Module B: Booking Management (Anne)
- Users request bookings with date, time range, and purpose.
- Conflict check prevents double-booking approved slots.
- Workflow: `PENDING → APPROVED / REJECTED`; users can self-cancel.
- Admin reviews all bookings; users see only their own.

### Module C: Maintenance & Incident Ticketing (Samantha)
- Users report issues with category, description, priority, contact info.
- Up to 3 image attachments per ticket.
- Ticket workflow: `OPEN → IN_PROGRESS → RESOLVED → CLOSED / REJECTED`.
- Technicians pick up and resolve tickets; all parties can comment.

### Module D: Notifications (Samantha)
- In-app notification panel shows all alerts.
- Unread badge count updates automatically.
- Mark individual or all notifications as read.
- Triggered on booking approval/rejection, ticket status changes, and new comments.

### Module E: Authentication & Authorization (Samantha)
- Google OAuth 2.0 login — no password management needed.
- JWT-based stateless session — token stored in `localStorage`.
- Roles: `ROLE_USER`, `ROLE_ADMIN`, `ROLE_TECHNICIAN`.
- Admin can list all users and promote/demote roles via API.

---

## 💻 Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, Vite, Vanilla CSS (custom design tokens) |
| Backend | Spring Boot 3.4.5, Spring Security, Jakarta Validation |
| Auth | Google OAuth2 + JWT (jjwt 0.11.5) |
| Database | MySQL 8.0 |
| Testing | JUnit 5, Mockito |
| CI/CD | GitHub Actions (build + test on every push) |

---

## 🚀 Getting Started

### Prerequisites
- Java 17+
- Node.js 18+
- MySQL 8.0+
- A Google Cloud OAuth2 Client ID & Secret

### 1. Database Setup
```sql
CREATE DATABASE smart_campus;
```

### 2. Backend Setup
1. Navigate to `/backend`
2. Copy credentials into `src/main/resources/application.properties`:
   ```properties
   spring.datasource.url=jdbc:mysql://localhost:3306/smart_campus
   spring.datasource.username=root
   spring.datasource.password=YOUR_PASSWORD
   spring.security.oauth2.client.registration.google.client-id=YOUR_CLIENT_ID
   spring.security.oauth2.client.registration.google.client-secret=YOUR_CLIENT_SECRET
   ```
3. Run: `./mvnw spring-boot:run`
4. Backend starts at **http://localhost:8080**

### 3. Frontend Setup
```bash
cd frontend
npm install
npm run dev
```
Frontend starts at **http://localhost:5173**

### 4. Running Tests
```bash
cd backend
./mvnw test
```

---

## 🔑 API Endpoint Reference

### Auth & Users (`/api/auth`)
| Method | Endpoint | Role | Description |
|--------|----------|------|-------------|
| GET | `/api/auth/me` | Any | Get current user profile |
| GET | `/api/auth/users` | ADMIN | List all users |
| PUT | `/api/auth/users/{id}/role` | ADMIN | Change user role |
| DELETE | `/api/auth/users/{id}` | ADMIN | Delete a user |

### Resources (`/api/resources`)
| Method | Endpoint | Role | Description |
|--------|----------|------|-------------|
| GET | `/api/resources` | Any | List resources (filter: `?type=LAB&minCapacity=20`) |
| GET | `/api/resources/{id}` | Any | Get resource by ID |
| GET | `/api/resources/{id}/image` | Any | Get resource image (binary) |
| POST | `/api/resources` | ADMIN | Create resource (multipart) |
| PUT | `/api/resources/{id}` | ADMIN | Update resource (multipart) |
| DELETE | `/api/resources/{id}` | ADMIN | Delete resource |

### Bookings (`/api/bookings`)
| Method | Endpoint | Role | Description |
|--------|----------|------|-------------|
| GET | `/api/bookings` | ADMIN | All bookings |
| GET | `/api/bookings/{id}` | Any | Single booking |
| GET | `/api/bookings/user/{userId}` | USER | Own bookings |
| POST | `/api/bookings` | USER | Create booking request |
| PUT | `/api/bookings/{id}/status` | ADMIN | Approve / Reject |
| DELETE | `/api/bookings/{id}/cancel` | USER | Cancel own booking |

### Tickets (`/api/tickets`)
| Method | Endpoint | Role | Description |
|--------|----------|------|-------------|
| GET | `/api/tickets` | ADMIN | All tickets |
| GET | `/api/tickets/{id}` | Any | Single ticket |
| GET | `/api/tickets/user/{userId}` | USER | Own tickets |
| GET | `/api/tickets/technician/{techId}` | TECHNICIAN | Assigned tickets |
| POST | `/api/tickets` | USER | Create ticket |
| PUT | `/api/tickets/{id}/assign/{techId}` | ADMIN | Assign technician |
| PUT | `/api/tickets/{id}/status` | TECHNICIAN/ADMIN | Update ticket status |
| POST | `/api/tickets/{id}/comments` | Any | Add comment |
| GET | `/api/tickets/{id}/comments` | Any | Get comments |
| DELETE | `/api/tickets/comments/{commentId}/user/{userId}` | USER | Delete own comment |
| POST | `/api/tickets/{id}/attachments` | USER | Upload attachment |
| GET | `/api/tickets/{id}/attachments` | Any | List attachments |

### Notifications (`/api/notifications`)
| Method | Endpoint | Role | Description |
|--------|----------|------|-------------|
| GET | `/api/notifications/user/{userId}` | Any | Get user notifications |
| GET | `/api/notifications/user/{userId}/unread-count` | Any | Unread badge count |
| PUT | `/api/notifications/{id}/read` | Any | Mark one as read |
| PUT | `/api/notifications/user/{userId}/read-all` | Any | Mark all as read |
| POST | `/api/notifications/send` | ADMIN | Manual notification |

---

## 🧪 Testing Evidence

Unit tests are located in `backend/src/test/java/com/smartcampus/backend/`:

| Test File | Coverage |
|-----------|---------|
| `ResourceServiceTest.java` | getAllResources, getById (found/not-found), create, delete, search by type/capacity |
| `BookingServiceTest.java` | Create (no conflict), create (conflict detected), approve, reject with reason, user cancel, unauthorized cancel, getUserBookings |
| `TicketServiceTest.java` | Create with OPEN status, getById (found/not-found), assign technician, status update with notes, add comment with notification, delete comment (owner/non-owner), getByCreator |

Run: `cd backend && ./mvnw test`

---

## 📸 Visual Tour

| Dashboard | Catalogue | Notifications |
|:----------|:----------|:--------------|
| ![User Dashboard](docs/images/user-dashaboard.png) | ![Catalogue](docs/images/user-catalogue-page.png) | ![Notifications](docs/images/user-notification-page.png) |

| Admin Dashboard | Resource Management | Booking Control |
|:----------------|:--------------------|:----------------|
| ![Admin Dashboard](docs/images/admin-dashboard.png) | ![Admin Catalogue](docs/images/admin-catalogue-page.png) | ![Bookings](docs/images/admin-manageBooking-page.png) |

> **Technician View**: Service Desk for rapid incident resolution.
> ![Service Desk](docs/images/admin-serviceDesk-page.png)

---

## ⚡ Optional Innovations Implemented

1. **Usage Analytics Ready** — Backend structured to track resource booking frequency.
2. **SLA Tracking** — Tickets track `createdAt` for time-to-resolution reporting.
3. **Skeleton Loaders** — Seamless loading states during data fetch.
4. **Unread Notification Badge** — Live count in the navbar.
5. **Mark All as Read** — Single-click bulk notification dismiss.
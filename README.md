# 🚀 SecureAPI — Auth & Role-Based REST API

A production-ready REST API with JWT authentication, role-based access control, and a React frontend. Built as part of the Primetrade.ai Backend Intern assignment.

---

## 📋 Table of Contents

- [Tech Stack](#tech-stack)
- [Features](#features)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [Security](#security)
- [Scalability Notes](#scalability-notes)

---

## 📸 Screenshots

### 🔐 Login Page
![Login Page](./screenshot-login.png)

### 🗂 Admin Dashboard
![Admin Dashboard](./screenshot-dashboard.png)

### ➕ Create Task Modal
![Create Task](./screenshot-create-task.png)

---

## 🛠 Tech Stack

| Layer        | Technology                        |
|--------------|-----------------------------------|
| Runtime      | Node.js + Express.js              |
| Database     | PostgreSQL (via Prisma ORM)       |
| Auth         | JWT (access + refresh tokens)     |
| Validation   | Zod / Joi                         |
| Docs         | Swagger UI (`/api/v1/docs`)       |
| Frontend     | React.js + Axios                  |
| Optional     | Redis (caching), Docker           |

---

## ✅ Features

### Authentication
- User registration with bcrypt password hashing
- Login with JWT (access token + optional refresh token)
- Protected routes via middleware

### Role-Based Access Control
- `user` — can manage their own resources
- `admin` — full access to all resources and user management

### CRUD — Tasks (or your chosen entity)
- Create, read, update, delete tasks
- Ownership enforced at the API level
- Admins can view/manage all tasks

### API Quality
- Versioned routes: `/api/v1/...`
- Consistent error responses with HTTP status codes
- Input validation and sanitization on all endpoints
- Swagger docs auto-generated

---

## 📁 Project Structure

```
├── src/
│   ├── config/          # DB connection, env config
│   ├── controllers/     # Route handlers
│   ├── middlewares/     # Auth, error handling, validation
│   ├── models/          # Prisma schema / DB models
│   ├── routes/
│   │   └── v1/          # Versioned API routes
│   ├── services/        # Business logic layer
│   ├── utils/           # JWT helpers, response formatters
│   └── app.js           # Express app setup
├── frontend/            # React UI
├── prisma/
│   └── schema.prisma    # DB schema
├── .env.example
├── docker-compose.yml   # Optional
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js v18+
- PostgreSQL running locally or via Docker
- (Optional) Redis

### 1. Clone the repo

```bash
git clone https://github.com/your-username/secure-api.git
cd secure-api
```

### 2. Install dependencies

```bash
npm install
```

### 3. Set up environment variables

```bash
cp .env.example .env
# Edit .env with your DB credentials and JWT secret
```

### 4. Run database migrations

```bash
npx prisma migrate dev --name init
```

### 5. Start the server

```bash
# Development
npm run dev

# Production
npm start
```

### 6. Start the frontend

```bash
cd frontend
npm install
npm start
```

Server runs at `http://localhost:5000`  
Frontend runs at `http://localhost:3000`  
Swagger docs at `http://localhost:5000/api/v1/docs`

---

## 🔐 Environment Variables

```env
# Server
PORT=5000
NODE_ENV=development

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/secureapi

# JWT
JWT_SECRET=your_super_secret_key
JWT_EXPIRES_IN=7d

# Optional: Redis
REDIS_URL=redis://localhost:6379
```

---

## 📖 API Documentation

Interactive Swagger docs are available at `/api/v1/docs` once the server is running.

### Key Endpoints

| Method | Endpoint                  | Access       | Description             |
|--------|---------------------------|--------------|-------------------------|
| POST   | `/api/v1/auth/register`   | Public       | Register a new user     |
| POST   | `/api/v1/auth/login`      | Public       | Login and get JWT       |
| GET    | `/api/v1/tasks`           | User / Admin | Get all tasks           |
| POST   | `/api/v1/tasks`           | User / Admin | Create a task           |
| PUT    | `/api/v1/tasks/:id`       | Owner / Admin| Update a task           |
| DELETE | `/api/v1/tasks/:id`       | Owner / Admin| Delete a task           |
| GET    | `/api/v1/admin/users`     | Admin only   | List all users          |

A full Postman collection is included in `/docs/postman_collection.json`.

---

## 🗃 Database Schema

```
users
  id          UUID (PK)
  name        VARCHAR
  email       VARCHAR (unique)
  password    VARCHAR (hashed)
  role        ENUM('user', 'admin')
  created_at  TIMESTAMP

tasks
  id          UUID (PK)
  title       VARCHAR
  description TEXT
  status      ENUM('pending', 'in_progress', 'done')
  user_id     UUID (FK → users.id)
  created_at  TIMESTAMP
  updated_at  TIMESTAMP
```

---

## 🔒 Security

- Passwords hashed with **bcrypt** (salt rounds: 12)
- JWT signed with `HS256`, short-lived access tokens
- Input validated and sanitized on every endpoint
- Role checks enforced in middleware, not controllers
- `.env` never committed — secrets managed via environment

---

## 📈 Scalability Notes

This project is structured to scale gracefully:

- **Modular architecture** — adding new modules (e.g., products, invoices) requires only a new route + controller + service, with zero changes to existing code
- **Service layer** — business logic is decoupled from HTTP handling, making it testable and reusable
- **Caching** — Redis can be plugged in for caching frequent reads (e.g., user profile, task lists) to reduce DB load
- **Horizontal scaling** — stateless JWT auth means multiple API instances can run behind a load balancer without session sharing
- **Docker** — `docker-compose.yml` included to spin up the API + DB + Redis as isolated containers
- **Future-ready** — service boundaries are clean enough to extract into microservices (auth service, task service) if traffic demands it

---

## 🧪 Running Tests

```bash
npm test
```

---

## 📬 Contact

Built by **[Your Name]**  
[your.email@example.com](mailto:your.email@example.com) · [GitHub](https://github.com/your-username)

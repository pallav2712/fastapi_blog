# FastAPI Blog

A full-stack blog application built with **FastAPI**, **PostgreSQL/SQLite**, **SQLAlchemy**, and **JWT authentication**.

The project provides a REST API for managing users and blog posts, along with a server-rendered web interface using **Jinja2 templates**. It also includes user authentication, password management, profile-picture uploads, pagination, database migrations, and automated tests.

## 🚀 Features

### 👤 User Management

* User registration
* User login
* JWT-based authentication
* Get current authenticated user
* Get user by ID
* Update user profile
* Delete user account
* View posts created by a user

### 🔐 Authentication & Security

* JWT access tokens
* OAuth2 password flow
* Password hashing using Argon2
* Protected routes using FastAPI dependencies
* Password change functionality
* Forgot-password flow
* Secure password-reset tokens
* Authorization checks for user-owned resources

### 📝 Blog Posts

* Create posts
* Retrieve a single post
* Retrieve paginated posts
* Update posts using `PUT`
* Partially update posts using `PATCH`
* Delete posts
* Associate posts with their authors
* Paginated API responses

### 🖼️ Profile Pictures

* Upload profile pictures
* Image validation and processing
* File-size validation
* Store profile images in Amazon S3
* Delete old profile images when replaced
* Delete profile images independently

### 🌐 Web Interface

* Server-rendered blog pages using Jinja2
* Home page displaying recent posts
* Individual post pages
* User post pages
* Static CSS/assets

### 🗄️ Database

* SQLAlchemy 2.0 ORM
* Asynchronous database operations
* Alembic database migrations
* SQLite support
* PostgreSQL support
* User/Post relationships
* Password-reset-token persistence

### 🧪 Testing

* Pytest-based test suite
* S3 testing support using Moto

---

## 🛠️ Tech Stack

| Technology              | Purpose                      |
| ----------------------- | ---------------------------- |
| **Python 3.12+**        | Programming language         |
| **FastAPI**             | Web framework / REST API     |
| **SQLAlchemy 2.0**      | ORM and database interaction |
| **Alembic**             | Database migrations          |
| **PostgreSQL / SQLite** | Database                     |
| **Pydantic**            | Request/response validation  |
| **JWT**                 | Authentication               |
| **Argon2**              | Password hashing             |
| **Jinja2**              | Server-side HTML templates   |
| **Amazon S3**           | Profile-image storage        |
| **Pillow**              | Image processing             |
| **Pytest**              | Testing                      |
| **Moto**                | AWS/S3 testing               |
| **Uvicorn**             | ASGI server                  |

The project's dependency configuration currently requires Python 3.12+ and includes FastAPI, SQLAlchemy, Alembic, PostgreSQL support, JWT, password hashing, Pillow, boto3, and testing dependencies.

---

## 📁 Project Structure

```text
fastapi_blog/
│
├── alembic/
│   └── ...                 # Database migration files
│
├── media/
│   └── profile_pics/       # Local media/profile-picture resources
│
├── populate_images/
│   └── ...                 # Image population utilities
│
├── routers/
│   ├── posts.py            # Post-related API endpoints
│   └── users.py            # User/auth-related endpoints
│
├── static/
│   └── ...                 # Static assets
│
├── templates/
│   ├── home.html
│   ├── post.html
│   └── ...                 # Jinja2 templates
│
├── tests/
│   └── ...                 # Automated tests
│
├── auth.py                 # Authentication and JWT logic
├── config.py               # Application configuration
├── database.py             # Database engine/session setup
├── email_utils.py          # Email/password-reset utilities
├── image_utils.py          # Image processing and S3 utilities
├── main.py                 # FastAPI application entry point
├── models.py               # SQLAlchemy database models
├── populate_db.py          # Database population utility
├── schemas.py              # Pydantic schemas
│
├── alembic.ini
├── pyproject.toml
└── README.md
```

The repository separates the API into `users` and `posts` routers, while `main.py` registers them under `/api/users` and `/api/posts`.

---

# ⚙️ Getting Started

## 1. Clone the Repository

```bash
git clone https://github.com/pallav2712/fastapi_blog.git
cd fastapi_blog
```

## 2. Create a Virtual Environment

```bash
python -m venv .venv
```

### Linux / macOS

```bash
source .venv/bin/activate
```

### Windows

```bash
.venv\Scripts\activate
```

---

## 3. Install Dependencies

If you are using `uv`:

```bash
uv sync
```

Or install the project normally:

```bash
pip install -e .
```

---

# 🔐 Environment Variables

Create a `.env` file in the project root.

Example:

```env
DATABASE_URL=postgresql+asyncpg://username:password@localhost/blog_db

SECRET_KEY=your-secret-key
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30

# Email configuration
SMTP_HOST=your-smtp-host
SMTP_PORT=587
SMTP_USERNAME=your-email
SMTP_PASSWORD=your-password

# AWS S3 configuration
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
S3_BUCKET_NAME=your-bucket-name
S3_REGION=your-region
```

> **Important:** Never commit your `.env` file, secret keys, database passwords, or AWS credentials to GitHub.

Use the variable names expected by `config.py` in your local setup.

---

# 🗄️ Database Setup

The project uses **SQLAlchemy's asynchronous API** for database operations and **Alembic** for schema migrations.

After configuring your database, run:

```bash
alembic upgrade head
```

This applies the existing database migrations.

---

# ▶️ Running the Application

Start the FastAPI development server with:

```bash
uv run fastapi dev main.py
```

Or with Uvicorn:

```bash
uvicorn main:app --reload
```

The application will be available at:

```text
http://127.0.0.1:8000
```

---

# 📚 API Documentation

FastAPI automatically generates interactive API documentation.

### Swagger UI

```text
http://127.0.0.1:8000/docs
```

### ReDoc

```text
http://127.0.0.1:8000/redoc
```

---

# 🔌 API Endpoints

## 👤 Users

Base URL:

```text
/api/users
```

| Method   | Endpoint                       | Description                 | Auth |
| -------- | ------------------------------ | --------------------------- | ---- |
| `POST`   | `/api/users`                   | Register a user             | ❌    |
| `POST`   | `/api/users/token`             | Login and receive JWT       | ❌    |
| `GET`    | `/api/users/me`                | Get current user            | ✅    |
| `POST`   | `/api/users/forgot-password`   | Request password reset      | ❌    |
| `POST`   | `/api/users/reset-password`    | Reset password              | ❌    |
| `PATCH`  | `/api/users/me/password`       | Change password             | ✅    |
| `GET`    | `/api/users/{user_id}`         | Get public user information | ❌    |
| `GET`    | `/api/users/{user_id}/posts`   | Get user's posts            | ❌    |
| `PATCH`  | `/api/users/{user_id}`         | Update user                 | ✅    |
| `DELETE` | `/api/users/{user_id}`         | Delete user                 | ✅    |
| `PATCH`  | `/api/users/{user_id}/picture` | Upload profile picture      | ✅    |
| `DELETE` | `/api/users/{user_id}/picture` | Delete profile picture      | ✅    |

These endpoints are implemented in the users router, including registration, token generation, password reset, profile management, and profile-picture operations.

---

## 📝 Posts

Base URL:

```text
/api/posts
```

| Method   | Endpoint               | Description             | Auth |
| -------- | ---------------------- | ----------------------- | ---- |
| `GET`    | `/api/posts`           | Get paginated posts     | ❌    |
| `POST`   | `/api/posts`           | Create a post           | ✅    |
| `GET`    | `/api/posts/{post_id}` | Get a single post       | ❌    |
| `PUT`    | `/api/posts/{post_id}` | Fully update a post     | ✅    |
| `PATCH`  | `/api/posts/{post_id}` | Partially update a post | ✅    |
| `DELETE` | `/api/posts/{post_id}` | Delete a post           | ✅    |

The posts API supports pagination using `skip` and `limit`, with ownership checks for modifying or deleting posts.

---

# 🔑 Authentication Flow

The application uses **OAuth2 Password Flow + JWT** for authentication.

### 1. Register

```http
POST /api/users
```

Create a new account with a username, email, and password.

### 2. Login

```http
POST /api/users/token
```

The login endpoint accepts the user's email and password and returns a JWT access token.

Example response:

```json
{
  "access_token": "your-jwt-token",
  "token_type": "bearer"
}
```

### 3. Send the token

For protected endpoints:

```http
Authorization: Bearer <access_token>
```

FastAPI's `OAuth2PasswordBearer` is used to extract the bearer token, while the application verifies the JWT before resolving the current user.

---

# 🧱 Database Models

The main database entities are:

### User

```text
User
├── id
├── username
├── email
├── password_hash
├── image_file
└── posts
```

### Post

```text
Post
├── id
├── title
├── content
├── user_id
├── date_posted
├── likes
└── author
```

### PasswordResetToken

```text
PasswordResetToken
├── id
├── user_id
├── token_hash
└── expires_at
```

A user can have multiple posts, and deleting a user cascades to their associated posts and password-reset tokens.

---

# 📄 Pagination

The API supports pagination for posts.

Example:

```http
GET /api/posts?skip=0&limit=10
```

Response:

```json
{
  "posts": [],
  "total": 25,
  "skip": 0,
  "limit": 10,
  "has_more": true
}
```

This allows clients to retrieve posts in smaller batches instead of loading the entire collection at once.

---

# 🖼️ Profile Image Upload

Profile pictures go through several steps:

```text
Upload Image
     ↓
Validate File Size
     ↓
Process Image
     ↓
Upload to S3
     ↓
Save Filename in Database
     ↓
Delete Previous Image
```

The application uses **Pillow** for image processing and **boto3** for Amazon S3 integration. Invalid images and oversized uploads are rejected.

---

# 🔄 Password Reset Flow

The application supports password recovery.

```text
User enters email
        ↓
Generate reset token
        ↓
Hash token
        ↓
Store token + expiry
        ↓
Send reset email
        ↓
User submits reset token
        ↓
Validate token
        ↓
Hash new password
        ↓
Update password
```

Reset tokens are stored as hashes and are checked for expiration before allowing a password change.

---

# 🧪 Running Tests

The project uses **Pytest**.

Run:

```bash
pytest
```

For more detailed output:

```bash
pytest -v
```

The project also includes Moto as a development dependency for testing AWS/S3-related functionality.

---

# 🏗️ Architecture

The application follows a layered structure:

```text
                    ┌─────────────────────┐
                    │       Client        │
                    │  Browser / API Tool │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │       FastAPI       │
                    │      main.py        │
                    └──────────┬──────────┘
                               │
                 ┌─────────────┴─────────────┐
                 ▼                           ▼
        ┌────────────────┐          ┌────────────────┐
        │ Users Router   │          │ Posts Router   │
        │ /api/users     │          │ /api/posts     │
        └───────┬────────┘          └───────┬────────┘
                │                           │
                └─────────────┬─────────────┘
                              ▼
                     ┌─────────────────┐
                     │   SQLAlchemy    │
                     │  AsyncSession   │
                     └────────┬────────┘
                              │
                              ▼
                     ┌─────────────────┐
                     │     Database    │
                     │ PostgreSQL/SQL  │
                     └─────────────────┘

                    Additional Services
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
        ┌───────────┐               ┌───────────┐
        │   AWS S3  │               │   SMTP    │
        │   Images  │               │   Email   │
        └───────────┘               └───────────┘
```

---

# 🎯 What This Project Demonstrates

This project demonstrates practical backend development concepts including:

* REST API design
* FastAPI routing
* Dependency injection
* Async programming
* SQLAlchemy ORM
* Database relationships
* Database migrations
* Pydantic schemas
* JWT authentication
* OAuth2 password flow
* Password hashing
* Authorization and ownership checks
* Pagination
* File uploads
* Image processing
* AWS S3 integration
* Email-based password recovery
* Jinja2 server-side rendering
* Automated testing

---

# 👨‍💻 Author

**Pallav Sharma**

GitHub: [@pallav2712](https://github.com/pallav2712)

---

## 📌 Project Status

This project is built as a practical backend application to demonstrate **FastAPI and modern Python backend development**, including authentication, database management, external service integration, and testing.

# Python API Development — Comprehensive Course for Beginners
**By Sanjeev Thiyagarajan | freeCodeCamp (19 Hours)**
**Stack: FastAPI · PostgreSQL · SQLAlchemy · Pydantic · JWT · Alembic · Docker · Pytest · GitHub Actions**

> 🔗 Course: [youtube.com/watch?v=0sOvCWFmrtA](https://youtu.be/0sOvCWFmrtA)
> 🏗️ Project: A full social-media-style REST API with posts, users, authentication, votes, testing, deployment & CI/CD.

---

## Table of Contents
1. [Intro & Project Overview](#section-1-intro--project-overview)
2. [Setup & Installation](#section-2-setup--installation)
3. [FastAPI Fundamentals](#section-3-fastapi-fundamentals)
4. [Databases (SQL & PostgreSQL)](#section-4-databases-sql--postgresql)
5. [Python + Raw SQL](#section-5-python--raw-sql)
6. [ORMs (SQLAlchemy)](#section-6-orms-sqlalchemy)
7. [Pydantic Models](#section-7-pydantic-models)
8. [Authentication & Users](#section-8-authentication--users)
9. [Relationships](#section-9-relationships)
10. [Vote / Like System](#section-10-vote--like-system)
11. [Database Migrations with Alembic](#section-11-database-migrations-with-alembic)
12. [Pre-Deployment Checklist](#section-12-pre-deployment-checklist)
13. [Deployment — Heroku](#section-13-deployment--heroku)
14. [Deployment — Ubuntu](#section-14-deployment--ubuntu)
15. [Docker](#section-15-docker)
16. [Testing with Pytest](#section-16-testing-with-pytest)
17. [CI/CD Pipeline with GitHub Actions](#section-17-cicd-pipeline-with-github-actions)

---

# Section 1: Intro & Project Overview

## 📖 Theory

Before writing a single line of code, you need to understand *what* you're building and *why* the stack was chosen this way. An API (Application Programming Interface) is a contract between a server and a client — the server agrees to respond to specific requests in specific formats. REST (Representational State Transfer) is the dominant style for web APIs: stateless, resource-based, and using HTTP methods as verbs (GET = read, POST = create, PUT/PATCH = update, DELETE = delete).

The reason this course matters is that most beginner tutorials show you a "hello world" API. Real production APIs are layered systems: a web framework to handle HTTP, a database to persist data, an ORM to talk to that database safely, an auth system to protect routes, migration tools to evolve the schema, tests to ensure correctness, containers to make it portable, and a CI/CD pipeline to ship it automatically. This course walks through every layer.

---

## What You'll Build
A **social media REST API** with:
- User registration & login (JWT authentication)
- Create, read, update, delete **Posts**
- **Vote/Like** system on posts
- Full SQL database integration (PostgreSQL)
- Deployed to both Heroku and Ubuntu server
- Dockerized application
- Automated test suite
- CI/CD pipeline via GitHub Actions

## Tech Stack Overview

| Layer | Tool | Why |
|-------|------|-----|
| Web Framework | **FastAPI** | Async, auto-docs, Pydantic-native, very fast |
| Database | **PostgreSQL** | Production-grade relational DB |
| ORM | **SQLAlchemy** | Safe DB interaction, avoids raw SQL vulnerabilities |
| Schema Validation | **Pydantic** | Auto-validates request/response data |
| Auth | **JWT + OAuth2** | Stateless, industry standard |
| Migrations | **Alembic** | Track and version database schema changes |
| Testing | **Pytest** | Python's de facto testing framework |
| Containerization | **Docker** | Environment consistency across machines |
| CI/CD | **GitHub Actions** | Automate test → build → deploy |

---

# Section 2: Setup & Installation

## 📖 Theory

Setting up a clean environment is foundational, not optional. The most common beginner mistake is installing packages globally — over time, different projects need different versions of the same library and they start conflicting. Python's **virtual environments** solve this by creating an isolated directory per project that has its own Python interpreter and packages. Every Python project should start with a virtual environment before any `pip install`.

---

## Python Installation
- Download Python 3.9+ from [python.org](https://python.org).
- Verify: `python --version` or `python3 --version`

## VS Code Setup
- Install [VS Code](https://code.visualstudio.com/).
- Install the **Python extension** by Microsoft.
- Set the Python interpreter to the virtual environment (bottom-left selector in VS Code).

## Virtual Environment

```bash
# Create virtual environment (inside your project folder)
python -m venv venv

# Activate — Windows
venv\Scripts\activate

# Activate — Mac/Linux
source venv/bin/activate

# Your terminal prompt should now show (venv)

# Install packages (only affects this venv, not global Python)
pip install fastapi uvicorn

# Deactivate when done
deactivate
```

> ⚠️ Always activate your virtual environment before running or installing anything in a project. If you see packages missing, you probably forgot to activate.

---

# Section 3: FastAPI Fundamentals

## 📖 Theory

FastAPI is built on two things: **Starlette** (the underlying async web framework) and **Pydantic** (the data validation engine). When a request comes in, Starlette handles routing it to the correct function. Pydantic then validates the incoming data against your schema and raises a 422 Unprocessable Entity response automatically if the data is wrong — no manual validation needed. The function runs and returns a Python object, which FastAPI serializes to JSON.

The order of route definitions matters because FastAPI tries to match routes top to bottom. A fixed route like `/posts/latest` must appear before the parameterized route `/posts/{id}`, otherwise `/posts/latest` will be incorrectly captured as `id = "latest"`.

---

## Installing Dependencies

```bash
pip install fastapi uvicorn[standard] psycopg2-binary sqlalchemy pydantic python-jose[cryptography] passlib[bcrypt] python-multipart
```

## Starting FastAPI

```python
# main.py
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def root():
    return {"message": "Hello World"}
```

```bash
# Run the dev server (auto-reload on file save)
uvicorn main:app --reload

# Access auto-generated interactive docs
# Swagger UI: http://127.0.0.1:8000/docs
# ReDoc:       http://127.0.0.1:8000/redoc
```

## Path Operations (Routes)

```python
from fastapi import FastAPI, Response, status, HTTPException

app = FastAPI()

# GET — retrieve data
@app.get("/posts")
def get_posts():
    return {"data": posts_list}

# POST — create new data
@app.post("/posts", status_code=status.HTTP_201_CREATED)
def create_post(post: PostCreate):
    return {"new_post": post}

# GET single item — path parameter
@app.get("/posts/{id}")
def get_post(id: int):   # FastAPI auto-converts string to int, raises 422 if invalid
    return {"post": find_post(id)}

# PUT — full update
@app.put("/posts/{id}")
def update_post(id: int, post: PostCreate):
    return {"updated": post}

# DELETE
@app.delete("/posts/{id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_post(id: int):
    # return nothing on 204
    return Response(status_code=status.HTTP_204_NO_CONTENT)
```

## ⚠️ Path Order Matters
```python
# CORRECT order:
@app.get("/posts/latest")   # specific — must come first
def get_latest(): ...

@app.get("/posts/{id}")     # parameterized — must come after
def get_post(id: int): ...

# If reversed: "latest" gets captured as id="latest" → crash
```

## HTTP Status Codes (Most Common)

| Code | Meaning | Use When |
|------|---------|---------|
| 200 | OK | Successful GET, PUT, PATCH |
| 201 | Created | Successful POST |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Malformed request from client |
| 401 | Unauthorized | No valid auth credentials |
| 403 | Forbidden | Authenticated but no permission |
| 404 | Not Found | Resource doesn't exist |
| 422 | Unprocessable Entity | Schema validation failed |
| 500 | Internal Server Error | Bug on the server |

## Raising HTTP Exceptions

```python
from fastapi import HTTPException, status

@app.get("/posts/{id}")
def get_post(id: int):
    post = find_post(id)
    if not post:
        raise HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=f"Post with id {id} was not found"
        )
    return post
```

## Schema Validation with Pydantic

```python
from pydantic import BaseModel
from typing import Optional

class PostBase(BaseModel):
    title: str
    content: str
    published: bool = True    # default value

class PostCreate(PostBase):
    pass    # inherits title, content, published

class PostUpdate(PostBase):
    title: Optional[str] = None    # all optional for updates
    content: Optional[str] = None
```

- FastAPI auto-reads the request body and validates against the Pydantic model.
- Invalid fields → 422 response with details (no extra code from you).
- Extra fields in the body are silently ignored (safe by default).

## Storing Posts (In-Memory Array)
```python
# Temporary in-memory store (before DB integration)
posts_db = [
    {"id": 1, "title": "First Post", "content": "Content here", "published": True}
]

def find_post(id: int):
    for p in posts_db:
        if p["id"] == id:
            return p
    return None

def find_index(id: int):
    for i, p in enumerate(posts_db):
        if p["id"] == id:
            return i
    return None
```

## CRUD Operations Summary

| Operation | HTTP Method | Route | Status Code |
|-----------|-------------|-------|-------------|
| Read all | GET | `/posts` | 200 |
| Read one | GET | `/posts/{id}` | 200 |
| Create | POST | `/posts` | 201 |
| Full Update | PUT | `/posts/{id}` | 200 |
| Delete | DELETE | `/posts/{id}` | 204 |

## Postman
- GUI tool for testing APIs without a frontend.
- Create a **Collection** to group all your API requests.
- Save requests with sample bodies for quick re-testing.
- Set **environment variables** (e.g., `{{base_url}}` = `http://127.0.0.1:8000`) for easy switching between dev/prod.

## Python Packages Structure
```
app/
├── main.py          # FastAPI app, all routes
├── models.py        # SQLAlchemy DB models
├── schemas.py       # Pydantic schemas (request/response shapes)
├── database.py      # DB connection setup
├── utils.py         # Helper functions (e.g., password hashing)
├── oauth2.py        # JWT auth logic
└── routers/
    ├── posts.py     # Post routes
    ├── users.py     # User routes
    └── auth.py      # Login route
```

---

# Section 4: Databases (SQL & PostgreSQL)

## 📖 Theory

A database persists data beyond a single server session. Without one, all data lives in memory and is lost when the server restarts. **Relational databases** organize data into **tables** (think spreadsheets) with **rows** (records) and **columns** (fields). Tables relate to each other via **foreign keys** — a post table can have a `user_id` column pointing to the users table, establishing ownership.

PostgreSQL is one of the most powerful open-source relational databases. It's the industry standard for production Python backends. Everything you learn here transfers directly to MySQL, SQLite, and other SQL databases — the syntax is 95% the same.

---

## Installing PostgreSQL
- **Windows**: Download from [postgresql.org](https://www.postgresql.org/download/).
- **Mac**: `brew install postgresql`
- Use **pgAdmin** (GUI) for visual exploration, or `psql` (CLI).

## Core SQL Commands

```sql
-- CREATE a table
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,          -- auto-incrementing integer ID
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    published BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT NOW()
);

-- INSERT data
INSERT INTO posts (title, content) VALUES ('My Post', 'Hello World');

-- SELECT data
SELECT * FROM posts;
SELECT title, content FROM posts;

-- WHERE filter
SELECT * FROM posts WHERE published = TRUE;
SELECT * FROM posts WHERE id = 3;

-- SQL Operators
SELECT * FROM posts WHERE id > 2 AND published = TRUE;
SELECT * FROM posts WHERE id = 1 OR id = 2;
SELECT * FROM posts WHERE id != 5;

-- IN keyword (match a list)
SELECT * FROM posts WHERE id IN (1, 2, 3);

-- Pattern matching with LIKE
SELECT * FROM posts WHERE title LIKE '%python%';  -- contains "python"
SELECT * FROM posts WHERE title LIKE 'python%';  -- starts with "python"
SELECT * FROM posts WHERE title LIKE '%python';  -- ends with "python"

-- ORDER results
SELECT * FROM posts ORDER BY created_at DESC;
SELECT * FROM posts ORDER BY title ASC;

-- LIMIT and OFFSET (pagination)
SELECT * FROM posts LIMIT 10 OFFSET 20;  -- page 3 of 10-per-page results

-- UPDATE
UPDATE posts SET title = 'New Title', published = FALSE WHERE id = 1;

-- DELETE
DELETE FROM posts WHERE id = 3;

-- Returning deleted/updated row
DELETE FROM posts WHERE id = 3 RETURNING *;
```

## Schema & Tables
- **Schema**: The structure/blueprint of your database (tables, columns, data types, constraints).
- **Primary Key**: Unique identifier for each row (usually `id`).
- **NOT NULL**: Column cannot be empty.
- **DEFAULT**: Value used when no value is provided.
- **SERIAL**: Auto-incrementing integer (PostgreSQL-specific).

---

# Section 5: Python + Raw SQL

## 📖 Theory

Before using an ORM, it's important to understand what's happening underneath. Raw SQL with Python uses the **psycopg2** library (the PostgreSQL adapter). Your Python code sends SQL strings directly to the database and receives results back. The risk here is **SQL injection** — if you concatenate user input directly into SQL strings, a malicious user can break out of your query and run arbitrary SQL. The fix is always using **parameterized queries** (using `%s` placeholders), never string concatenation.

---

## Connecting to PostgreSQL

```python
# database.py
import psycopg2
from psycopg2.extras import RealDictCursor
import time

while True:
    try:
        conn = psycopg2.connect(
            host='localhost',
            database='fastapi',
            user='postgres',
            password='yourpassword',
            cursor_factory=RealDictCursor   # returns rows as dicts (key: column name)
        )
        cursor = conn.cursor()
        print("Database connection successful")
        break
    except Exception as error:
        print("Connection failed:", error)
        time.sleep(2)   # retry every 2 seconds
```

## Raw SQL CRUD

```python
# GET all posts
@app.get("/posts")
def get_posts():
    cursor.execute("SELECT * FROM posts")
    posts = cursor.fetchall()
    return {"data": posts}

# CREATE post — use %s placeholders (NEVER concatenate user input)
@app.post("/posts", status_code=status.HTTP_201_CREATED)
def create_post(post: PostCreate):
    cursor.execute(
        """INSERT INTO posts (title, content, published)
           VALUES (%s, %s, %s) RETURNING *""",
        (post.title, post.content, post.published)
    )
    new_post = cursor.fetchone()
    conn.commit()   # must commit to persist changes
    return {"data": new_post}

# GET one post
@app.get("/posts/{id}")
def get_post(id: int):
    cursor.execute("SELECT * FROM posts WHERE id = %s", (str(id),))
    post = cursor.fetchone()
    if not post:
        raise HTTPException(status_code=404, detail=f"Post {id} not found")
    return {"data": post}

# DELETE post
@app.delete("/posts/{id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_post(id: int):
    cursor.execute("DELETE FROM posts WHERE id = %s RETURNING *", (str(id),))
    deleted = cursor.fetchone()
    conn.commit()
    if not deleted:
        raise HTTPException(status_code=404, detail=f"Post {id} not found")
    return Response(status_code=status.HTTP_204_NO_CONTENT)

# UPDATE post
@app.put("/posts/{id}")
def update_post(id: int, post: PostCreate):
    cursor.execute(
        """UPDATE posts SET title = %s, content = %s, published = %s
           WHERE id = %s RETURNING *""",
        (post.title, post.content, post.published, str(id))
    )
    updated = cursor.fetchone()
    conn.commit()
    if not updated:
        raise HTTPException(status_code=404, detail=f"Post {id} not found")
    return {"data": updated}
```

> ⚠️ Always call `conn.commit()` after INSERT/UPDATE/DELETE. Without it, changes are in a transaction and never actually written to the database.

---

# Section 6: ORMs (SQLAlchemy)

## 📖 Theory

An ORM (Object-Relational Mapper) lets you interact with your database using Python objects instead of raw SQL strings. You define a Python class for each table, and SQLAlchemy translates your Python method calls into the correct SQL behind the scenes. This has several advantages: you get Python syntax (not SQL strings), you're protected from SQL injection by default, switching databases requires minimal code changes, and your models serve as documentation for your schema.

The trade-off: ORMs add a layer of abstraction that can obscure what SQL is actually being run. For complex queries, sometimes raw SQL (or SQLAlchemy's Core expression language) is clearer and faster. In this course, SQLAlchemy's ORM is used for standard CRUD, which is the right call for most operations.

---

## SQLAlchemy Setup

```python
# database.py
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

SQLALCHEMY_DATABASE_URL = "postgresql://postgres:password@localhost/fastapi"

engine = create_engine(SQLALCHEMY_DATABASE_URL)

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()

# Dependency — injects a DB session into route functions
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

## Defining Models

```python
# models.py
from sqlalchemy import Column, Integer, String, Boolean, TIMESTAMP
from sqlalchemy.sql.expression import text
from .database import Base

class Post(Base):
    __tablename__ = "posts"

    id = Column(Integer, primary_key=True, nullable=False)
    title = Column(String, nullable=False)
    content = Column(String, nullable=False)
    published = Column(Boolean, server_default='TRUE', nullable=False)
    created_at = Column(TIMESTAMP(timezone=True),
                        nullable=False,
                        server_default=text('now()'))
```

```python
# In main.py — create tables from models (dev only; use Alembic in prod)
from . import models
from .database import engine

models.Base.metadata.create_all(bind=engine)
```

## SQLAlchemy CRUD Operations

```python
from fastapi import Depends
from sqlalchemy.orm import Session
from . import models, schemas
from .database import get_db

# GET all posts
@app.get("/posts")
def get_posts(db: Session = Depends(get_db)):
    posts = db.query(models.Post).all()
    return posts

# CREATE post
@app.post("/posts", status_code=status.HTTP_201_CREATED)
def create_post(post: schemas.PostCreate, db: Session = Depends(get_db)):
    new_post = models.Post(**post.dict())   # unpack Pydantic model to ORM model
    db.add(new_post)
    db.commit()
    db.refresh(new_post)   # reload the row (to get DB-generated fields like id)
    return new_post

# GET one post
@app.get("/posts/{id}")
def get_post(id: int, db: Session = Depends(get_db)):
    post = db.query(models.Post).filter(models.Post.id == id).first()
    if not post:
        raise HTTPException(status_code=404, detail=f"Post {id} not found")
    return post

# DELETE post
@app.delete("/posts/{id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_post(id: int, db: Session = Depends(get_db)):
    post_query = db.query(models.Post).filter(models.Post.id == id)
    if post_query.first() is None:
        raise HTTPException(status_code=404, detail=f"Post {id} not found")
    post_query.delete(synchronize_session=False)
    db.commit()
    return Response(status_code=status.HTTP_204_NO_CONTENT)

# UPDATE post
@app.put("/posts/{id}")
def update_post(id: int, post: schemas.PostCreate, db: Session = Depends(get_db)):
    post_query = db.query(models.Post).filter(models.Post.id == id)
    if post_query.first() is None:
        raise HTTPException(status_code=404, detail=f"Post {id} not found")
    post_query.update(post.dict(), synchronize_session=False)
    db.commit()
    return post_query.first()
```

---

# Section 7: Pydantic Models

## 📖 Theory

Pydantic models serve a completely different purpose than SQLAlchemy models, and confusing the two is the most common beginner mistake in FastAPI. **SQLAlchemy models** define the shape of your database tables. **Pydantic models** define the shape of data flowing in and out of your API — what the client sends and what the server returns. They are deliberately kept separate because what you store in the DB and what you expose via API are often different (e.g., you store hashed passwords, but you never return them in responses).

---

## Pydantic vs ORM Models

| | Pydantic (schemas.py) | SQLAlchemy (models.py) |
|---|---|---|
| Purpose | Request/Response validation | Database table definition |
| Used for | Parsing input, shaping output | Querying, writing to DB |
| Lives in | `schemas.py` | `models.py` |
| Base class | `pydantic.BaseModel` | `sqlalchemy.Base` |

## Pydantic Schema Design

```python
# schemas.py
from pydantic import BaseModel
from datetime import datetime
from typing import Optional

# --- Post Schemas ---
class PostBase(BaseModel):
    title: str
    content: str
    published: bool = True

class PostCreate(PostBase):
    pass   # same fields for creation

class PostResponse(PostBase):
    id: int
    created_at: datetime
    owner_id: int

    class Config:
        orm_mode = True  # allows reading from SQLAlchemy ORM objects (not just dicts)
        # In Pydantic v2: model_config = ConfigDict(from_attributes=True)

# --- User Schemas ---
class UserCreate(BaseModel):
    email: str
    password: str

class UserResponse(BaseModel):
    id: int
    email: str
    created_at: datetime

    class Config:
        orm_mode = True
```

## Response Model

```python
# Tell FastAPI exactly what to return — filters out sensitive fields
@app.get("/posts/{id}", response_model=schemas.PostResponse)
def get_post(id: int, db: Session = Depends(get_db)):
    post = db.query(models.Post).filter(models.Post.id == id).first()
    return post   # FastAPI uses PostResponse schema to serialize this
```

- `response_model` is critical for security — it prevents accidentally leaking fields like `password` or internal IDs.
- `orm_mode = True` tells Pydantic to read attributes from ORM objects (not just dicts).

---

# Section 8: Authentication & Users

## 📖 Theory

Authentication answers the question: "who are you?" The standard approach in modern REST APIs is **JWT (JSON Web Token)**. When a user logs in with correct credentials, the server creates a signed token containing their identity (e.g., user ID). The client stores this token and sends it in the `Authorization` header with every subsequent request. The server verifies the signature — if valid, it trusts the identity inside the token without checking the database again. This is **stateless** — the server doesn't need to store sessions anywhere.

Passwords must **never** be stored as plain text. A hash function (like bcrypt) converts a password into a fixed-length string that cannot be reversed. When a user logs in, you hash their input and compare it to the stored hash — you never see the original password.

---

## Creating the Users Table

```python
# models.py
class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, nullable=False)
    email = Column(String, nullable=False, unique=True)
    password = Column(String, nullable=False)
    created_at = Column(TIMESTAMP(timezone=True),
                        nullable=False,
                        server_default=text('now()'))
```

## Password Hashing

```python
# utils.py
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def hash_password(password: str) -> str:
    return pwd_context.hash(password)

def verify_password(plain_password: str, hashed_password: str) -> bool:
    return pwd_context.verify(plain_password, hashed_password)
```

## User Registration Route

```python
# routers/users.py
@router.post("/users", status_code=201, response_model=schemas.UserResponse)
def create_user(user: schemas.UserCreate, db: Session = Depends(get_db)):
    # Hash password before storing
    user.password = utils.hash_password(user.password)
    new_user = models.User(**user.dict())
    db.add(new_user)
    db.commit()
    db.refresh(new_user)
    return new_user
```

## JWT Token Flow

```
Client sends: POST /login  {email, password}
            ↓
Server: verify credentials against DB
            ↓
Server: create JWT token  {sub: user_id, exp: expiry}
            ↓
Server returns: {access_token: "eyJ...", token_type: "bearer"}
            ↓
Client stores the token
            ↓
Client sends: GET /posts  Authorization: Bearer eyJ...
            ↓
Server: decode & verify token → extract user_id → process request
```

## JWT Implementation

```python
# oauth2.py
from jose import JWTError, jwt
from datetime import datetime, timedelta
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer

SECRET_KEY = "your-secret-key-here"   # use env variable in production
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="login")

def create_access_token(data: dict):
    to_encode = data.copy()
    expire = datetime.utcnow() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

def verify_access_token(token: str, credentials_exception):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        user_id: str = payload.get("sub")
        if user_id is None:
            raise credentials_exception
        return schemas.TokenData(id=user_id)
    except JWTError:
        raise credentials_exception

def get_current_user(token: str = Depends(oauth2_scheme),
                     db: Session = Depends(get_db)):
    credentials_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Could not validate credentials",
        headers={"WWW-Authenticate": "Bearer"},
    )
    token_data = verify_access_token(token, credentials_exception)
    user = db.query(models.User).filter(models.User.id == token_data.id).first()
    return user
```

## Login Route

```python
# routers/auth.py
from fastapi.security import OAuth2PasswordRequestForm

@router.post("/login")
def login(user_credentials: OAuth2PasswordRequestForm = Depends(),
          db: Session = Depends(get_db)):
    # OAuth2PasswordRequestForm gives .username and .password
    user = db.query(models.User).filter(
        models.User.email == user_credentials.username
    ).first()

    if not user or not utils.verify_password(user_credentials.password, user.password):
        raise HTTPException(status_code=403, detail="Invalid credentials")

    access_token = oauth2.create_access_token(data={"sub": str(user.id)})
    return {"access_token": access_token, "token_type": "bearer"}
```

## Protecting Routes

```python
# Any route that requires authentication:
@router.post("/posts", status_code=201)
def create_post(
    post: schemas.PostCreate,
    db: Session = Depends(get_db),
    current_user: models.User = Depends(oauth2.get_current_user)   # ← protects route
):
    new_post = models.Post(owner_id=current_user.id, **post.dict())
    db.add(new_post)
    db.commit()
    db.refresh(new_post)
    return new_post
```

## FastAPI Routers (Splitting Routes)

```python
# routers/posts.py
from fastapi import APIRouter

router = APIRouter(
    prefix="/posts",          # all routes here start with /posts
    tags=["Posts"]            # groups routes in Swagger docs
)

@router.get("/")              # full path: /posts/
@router.post("/")             # full path: /posts/
@router.get("/{id}")          # full path: /posts/{id}

# main.py
from .routers import posts, users, auth
app.include_router(posts.router)
app.include_router(users.router)
app.include_router(auth.router)
```

---

# Section 9: Relationships

## 📖 Theory

Relational databases are powerful because tables can *relate* to each other. A **foreign key** is a column in one table that references the primary key of another table. For example, a `posts` table has an `owner_id` column that references `users.id`. This enforces data integrity — you cannot create a post for a user that doesn't exist.

SQL **JOINs** combine rows from multiple tables based on a related column. This is the bread and butter of relational databases. SQLAlchemy's `relationship()` function mirrors this at the ORM level — it lets you access related objects via Python attributes (e.g., `post.owner` to get the User who wrote a post) without writing any JOIN SQL yourself.

---

## Foreign Keys in PostgreSQL

```sql
ALTER TABLE posts
ADD COLUMN owner_id INT REFERENCES users(id) ON DELETE CASCADE;
-- ON DELETE CASCADE: if the user is deleted, their posts are also deleted
```

## SQLAlchemy Foreign Keys & Relationships

```python
# models.py
from sqlalchemy import ForeignKey
from sqlalchemy.orm import relationship

class Post(Base):
    __tablename__ = "posts"
    # ... existing columns ...
    owner_id = Column(Integer, ForeignKey("users.id", ondelete="CASCADE"),
                      nullable=False)

    # This creates a Python attribute to access the related User object
    owner = relationship("User")   # returns a User object when accessed

class User(Base):
    __tablename__ = "users"
    # ... existing columns ...
```

## Using Relationships in Routes

```python
# Accessing the owner of a post (SQLAlchemy auto-JOINs)
post = db.query(models.Post).filter(models.Post.id == id).first()
print(post.owner.email)   # fetches related User automatically
```

## Ownership Enforcement

```python
@router.delete("/{id}", status_code=204)
def delete_post(id: int, db: Session = Depends(get_db),
                current_user = Depends(oauth2.get_current_user)):
    post_query = db.query(models.Post).filter(models.Post.id == id)
    post = post_query.first()

    if post is None:
        raise HTTPException(status_code=404, detail="Not found")

    if post.owner_id != current_user.id:
        raise HTTPException(status_code=403,
                           detail="Not authorized to perform this action")

    post_query.delete(synchronize_session=False)
    db.commit()
```

## Query Parameters

```python
@router.get("/")
def get_posts(
    db: Session = Depends(get_db),
    limit: int = 10,         # /posts?limit=5
    skip: int = 0,           # /posts?skip=20
    search: Optional[str] = ""  # /posts?search=python
):
    posts = db.query(models.Post)\
               .filter(models.Post.title.contains(search))\
               .limit(limit)\
               .offset(skip)\
               .all()
    return posts
```

## Environment Variables

```python
# .env file (never commit to git)
DATABASE_HOSTNAME=localhost
DATABASE_PORT=5432
DATABASE_NAME=fastapi
DATABASE_USERNAME=postgres
DATABASE_PASSWORD=yourpassword
SECRET_KEY=your-super-secret-key
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30

# config.py — Pydantic validates env vars too
from pydantic import BaseSettings

class Settings(BaseSettings):
    database_hostname: str
    database_port: str
    database_name: str
    database_username: str
    database_password: str
    secret_key: str
    algorithm: str
    access_token_expire_minutes: int

    class Config:
        env_file = ".env"

settings = Settings()

# Usage
from .config import settings
DATABASE_URL = f"postgresql://{settings.database_username}:{settings.database_password}@{settings.database_hostname}/{settings.database_name}"
```

---

# Section 10: Vote / Like System

## 📖 Theory

A like/vote system looks simple on the surface — a user clicks like on a post. But the data model requires thought. You can't add a "likes" column to the posts table (that would only store a count, not *who* liked it, so you couldn't prevent double-voting). The correct design is a separate **votes table** with a **composite primary key** on `(post_id, user_id)`. This means each combination of post + user can only exist once — the database itself enforces the "one vote per user per post" rule.

---

## Votes Table Schema

```python
# models.py
class Vote(Base):
    __tablename__ = "votes"

    post_id = Column(Integer, ForeignKey("posts.id", ondelete="CASCADE"),
                     primary_key=True)
    user_id = Column(Integer, ForeignKey("users.id", ondelete="CASCADE"),
                     primary_key=True)
    # Composite primary key: (post_id, user_id) pair must be unique
```

## Vote Route

```python
# schemas.py
class Vote(BaseModel):
    post_id: int
    dir: int   # 1 = upvote, 0 = remove vote

# routers/votes.py
@router.post("/vote")
def vote(vote: schemas.Vote, db: Session = Depends(get_db),
         current_user = Depends(oauth2.get_current_user)):
    post = db.query(models.Post).filter(models.Post.id == vote.post_id).first()
    if not post:
        raise HTTPException(status_code=404, detail="Post not found")

    vote_query = db.query(models.Vote).filter(
        models.Vote.post_id == vote.post_id,
        models.Vote.user_id == current_user.id
    )
    found_vote = vote_query.first()

    if vote.dir == 1:   # upvote
        if found_vote:
            raise HTTPException(status_code=409, detail="Already voted")
        new_vote = models.Vote(post_id=vote.post_id, user_id=current_user.id)
        db.add(new_vote)
        db.commit()
        return {"message": "Successfully voted"}
    else:               # remove vote
        if not found_vote:
            raise HTTPException(status_code=404, detail="Vote does not exist")
        vote_query.delete(synchronize_session=False)
        db.commit()
        return {"message": "Vote removed"}
```

## SQL JOINs

```sql
-- LEFT JOIN: all posts + vote count (including posts with 0 votes)
SELECT posts.*, COUNT(votes.post_id) AS votes
FROM posts
LEFT JOIN votes ON posts.id = votes.post_id
GROUP BY posts.id;

-- INNER JOIN: only posts that have at least one vote
SELECT posts.*, COUNT(votes.post_id) AS votes
FROM posts
INNER JOIN votes ON posts.id = votes.post_id
GROUP BY posts.id;
```

## SQLAlchemy Joins

```python
from sqlalchemy import func

# Get all posts with their vote count
posts = db.query(models.Post, func.count(models.Vote.post_id).label("votes"))\
           .join(models.Vote, models.Vote.post_id == models.Post.id,
                 isouter=True)\   # isouter=True = LEFT JOIN
           .group_by(models.Post.id)\
           .all()
```

---

# Section 11: Database Migrations with Alembic

## 📖 Theory

`models.Base.metadata.create_all()` is fine for development, but it has a critical flaw in production: it only creates tables that don't exist yet — it **never alters** existing tables. So if you add a column to a model, `create_all` won't add that column to your production database. Alembic solves this by tracking every schema change as a versioned "revision" file. You can apply migrations forward (upgrade) or backward (rollback/downgrade) at any time — it's Git for your database schema.

---

## Alembic Setup

```bash
pip install alembic
alembic init alembic   # creates alembic/ directory and alembic.ini
```

```python
# alembic/env.py — edit these two lines:
from app.models import Base          # import your models
target_metadata = Base.metadata      # point to your models' metadata
```

```ini
# alembic.ini — set your database URL
sqlalchemy.url = postgresql://postgres:password@localhost/fastapi
```

## Creating & Running Migrations

```bash
# Create a new revision (migration file)
alembic revision --autogenerate -m "create posts table"
# --autogenerate compares your models to the current DB and generates the diff

# Apply all pending migrations
alembic upgrade head

# Roll back the most recent migration
alembic downgrade -1

# Roll back to a specific revision
alembic downgrade abc123

# View migration history
alembic history
alembic current   # shows current revision applied to DB
```

## Migration File Structure

```python
# alembic/versions/abc123_create_posts_table.py
def upgrade():
    op.create_table('posts',
        sa.Column('id', sa.Integer(), nullable=False),
        sa.Column('title', sa.String(), nullable=False),
        sa.PrimaryKeyConstraint('id')
    )

def downgrade():
    op.drop_table('posts')
```

> ✅ In production, **always use Alembic** for schema changes. Remove `create_all()` from `main.py` once Alembic is set up.

---

# Section 12: Pre-Deployment Checklist

## 📖 Theory

CORS (Cross-Origin Resource Sharing) is a browser security mechanism that blocks web pages from making requests to a different domain than the one that served the page. By default, if your frontend is at `http://myapp.com` and your API is at `http://api.myapp.com`, the browser will block the API calls. You must explicitly tell your API which origins are allowed to access it. In FastAPI, this is handled by middleware.

---

## CORS Setup

```python
from fastapi.middleware.cors import CORSMiddleware

# List of allowed frontend origins
origins = [
    "https://www.google.com",      # example — add your real frontend URL
    "http://localhost:3000",        # React dev server
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,         # use ["*"] only for fully public APIs
    allow_credentials=True,
    allow_methods=["*"],           # GET, POST, PUT, DELETE, etc.
    allow_headers=["*"],
)
```

## Git & GitHub Prep
- Initialize Git repo: `git init`
- Create `.gitignore` — exclude `venv/`, `.env`, `__pycache__/`, `*.pyc`
- Push to GitHub (private repo recommended for production code with secrets)
- Create `requirements.txt`:
```bash
pip freeze > requirements.txt
```

---

# Section 13: Deployment — Heroku

## 📖 Theory

Heroku is a **Platform as a Service (PaaS)** — you push code, Heroku handles the server, OS, and infrastructure. It's beginner-friendly but has less control than managing your own server. The `Procfile` tells Heroku what command to run to start your app.

---

## Steps

```bash
# 1. Install Heroku CLI, login
heroku login

# 2. Create Heroku app
heroku create your-app-name

# 3. Procfile (in project root — no extension)
web: uvicorn app.main:app --host 0.0.0.0 --port $PORT

# 4. Add Postgres addon
heroku addons:create heroku-postgresql:hobby-dev

# 5. Set environment variables on Heroku
heroku config:set SECRET_KEY=yourkey DATABASE_HOSTNAME=... etc

# 6. Run Alembic migrations on Heroku Postgres
heroku run "alembic upgrade head"

# 7. Deploy
git push heroku main

# 8. View logs
heroku logs --tail
```

---

# Section 14: Deployment — Ubuntu (VPS)

## 📖 Theory

Deploying to a **VPS (Virtual Private Server)** like a DigitalOcean Droplet or AWS EC2 instance gives full control over the server. You manage the OS, packages, and processes yourself. **Gunicorn** is a production-grade WSGI/ASGI server — it manages multiple worker processes for handling concurrent requests. **Nginx** sits in front as a reverse proxy — it handles SSL, static files, and forwards API requests to Gunicorn.

---

## Steps Overview

```bash
# 1. Create Ubuntu VM (DigitalOcean / AWS / Azure)
# 2. SSH into the server
ssh root@your-server-ip

# 3. Update packages
sudo apt update && sudo apt upgrade -y

# 4. Install Python, pip, venv
sudo apt install python3 python3-pip python3-venv -y

# 5. Install Postgres
sudo apt install postgresql postgresql-contrib -y
sudo systemctl start postgresql

# 6. Create DB user and database
sudo -u postgres psql
CREATE USER fastapi WITH PASSWORD 'password';
CREATE DATABASE fastapi OWNER fastapi;

# 7. Create app user (don't run app as root)
sudo adduser deploy
sudo su - deploy

# 8. Clone repo and set up venv
git clone https://github.com/yourrepo.git
cd yourrepo
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt

# 9. Set environment variables
# Edit /etc/environment or use .env file

# 10. Run Alembic migrations
alembic upgrade head

# 11. Install Gunicorn
pip install gunicorn

# 12. Create systemd service (auto-start on boot)
# /etc/systemd/system/fastapi.service
[Unit]
Description=FastAPI Application
After=network.target

[Service]
User=deploy
WorkingDirectory=/home/deploy/yourrepo
Environment="PATH=/home/deploy/yourrepo/venv/bin"
ExecStart=/home/deploy/yourrepo/venv/bin/gunicorn -w 4 -k uvicorn.workers.UvicornWorker app.main:app

[Install]
WantedBy=multi-user.target

sudo systemctl start fastapi
sudo systemctl enable fastapi   # start on boot

# 13. Install Nginx
sudo apt install nginx -y

# 14. Configure Nginx as reverse proxy
# /etc/nginx/sites-available/fastapi
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

sudo ln -s /etc/nginx/sites-available/fastapi /etc/nginx/sites-enabled/
sudo systemctl restart nginx

# 15. Set up SSL with Certbot
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d yourdomain.com

# 16. Set up Firewall
sudo ufw allow OpenSSH
sudo ufw allow 'Nginx Full'
sudo ufw enable
```

---

# Section 15: Docker

## 📖 Theory

Docker solves the "works on my machine" problem. A **Docker image** is a complete, self-contained snapshot of your application and all its dependencies (Python version, pip packages, OS libraries). A **container** is a running instance of that image. Because the image includes everything needed, your app runs identically on your laptop, a teammate's Mac, and a production Ubuntu server. **Docker Compose** extends this to run multiple containers together (your API + your database) with a single command.

---

## Dockerfile

```dockerfile
# Use official Python image as base
FROM python:3.9

# Set working directory inside container
WORKDIR /usr/src/app

# Copy and install dependencies first (Docker layer caching optimization)
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the app code
COPY . .

# Run the app
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

## Docker Compose

```yaml
# docker-compose.yml
version: "3"

services:
  api:
    build: .
    ports:
      - "8000:8000"
    volumes:
      - .:/usr/src/app    # bind mount: local changes reflect in container
    env_file:
      - ./.env
    depends_on:
      - postgres
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

  postgres:
    image: postgres
    env_file:
      - ./.env
    volumes:
      - postgres-data:/var/lib/postgresql/data   # named volume: persists data

volumes:
  postgres-data:
```

```bash
# Build and start all containers
docker-compose up -d --build

# Stop all containers
docker-compose down

# View logs
docker-compose logs -f

# Exec into running container
docker-compose exec api bash

# Run Alembic inside the container
docker-compose exec api alembic upgrade head
```

## Production vs Development
- **Dev**: Use bind mounts (`volumes: .:/app`) + `--reload` flag for hot reload.
- **Prod**: No bind mounts (image contains all code), no `--reload`, use `gunicorn` with multiple workers.

---

# Section 16: Testing with Pytest

## 📖 Theory

Testing is how you verify your code is correct today and stays correct as you change it tomorrow. **Unit tests** test individual functions in isolation. **Integration tests** test how components work together — in this context, making real HTTP requests to your FastAPI app against a real (test) database. The key principle: **tests should be isolated and repeatable**. Each test should start with a clean state, run, assert, and clean up — so tests never depend on each other's side effects. Pytest **fixtures** are the mechanism that handles this setup/teardown lifecycle cleanly.

---

## Installation

```bash
pip install pytest httpx pytest-asyncio
```

## Your First Test

```python
# tests/test_calculations.py
def add(a, b):
    return a + b

def test_add():
    result = add(2, 3)
    assert result == 5

def test_add_negative():
    assert add(-1, 1) == 0
```

```bash
pytest                     # run all tests
pytest -v                  # verbose (show each test name)
pytest -s                  # show print/stdout output
pytest -v -s               # combine both
pytest tests/test_users.py # run specific file
```

## Parametrize (DRY Testing)

```python
import pytest

@pytest.mark.parametrize("a, b, expected", [
    (2, 3, 5),
    (-1, 1, 0),
    (0, 0, 0),
    (100, -50, 50),
])
def test_add(a, b, expected):
    assert add(a, b) == expected
# Runs 4 tests from one function
```

## Fixtures

```python
import pytest

@pytest.fixture
def client(session):
    # Runs before each test that uses this fixture
    def override_get_db():
        yield session

    app.dependency_overrides[get_db] = override_get_db
    yield TestClient(app)
    # Runs after each test (teardown)

@pytest.fixture
def session():
    # Create a fresh test database for each test
    Base.metadata.create_all(bind=engine)
    db = TestingSessionLocal()
    try:
        yield db
    finally:
        db.close()
        Base.metadata.drop_all(bind=engine)
```

## Test Database Setup

```python
# tests/database.py
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from app.database import Base

SQLALCHEMY_TEST_DATABASE_URL = "postgresql://postgres:password@localhost/fastapi_test"

engine = create_engine(SQLALCHEMY_TEST_DATABASE_URL)
TestingSessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
```

## FastAPI TestClient

```python
from fastapi.testclient import TestClient
from app.main import app

def test_root(client):
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"message": "Hello World"}
```

## Integration Tests — Users & Posts

```python
def test_create_user(client):
    response = client.post("/users", json={
        "email": "test@example.com",
        "password": "password123"
    })
    assert response.status_code == 201
    data = response.json()
    assert data["email"] == "test@example.com"
    assert "id" in data
    assert "password" not in data  # ensure password not returned

def test_login(client, test_user):
    response = client.post("/login", data={
        "username": test_user["email"],
        "password": test_user["password"]
    })
    assert response.status_code == 200
    assert "access_token" in response.json()

def test_get_all_posts_unauthorized(client):
    response = client.get("/posts")
    assert response.status_code == 401

def test_create_post(authorized_client, test_user):
    response = authorized_client.post("/posts", json={
        "title": "Test Post",
        "content": "Test Content"
    })
    assert response.status_code == 201
    data = response.json()
    assert data["title"] == "Test Post"
    assert data["owner_id"] == test_user["id"]
```

## Conftest.py
```python
# tests/conftest.py — shared fixtures available to ALL test files
# Place test_user, authorized_client, session, client fixtures here
# Pytest automatically discovers conftest.py
```

---

# Section 17: CI/CD Pipeline with GitHub Actions

## 📖 Theory

CI/CD (Continuous Integration / Continuous Deployment) automates the journey from "push code to GitHub" to "code running in production." **Continuous Integration** means every push automatically runs your test suite — you catch bugs before they merge into the main branch. **Continuous Deployment** means if tests pass, the code is automatically deployed. This is how professional engineering teams ship: you push code, the pipeline handles the rest. GitHub Actions is GitHub's built-in CI/CD platform — it runs your pipeline in containers defined by YAML files in `.github/workflows/`.

---

## GitHub Actions Workflow

```yaml
# .github/workflows/build-deploy.yml
name: Build and Deploy

on:
  push:
    branches: [main]    # run on every push to main
  pull_request:
    branches: [main]    # run on every PR targeting main

jobs:
  build:
    runs-on: ubuntu-latest   # GitHub provides a fresh Ubuntu VM

    # Postgres service container for integration tests
    services:
      postgres:
        image: postgres
        env:
          POSTGRES_PASSWORD: ${{ secrets.DATABASE_PASSWORD }}
          POSTGRES_DB: fastapi_test
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      # 1. Checkout code
      - name: Checkout code
        uses: actions/checkout@v2

      # 2. Set up Python
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: "3.9"

      # 3. Install dependencies
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      # 4. Set environment variables from GitHub Secrets
      - name: Set environment variables
        run: |
          echo "DATABASE_HOSTNAME=${{ secrets.DATABASE_HOSTNAME }}" >> $GITHUB_ENV
          echo "DATABASE_PASSWORD=${{ secrets.DATABASE_PASSWORD }}" >> $GITHUB_ENV
          echo "SECRET_KEY=${{ secrets.SECRET_KEY }}" >> $GITHUB_ENV
          # ... other secrets

      # 5. Run tests
      - name: Run Tests
        run: pytest -v

      # 6. (Optional) Build Docker image
      - name: Build Docker image
        run: docker build -t myapp .

      # 7. Deploy to Heroku (if tests passed)
      - name: Deploy to Heroku
        if: github.ref == 'refs/heads/main'
        uses: akhileshns/heroku-deploy@v3.12.12
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_app_name: "your-heroku-app"
          heroku_email: "you@example.com"
```

## GitHub Secrets
- Store all sensitive values (DB password, API keys, SECRET_KEY) in:
  **GitHub Repo → Settings → Secrets and Variables → Actions → New repository secret**
- Reference in workflow: `${{ secrets.YOUR_SECRET_NAME }}`
- Never hardcode secrets in `.yml` files.

---

# Quick Reference — Key Concepts

## REST API Design

| Endpoint | Method | Action | Auth Required |
|----------|--------|--------|---------------|
| `/posts` | GET | Get all posts | Optional |
| `/posts` | POST | Create post | ✅ Yes |
| `/posts/{id}` | GET | Get one post | Optional |
| `/posts/{id}` | PUT | Update post (owner only) | ✅ Yes |
| `/posts/{id}` | DELETE | Delete post (owner only) | ✅ Yes |
| `/users` | POST | Register | ❌ No |
| `/users/{id}` | GET | Get user | ✅ Yes |
| `/login` | POST | Login, get token | ❌ No |
| `/vote` | POST | Vote on post | ✅ Yes |

## Full Request Lifecycle

```
Client Request
    ↓
FastAPI Router (matches path + method)
    ↓
Middleware (CORS, auth headers)
    ↓
Dependencies (get_db → DB session, get_current_user → validates JWT)
    ↓
Pydantic Schema Validation (request body)
    ↓
Route Function (your logic)
    ↓
SQLAlchemy (queries DB)
    ↓
PostgreSQL
    ↓
Response serialized via Pydantic response_model
    ↓
Client receives JSON
```

## Key Takeaways

1. **Pydantic ≠ SQLAlchemy models.** Keep them separate. One defines API shape, the other defines DB shape.
2. **Never store plain text passwords.** Always bcrypt hash before writing to DB.
3. **JWT is stateless.** The server never stores sessions — the token *is* the proof of identity.
4. **Alembic > `create_all()` for production.** Schema evolution requires migration tracking.
5. **Environment variables > hardcoded credentials.** Use `.env` locally, GitHub Secrets in CI/CD.
6. **Test against a separate database.** Never run tests against your production or development DB.
7. **Docker makes your app portable.** Container = code + dependencies + runtime, all in one.
8. **CI/CD is not optional at professional level.** Automating test → deploy is standard practice.

---

*Notes compiled from: Python API Development - Comprehensive Course for Beginners by Sanjeev Thiyagarajan | freeCodeCamp*
# Python API Development — Comprehensive Course
### By Sanjeev Thiyagarajan | freeCodeCamp.org
---

## Table of Contents
1. [PART 1 — Intro to APIs & HTTP](#part-1--intro-to-apis--http)
2. [PART 2 — FastAPI Basics](#part-2--fastapi-basics)
3. [PART 3 — CRUD & HTTP Methods](#part-3--crud--http-methods)
4. [PART 4 — Pydantic & Schema Validation](#part-4--pydantic--schema-validation)
5. [PART 5 — PostgreSQL & Raw SQL](#part-5--postgresql--raw-sql)
6. [PART 6 — SQLAlchemy ORM](#part-6--sqlalchemy-orm)
7. [PART 7 — Response Schemas & Pydantic Models](#part-7--response-schemas--pydantic-models)
8. [PART 8 — Authentication & Passwords](#part-8--authentication--passwords)
9. [PART 9 — JWT Tokens](#part-9--jwt-tokens)
10. [PART 10 — Routers & Code Organization](#part-10--routers--code-organization)
11. [PART 11 — Database Relationships](#part-11--database-relationships)
12. [PART 12 — Votes & SQL Joins](#part-12--votes--sql-joins)
13. [PART 13 — Environment Variables](#part-13--environment-variables)
14. [PART 14 — CORS](#part-14--cors)
15. [PART 15 — Git Basics](#part-15--git-basics)
16. [PART 16 — Deployment on Linux Server](#part-16--deployment-on-linux-server)
17. [PART 17 — Nginx & Process Management](#part-17--nginx--process-management)
18. [PART 18 — Docker](#part-18--docker)
19. [PART 19 — CI/CD with GitHub Actions](#part-19--cicd-with-github-actions)

---

# PART 1 — Intro to APIs & HTTP

---

## What is an API?

API stands for **Application Programming Interface**.  
It is a way for two programs to communicate with each other.

- Think of an API as a waiter in a restaurant:
  - You (the client) give the waiter (API) your order
  - The waiter goes to the kitchen (server/database)
  - The waiter brings back your food (response)
  - You never directly touch the kitchen
- APIs define the rules for HOW two programs talk to each other
- A **Web API** specifically communicates over the internet using HTTP
- Examples in real life:
  - Weather app fetching data from a weather service API
  - Paying via Razorpay — your app talks to Razorpay's API
  - Instagram showing posts — your phone talks to Instagram's API

---

## What is a REST API?

REST = **Representational State Transfer**. It is the most common style of web API.

**Rules of REST (constraints):**
- **Client-Server** → the frontend and backend are separate, they only talk via API
- **Stateless** → every request from the client must contain ALL the info needed. The server never remembers previous requests
- **Uniform Interface** → consistent structure for all endpoints
- **Resources** → everything the API exposes is a "resource" (a post, a user, a comment)
- **Representation** → resources are sent back in a format like JSON or XML

**Stateless** is important to understand:
- The server treats every request as brand new
- Session data is NOT stored on the server
- The client must include authentication info (token) in EVERY request

---

## HTTP (HyperText Transfer Protocol)

HTTP is the protocol (set of rules) used to transfer data over the web.

- Every API request is an HTTP request
- HTTP defines how the request is structured and how the response comes back
- HTTP is **request-response based** — client sends a request, server sends a response

---

## HTTP Request Structure

Every HTTP request has these parts:

**1. Method (Verb)** — what action you want to perform
- `GET` → retrieve/read data
- `POST` → create new data
- `PUT` → fully update/replace existing data
- `PATCH` → partially update existing data
- `DELETE` → delete data

**2. URL (Endpoint)** — where you're sending the request
- Example: `https://api.myapp.com/posts/5`
- The path (`/posts/5`) identifies the resource

**3. Headers** — metadata about the request
- `Content-Type: application/json` → tells server what format the body is in
- `Authorization: Bearer <token>` → authentication info
- Headers are key-value pairs, not visible in URL

**4. Body (optional)** — the data sent WITH the request
- Only for POST, PUT, PATCH
- GET and DELETE don't have a body
- Usually formatted as JSON

---

## HTTP Response Structure

Every response has:

**1. Status Code** — a 3-digit number telling you what happened

| Range | Meaning | Examples |
|-------|---------|---------|
| 2xx | Success | 200 OK, 201 Created, 204 No Content |
| 3xx | Redirect | 301 Moved Permanently |
| 4xx | Client Error | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 422 Unprocessable |
| 5xx | Server Error | 500 Internal Server Error |

**2. Headers** — metadata about the response
- `Content-Type: application/json` → tells client what format the body is in

**3. Body** — the actual data returned (usually JSON)

> Rule: 4xx = it's the client's fault (wrong request). 5xx = it's the server's fault (bug or crash).

---

## JSON — The Language of APIs

JSON = **JavaScript Object Notation**. The standard format for sending data in APIs.

- Looks like a Python dictionary
- Key-value pairs wrapped in `{}`
- Keys are always strings (with double quotes)
- Values can be: string, number, boolean, null, array, or another object
- Lists are wrapped in `[]`

```
{
  "id": 1,
  "title": "My Post",
  "published": true,
  "author": { "name": "Anik", "age": 20 }
}
```

> Why JSON? It's lightweight, human-readable, and every programming language can parse it.

---

## Postman

Postman is a tool to test APIs without needing a frontend.

- You can manually send HTTP requests (GET, POST, etc.) and see the response
- Set headers, body, authentication all from a GUI
- Very useful during development to verify your API before connecting a frontend
- Alternative: `curl` in the terminal, or **Thunder Client** (VS Code extension)

---

# PART 2 — FastAPI Basics

---

## What is FastAPI?

FastAPI is a modern Python web framework for building APIs quickly.

- Based on Python **type hints** — you declare what type each parameter is, FastAPI handles the rest
- Extremely fast — one of the fastest Python frameworks (comparable to Node.js and Go)
- **Automatic documentation** — FastAPI generates interactive API docs automatically at `/docs` and `/redoc`
- Built on top of **Starlette** (web framework) and **Pydantic** (data validation)
- Uses **ASGI** (Asynchronous Server Gateway Interface) — supports async requests natively

**Why FastAPI over Flask/Django?**
- Flask: minimal, but no built-in validation, no auto docs, slower
- Django: full-featured but heavy and opinionated, overkill for pure APIs
- FastAPI: best of both — lightweight but powerful, fast, with validation and docs built-in

---

## Setup & Installation

```bash
pip install fastapi uvicorn[standard]
```

- **fastapi** → the framework itself
- **uvicorn** → the ASGI server that runs your FastAPI app (like Apache/Nginx but for Python async)
- Always use a **virtual environment** to isolate project dependencies:

```bash
python -m venv venv
source venv/bin/activate      # Mac/Linux
venv\Scripts\activate         # Windows
```

> Why virtual environment? Keeps each project's packages separate. Without it, packages from one project can break another.

---

## Creating Your First FastAPI App

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def root():
    return {"message": "Hello World"}
```

- `app = FastAPI()` → creates the application instance
- `@app.get("/")` → a **decorator** that registers this function as the handler for GET requests to "/"
- The function returns a Python dict — FastAPI automatically converts it to JSON
- Run with: `uvicorn main:app --reload`
  - `main` = the filename (main.py)
  - `app` = the FastAPI instance
  - `--reload` = hot-reload on file changes (development only)

---

## Path Operations (Routes)

A **path operation** = URL path + HTTP method together.

- The combination of a path and a method is what uniquely identifies an endpoint
- Example: `GET /posts` and `POST /posts` are TWO different endpoints even though same URL
- The function attached to a path operation is called a **path operation function**
- FastAPI matches incoming requests to the right function automatically

---

## Automatic API Documentation

FastAPI auto-generates documentation from your code:

- **Swagger UI** → `http://localhost:8000/docs` → interactive, you can test endpoints directly
- **ReDoc** → `http://localhost:8000/redoc` → cleaner, read-only documentation
- The docs are generated from your type hints, function names, docstrings, and Pydantic models
- In production you might want to disable docs for security

---

## Path Parameters

Path parameters are variable parts of the URL path — identified by `{name}` in the route.

```python
@app.get("/posts/{id}")
def get_post(id: int):
    return {"id": id}
```

- FastAPI extracts the value from the URL and passes it to your function
- The type hint (`int`) tells FastAPI to automatically convert the string from the URL to an integer
- If conversion fails (e.g., `/posts/abc` when expecting int) → FastAPI returns 422 automatically
- **Order matters** — more specific routes must come BEFORE parameterized ones in your file

---

## Query Parameters

Query parameters come after `?` in the URL: `/posts?limit=10&skip=5&search=python`

- Any function parameter that is NOT a path parameter is automatically treated as a query param
- They are optional by default if you give them a default value
- FastAPI extracts and validates them automatically

```python
@app.get("/posts")
def get_posts(limit: int = 10, skip: int = 0, search: str = ""):
    # limit, skip, search come from URL query string
    pass
```

---

# PART 3 — CRUD & HTTP Methods

---

## What is CRUD?

CRUD = **Create, Read, Update, Delete** — the four fundamental operations on data.

| Operation | HTTP Method | Typical Path |
|-----------|------------|-------------|
| Create | POST | /posts |
| Read (all) | GET | /posts |
| Read (one) | GET | /posts/{id} |
| Update | PUT or PATCH | /posts/{id} |
| Delete | DELETE | /posts/{id} |

- Every real-world feature in an app maps to one or more CRUD operations
- REST APIs are designed around resources (nouns like "posts", "users") and CRUD operations (verbs via HTTP methods)

---

## GET — Reading Data

- Used to retrieve data — never changes data on the server
- Parameters come from the URL (path or query) — no body
- Should be **idempotent** — calling it 10 times has the same result as calling it once
- Returns 200 OK on success

---

## POST — Creating Data

- Used to create new resources
- Data is sent in the **request body** as JSON
- NOT idempotent — sending the same POST twice creates two records
- Returns 201 Created on success (though 200 is also common)
- `status_code=201` in FastAPI: `@app.post("/posts", status_code=201)`

---

## PUT vs PATCH — Updating Data

**PUT:**
- Replaces the ENTIRE resource with what you send
- If you don't include a field, it becomes null/default
- Used when the client sends the full updated object

**PATCH:**
- Partially updates a resource — only the fields you send are changed
- Other fields remain unchanged
- More commonly used in practice — you only send what changed

---

## DELETE — Removing Data

- Deletes a resource identified by its ID in the path
- No body needed
- Returns 204 No Content (no body in response) or 200 OK
- Should be idempotent — deleting the same thing twice should be safe (second delete returns 404, not crash)

---

## HTTPException — Returning Errors

When something goes wrong (resource not found, not authorized), you raise an exception:

```python
from fastapi import HTTPException, status

raise HTTPException(
    status_code=status.HTTP_404_NOT_FOUND,
    detail="Post not found"
)
```

- FastAPI catches it and returns the proper HTTP error response automatically
- Use `status` module from FastAPI for named status codes (readable and less error-prone than raw numbers)
- Always return meaningful error messages in `detail` — helps debugging

---

# PART 4 — Pydantic & Schema Validation

---

## What is Pydantic?

Pydantic is a Python library for **data validation and settings management** using type hints.

- You define a model (class) with typed fields
- Pydantic automatically validates incoming data against that model
- If data doesn't match → Pydantic raises a clear validation error automatically
- FastAPI uses Pydantic deeply — request bodies, query params, responses all validated via Pydantic

> Think of Pydantic as a strict bouncer — it checks every piece of data before it enters your system.

---

## Pydantic BaseModel

```python
from pydantic import BaseModel

class PostCreate(BaseModel):
    title: str
    content: str
    published: bool = True   # default value — optional field
```

- Inherit from `BaseModel`
- Each field is a class attribute with a type annotation
- Optional fields get a default value
- Required fields have no default — must be provided

---

## Request Body Validation

When you declare a parameter with a Pydantic model type, FastAPI:
1. Reads the JSON from the request body
2. Validates it against the model
3. Converts it to the Python object
4. Passes it to your function

If validation fails → 422 Unprocessable Entity is returned automatically with a clear error message telling exactly which field is wrong and why.

---

## Why Separate Schemas Matter

You'll often need DIFFERENT models for different purposes:

- **Request schema** (input) → what the client sends you. Doesn't include `id` (server generates it) or `created_at`
- **Response schema** (output) → what you send back to the client. Includes `id`, `created_at`, etc.
- **DB model** → how data is stored. May include `password` hash (which you'd NEVER send in response)

Keeping these separate prevents accidentally exposing sensitive data.

---

## Field Validation with Pydantic

Beyond just types, Pydantic lets you add constraints:

```python
from pydantic import BaseModel, EmailStr
from typing import Optional

class UserCreate(BaseModel):
    email: EmailStr          # validates email format automatically
    password: str
    age: Optional[int] = None  # optional — can be None
```

- `EmailStr` → validates proper email format (needs `pip install email-validator`)
- `Optional[T]` → field can be None (nullable)
- You can also add min/max length, regex patterns, value ranges

---

# PART 5 — PostgreSQL & Raw SQL

---

## What is a Database?

A database is an organized system for storing, managing, and retrieving data persistently.

- Without a database, your data lives only in memory — lost when the server restarts
- Databases store data on disk — persists forever until you delete it
- **RDBMS** (Relational Database Management System) — organizes data in tables with rows and columns
- Tables have relationships with each other (hence "relational")

---

## What is PostgreSQL?

PostgreSQL (Postgres) is one of the most powerful open-source relational databases.

- Stores data in **tables** (like Excel spreadsheets but much more powerful)
- Uses **SQL** (Structured Query Language) to interact with data
- Supports complex queries, relationships, constraints, transactions
- Free, open-source, battle-tested at massive scale (used by Instagram, Reddit, etc.)

**Key concepts:**
- **Table** → a collection of related data (like a spreadsheet tab)
- **Row** → one record (one post, one user)
- **Column** → a specific attribute (title, content, created_at)
- **Primary Key** → unique identifier for each row (usually `id`, auto-incremented)
- **Schema** → the structure/blueprint of a table (which columns, what types, constraints)

---

## SQL Basics

SQL is the language used to talk to relational databases.

**Common commands:**

```sql
-- Read all rows
SELECT * FROM posts;

-- Read specific columns with a condition
SELECT id, title FROM posts WHERE published = true;

-- Create a new row
INSERT INTO posts (title, content) VALUES ('Hello', 'My first post');

-- Update a row
UPDATE posts SET title = 'New Title' WHERE id = 5;

-- Delete a row
DELETE FROM posts WHERE id = 5;
```

- `*` means all columns
- `WHERE` filters rows based on condition
- Every statement ends with `;`
- SQL is case-insensitive for keywords (SELECT = select) but column/table names are case-sensitive

---

## Connecting to PostgreSQL with psycopg2

**psycopg2** is the most popular PostgreSQL driver for Python.

- A "driver" is a library that lets your Python code talk to a specific database
- `pip install psycopg2-binary`

**Flow:**
1. Create a connection to the database
2. Create a cursor (like a pointer that executes SQL)
3. Execute SQL using the cursor
4. Commit the transaction (save changes)
5. Close connection when done

> Using raw SQL with psycopg2 works but is verbose and error-prone. This is why ORMs exist.

---

## What is a Transaction?

A transaction is a group of SQL operations that either ALL succeed or ALL fail together.

- **COMMIT** → save all changes in the current transaction permanently
- **ROLLBACK** → undo all changes in the current transaction (if something went wrong)
- This protects data integrity — you never have half-saved data
- Example: bank transfer — deduct from account A AND add to account B. If adding fails, deducting must also be undone.

---

# PART 6 — SQLAlchemy ORM

---

## What is an ORM?

ORM = **Object Relational Mapper**. It lets you interact with your database using Python code instead of raw SQL.

- You define your tables as Python classes
- ORM translates your Python operations into SQL automatically
- You never (or rarely) write SQL directly
- Result: cleaner, more Pythonic, less error-prone code

**Without ORM (raw SQL):**
```python
cursor.execute("SELECT * FROM posts WHERE id = %s", (id,))
```

**With ORM:**
```python
db.query(Post).filter(Post.id == id).first()
```

The ORM version is easier to read, write, and less vulnerable to SQL injection.

---

## SQLAlchemy

SQLAlchemy is Python's most powerful ORM — and also a full SQL toolkit.

- Two levels:
  - **Core** → low-level SQL expression language (still writes SQL-like code)
  - **ORM** → high-level, work with Python classes and objects
- FastAPI commonly uses SQLAlchemy ORM with PostgreSQL
- `pip install sqlalchemy`

---

## Defining Models (Tables)

Each table is a Python class that inherits from SQLAlchemy's `Base`.

```python
from sqlalchemy import Column, Integer, String, Boolean
from database import Base

class Post(Base):
    __tablename__ = "posts"        # the actual table name in DB

    id = Column(Integer, primary_key=True, nullable=False)
    title = Column(String, nullable=False)
    content = Column(String, nullable=False)
    published = Column(Boolean, default=True)
```

- `__tablename__` → name of the table in the database
- `Column(type, constraints)` → defines a column
- `primary_key=True` → this column is the unique identifier
- `nullable=False` → this column is required, cannot be empty
- `default=True` → default value if none provided

---

## Database Connection Setup

```python
# database.py
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, declarative_base

DATABASE_URL = "postgresql://user:password@localhost/dbname"

engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
Base = declarative_base()
```

- **engine** → the connection to the database
- **SessionLocal** → factory for creating database sessions
- **Base** → parent class for all your ORM models

---

## Database Session (Dependency Injection)

A **session** is an active database connection for a single request.

- Each request gets its OWN session → opened at start, closed at end
- FastAPI handles this via **dependency injection** — you declare what your function needs and FastAPI provides it

```python
def get_db():
    db = SessionLocal()
    try:
        yield db       # give the session to the route function
    finally:
        db.close()    # ALWAYS close, even if an error occurred
```

- Inject it into routes: `db: Session = Depends(get_db)`
- `Depends()` is FastAPI's dependency injection system
- Session is automatically closed after each request

---

## CRUD with SQLAlchemy

**Read all:**
```python
posts = db.query(Post).all()
posts = db.query(Post).filter(Post.published == True).all()
```

**Read one:**
```python
post = db.query(Post).filter(Post.id == id).first()
```

**Create:**
```python
new_post = Post(**post_data.dict())
db.add(new_post)
db.commit()
db.refresh(new_post)   # refresh to get server-generated fields like id
```

**Update:**
```python
post_query = db.query(Post).filter(Post.id == id)
post_query.update(updated_data.dict(), synchronize_session=False)
db.commit()
```

**Delete:**
```python
post_query = db.query(Post).filter(Post.id == id)
post_query.delete(synchronize_session=False)
db.commit()
```

> `db.commit()` saves changes. `db.refresh()` re-fetches the object from DB (needed to get auto-generated values like `id` and `created_at`).

---

## Alembic — Database Migrations

Migrations track changes to your database schema over time.

- When you add a new column, change a type, or add a table — you need a migration
- **Alembic** is the migration tool for SQLAlchemy
- Each migration = a versioned file that describes what changed
- You can apply migrations forward (upgrade) or reverse them (downgrade)
- Think of migrations like Git commits — but for your database structure
- `pip install alembic`
- `alembic init alembic` → setup migration folder
- `alembic revision --autogenerate -m "add column"` → auto-detect changes and generate migration
- `alembic upgrade head` → apply all pending migrations

---

# PART 7 — Response Schemas & Pydantic Models

---

## Input vs Output Models

You need different Pydantic models for different phases:

**Input model (request body):**
- What the client sends you
- No `id`, no `created_at` (these are server-generated)
- May include password in plain text

**Output model (response):**
- What you send back to the client
- Includes `id`, `created_at`
- NEVER includes `password` hash

**DB model (SQLAlchemy):**
- Represents the table row
- Has all fields including internal ones

---

## Pydantic's `model_config` and `from_orm`

By default Pydantic only reads from dicts. To read from SQLAlchemy objects:

```python
class PostResponse(BaseModel):
    id: int
    title: str
    published: bool

    model_config = ConfigDict(from_attributes=True)
    # Pydantic v2 style — allows reading from ORM objects
```

- `from_attributes=True` (previously `orm_mode=True` in Pydantic v1) → tells Pydantic to read data from object attributes, not just dictionaries
- Without this, returning a SQLAlchemy object directly would fail

---

## Controlling Response with `response_model`

```python
@app.get("/posts/{id}", response_model=PostResponse)
def get_post(id: int, db: Session = Depends(get_db)):
    ...
```

- `response_model` → FastAPI uses this to filter and shape the response
- Even if the database returns more fields (like password), only fields in `PostResponse` will be included in the API response
- FastAPI validates the response data too — not just input

---

# PART 8 — Authentication & Passwords

---

## Why Authentication Matters

Authentication = verifying WHO the user is.  
Authorization = verifying WHAT the user is allowed to do.

- Without authentication, anyone could create, read, update, or delete any data
- Most API endpoints should be protected — only logged-in users can access them

---

## Never Store Plain-Text Passwords

This is a hard rule, no exceptions.

- If your database is breached and passwords are plain text → everyone's account on every site (because people reuse passwords) is compromised
- Solution: **hash** the password before storing it
- A hash is a one-way transformation — you can't reverse a hash back to the original password
- To verify: hash the input password and compare it to the stored hash
- Same input always produces same hash. Different input never produces same hash.

---

## Password Hashing with bcrypt

**Passlib** is a Python library for password hashing. It uses bcrypt by default.

- `pip install passlib[bcrypt]`
- `bcrypt` is a slow hashing algorithm — intentionally slow to make brute-force attacks expensive
- It also adds a **salt** automatically — a random value added before hashing to prevent "rainbow table" attacks

```python
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"])

def hash_password(plain_password: str) -> str:
    return pwd_context.hash(plain_password)

def verify_password(plain_password: str, hashed_password: str) -> bool:
    return pwd_context.verify(plain_password, hashed_password)
```

- `hash()` → converts plain password to a bcrypt hash
- `verify()` → checks if plain password matches the stored hash (returns True/False)

---

## User Registration Flow

1. Client sends email + password
2. Server hashes the password
3. Server stores email + hashed_password in users table
4. Server returns user info (without password) in response
5. Server returns 201 Created

---

## Login Flow

1. Client sends email + password
2. Server looks up user by email
3. If user not found → 403 Forbidden
4. Server verifies password against stored hash
5. If password wrong → 403 Forbidden
6. If correct → generate and return a JWT token
7. Client stores the token and sends it with every subsequent request

---

# PART 9 — JWT Tokens

---

## What is a JWT?

JWT = **JSON Web Token**. A secure way to transmit information as a compact, self-contained token.

- Pronounced "jot"
- When a user logs in, the server gives them a JWT
- The client stores it and sends it back with every request (in the Authorization header)
- The server verifies the token and knows who the user is

**Why JWT is better than sessions:**
- Sessions require server to store data → doesn't scale well
- JWT is stateless — the token itself contains all needed info
- Server just needs to verify the signature → no database lookup needed

---

## JWT Structure

A JWT has 3 parts separated by dots: `header.payload.signature`

**1. Header** (base64 encoded)
- Algorithm used for signing (e.g., HS256)
- Token type: "JWT"

**2. Payload** (base64 encoded)
- The actual data (claims) — user id, email, expiry time
- NOT encrypted — anyone can decode it. Don't put sensitive data here.
- Common claims: `sub` (subject = user id), `exp` (expiry), `iat` (issued at)

**3. Signature**
- Header + Payload signed with a secret key using the algorithm specified
- This is what makes JWT secure — only your server knows the secret key
- If anyone tampers with the payload, the signature won't match and the token is rejected

> JWT is like a signed document. Anyone can read it. But only the signer (your server) can create one that passes verification.

---

## Creating JWT Tokens

```python
from jose import jwt
from datetime import datetime, timedelta

SECRET_KEY = "your-secret-key"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

def create_access_token(data: dict):
    to_encode = data.copy()
    expire = datetime.utcnow() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
```

- `pip install python-jose[cryptography]`
- `data` → the payload (usually `{"sub": str(user_id)}`)
- `exp` → expiry time — tokens expire for security (user must re-login)
- Secret key should be long, random, and NEVER committed to git

---

## Verifying JWT Tokens (OAuth2 & Depends)

FastAPI has built-in OAuth2 support:

```python
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="login")
```

- `OAuth2PasswordBearer` → tells FastAPI where the login endpoint is
- When used as a dependency, it automatically extracts the Bearer token from the Authorization header
- Your verify function decodes the token, checks expiry, and returns the user ID

**Protecting a route:**
```python
@app.get("/posts")
def get_posts(current_user = Depends(get_current_user)):
    # only runs if token is valid
    pass
```

- `Depends(get_current_user)` → runs the auth check before the function. If auth fails, request is rejected automatically.

---

## Token Expiry

- Short-lived tokens (15-60 min) are standard for security
- After expiry, user must log in again to get a new token
- In production apps: use **refresh tokens** — long-lived tokens used only to get new access tokens without re-login

---

# PART 10 — Routers & Code Organization

---

## Why Organize Code?

Keeping all routes in `main.py` becomes unmanageable as the app grows.

- 200+ lines in one file → hard to read, debug, and maintain
- Multiple people working on the same file → constant merge conflicts
- Solution: split routes into separate files using **Routers**

---

## APIRouter

```python
# routers/post.py
from fastapi import APIRouter

router = APIRouter(
    prefix="/posts",        # all routes here automatically start with /posts
    tags=["Posts"]          # groups these routes in the docs
)

@router.get("/")            # actual path becomes /posts/
def get_posts():
    pass
```

- `prefix` → prepended to all routes in this router. Avoids repeating `/posts` in every route
- `tags` → groups endpoints in Swagger docs (makes docs clean)

**Registering the router in main.py:**
```python
from routers import post, user, auth

app.include_router(post.router)
app.include_router(user.router)
app.include_router(auth.router)
```

---

## Recommended File Structure

```
myapp/
├── main.py              → app setup, include routers
├── database.py          → DB connection, Session
├── models.py            → SQLAlchemy table models
├── schemas.py           → Pydantic input/output models
├── oauth2.py            → JWT token logic
├── utils.py             → helper functions (password hash, etc.)
└── routers/
    ├── post.py          → all /posts endpoints
    ├── user.py          → all /users endpoints
    └── auth.py          → /login endpoint
```

---

# PART 11 — Database Relationships

---

## What are Relationships?

Data is rarely isolated — posts have authors, comments belong to posts, orders have products.

- **Foreign Key** → a column in one table that references the primary key of another table
- This creates a link between the two tables
- The database enforces this link — you can't create a post with a non-existent user_id

**One-to-Many:**
- One user → many posts
- One post → many comments
- The "many" side holds the foreign key

**Many-to-Many:**
- Many users can like many posts
- Requires a **junction table** (e.g., votes table with user_id and post_id)

---

## Adding Foreign Key in SQLAlchemy

```python
class Post(Base):
    __tablename__ = "posts"

    id = Column(Integer, primary_key=True)
    title = Column(String, nullable=False)
    owner_id = Column(Integer, ForeignKey("users.id", ondelete="CASCADE"), nullable=False)
    owner = relationship("User")  # sets up the Python-level relationship
```

- `ForeignKey("users.id")` → this column references the `id` column in the `users` table
- `ondelete="CASCADE"` → if the user is deleted, all their posts are deleted automatically
- `relationship("User")` → lets you access `post.owner` and get the User object (SQLAlchemy handles the JOIN)

---

## ondelete Options

| Option | Behavior |
|--------|---------|
| `CASCADE` | Delete child rows when parent is deleted |
| `SET NULL` | Set the foreign key to NULL when parent is deleted |
| `RESTRICT` | Prevent deletion of parent if child rows exist |
| `SET DEFAULT` | Set foreign key to column's default value |

---

## Ownership & Authorization

Once posts have an `owner_id`:
- Only the owner should be able to update or delete their own posts
- Check: `if post.owner_id != current_user.id: raise HTTPException(403, "Not authorized")`
- 403 Forbidden → you're authenticated (logged in) but not authorized to do this action

---

# PART 12 — Votes & SQL Joins

---

## Votes / Likes Feature

Users can upvote posts — but only once per post per user.

**Junction Table design:**
```
votes table:
- post_id (FK → posts.id)
- user_id (FK → users.id)
- Primary Key = (post_id, user_id) combined → composite primary key
```

- Composite primary key = combination of two columns forms the unique ID
- This automatically prevents the same user from voting on the same post twice (DB-level enforcement)

---

## SQL Joins

A JOIN combines rows from two tables based on a related column.

**Why JOINs?**
- You often need data from multiple tables in one query
- Example: Get all posts WITH the owner's name AND vote count
- Without JOIN: 3 separate queries. With JOIN: 1 query.

**Types of JOINs:**

| Type | Returns |
|------|---------|
| `INNER JOIN` | Only rows where the condition matches in BOTH tables |
| `LEFT JOIN` | All rows from left table + matching rows from right (NULL if no match) |
| `RIGHT JOIN` | All rows from right table + matching rows from left |
| `FULL OUTER JOIN` | All rows from both tables |

**Most common:** LEFT JOIN (keep all posts even if they have 0 votes)

---

## JOINs in SQLAlchemy

```python
from sqlalchemy import func

results = db.query(
    Post, func.count(Vote.post_id).label("votes")
).join(
    Vote, Vote.post_id == Post.id, isouter=True   # LEFT JOIN
).group_by(Post.id).all()
```

- `func.count()` → SQL COUNT aggregate function
- `.label("votes")` → alias the result column
- `isouter=True` → makes it a LEFT JOIN (include posts with 0 votes)
- `.group_by(Post.id)` → needed when using aggregate functions — groups rows before counting

---

# PART 13 — Environment Variables

---

## What are Environment Variables?

Environment variables are configuration values stored OUTSIDE your code.

**Why?**
- Database passwords, secret keys, API keys — these must NOT be in your source code
- If you push your code to GitHub with your DB password → anyone can access your database
- Different environments (dev, staging, production) need different values
- Environment variables let you change config without changing code

---

## .env File and python-dotenv

A `.env` file stores your environment variables locally.

```
DATABASE_URL=postgresql://user:password@localhost/mydb
SECRET_KEY=your-super-secret-key
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
```

- `pip install python-dotenv`
- Load it with `load_dotenv()` — reads the .env file into environment
- **ALWAYS add `.env` to `.gitignore`** — never commit this file

---

## Pydantic Settings (Better Approach)

FastAPI's recommended way to handle config:

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: str
    algorithm: str = "HS256"
    access_token_expire_minutes: int = 30

    class Config:
        env_file = ".env"

settings = Settings()
```

- `pydantic_settings` automatically reads from environment variables AND `.env` file
- Validates types — if `access_token_expire_minutes` is missing or not an int, it fails at startup
- Access anywhere: `settings.secret_key`
- `pip install pydantic-settings`

---

# PART 14 — CORS

---

## What is CORS?

CORS = **Cross-Origin Resource Sharing**. A browser security mechanism.

- **Origin** = scheme + domain + port (e.g., `http://localhost:3000`)
- By default, browsers block JavaScript from making requests to a DIFFERENT origin than the page
- Example: your React app at `localhost:3000` can't call your API at `localhost:8000` — DIFFERENT ports = different origins
- CORS is a BROWSER restriction — Postman and mobile apps are NOT affected (they're not browsers)

---

## How CORS Works

1. Browser sends a **preflight** request (`OPTIONS` method) asking "is this allowed?"
2. Server responds with headers saying which origins, methods, and headers are allowed
3. If allowed, browser proceeds with the actual request
4. If not allowed, browser blocks the request (even if server would have processed it)

---

## Enabling CORS in FastAPI

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://yourfrontend.com"],   # which origins can call this API
    allow_credentials=True,    # allow cookies/auth headers
    allow_methods=["*"],       # which HTTP methods are allowed
    allow_headers=["*"],       # which headers are allowed
)
```

- `allow_origins=["*"]` → allow ALL origins (development only — dangerous in production)
- In production: list only the specific frontend domains you trust
- CORS must be added as middleware BEFORE your routes

---

# PART 15 — Git Basics

---

## What is Git?

Git is a **version control system** — it tracks changes to your code over time.

- Every change is saved as a "commit" — a snapshot of your code at that point
- You can go back to any previous commit
- Multiple people can work on the same codebase without overwriting each other
- **GitHub/GitLab** → cloud hosting for git repositories

---

## Core Git Concepts

- **Repository (repo)** → the folder being tracked by git
- **Commit** → a saved snapshot of changes with a message describing what changed
- **Branch** → an independent line of development
- **main/master** → the default branch
- **Remote** → a copy of the repo on another server (GitHub)
- **Clone** → download a remote repo to your machine
- **Push** → upload your commits to the remote
- **Pull** → download and merge new commits from remote

---

## Essential Git Commands

```bash
git init                          # start tracking a folder
git clone <url>                   # copy a remote repo locally

git status                        # see what files changed
git add .                         # stage all changes
git add filename                  # stage specific file
git commit -m "message"           # save snapshot

git push origin main              # upload to GitHub
git pull origin main              # download from GitHub

git branch feature-login          # create a new branch
git checkout feature-login        # switch to that branch
git merge feature-login           # merge branch into current
```

---

## .gitignore

A `.gitignore` file tells Git which files and folders to NEVER track.

```
.env                 # environment variables — NEVER commit this
__pycache__/         # Python bytecode cache
venv/                # virtual environment — huge, not needed
*.pyc                # compiled Python files
```

> Critical: if you accidentally commit `.env`, change your secrets immediately even after removing the file — git history keeps old content.

---

# PART 16 — Deployment on Linux Server

---

## Deployment Overview

Deployment = making your API accessible on the internet, running 24/7 on a real server.

**Options:**
- **VPS (Virtual Private Server)** → e.g., DigitalOcean Droplet, AWS EC2, Linode. You manage everything.
- **PaaS (Platform as a Service)** → e.g., Render, Railway, Heroku. They manage the server for you.
- **Serverless** → e.g., AWS Lambda. You pay per request, not per server.

---

## Setting Up a Ubuntu Server (VPS)

```bash
# On your local machine — connect to server
ssh root@your-server-ip

# Update the system packages
apt update && apt upgrade -y

# Install Python and pip
apt install python3 python3-pip -y

# Install PostgreSQL
apt install postgresql postgresql-contrib -y
```

---

## Running Your App on Server

```bash
# Clone your code
git clone https://github.com/youruser/yourapp.git
cd yourapp

# Install dependencies
pip install -r requirements.txt

# Set up environment variables (create .env manually on server)
nano .env

# Test it runs
uvicorn main:app
```

---

## requirements.txt

A file that lists ALL Python packages your project needs.

```bash
pip freeze > requirements.txt    # generate from your current environment
pip install -r requirements.txt  # install everything from the file
```

- Always keep this updated before pushing code
- This is how the server knows what to install

---

## Running as a Background Process

You don't want your app tied to your SSH session. Use a process manager.

**Systemd service (Linux):**
- Create a service file that tells Linux to run your app
- Auto-starts on server reboot
- Restarts automatically if it crashes
- `systemctl start myapp`, `systemctl status myapp`, `systemctl enable myapp`

---

# PART 17 — Nginx & Process Management

---

## What is Nginx?

Nginx (pronounced "engine-x") is a high-performance web server.

**Why do you need Nginx if you already have Uvicorn?**
- Uvicorn is an ASGI server — good at running Python async code
- Nginx sits in FRONT of Uvicorn as a **reverse proxy**

**What Nginx does:**
- Handles incoming HTTPS traffic and SSL certificates
- Forwards requests to Uvicorn running on localhost
- Serves static files directly (much faster than Python)
- Load balancing (distribute traffic across multiple app instances)
- Rate limiting and security features
- Handles many concurrent connections efficiently

---

## Nginx as Reverse Proxy

```
Internet → Nginx (port 443/80) → Uvicorn (port 8000, localhost only)
```

- Nginx accepts the public request
- Forwards it to Uvicorn internally
- Uvicorn processes it with Python
- Response goes back through Nginx to the internet
- Uvicorn is never directly exposed to the internet

---

## Gunicorn

Gunicorn is a WSGI/ASGI process manager — runs multiple instances of Uvicorn.

- `gunicorn -w 4 -k uvicorn.workers.UvicornWorker main:app`
- `-w 4` → 4 worker processes (handle 4 requests simultaneously)
- A single Uvicorn process = single-threaded. Multiple workers = parallelism.
- Rule of thumb for workers: `2 × number of CPU cores + 1`

---

## SSL / HTTPS

- HTTPS = HTTP with SSL encryption
- All data is encrypted between browser and server
- Required for production — browsers show "Not Secure" for HTTP
- Use **Let's Encrypt** + **Certbot** for free SSL certificates
- Certbot automatically configures Nginx for HTTPS

---

# PART 18 — Docker

---

## What is Docker?

Docker is a tool that packages your application and ALL its dependencies into a portable unit called a **container**.

- The container runs the same way everywhere — your machine, teammate's machine, server — no "it works on my machine" problems
- Containers are isolated — they don't interfere with each other or the host system
- Much lighter than Virtual Machines — containers share the OS kernel

---

## Key Docker Concepts

**Image** → a blueprint for a container. Read-only. Like a class.  
**Container** → a running instance of an image. Like an object.  
**Dockerfile** → instructions for building an image. Step-by-step recipe.  
**Docker Hub** → public registry of pre-built images (like GitHub but for Docker images).  
**Volume** → persistent storage attached to a container (data survives container restarts).  
**Docker Compose** → tool to run multiple containers together (app + database).

---

## Dockerfile — Building Your App Image

```dockerfile
FROM python:3.11            # start from official Python image

WORKDIR /app                # set working directory inside container

COPY requirements.txt .     # copy requirements first (caching optimization)
RUN pip install -r requirements.txt   # install dependencies

COPY . .                    # copy rest of your code

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

- **Each line is a layer** — Docker caches layers. If requirements.txt didn't change, that layer isn't rebuilt.
- `FROM` → always start from a base image
- `WORKDIR` → all subsequent commands run from this directory
- `COPY` → copy files from host into the container
- `RUN` → execute a command during build
- `CMD` → command to run when container starts

---

## Docker Compose — Multiple Containers

Real apps need multiple containers (API + database + cache). Docker Compose manages them together.

```yaml
# docker-compose.yml
version: "3"
services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://postgres:password@db/mydb
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=mydb
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```

- `services` → each container is a service
- `ports: "8000:8000"` → maps host port to container port (host:container)
- `depends_on` → start db before api
- `volumes` → persists database data (without this, data is lost when container stops)

**Commands:**
```bash
docker compose up -d          # start all services in background
docker compose down           # stop and remove containers
docker compose logs api       # view logs of a service
docker compose exec api bash  # get a terminal inside a container
```

---

## Why Docker Matters

- Dev environment exactly matches production → fewer "works on my machine" bugs
- Easy to scale → spin up 10 containers from the same image
- Rollbacks are easy → revert to a previous image version
- Standard across the industry → every cloud platform supports Docker

---

# PART 19 — CI/CD with GitHub Actions

---

## What is CI/CD?

CI/CD = **Continuous Integration / Continuous Deployment**

**Continuous Integration (CI):**
- Every time you push code, automated tests run
- If tests fail, you're immediately notified — don't break the main branch
- Catches bugs early, before they reach production

**Continuous Deployment (CD):**
- After tests pass, code is automatically deployed to the server
- No manual deployment steps → ship faster, more reliably
- Push to `main` → tests run → if pass → automatically live in production

---

## GitHub Actions

GitHub Actions is GitHub's built-in CI/CD platform.

- Define automation workflows in YAML files inside `.github/workflows/`
- Triggered by events: push, pull request, schedule, manual
- Runs on **runners** → GitHub-managed or self-hosted machines that execute your workflow

---

## Workflow Concepts

**Workflow** → a full automation pipeline (defined in one YAML file)  
**Trigger (on)** → what event starts the workflow (push to main, PR opened, etc.)  
**Job** → a group of steps that run on one runner  
**Step** → a single task (run a command, call an action)  
**Action** → a reusable step you can import (like a plugin)

---

## Basic CI Pipeline Example

```yaml
# .github/workflows/build-and-test.yml
name: Build and Test

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    services:
      db:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: password
          POSTGRES_DB: testdb
        options: >-
          --health-cmd pg_isready
          --health-interval 10s

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.11"

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run tests
        env:
          DATABASE_URL: postgresql://postgres:password@localhost/testdb
        run: pytest
```

---

## CD — Deploying After Tests Pass

After tests pass, automatically SSH into your server and pull the latest code:

```yaml
  deploy:
    needs: test         # only runs if test job passes
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to server
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_IP }}
          username: ubuntu
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /app
            git pull origin main
            pip install -r requirements.txt
            sudo systemctl restart myapp
```

- **Secrets** → store sensitive values (SSH key, server IP) in GitHub repo settings → used in workflow as `${{ secrets.NAME }}`
- Never hardcode credentials in workflow files

---

## Testing with Pytest

```python
# tests/test_posts.py
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_get_posts():
    response = client.get("/posts")
    assert response.status_code == 200

def test_create_post_unauthorized():
    response = client.post("/posts", json={"title": "Test", "content": "Hi"})
    assert response.status_code == 403
```

- `TestClient` → simulates HTTP requests to your app without a real server
- `assert` → if condition is false, test fails
- `pytest` → discovers and runs all `test_*.py` files automatically
- Test database → use a separate test DB and clear it between tests

---

## Quick Reference — Status Codes

| Code | Meaning | When to use |
|------|---------|------------|
| 200 | OK | Successful GET, PUT, PATCH |
| 201 | Created | Successful POST |
| 204 | No Content | Successful DELETE |
| 400 | Bad Request | Malformed request |
| 401 | Unauthorized | Not logged in |
| 403 | Forbidden | Logged in but not allowed |
| 404 | Not Found | Resource doesn't exist |
| 422 | Unprocessable | Validation error (wrong data type) |
| 500 | Server Error | Bug in your code |

---

## Quick Reference — Full Stack Summary

| Layer | Tool | Purpose |
|-------|------|---------|
| Language | Python | Backend logic |
| Framework | FastAPI | Route handling, request/response |
| Validation | Pydantic | Input/output data validation |
| Database | PostgreSQL | Persistent data storage |
| ORM | SQLAlchemy | Python ↔ database bridge |
| Migrations | Alembic | Schema change management |
| Auth | JWT + bcrypt | Secure authentication |
| Server | Uvicorn + Gunicorn | Run the Python app |
| Proxy | Nginx | HTTPS, routing, performance |
| Container | Docker | Portable, reproducible environment |
| CI/CD | GitHub Actions | Automated testing and deployment |

---
*Notes by Anik | freeCodeCamp.org — Python API Development Comprehensive Course*
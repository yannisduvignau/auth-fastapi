# ⚡ auth-fastapi

A hands-on **FastAPI learning project** exploring authentication, CRUD operations, and security vulnerabilities. Covers JWT-based auth, SQLite database integration, and documented exploitable weaknesses — all in Python.

---

## 📋 Description

**auth-fastapi** is a practical study project built around [FastAPI](https://fastapi.tiangolo.com/). It is organized as a collection of standalone test scripts that progressively explore:

- **Authentication** — JWT token generation, validation, and protected routes
- **CRUD with a database** — Operations against a real SQLite database (`test.db`)
- **CRUD with local data** — In-memory data manipulation without a database
- **Security research** — Documented exploitable vulnerabilities in the implementation

This project serves as both a reference and a playground for understanding FastAPI security patterns and their common pitfalls.

---

## 🗂️ Project Structure

```
auth-fastapi/
├── authTest.py                   # JWT authentication: login, token generation & protected routes
├── crudWithDBTest.py             # CRUD operations using SQLite (SQLAlchemy)
├── crudWithLocalDataTest.py      # CRUD operations using in-memory / local data
├── test.db                       # SQLite database file
├── librairies.md                 # Library reference & install notes
├── recap.md                      # Concepts recap & personal notes
├── vulnerabilite_exploitable.md  # Documented exploitable vulnerabilities
├── myenv/                        # Python virtual environment
└── .gitignore
```

---

## 🛠️ Tech Stack

| Layer           | Technology                                      |
|-----------------|-------------------------------------------------|
| Language        | Python 3.x                                      |
| Framework       | FastAPI                                         |
| Server          | Uvicorn (ASGI)                                  |
| Authentication  | OAuth2 + JWT (`python-jose` / `PyJWT`)          |
| Password hashing| `passlib` + `bcrypt`                            |
| ORM             | SQLAlchemy                                      |
| Database        | SQLite (`test.db`)                              |
| Validation      | Pydantic                                        |
| Docs            | Swagger UI (auto-generated at `/docs`)          |

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- pip

---

### 1. Clone the repository

```bash
git clone https://github.com/yannisduvignau/auth-fastapi.git
cd auth-fastapi
```

### 2. Create and activate a virtual environment

```bash
python -m venv myenv

# macOS / Linux
source myenv/bin/activate

# Windows
myenv\Scripts\activate
```

### 3. Install dependencies

Refer to `librairies.md` for the full list. Main packages:

```bash
pip install fastapi uvicorn sqlalchemy python-jose passlib[bcrypt] pydantic
```

---

## 📂 Running the Scripts

Each file is an independent FastAPI application. Launch any of them with Uvicorn:

### Authentication demo

```bash
uvicorn authTest:app --reload
```

### CRUD with SQLite

```bash
uvicorn crudWithDBTest:app --reload
```

### CRUD with local data

```bash
uvicorn crudWithLocalDataTest:app --reload
```

Once running, open the interactive API docs at:

- **Swagger UI** → [http://localhost:8000/docs](http://localhost:8000/docs)
- **ReDoc** → [http://localhost:8000/redoc](http://localhost:8000/redoc)

---

## 🔐 Authentication Flow (`authTest.py`)

The authentication module implements a standard **OAuth2 Password + JWT** flow:

```
POST /token          →  Submit username & password → receive JWT access token
GET  /users/me       →  Protected route → requires valid Bearer token
```

**Token usage:**

```bash
# 1. Get a token
curl -X POST "http://localhost:8000/token" \
  -d "username=johndoe&password=secret"

# 2. Call a protected route
curl -H "Authorization: Bearer <your_token>" \
  http://localhost:8000/users/me
```

---

## 🗃️ CRUD Endpoints

### With database (`crudWithDBTest.py`)

| Method | Endpoint       | Description         |
|--------|---------------|---------------------|
| GET    | `/items`       | List all items      |
| GET    | `/items/{id}`  | Get item by ID      |
| POST   | `/items`       | Create a new item   |
| PUT    | `/items/{id}`  | Update an item      |
| DELETE | `/items/{id}`  | Delete an item      |

### With local data (`crudWithLocalDataTest.py`)

Same endpoints as above, but backed by an in-memory Python list instead of a database.

---

## 🔓 Documented Vulnerabilities (`vulnerabilite_exploitable.md`)

This project intentionally documents known weaknesses for educational purposes:

| # | Vulnerability | Description |
|---|---------------|-------------|
| 1 | Weak JWT secret | Hardcoded or short secret key easily brute-forced |
| 2 | No token expiry enforcement | Tokens with far-future or no expiry |
| 3 | SQL Injection risk | Unsafe raw queries in some CRUD implementations |
| 4 | Plaintext credentials in local data | Passwords stored without proper hashing |
| 5 | Missing authorization checks | Endpoints accessible without proper role validation |

> See `vulnerabilite_exploitable.md` for detailed exploitation steps and remediation advice.

---

## 📖 Notes & Recap

The `recap.md` file contains a personal summary of key FastAPI concepts covered in this project:

- Dependency injection with `Depends()`
- Pydantic models for request/response validation
- SQLAlchemy session management
- OAuth2 scheme with `fastapi.security`
- Route protection patterns

---

## 🤝 Contributing

1. Fork the project
2. Create your branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -m 'Add improvement'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

---

## 👤 Author

**Yannis Duvignau**  
[GitHub](https://github.com/yannisduvignau)

---

## 📄 License

This project is distributed under an open license. See the `LICENSE` file for more details.
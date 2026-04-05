# 📘 FastAPI - Modern API Framework

## 📌 Introduction

**FastAPI** is a modern, fast web framework for building APIs with Python 3.7+. It's built on top of Starlette (async) and Pydantic (data validation), providing automatic documentation and type hints.

Think of FastAPI as **Flask's younger, faster sibling** — it does everything Flask does, plus automatic validation, async support, and auto-generated docs!

```
┌────────────────────────────────────────────────────────────────┐
│                    FASTAPI FEATURES                             │
│                                                                 │
│   ┌──────────────────┐   ┌──────────────────┐                 │
│   │  Type Hints      │   │  Automatic Docs  │                 │
│   │  def func(id:int)│   │  /docs, /redoc   │                 │
│   └────────┬─────────┘   └────────┬─────────┘                 │
│            │                       │                           │
│            ▼                       ▼                           │
│   ┌────────────────────────────────────────────┐              │
│   │              FASTAPI                        │              │
│   │                                             │              │
│   │  • Async/Await Support                     │              │
│   │  • Pydantic Validation                     │              │
│   │  • Dependency Injection                    │              │
│   │  • OAuth2 / JWT Ready                      │              │
│   └────────────────────────────────────────────┘              │
│                                                                 │
│   Performance: One of the fastest Python frameworks           │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### FastAPI vs Flask

| Feature | FastAPI | Flask |
|---------|---------|-------|
| Speed | Very Fast | Fast |
| Async | Built-in | Extension |
| Validation | Automatic | Manual |
| Docs | Auto-generated | Manual |
| Type Hints | Required | Optional |

### Installation

```bash
pip install fastapi uvicorn
```

### Running the App

```bash
uvicorn main:app --reload
```

---

## 💻 Code Examples

### 🟢 Basic: Hello World

```python
from fastapi import FastAPI

# Create FastAPI app
app = FastAPI()

# Define route
@app.get("/")
def home():
    return {"message": "Hello, World!"}

# Run with: uvicorn main:app --reload
# Visit: http://127.0.0.1:8000/
# Docs: http://127.0.0.1:8000/docs
```

### 🟢 Basic: Path Parameters

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}

@app.get("/items/{item_id}")
def get_item(item_id: int, q: str = None):
    """
    item_id: Path parameter (required)
    q: Query parameter (optional)
    """
    if q:
        return {"item_id": item_id, "query": q}
    return {"item_id": item_id}

# Type validation is automatic!
# GET /users/abc -> 422 Validation Error
# GET /users/123 -> {"user_id": 123}
```

### 🟢 Basic: Query Parameters

```python
from fastapi import FastAPI
from typing import Optional

app = FastAPI()

@app.get("/items/")
def get_items(
    skip: int = 0,           # Default value
    limit: int = 10,         # Default value
    q: Optional[str] = None  # Optional
):
    """
    GET /items/                     -> skip=0, limit=10, q=None
    GET /items/?skip=5              -> skip=5, limit=10, q=None
    GET /items/?skip=5&limit=20     -> skip=5, limit=20, q=None
    GET /items/?q=search            -> skip=0, limit=10, q="search"
    """
    return {
        "skip": skip,
        "limit": limit,
        "query": q
    }

# Boolean query params
@app.get("/search/")
def search(
    q: str,
    include_archived: bool = False
):
    # FastAPI converts "true", "True", "1", "on", "yes" to True
    return {"query": q, "include_archived": include_archived}
```

### 🟡 Intermediate: Request Body with Pydantic

```python
from fastapi import FastAPI
from pydantic import BaseModel, Field, EmailStr
from typing import Optional
from datetime import datetime

app = FastAPI()

# Pydantic model for request body
class User(BaseModel):
    name: str
    email: EmailStr
    age: int = Field(ge=0, le=150)  # 0 <= age <= 150
    is_active: bool = True
    
    class Config:
        json_schema_extra = {
            "example": {
                "name": "John Doe",
                "email": "john@example.com",
                "age": 30,
                "is_active": True
            }
        }

# Response model
class UserResponse(BaseModel):
    id: int
    name: str
    email: EmailStr
    created_at: datetime

@app.post("/users/", response_model=UserResponse)
def create_user(user: User):
    """
    Request body is automatically validated against User model.
    Response is validated against UserResponse model.
    """
    return {
        "id": 1,
        "name": user.name,
        "email": user.email,
        "created_at": datetime.now()
    }

# Optional fields
class ItemUpdate(BaseModel):
    name: Optional[str] = None
    price: Optional[float] = None
    description: Optional[str] = None

@app.patch("/items/{item_id}")
def update_item(item_id: int, item: ItemUpdate):
    # Only provided fields will be set
    update_data = item.dict(exclude_unset=True)
    return {"item_id": item_id, "updated": update_data}
```

### 🟡 Intermediate: HTTP Methods and Status Codes

```python
from fastapi import FastAPI, status, HTTPException
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    price: float

# Database simulation
items_db = {}

@app.get("/items/")
def list_items():
    return list(items_db.values())

@app.post("/items/", status_code=status.HTTP_201_CREATED)
def create_item(item: Item):
    item_id = len(items_db) + 1
    items_db[item_id] = {"id": item_id, **item.dict()}
    return items_db[item_id]

@app.get("/items/{item_id}")
def get_item(item_id: int):
    if item_id not in items_db:
        raise HTTPException(
            status_code=404,
            detail=f"Item {item_id} not found"
        )
    return items_db[item_id]

@app.put("/items/{item_id}")
def update_item(item_id: int, item: Item):
    if item_id not in items_db:
        raise HTTPException(status_code=404, detail="Item not found")
    items_db[item_id] = {"id": item_id, **item.dict()}
    return items_db[item_id]

@app.delete("/items/{item_id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_item(item_id: int):
    if item_id not in items_db:
        raise HTTPException(status_code=404, detail="Item not found")
    del items_db[item_id]
    return None  # 204 No Content
```

### 🟡 Intermediate: Form Data and File Uploads

```python
from fastapi import FastAPI, Form, File, UploadFile
from typing import List

app = FastAPI()

# Form data
@app.post("/login/")
def login(
    username: str = Form(...),  # ... means required
    password: str = Form(...)
):
    return {"username": username}

# File upload
@app.post("/upload/")
async def upload_file(file: UploadFile = File(...)):
    contents = await file.read()
    return {
        "filename": file.filename,
        "content_type": file.content_type,
        "size": len(contents)
    }

# Multiple files
@app.post("/upload-multiple/")
async def upload_files(files: List[UploadFile] = File(...)):
    return [{"filename": f.filename} for f in files]

# File with form data
@app.post("/submit/")
async def submit_form(
    name: str = Form(...),
    file: UploadFile = File(...)
):
    return {
        "name": name,
        "filename": file.filename
    }
```

### 🔴 Advanced: Async Operations

```python
from fastapi import FastAPI
import asyncio
import httpx

app = FastAPI()

# Async endpoint
@app.get("/async-data")
async def get_async_data():
    await asyncio.sleep(1)  # Simulate async operation
    return {"data": "fetched asynchronously"}

# Parallel API calls
@app.get("/aggregate")
async def aggregate_data():
    async with httpx.AsyncClient() as client:
        # Parallel requests
        tasks = [
            client.get("https://api.example.com/data1"),
            client.get("https://api.example.com/data2"),
            client.get("https://api.example.com/data3")
        ]
        responses = await asyncio.gather(*tasks)
    
    return {
        "data1": responses[0].json(),
        "data2": responses[1].json(),
        "data3": responses[2].json()
    }

# Background tasks
from fastapi import BackgroundTasks

def send_email(email: str, message: str):
    # Simulated email sending
    print(f"Sending email to {email}: {message}")

@app.post("/send-notification/")
async def send_notification(
    email: str,
    background_tasks: BackgroundTasks
):
    background_tasks.add_task(send_email, email, "Welcome!")
    return {"message": "Notification scheduled"}
```

### 🔴 Advanced: Dependency Injection

```python
from fastapi import FastAPI, Depends, HTTPException, Header
from typing import Optional

app = FastAPI()

# Simple dependency
def get_query_params(q: Optional[str] = None, limit: int = 10):
    return {"q": q, "limit": limit}

@app.get("/items/")
def get_items(params: dict = Depends(get_query_params)):
    return params

# Authentication dependency
def get_current_user(token: str = Header(...)):
    if token != "valid-token":
        raise HTTPException(status_code=401, detail="Invalid token")
    return {"username": "john", "token": token}

@app.get("/protected/")
def protected_route(user: dict = Depends(get_current_user)):
    return {"message": f"Hello, {user['username']}"}

# Database dependency
class Database:
    def __init__(self):
        self.connected = False
    
    def connect(self):
        self.connected = True
        return self
    
    def disconnect(self):
        self.connected = False

def get_db():
    db = Database()
    db.connect()
    try:
        yield db  # Provide to endpoint
    finally:
        db.disconnect()  # Cleanup

@app.get("/db-items/")
def get_db_items(db: Database = Depends(get_db)):
    return {"connected": db.connected}

# Class-based dependencies
class CommonParams:
    def __init__(self, q: Optional[str] = None, skip: int = 0, limit: int = 100):
        self.q = q
        self.skip = skip
        self.limit = limit

@app.get("/search/")
def search(commons: CommonParams = Depends()):
    return {"q": commons.q, "skip": commons.skip, "limit": commons.limit}
```

### 🔴 Advanced: Authentication (JWT)

```python
from fastapi import FastAPI, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from pydantic import BaseModel
from jose import JWTError, jwt
from passlib.context import CryptContext
from datetime import datetime, timedelta

app = FastAPI()

# Security configuration
SECRET_KEY = "your-secret-key"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

# Models
class User(BaseModel):
    username: str
    email: str

class Token(BaseModel):
    access_token: str
    token_type: str

# Fake database
fake_users_db = {
    "john": {
        "username": "john",
        "email": "john@example.com",
        "hashed_password": pwd_context.hash("secret123")
    }
}

# Helper functions
def verify_password(plain_password, hashed_password):
    return pwd_context.verify(plain_password, hashed_password)

def create_access_token(data: dict):
    to_encode = data.copy()
    expire = datetime.utcnow() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username = payload.get("sub")
        if username is None:
            raise HTTPException(status_code=401, detail="Invalid token")
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid token")
    
    user = fake_users_db.get(username)
    if user is None:
        raise HTTPException(status_code=401, detail="User not found")
    return User(**user)

# Endpoints
@app.post("/token", response_model=Token)
def login(form_data: OAuth2PasswordRequestForm = Depends()):
    user = fake_users_db.get(form_data.username)
    if not user or not verify_password(form_data.password, user["hashed_password"]):
        raise HTTPException(status_code=401, detail="Incorrect credentials")
    
    access_token = create_access_token(data={"sub": user["username"]})
    return {"access_token": access_token, "token_type": "bearer"}

@app.get("/users/me", response_model=User)
def read_users_me(current_user: User = Depends(get_current_user)):
    return current_user
```

### 🔴 Advanced: Routers (Modular Apps)

```python
# users.py
from fastapi import APIRouter

router = APIRouter(
    prefix="/users",
    tags=["users"],
    responses={404: {"description": "Not found"}}
)

@router.get("/")
def list_users():
    return [{"id": 1, "name": "John"}]

@router.get("/{user_id}")
def get_user(user_id: int):
    return {"id": user_id, "name": f"User {user_id}"}

# products.py
from fastapi import APIRouter

router = APIRouter(
    prefix="/products",
    tags=["products"]
)

@router.get("/")
def list_products():
    return [{"id": 1, "name": "Phone"}]

# main.py
from fastapi import FastAPI
from users import router as users_router
from products import router as products_router

app = FastAPI(
    title="My API",
    description="A modern API",
    version="1.0.0"
)

app.include_router(users_router)
app.include_router(products_router)

@app.get("/")
def root():
    return {"message": "Welcome to the API"}

# Auto-generated docs at /docs and /redoc
# Tags organize endpoints in documentation
```

---

## 📊 Pydantic Validation

| Validator | Example |
|-----------|---------|
| Required | `name: str` |
| Optional | `name: Optional[str] = None` |
| Default | `age: int = 18` |
| Min/Max | `Field(ge=0, le=100)` |
| Length | `Field(min_length=1, max_length=50)` |
| Regex | `Field(regex="^[a-z]+$")` |
| Email | `email: EmailStr` |
| URL | `url: HttpUrl` |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Forgetting async/await

```python
# WRONG - Blocking call in async function
@app.get("/data")
async def get_data():
    response = requests.get(url)  # Blocks!
    return response.json()

# CORRECT
@app.get("/data")
async def get_data():
    async with httpx.AsyncClient() as client:
        response = await client.get(url)
    return response.json()
```

### ❌ Mistake 2: Not Using Response Models

```python
# WRONG - Exposes all fields including sensitive ones
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return users_db[user_id]  # May include password!

# CORRECT
class UserResponse(BaseModel):
    id: int
    name: str
    email: str

@app.get("/users/{user_id}", response_model=UserResponse)
def get_user(user_id: int):
    return users_db[user_id]  # Only safe fields returned
```

---

## 💡 Pro Tips

### 1. Custom Validation

```python
from pydantic import BaseModel, validator

class User(BaseModel):
    name: str
    age: int
    
    @validator('name')
    def name_must_not_be_empty(cls, v):
        if not v.strip():
            raise ValueError('Name cannot be empty')
        return v.title()
```

### 2. Testing FastAPI

```python
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_read_root():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"message": "Hello"}
```

---

## 💼 Interview Insight

### Q1: Why FastAPI over Flask?

> FastAPI provides automatic validation via Pydantic, native async support, and auto-generated OpenAPI documentation. It's also significantly faster due to Starlette.

### Q2: How does dependency injection work?

> FastAPI's `Depends()` injects dependencies into route functions. Dependencies can be functions, classes, or generators (for cleanup). They're resolved at runtime.

---

## 🧪 Practice Questions

### Easy:
1. Create a FastAPI app with GET and POST endpoints
2. Add path and query parameters
3. Use Pydantic models for request validation

### Medium:
4. Implement CRUD operations with proper status codes
5. Add authentication with JWT
6. Create modular routes with APIRouter

### Hard:
7. Build async endpoints with parallel API calls
8. Implement rate limiting with dependencies
9. Create a complete REST API with docs

---

## 📖 Summary

| Concept | Code |
|---------|------|
| Route | `@app.get("/path")` |
| Path param | `def func(id: int)` |
| Query param | `def func(q: str = None)` |
| Body | `def func(item: Model)` |
| Dependency | `Depends(function)` |
| Response model | `response_model=Model` |
| Status code | `status_code=201` |

---

## ⏭️ Next Module

**[Module 9: Database and APIs](../09_Database_and_APIs/README.md)** — Connect to databases and external services!

---

*FastAPI: Fast to code, fast to run!* ⚡

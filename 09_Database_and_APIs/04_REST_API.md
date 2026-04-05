# 📘 REST API Principles

## 📌 Introduction

**REST** (Representational State Transfer) is an architectural style for designing networked applications. It uses HTTP methods to perform operations on resources, making APIs intuitive and standardized.

Think of REST as a **common language** that allows different systems to communicate — like a universal translator for the web!

```
┌────────────────────────────────────────────────────────────────┐
│                      REST ARCHITECTURE                          │
│                                                                 │
│   Client                              Server                   │
│   ┌─────────┐                        ┌─────────┐              │
│   │ Browser │   HTTP Request         │   API   │              │
│   │   or    │ ─────────────────────► │ Server  │              │
│   │  App    │   GET /users/123       │         │              │
│   │         │ ◄───────────────────── │         │              │
│   └─────────┘   HTTP Response        └─────────┘              │
│                 JSON/XML                                        │
│                                                                 │
│   RESOURCE: /users/123                                         │
│   REPRESENTATION: { "id": 123, "name": "Alice" }              │
│   STATE: The current data of the resource                      │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### REST Principles

| Principle | Description |
|-----------|-------------|
| Stateless | Each request contains all needed information |
| Client-Server | Client and server are independent |
| Uniform Interface | Consistent URLs and methods |
| Resource-Based | Everything is a resource with a URL |
| Cacheable | Responses can be cached |
| Layered System | Can have intermediaries (proxies, gateways) |

### HTTP Methods

| Method | CRUD | Description | Safe | Idempotent |
|--------|------|-------------|------|------------|
| GET | Read | Retrieve resource | ✅ | ✅ |
| POST | Create | Create new resource | ❌ | ❌ |
| PUT | Update | Replace entire resource | ❌ | ✅ |
| PATCH | Update | Partial update | ❌ | ❌ |
| DELETE | Delete | Remove resource | ❌ | ✅ |

---

## 💻 Code Examples

### 🟢 Basic: RESTful URL Design

```python
# RESOURCE: Users
# Collection: /users
# Item: /users/{id}

# Good URL design (noun-based, hierarchical)
GET    /users              # List all users
GET    /users/123          # Get user 123
POST   /users              # Create new user
PUT    /users/123          # Replace user 123
PATCH  /users/123          # Update user 123
DELETE /users/123          # Delete user 123

# Nested resources
GET    /users/123/orders          # User's orders
GET    /users/123/orders/456      # Specific order
POST   /users/123/orders          # Create order for user

# BAD URL design (avoid)
GET    /getUsers                   # Verb in URL
POST   /createUser                 # Action in URL
GET    /users/delete/123           # Wrong method
GET    /getAllUsersFromDatabase    # Too verbose
```

### 🟢 Basic: Flask REST API

```python
from flask import Flask, jsonify, request, abort

app = Flask(__name__)

# In-memory database
users = {
    1: {"id": 1, "name": "Alice", "email": "alice@example.com"},
    2: {"id": 2, "name": "Bob", "email": "bob@example.com"}
}
next_id = 3

# GET all users
@app.route('/api/users', methods=['GET'])
def get_users():
    return jsonify(list(users.values()))

# GET single user
@app.route('/api/users/<int:user_id>', methods=['GET'])
def get_user(user_id):
    if user_id not in users:
        abort(404)
    return jsonify(users[user_id])

# POST - Create user
@app.route('/api/users', methods=['POST'])
def create_user():
    global next_id
    
    if not request.json or 'name' not in request.json:
        abort(400)
    
    user = {
        "id": next_id,
        "name": request.json['name'],
        "email": request.json.get('email', '')
    }
    users[next_id] = user
    next_id += 1
    
    return jsonify(user), 201

# PUT - Replace user
@app.route('/api/users/<int:user_id>', methods=['PUT'])
def update_user(user_id):
    if user_id not in users:
        abort(404)
    if not request.json:
        abort(400)
    
    users[user_id] = {
        "id": user_id,
        "name": request.json.get('name', ''),
        "email": request.json.get('email', '')
    }
    return jsonify(users[user_id])

# PATCH - Partial update
@app.route('/api/users/<int:user_id>', methods=['PATCH'])
def patch_user(user_id):
    if user_id not in users:
        abort(404)
    
    user = users[user_id]
    if 'name' in request.json:
        user['name'] = request.json['name']
    if 'email' in request.json:
        user['email'] = request.json['email']
    
    return jsonify(user)

# DELETE
@app.route('/api/users/<int:user_id>', methods=['DELETE'])
def delete_user(user_id):
    if user_id not in users:
        abort(404)
    del users[user_id]
    return '', 204

if __name__ == '__main__':
    app.run(debug=True)
```

### 🟡 Intermediate: HTTP Status Codes

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

# 2xx Success
@app.route('/api/resource', methods=['GET'])
def get_resource():
    return jsonify({"data": "value"}), 200  # OK

@app.route('/api/resource', methods=['POST'])
def create_resource():
    # Resource created
    return jsonify({"id": 1}), 201  # Created

@app.route('/api/resource', methods=['DELETE'])
def delete_resource():
    # No content to return
    return '', 204  # No Content

# 4xx Client Errors
@app.route('/api/item/<int:item_id>')
def get_item(item_id):
    if item_id < 0:
        return jsonify({"error": "Invalid ID"}), 400  # Bad Request
    
    if not is_authenticated():
        return jsonify({"error": "Login required"}), 401  # Unauthorized
    
    if not has_permission():
        return jsonify({"error": "Access denied"}), 403  # Forbidden
    
    if not item_exists(item_id):
        return jsonify({"error": "Item not found"}), 404  # Not Found
    
    return jsonify({"id": item_id})

# 5xx Server Errors
@app.route('/api/process')
def process():
    try:
        result = some_operation()
        return jsonify(result)
    except Exception as e:
        return jsonify({"error": "Server error"}), 500  # Internal Error

# Status code reference
"""
200 OK           - Request succeeded
201 Created      - Resource created
204 No Content   - Success with no body
400 Bad Request  - Invalid request
401 Unauthorized - Authentication required
403 Forbidden    - Permission denied
404 Not Found    - Resource doesn't exist
409 Conflict     - Resource conflict
422 Unprocessable - Validation error
500 Internal Error - Server error
503 Service Unavailable - Temporarily down
"""
```

### 🟡 Intermediate: Query Parameters and Filtering

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

products = [
    {"id": 1, "name": "Laptop", "category": "electronics", "price": 999},
    {"id": 2, "name": "Phone", "category": "electronics", "price": 699},
    {"id": 3, "name": "Desk", "category": "furniture", "price": 299},
    {"id": 4, "name": "Chair", "category": "furniture", "price": 199},
]

@app.route('/api/products', methods=['GET'])
def get_products():
    """
    GET /api/products
    GET /api/products?category=electronics
    GET /api/products?min_price=200&max_price=800
    GET /api/products?sort=price&order=desc
    GET /api/products?page=1&limit=10
    GET /api/products?search=phone
    """
    result = products.copy()
    
    # Filter by category
    category = request.args.get('category')
    if category:
        result = [p for p in result if p['category'] == category]
    
    # Filter by price range
    min_price = request.args.get('min_price', type=float)
    max_price = request.args.get('max_price', type=float)
    if min_price is not None:
        result = [p for p in result if p['price'] >= min_price]
    if max_price is not None:
        result = [p for p in result if p['price'] <= max_price]
    
    # Search
    search = request.args.get('search', '').lower()
    if search:
        result = [p for p in result if search in p['name'].lower()]
    
    # Sort
    sort_by = request.args.get('sort')
    order = request.args.get('order', 'asc')
    if sort_by and sort_by in ['name', 'price']:
        result.sort(key=lambda x: x[sort_by], reverse=(order == 'desc'))
    
    # Pagination
    page = request.args.get('page', 1, type=int)
    limit = request.args.get('limit', 10, type=int)
    start = (page - 1) * limit
    end = start + limit
    
    return jsonify({
        "data": result[start:end],
        "total": len(result),
        "page": page,
        "limit": limit,
        "pages": (len(result) + limit - 1) // limit
    })

if __name__ == '__main__':
    app.run(debug=True)
```

### 🟡 Intermediate: Request/Response Headers

```python
from flask import Flask, jsonify, request, make_response

app = Flask(__name__)

@app.route('/api/data', methods=['GET'])
def get_data():
    # Read request headers
    auth_token = request.headers.get('Authorization')
    content_type = request.headers.get('Content-Type')
    user_agent = request.headers.get('User-Agent')
    
    # Create response with custom headers
    response = make_response(jsonify({"data": "value"}))
    
    # Common response headers
    response.headers['Content-Type'] = 'application/json'
    response.headers['Cache-Control'] = 'max-age=3600'
    response.headers['X-Request-ID'] = 'abc123'
    response.headers['X-RateLimit-Limit'] = '100'
    response.headers['X-RateLimit-Remaining'] = '99'
    
    return response

# CORS headers (Cross-Origin Resource Sharing)
@app.after_request
def add_cors_headers(response):
    response.headers['Access-Control-Allow-Origin'] = '*'
    response.headers['Access-Control-Allow-Methods'] = 'GET, POST, PUT, DELETE'
    response.headers['Access-Control-Allow-Headers'] = 'Content-Type, Authorization'
    return response

# Content negotiation
@app.route('/api/resource')
def get_resource():
    accept = request.headers.get('Accept', 'application/json')
    
    data = {"id": 1, "name": "Example"}
    
    if 'application/xml' in accept:
        xml = f'<resource><id>1</id><name>Example</name></resource>'
        response = make_response(xml)
        response.headers['Content-Type'] = 'application/xml'
        return response
    
    return jsonify(data)
```

### 🔴 Advanced: FastAPI REST Implementation

```python
from fastapi import FastAPI, HTTPException, Query, Path, Depends, status
from pydantic import BaseModel, Field, EmailStr
from typing import Optional, List
from datetime import datetime

app = FastAPI(
    title="Users API",
    description="REST API for user management",
    version="1.0.0"
)

# Pydantic models
class UserCreate(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    email: EmailStr
    age: Optional[int] = Field(None, ge=0, le=150)

class UserUpdate(BaseModel):
    name: Optional[str] = None
    email: Optional[EmailStr] = None
    age: Optional[int] = None

class UserResponse(BaseModel):
    id: int
    name: str
    email: str
    age: Optional[int]
    created_at: datetime
    
    class Config:
        orm_mode = True

class PaginatedResponse(BaseModel):
    data: List[UserResponse]
    total: int
    page: int
    limit: int
    pages: int

# In-memory storage
users_db = {}
next_id = 1

# CRUD endpoints
@app.get("/api/users", response_model=PaginatedResponse)
def list_users(
    page: int = Query(1, ge=1),
    limit: int = Query(10, ge=1, le=100),
    search: Optional[str] = None,
    sort_by: Optional[str] = Query(None, regex="^(name|email|created_at)$"),
    order: Optional[str] = Query("asc", regex="^(asc|desc)$")
):
    """Get all users with pagination, search, and sorting"""
    result = list(users_db.values())
    
    # Search
    if search:
        result = [u for u in result if search.lower() in u['name'].lower()]
    
    # Sort
    if sort_by:
        result.sort(key=lambda x: x[sort_by], reverse=(order == "desc"))
    
    total = len(result)
    start = (page - 1) * limit
    result = result[start:start + limit]
    
    return {
        "data": result,
        "total": total,
        "page": page,
        "limit": limit,
        "pages": (total + limit - 1) // limit
    }

@app.get("/api/users/{user_id}", response_model=UserResponse)
def get_user(user_id: int = Path(..., ge=1)):
    """Get user by ID"""
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    return users_db[user_id]

@app.post("/api/users", response_model=UserResponse, status_code=status.HTTP_201_CREATED)
def create_user(user: UserCreate):
    """Create a new user"""
    global next_id
    
    # Check email uniqueness
    if any(u['email'] == user.email for u in users_db.values()):
        raise HTTPException(status_code=409, detail="Email already exists")
    
    new_user = {
        "id": next_id,
        "name": user.name,
        "email": user.email,
        "age": user.age,
        "created_at": datetime.now()
    }
    users_db[next_id] = new_user
    next_id += 1
    
    return new_user

@app.put("/api/users/{user_id}", response_model=UserResponse)
def replace_user(user_id: int, user: UserCreate):
    """Replace entire user"""
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    
    users_db[user_id] = {
        "id": user_id,
        "name": user.name,
        "email": user.email,
        "age": user.age,
        "created_at": users_db[user_id]['created_at']
    }
    return users_db[user_id]

@app.patch("/api/users/{user_id}", response_model=UserResponse)
def update_user(user_id: int, user: UserUpdate):
    """Partial update user"""
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    
    stored_user = users_db[user_id]
    update_data = user.dict(exclude_unset=True)
    
    for field, value in update_data.items():
        stored_user[field] = value
    
    return stored_user

@app.delete("/api/users/{user_id}", status_code=status.HTTP_204_NO_CONTENT)
def delete_user(user_id: int):
    """Delete user"""
    if user_id not in users_db:
        raise HTTPException(status_code=404, detail="User not found")
    del users_db[user_id]
    return None
```

### 🔴 Advanced: Error Handling

```python
from flask import Flask, jsonify
from werkzeug.exceptions import HTTPException

app = Flask(__name__)

# Custom error class
class APIError(Exception):
    def __init__(self, message, status_code=400, payload=None):
        super().__init__()
        self.message = message
        self.status_code = status_code
        self.payload = payload
    
    def to_dict(self):
        rv = dict(self.payload or ())
        rv['error'] = {
            'message': self.message,
            'status': self.status_code
        }
        return rv

# Register error handler
@app.errorhandler(APIError)
def handle_api_error(error):
    response = jsonify(error.to_dict())
    response.status_code = error.status_code
    return response

@app.errorhandler(HTTPException)
def handle_http_error(error):
    return jsonify({
        'error': {
            'message': error.description,
            'status': error.code
        }
    }), error.code

@app.errorhandler(Exception)
def handle_generic_error(error):
    return jsonify({
        'error': {
            'message': 'Internal server error',
            'status': 500
        }
    }), 500

# Usage
@app.route('/api/users/<int:user_id>')
def get_user(user_id):
    user = find_user(user_id)
    if not user:
        raise APIError('User not found', status_code=404)
    if not user.is_active:
        raise APIError('User is deactivated', status_code=410, 
                      payload={'user_id': user_id})
    return jsonify(user.to_dict())
```

### 🔴 Advanced: API Versioning

```python
from flask import Flask, Blueprint, jsonify

app = Flask(__name__)

# Version 1
v1 = Blueprint('v1', __name__, url_prefix='/api/v1')

@v1.route('/users')
def get_users_v1():
    return jsonify({
        "users": [{"id": 1, "name": "Alice"}]
    })

# Version 2 (with different structure)
v2 = Blueprint('v2', __name__, url_prefix='/api/v2')

@v2.route('/users')
def get_users_v2():
    return jsonify({
        "data": [{"id": 1, "name": "Alice", "email": "alice@example.com"}],
        "meta": {"total": 1, "version": "2.0"}
    })

app.register_blueprint(v1)
app.register_blueprint(v2)

# Alternative: Header-based versioning
@app.route('/api/users')
def get_users():
    version = request.headers.get('API-Version', '1')
    
    if version == '1':
        return jsonify({"users": [...]})
    elif version == '2':
        return jsonify({"data": [...], "meta": {...}})
    else:
        return jsonify({"error": "Unsupported version"}), 400

# URLs:
# /api/v1/users - Version 1
# /api/v2/users - Version 2
```

---

## 📊 REST Best Practices

| Practice | Good | Bad |
|----------|------|-----|
| URLs | `/users/123` | `/getUser?id=123` |
| Methods | GET for read | POST for read |
| Status | 201 for create | 200 for everything |
| Naming | Nouns (users) | Verbs (getUsers) |
| Plurals | `/users` | `/user` |
| Nesting | `/users/123/orders` | `/userOrders?uid=123` |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Using GET for State Changes

```python
# WRONG
@app.route('/api/users/delete/<int:id>')
def delete_user(id):  # Using GET!
    ...

# CORRECT
@app.route('/api/users/<int:id>', methods=['DELETE'])
def delete_user(id):
    ...
```

### ❌ Mistake 2: Not Using Proper Status Codes

```python
# WRONG
return jsonify({"success": False, "error": "Not found"}), 200

# CORRECT
return jsonify({"error": "User not found"}), 404
```

---

## 💡 Pro Tips

### 1. Use HATEOAS for Discoverability

```python
@app.route('/api/users/<int:id>')
def get_user(id):
    return jsonify({
        "id": id,
        "name": "Alice",
        "links": {
            "self": f"/api/users/{id}",
            "orders": f"/api/users/{id}/orders",
            "collection": "/api/users"
        }
    })
```

### 2. Implement Rate Limiting

```python
# Return rate limit info in headers
response.headers['X-RateLimit-Limit'] = '100'
response.headers['X-RateLimit-Remaining'] = '95'
response.headers['X-RateLimit-Reset'] = '1609459200'
```

---

## 💼 Interview Insight

### Q1: What makes an API RESTful?

> RESTful APIs follow REST principles: stateless, uniform interface (consistent URLs/methods), resource-based (nouns not verbs), use proper HTTP methods and status codes.

### Q2: PUT vs PATCH?

> PUT replaces the entire resource and requires all fields. PATCH partially updates only provided fields. Use PUT for complete replacement, PATCH for partial updates.

---

## 🧪 Practice Questions

### Easy:
1. Design URL structure for a blog API
2. Map CRUD operations to HTTP methods
3. Choose correct status codes for scenarios

### Medium:
4. Implement pagination for list endpoints
5. Add filtering and sorting to GET requests
6. Create error handling middleware

### Hard:
7. Design API versioning strategy
8. Implement HATEOAS in your API
9. Build a rate-limited API with proper headers

---

## 📖 Summary

| Method | URL | Action | Status |
|--------|-----|--------|--------|
| GET | /resources | List all | 200 |
| GET | /resources/:id | Get one | 200/404 |
| POST | /resources | Create | 201 |
| PUT | /resources/:id | Replace | 200/404 |
| PATCH | /resources/:id | Update | 200/404 |
| DELETE | /resources/:id | Delete | 204/404 |

---

## ⏭️ Next Topic

**[Requests Library](05_Requests_Library.md)** — Consume APIs in Python!

---

*REST: The universal language of web services!* 🌐

# 📘 Flask - Micro Web Framework

## 📌 Introduction

**Flask** is a lightweight, flexible web framework for Python. It's called a "micro" framework because it doesn't include ORM, form validation, or other features by default — you add only what you need.

Think of Flask as a **LEGO set** — it gives you basic building blocks, and you construct exactly what you want!

```
┌────────────────────────────────────────────────────────────────┐
│                    FLASK REQUEST FLOW                           │
│                                                                 │
│   Browser                                                       │
│      │                                                          │
│      ▼                                                          │
│   ┌──────────────────┐                                         │
│   │   HTTP Request   │  GET /users                             │
│   └────────┬─────────┘                                         │
│            ▼                                                    │
│   ┌──────────────────┐                                         │
│   │  Flask Router    │  @app.route('/users')                   │
│   └────────┬─────────┘                                         │
│            ▼                                                    │
│   ┌──────────────────┐                                         │
│   │  View Function   │  def get_users():                       │
│   └────────┬─────────┘                                         │
│            ▼                                                    │
│   ┌──────────────────┐                                         │
│   │  HTTP Response   │  JSON / HTML / Template                 │
│   └──────────────────┘                                         │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### Flask vs Django

| Feature | Flask | Django |
|---------|-------|--------|
| Type | Micro | Full-stack |
| Learning | Easy | Steeper |
| Flexibility | High | Opinionated |
| Built-in | Minimal | Everything |
| Best For | APIs, small apps | Large apps |

### Installation

```bash
pip install flask
```

---

## 💻 Code Examples

### 🟢 Basic: Hello World

```python
from flask import Flask

# Create Flask app
app = Flask(__name__)

# Define route
@app.route('/')
def home():
    return 'Hello, World!'

# Run the app
if __name__ == '__main__':
    app.run(debug=True)

# Output: Server runs at http://127.0.0.1:5000/
# Visit in browser to see "Hello, World!"
```

### 🟢 Basic: Multiple Routes

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return '<h1>Welcome Home</h1>'

@app.route('/about')
def about():
    return '<h1>About Page</h1>'

@app.route('/contact')
def contact():
    return '<h1>Contact Page</h1>'

# Multiple routes for same function
@app.route('/hello')
@app.route('/hi')
def greet():
    return 'Hello there!'

if __name__ == '__main__':
    app.run(debug=True)
```

### 🟢 Basic: Dynamic Routes

```python
from flask import Flask

app = Flask(__name__)

# URL parameters
@app.route('/user/<username>')
def show_user(username):
    return f'User: {username}'

# Type converters
@app.route('/post/<int:post_id>')
def show_post(post_id):
    return f'Post ID: {post_id}'

@app.route('/price/<float:amount>')
def show_price(amount):
    return f'Price: ${amount:.2f}'

# Multiple parameters
@app.route('/user/<username>/post/<int:post_id>')
def user_post(username, post_id):
    return f'{username}\'s post #{post_id}'

# Converters:
# string (default): any text without slash
# int: positive integers
# float: positive floating point values
# path: like string but includes slashes
# uuid: UUID strings

if __name__ == '__main__':
    app.run(debug=True)
```

### 🟡 Intermediate: HTTP Methods

```python
from flask import Flask, request

app = Flask(__name__)

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        return f'Logging in {username}'
    else:
        return '''
            <form method="post">
                <input name="username" placeholder="Username">
                <input name="password" type="password" placeholder="Password">
                <button type="submit">Login</button>
            </form>
        '''

# RESTful routes
@app.route('/api/users', methods=['GET'])
def get_users():
    return {'users': ['Alice', 'Bob', 'Charlie']}

@app.route('/api/users', methods=['POST'])
def create_user():
    data = request.json
    return {'message': f'Created user {data["name"]}'}

@app.route('/api/users/<int:user_id>', methods=['PUT'])
def update_user(user_id):
    data = request.json
    return {'message': f'Updated user {user_id}'}

@app.route('/api/users/<int:user_id>', methods=['DELETE'])
def delete_user(user_id):
    return {'message': f'Deleted user {user_id}'}

if __name__ == '__main__':
    app.run(debug=True)
```

### 🟡 Intermediate: Request Data

```python
from flask import Flask, request

app = Flask(__name__)

@app.route('/data', methods=['POST'])
def handle_data():
    # Form data
    name = request.form.get('name')
    
    # Query parameters (?key=value)
    page = request.args.get('page', 1, type=int)
    
    # JSON data
    json_data = request.json
    
    # Headers
    user_agent = request.headers.get('User-Agent')
    
    # Cookies
    session_id = request.cookies.get('session_id')
    
    # Files
    if 'file' in request.files:
        file = request.files['file']
        file.save(f'uploads/{file.filename}')
    
    return {
        'name': name,
        'page': page,
        'json': json_data,
        'user_agent': user_agent
    }

# Check request properties
@app.route('/info')
def request_info():
    return {
        'method': request.method,
        'url': request.url,
        'path': request.path,
        'host': request.host,
        'remote_addr': request.remote_addr
    }

if __name__ == '__main__':
    app.run(debug=True)
```

### 🟡 Intermediate: Templates (Jinja2)

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html', title='Home')

@app.route('/users')
def users():
    users = [
        {'name': 'Alice', 'age': 25},
        {'name': 'Bob', 'age': 30},
        {'name': 'Charlie', 'age': 35}
    ]
    return render_template('users.html', users=users)

if __name__ == '__main__':
    app.run(debug=True)
```

**templates/index.html:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>{{ title }}</title>
</head>
<body>
    <h1>Welcome to {{ title }}</h1>
</body>
</html>
```

**templates/users.html:**
```html
<!DOCTYPE html>
<html>
<head>
    <title>Users</title>
</head>
<body>
    <h1>User List</h1>
    <ul>
    {% for user in users %}
        <li>{{ user.name }} ({{ user.age }} years old)</li>
    {% endfor %}
    </ul>
    
    {% if users %}
        <p>Total: {{ users|length }} users</p>
    {% else %}
        <p>No users found.</p>
    {% endif %}
</body>
</html>
```

**templates/base.html (Template Inheritance):**
```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}Default{% endblock %}</title>
</head>
<body>
    <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
    </nav>
    
    <main>
        {% block content %}{% endblock %}
    </main>
    
    <footer>© 2024</footer>
</body>
</html>
```

**templates/page.html:**
```html
{% extends 'base.html' %}

{% block title %}My Page{% endblock %}

{% block content %}
    <h1>Page Content</h1>
    <p>This extends the base template.</p>
{% endblock %}
```

### 🟡 Intermediate: Responses and Redirects

```python
from flask import Flask, redirect, url_for, jsonify, make_response, abort

app = Flask(__name__)

# Redirect
@app.route('/old-page')
def old_page():
    return redirect(url_for('new_page'))

@app.route('/new-page')
def new_page():
    return 'This is the new page'

# JSON response
@app.route('/api/data')
def api_data():
    return jsonify({
        'status': 'success',
        'data': [1, 2, 3]
    })

# Custom response
@app.route('/custom')
def custom_response():
    response = make_response('Custom response')
    response.headers['X-Custom-Header'] = 'Hello'
    response.set_cookie('user', 'John')
    return response

# Error handling
@app.route('/protected')
def protected():
    is_authorized = False
    if not is_authorized:
        abort(403)  # Forbidden
    return 'Secret data'

# Custom error pages
@app.errorhandler(404)
def page_not_found(e):
    return render_template('404.html'), 404

@app.errorhandler(500)
def internal_error(e):
    return 'Internal Server Error', 500

if __name__ == '__main__':
    app.run(debug=True)
```

### 🔴 Advanced: Sessions and Authentication

```python
from flask import Flask, session, redirect, url_for, request
from functools import wraps

app = Flask(__name__)
app.secret_key = 'your-secret-key-here'  # Required for sessions

# Login required decorator
def login_required(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if 'user' not in session:
            return redirect(url_for('login'))
        return f(*args, **kwargs)
    return decorated_function

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form['username']
        # In real app, verify password here
        session['user'] = username
        return redirect(url_for('dashboard'))
    return '''
        <form method="post">
            <input name="username" placeholder="Username">
            <button type="submit">Login</button>
        </form>
    '''

@app.route('/logout')
def logout():
    session.pop('user', None)
    return redirect(url_for('login'))

@app.route('/dashboard')
@login_required
def dashboard():
    return f'Welcome, {session["user"]}!'

# Session configuration
app.config['SESSION_COOKIE_SECURE'] = True  # HTTPS only
app.config['SESSION_COOKIE_HTTPONLY'] = True  # No JavaScript access
app.config['PERMANENT_SESSION_LIFETIME'] = 3600  # 1 hour

if __name__ == '__main__':
    app.run(debug=True)
```

### 🔴 Advanced: Blueprints (Modular Apps)

```python
# users.py (Blueprint)
from flask import Blueprint, jsonify

users_bp = Blueprint('users', __name__, url_prefix='/users')

@users_bp.route('/')
def list_users():
    return jsonify(['Alice', 'Bob', 'Charlie'])

@users_bp.route('/<int:user_id>')
def get_user(user_id):
    return jsonify({'id': user_id, 'name': f'User {user_id}'})

# products.py (Blueprint)
from flask import Blueprint, jsonify

products_bp = Blueprint('products', __name__, url_prefix='/products')

@products_bp.route('/')
def list_products():
    return jsonify(['Phone', 'Laptop', 'Tablet'])

# app.py (Main app)
from flask import Flask
from users import users_bp
from products import products_bp

app = Flask(__name__)

# Register blueprints
app.register_blueprint(users_bp)
app.register_blueprint(products_bp)

@app.route('/')
def home():
    return 'Welcome to the API'

if __name__ == '__main__':
    app.run(debug=True)

# Routes:
# GET /              -> home()
# GET /users/        -> list_users()
# GET /users/1       -> get_user(1)
# GET /products/     -> list_products()
```

### 🔴 Advanced: Database Integration

```python
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)

# Define model
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    
    def to_dict(self):
        return {
            'id': self.id,
            'username': self.username,
            'email': self.email
        }

# Create tables
with app.app_context():
    db.create_all()

# CRUD endpoints
@app.route('/users', methods=['GET'])
def get_users():
    users = User.query.all()
    return jsonify([u.to_dict() for u in users])

@app.route('/users', methods=['POST'])
def create_user():
    data = request.json
    user = User(username=data['username'], email=data['email'])
    db.session.add(user)
    db.session.commit()
    return jsonify(user.to_dict()), 201

@app.route('/users/<int:user_id>', methods=['GET'])
def get_user(user_id):
    user = User.query.get_or_404(user_id)
    return jsonify(user.to_dict())

@app.route('/users/<int:user_id>', methods=['PUT'])
def update_user(user_id):
    user = User.query.get_or_404(user_id)
    data = request.json
    user.username = data.get('username', user.username)
    user.email = data.get('email', user.email)
    db.session.commit()
    return jsonify(user.to_dict())

@app.route('/users/<int:user_id>', methods=['DELETE'])
def delete_user(user_id):
    user = User.query.get_or_404(user_id)
    db.session.delete(user)
    db.session.commit()
    return '', 204

if __name__ == '__main__':
    app.run(debug=True)
```

---

## 📁 Project Structure

```
my_flask_app/
├── app/
│   ├── __init__.py
│   ├── routes.py
│   ├── models.py
│   ├── templates/
│   │   ├── base.html
│   │   ├── index.html
│   │   └── users.html
│   └── static/
│       ├── css/
│       └── js/
├── config.py
├── requirements.txt
└── run.py
```

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Hardcoded Secret Key

```python
# WRONG
app.secret_key = 'my-secret-key'

# CORRECT
import os
app.secret_key = os.environ.get('SECRET_KEY')
```

### ❌ Mistake 2: Debug Mode in Production

```python
# WRONG
app.run(debug=True)  # Never in production!

# CORRECT
if __name__ == '__main__':
    debug = os.environ.get('FLASK_DEBUG', False)
    app.run(debug=debug)
```

---

## 💡 Pro Tips

### 1. Use Application Factory

```python
def create_app(config_name):
    app = Flask(__name__)
    app.config.from_object(config[config_name])
    
    db.init_app(app)
    
    from .routes import main_bp
    app.register_blueprint(main_bp)
    
    return app
```

### 2. Environment Configuration

```python
# config.py
class Config:
    SECRET_KEY = os.environ.get('SECRET_KEY')
    SQLALCHEMY_TRACK_MODIFICATIONS = False

class DevelopmentConfig(Config):
    DEBUG = True
    SQLALCHEMY_DATABASE_URI = 'sqlite:///dev.db'

class ProductionConfig(Config):
    DEBUG = False
    SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL')
```

---

## 💼 Interview Insight

### Q1: What is Flask's application context?

> Application context provides access to app-level data (like config) without passing the app object around. It's created automatically during request handling.

### Q2: How do you handle CORS in Flask?

> Use flask-cors extension: `from flask_cors import CORS; CORS(app)` allows cross-origin requests.

---

## 🧪 Practice Questions

### Easy:
1. Create a "Hello World" Flask app
2. Add multiple routes with different HTTP methods
3. Return JSON data from an endpoint

### Medium:
4. Build a simple form with POST handling
5. Use templates with Jinja2
6. Implement user authentication

### Hard:
7. Build a complete CRUD API
8. Add database integration with SQLAlchemy
9. Deploy to production server

---

## 📖 Summary

| Concept | Code |
|---------|------|
| Route | `@app.route('/path')` |
| Methods | `methods=['GET', 'POST']` |
| URL params | `<variable>`, `<int:id>` |
| Request data | `request.form`, `request.json` |
| Response | `jsonify()`, `make_response()` |
| Redirect | `redirect(url_for('func'))` |
| Template | `render_template('file.html')` |

---

## ⏭️ Next Topic

**[FastAPI - Modern API Framework](05_FastAPI.md)** — Build faster APIs!

---

*Flask: Simple, flexible, powerful!* 🧪

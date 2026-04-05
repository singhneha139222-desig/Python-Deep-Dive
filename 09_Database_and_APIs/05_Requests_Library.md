# 📘 Requests Library - HTTP Client

## 📌 Introduction

The **requests** library is Python's de facto standard for making HTTP requests. It's simple, elegant, and powerful — making API calls as easy as writing Python code.

Think of it as **Python's web browser** — it can fetch pages, submit forms, download files, and interact with APIs!

```
┌────────────────────────────────────────────────────────────────┐
│                    REQUESTS WORKFLOW                            │
│                                                                 │
│   Your Python Script                                           │
│         │                                                       │
│         │  requests.get(url)                                   │
│         ▼                                                       │
│   ┌─────────────────┐                                          │
│   │   HTTP Request  │  GET /api/users HTTP/1.1                │
│   │                 │  Host: api.example.com                   │
│   │                 │  Authorization: Bearer xxx               │
│   └────────┬────────┘                                          │
│            │                                                    │
│            ▼                                                    │
│   ┌─────────────────┐                                          │
│   │  API Server     │                                          │
│   └────────┬────────┘                                          │
│            │                                                    │
│            ▼                                                    │
│   ┌─────────────────┐                                          │
│   │ HTTP Response   │  200 OK                                  │
│   │                 │  {"users": [...]}                        │
│   └────────┬────────┘                                          │
│            │                                                    │
│            ▼                                                    │
│   response.json()  →  Python dict                              │
└────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Core Concepts

### HTTP Methods in Requests

| Method | Function | Use Case |
|--------|----------|----------|
| GET | `requests.get()` | Fetch data |
| POST | `requests.post()` | Send data |
| PUT | `requests.put()` | Replace resource |
| PATCH | `requests.patch()` | Update resource |
| DELETE | `requests.delete()` | Remove resource |

### Installation

```bash
pip install requests
```

---

## 💻 Code Examples

### 🟢 Basic: GET Requests

```python
import requests

# Simple GET request
response = requests.get('https://api.github.com')
print(response.status_code)  # 200
print(response.text)         # Raw response body

# Response properties
print(response.status_code)  # HTTP status code
print(response.headers)      # Response headers
print(response.url)          # Final URL (after redirects)
print(response.encoding)     # Character encoding
print(response.elapsed)      # Time taken

# Parse JSON response
response = requests.get('https://api.github.com/users/octocat')
data = response.json()  # Convert to Python dict
print(data['login'])    # 'octocat'
print(data['name'])     # 'The Octocat'

# Check if request succeeded
if response.ok:  # True if status < 400
    print("Success!")

if response.status_code == 200:
    print("OK!")
```

### 🟢 Basic: Query Parameters

```python
import requests

# Using params argument
params = {
    'q': 'python',
    'sort': 'stars',
    'order': 'desc'
}
response = requests.get(
    'https://api.github.com/search/repositories',
    params=params
)

# URL becomes: https://api.github.com/search/repositories?q=python&sort=stars&order=desc
print(response.url)

# List values (multiple values for same key)
params = {
    'tags': ['python', 'api', 'tutorial']
}
# Becomes: ?tags=python&tags=api&tags=tutorial

# Manual URL construction (less safe)
response = requests.get('https://api.example.com/search?q=python&page=1')
```

### 🟢 Basic: POST Requests

```python
import requests

# Send form data
data = {
    'username': 'john',
    'password': 'secret123'
}
response = requests.post(
    'https://httpbin.org/post',
    data=data  # Form-encoded
)

# Send JSON data
import json
json_data = {
    'name': 'Alice',
    'email': 'alice@example.com'
}
response = requests.post(
    'https://api.example.com/users',
    json=json_data  # Automatically sets Content-Type
)

# Using json parameter is equivalent to:
response = requests.post(
    'https://api.example.com/users',
    data=json.dumps(json_data),
    headers={'Content-Type': 'application/json'}
)

# Check response
if response.status_code == 201:
    created_user = response.json()
    print(f"Created user with ID: {created_user['id']}")
```

### 🟡 Intermediate: Headers and Authentication

```python
import requests

# Custom headers
headers = {
    'User-Agent': 'MyApp/1.0',
    'Accept': 'application/json',
    'Authorization': 'Bearer your_token_here'
}
response = requests.get(
    'https://api.example.com/data',
    headers=headers
)

# Basic authentication
response = requests.get(
    'https://api.example.com/private',
    auth=('username', 'password')
)

# Bearer token authentication
token = 'your_jwt_token'
headers = {'Authorization': f'Bearer {token}'}
response = requests.get(
    'https://api.example.com/protected',
    headers=headers
)

# API key in header
headers = {'X-API-Key': 'your_api_key'}
response = requests.get(
    'https://api.example.com/data',
    headers=headers
)

# API key as query parameter
response = requests.get(
    'https://api.example.com/data',
    params={'api_key': 'your_api_key'}
)
```

### 🟡 Intermediate: Error Handling

```python
import requests
from requests.exceptions import (
    RequestException, Timeout, ConnectionError,
    HTTPError, TooManyRedirects
)

def fetch_data(url):
    try:
        response = requests.get(url, timeout=10)
        
        # Raise exception for 4xx/5xx status codes
        response.raise_for_status()
        
        return response.json()
    
    except Timeout:
        print("Request timed out")
        return None
    
    except ConnectionError:
        print("Failed to connect to server")
        return None
    
    except HTTPError as e:
        print(f"HTTP error: {e.response.status_code}")
        if e.response.status_code == 404:
            print("Resource not found")
        elif e.response.status_code == 401:
            print("Authentication required")
        elif e.response.status_code >= 500:
            print("Server error")
        return None
    
    except TooManyRedirects:
        print("Too many redirects")
        return None
    
    except RequestException as e:
        print(f"Request failed: {e}")
        return None

# Usage
data = fetch_data('https://api.example.com/data')
if data:
    print(data)
```

### 🟡 Intermediate: Sessions

```python
import requests

# Session maintains cookies and connection pooling
session = requests.Session()

# Set default headers for all requests
session.headers.update({
    'User-Agent': 'MyApp/1.0',
    'Accept': 'application/json'
})

# Login (session stores cookies)
login_data = {'username': 'john', 'password': 'secret'}
response = session.post('https://api.example.com/login', data=login_data)

# Subsequent requests include session cookies
response = session.get('https://api.example.com/profile')
profile = response.json()

# All requests use same connection pool (faster)
for i in range(10):
    response = session.get(f'https://api.example.com/items/{i}')

# Close session when done
session.close()

# Better: use as context manager
with requests.Session() as session:
    session.auth = ('user', 'pass')
    response = session.get('https://api.example.com/data')
```

### 🟡 Intermediate: File Upload and Download

```python
import requests

# Upload file
with open('document.pdf', 'rb') as f:
    files = {'file': f}
    response = requests.post(
        'https://api.example.com/upload',
        files=files
    )

# Upload with metadata
files = {
    'file': ('report.pdf', open('report.pdf', 'rb'), 'application/pdf')
}
data = {'description': 'Monthly report'}
response = requests.post(
    'https://api.example.com/upload',
    files=files,
    data=data
)

# Download file
response = requests.get('https://example.com/large-file.zip', stream=True)

with open('downloaded.zip', 'wb') as f:
    for chunk in response.iter_content(chunk_size=8192):
        f.write(chunk)

# Download with progress
import os

response = requests.get(url, stream=True)
total_size = int(response.headers.get('content-length', 0))
downloaded = 0

with open('file.zip', 'wb') as f:
    for chunk in response.iter_content(chunk_size=8192):
        f.write(chunk)
        downloaded += len(chunk)
        progress = (downloaded / total_size) * 100
        print(f"Progress: {progress:.1f}%")
```

### 🔴 Advanced: Retry Logic

```python
import requests
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

def create_session_with_retries(
    retries=3,
    backoff_factor=0.3,
    status_forcelist=(500, 502, 503, 504)
):
    """Create session with automatic retry logic"""
    session = requests.Session()
    
    retry = Retry(
        total=retries,
        read=retries,
        connect=retries,
        backoff_factor=backoff_factor,  # Wait time between retries
        status_forcelist=status_forcelist  # Retry on these status codes
    )
    
    adapter = HTTPAdapter(max_retries=retry)
    session.mount('http://', adapter)
    session.mount('https://', adapter)
    
    return session

# Usage
session = create_session_with_retries()

# Will automatically retry on 500 errors
response = session.get('https://api.example.com/data')
```

### 🔴 Advanced: Async Requests (with aiohttp)

```python
import asyncio
import aiohttp

async def fetch_one(session, url):
    async with session.get(url) as response:
        return await response.json()

async def fetch_all(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_one(session, url) for url in urls]
        results = await asyncio.gather(*tasks)
        return results

# Usage
urls = [
    'https://api.example.com/users/1',
    'https://api.example.com/users/2',
    'https://api.example.com/users/3',
]

# Much faster than sequential requests
results = asyncio.run(fetch_all(urls))
print(results)

# Comparison with sync
import time

# Sync (sequential) - slow
start = time.time()
for url in urls:
    requests.get(url)
print(f"Sync: {time.time() - start:.2f}s")

# Async (parallel) - fast
start = time.time()
asyncio.run(fetch_all(urls))
print(f"Async: {time.time() - start:.2f}s")
```

### 🔴 Advanced: API Client Class

```python
import requests
from typing import Optional, Dict, Any
from dataclasses import dataclass

@dataclass
class APIResponse:
    success: bool
    data: Any
    status_code: int
    error: Optional[str] = None

class APIClient:
    """Reusable API client with error handling and authentication"""
    
    def __init__(self, base_url: str, api_key: Optional[str] = None):
        self.base_url = base_url.rstrip('/')
        self.session = requests.Session()
        
        if api_key:
            self.session.headers['Authorization'] = f'Bearer {api_key}'
        
        self.session.headers['Content-Type'] = 'application/json'
        self.session.headers['Accept'] = 'application/json'
    
    def _request(
        self,
        method: str,
        endpoint: str,
        params: Optional[Dict] = None,
        data: Optional[Dict] = None,
        timeout: int = 30
    ) -> APIResponse:
        """Make HTTP request and return standardized response"""
        url = f"{self.base_url}/{endpoint.lstrip('/')}"
        
        try:
            response = self.session.request(
                method=method,
                url=url,
                params=params,
                json=data,
                timeout=timeout
            )
            
            response.raise_for_status()
            
            return APIResponse(
                success=True,
                data=response.json() if response.text else None,
                status_code=response.status_code
            )
        
        except requests.exceptions.HTTPError as e:
            error_message = str(e)
            try:
                error_data = e.response.json()
                error_message = error_data.get('error', {}).get('message', str(e))
            except:
                pass
            
            return APIResponse(
                success=False,
                data=None,
                status_code=e.response.status_code,
                error=error_message
            )
        
        except requests.exceptions.RequestException as e:
            return APIResponse(
                success=False,
                data=None,
                status_code=0,
                error=str(e)
            )
    
    def get(self, endpoint: str, params: Optional[Dict] = None) -> APIResponse:
        return self._request('GET', endpoint, params=params)
    
    def post(self, endpoint: str, data: Dict) -> APIResponse:
        return self._request('POST', endpoint, data=data)
    
    def put(self, endpoint: str, data: Dict) -> APIResponse:
        return self._request('PUT', endpoint, data=data)
    
    def patch(self, endpoint: str, data: Dict) -> APIResponse:
        return self._request('PATCH', endpoint, data=data)
    
    def delete(self, endpoint: str) -> APIResponse:
        return self._request('DELETE', endpoint)

# Usage
client = APIClient(
    base_url='https://api.example.com',
    api_key='your_api_key'
)

# GET request
response = client.get('/users', params={'page': 1, 'limit': 10})
if response.success:
    users = response.data
    print(f"Found {len(users)} users")
else:
    print(f"Error: {response.error}")

# POST request
response = client.post('/users', data={
    'name': 'Alice',
    'email': 'alice@example.com'
})
if response.success:
    print(f"Created user: {response.data}")

# DELETE request
response = client.delete('/users/123')
if response.success:
    print("User deleted")
```

### 🔴 Advanced: Rate Limiting

```python
import requests
import time
from functools import wraps

def rate_limited(max_per_second):
    """Decorator to rate limit function calls"""
    min_interval = 1.0 / max_per_second
    last_called = [0.0]
    
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            elapsed = time.time() - last_called[0]
            wait_time = min_interval - elapsed
            
            if wait_time > 0:
                time.sleep(wait_time)
            
            result = func(*args, **kwargs)
            last_called[0] = time.time()
            return result
        return wrapper
    return decorator

class RateLimitedClient:
    def __init__(self, base_url, requests_per_second=2):
        self.base_url = base_url
        self.session = requests.Session()
        self.requests_per_second = requests_per_second
        self.last_request_time = 0
    
    def _wait_if_needed(self):
        """Enforce rate limit"""
        min_interval = 1.0 / self.requests_per_second
        elapsed = time.time() - self.last_request_time
        
        if elapsed < min_interval:
            time.sleep(min_interval - elapsed)
        
        self.last_request_time = time.time()
    
    def get(self, endpoint, **kwargs):
        self._wait_if_needed()
        return self.session.get(f"{self.base_url}{endpoint}", **kwargs)

# Usage
client = RateLimitedClient('https://api.example.com', requests_per_second=2)

# These requests will be automatically spaced out
for i in range(10):
    response = client.get(f'/items/{i}')
    print(f"Fetched item {i}")
```

---

## 📊 Response Object Attributes

| Attribute | Description |
|-----------|-------------|
| `status_code` | HTTP status code (200, 404, etc.) |
| `text` | Response body as string |
| `content` | Response body as bytes |
| `json()` | Parse JSON response |
| `headers` | Response headers dict |
| `url` | Final URL (after redirects) |
| `ok` | True if status < 400 |
| `elapsed` | Time taken for request |

---

## ⚠️ Common Mistakes

### ❌ Mistake 1: Not Checking Status Code

```python
# WRONG - Assumes success
response = requests.get(url)
data = response.json()  # May fail if error response

# CORRECT
response = requests.get(url)
response.raise_for_status()  # Raises exception on error
data = response.json()
```

### ❌ Mistake 2: Not Using Timeout

```python
# WRONG - Can hang forever
response = requests.get(url)

# CORRECT - Set timeout
response = requests.get(url, timeout=10)  # 10 seconds
```

---

## 💡 Pro Tips

### 1. Use Environment Variables for Secrets

```python
import os
api_key = os.environ.get('API_KEY')
headers = {'Authorization': f'Bearer {api_key}'}
```

### 2. Use Sessions for Multiple Requests

```python
# Reuses connection, faster
with requests.Session() as s:
    for url in urls:
        s.get(url)
```

---

## 💼 Interview Insight

### Q1: Difference between `data` and `json` parameter?

> `data` sends form-encoded data (like HTML forms). `json` sends JSON data and automatically sets Content-Type header to application/json.

### Q2: What is a Session in requests?

> A Session persists settings (cookies, headers, auth) across requests and reuses TCP connections for better performance.

---

## 🧪 Practice Questions

### Easy:
1. Fetch data from a public API
2. Send POST request with JSON body
3. Handle 404 errors gracefully

### Medium:
4. Implement API authentication
5. Download a file with progress bar
6. Create a session with retry logic

### Hard:
7. Build a complete API client class
8. Implement rate limiting
9. Make parallel requests with asyncio

---

## 📖 Summary

| Task | Code |
|------|------|
| GET | `requests.get(url)` |
| POST JSON | `requests.post(url, json=data)` |
| Headers | `requests.get(url, headers={...})` |
| Auth | `requests.get(url, auth=(user, pass))` |
| Params | `requests.get(url, params={...})` |
| Timeout | `requests.get(url, timeout=10)` |
| Check status | `response.raise_for_status()` |
| Parse JSON | `response.json()` |

---

## ⏭️ Next Module

**[Module 10: Projects and Interview Prep](../10_Projects_and_Interview_Prep/README.md)** — Apply everything you've learned!

---

*Requests: Simple HTTP for Humans!* 🌐

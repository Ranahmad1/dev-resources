# REST API Design Guide

## HTTP Methods
| Method | Purpose | Example |
|--------|---------|--------|
| GET | Read/Fetch data | GET /users |
| POST | Create new data | POST /users |
| PUT | Replace entire resource | PUT /users/1 |
| PATCH | Partial update | PATCH /users/1 |
| DELETE | Delete resource | DELETE /users/1 |

## HTTP Status Codes
### 2xx Success
| Code | Meaning |
|------|--------|
| 200 | OK |
| 201 | Created |
| 204 | No Content (after delete) |

### 3xx Redirection
| Code | Meaning |
|------|--------|
| 301 | Moved Permanently |
| 304 | Not Modified |

### 4xx Client Errors
| Code | Meaning |
|------|--------|
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Unprocessable Entity |
| 429 | Too Many Requests |

### 5xx Server Errors
| Code | Meaning |
|------|--------|
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |

## RESTful URL Conventions
```
✅ Good
GET    /api/users             → Get all users
GET    /api/users/123         → Get user by ID
POST   /api/users             → Create user
PUT    /api/users/123         → Replace user
PATCH  /api/users/123         → Update user
DELETE /api/users/123         → Delete user
GET    /api/users/123/orders  → Get user's orders

❌ Avoid
GET /getUsers
POST /deleteUser/123
GET /api/user-list
```

## Standard Response Format
```json
// Success
{
  "success": true,
  "message": "User created successfully",
  "data": {
    "id": 1,
    "name": "Ahmad",
    "email": "ahmad@example.com"
  }
}

// Error
{
  "success": false,
  "message": "Validation failed",
  "errors": [
    { "field": "email", "message": "Email is required" }
  ]
}

// Paginated
{
  "success": true,
  "data": [...],
  "pagination": {
    "page": 1,
    "per_page": 10,
    "total": 100,
    "total_pages": 10
  }
}
```

## Fetch API Example (JavaScript)
```js
// GET
const res = await fetch('/api/users');
const data = await res.json();

// POST
const res = await fetch('/api/users', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'Ahmad', email: 'ahmad@mail.com' })
});

// With auth token
const res = await fetch('/api/profile', {
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  }
});

// Error handling
const res = await fetch('/api/data');
if (!res.ok) throw new Error(`HTTP error: ${res.status}`);
const data = await res.json();
```

## Authentication Methods
```
1. API Key       → X-API-Key: your-key (header)
2. Bearer Token  → Authorization: Bearer <token>
3. Basic Auth    → Authorization: Basic base64(user:pass)
4. JWT           → JSON Web Token in Authorization header
5. OAuth 2.0     → Token-based delegated access
```

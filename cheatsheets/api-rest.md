# REST API Design Guide

## HTTP Methods
| Method | Purpose | Idempotent | Body |
|--------|---------|-----------|------|
| GET | Read data | ✅ Yes | ❌ No |
| POST | Create resource | ❌ No | ✅ Yes |
| PUT | Replace entire resource | ✅ Yes | ✅ Yes |
| PATCH | Partial update | ❌ No | ✅ Yes |
| DELETE | Delete resource | ✅ Yes | Optional |
| HEAD | GET without body | ✅ Yes | ❌ No |
| OPTIONS | Available methods | ✅ Yes | ❌ No |

## HTTP Status Codes

### 2xx — Success
| Code | Meaning | When to use |
|------|---------|------------|
| 200 | OK | GET, PUT, PATCH successful |
| 201 | Created | POST created a resource |
| 202 | Accepted | Async operation queued |
| 204 | No Content | DELETE successful, or PATCH with no body |

### 3xx — Redirection
| Code | Meaning |
|------|--------|
| 301 | Moved Permanently |
| 302 | Found (temporary redirect) |
| 304 | Not Modified (use cache) |

### 4xx — Client Errors
| Code | Meaning | When to use |
|------|---------|------------|
| 400 | Bad Request | Malformed request, invalid syntax |
| 401 | Unauthorized | Not authenticated — login required |
| 403 | Forbidden | Authenticated but no permission |
| 404 | Not Found | Resource doesn't exist |
| 405 | Method Not Allowed | GET on a POST-only route |
| 409 | Conflict | Duplicate email, version conflict |
| 410 | Gone | Resource permanently deleted |
| 422 | Unprocessable Entity | Validation errors |
| 429 | Too Many Requests | Rate limit exceeded |

### 5xx — Server Errors
| Code | Meaning |
|------|--------|
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |
| 504 | Gateway Timeout |

## RESTful URL Conventions
```
✅ Good URLs
GET    /api/v1/users             → list all users
GET    /api/v1/users/123         → get user 123
POST   /api/v1/users             → create user
PUT    /api/v1/users/123         → replace user 123
PATCH  /api/v1/users/123         → update user 123 partially
DELETE /api/v1/users/123         → delete user 123
GET    /api/v1/users/123/orders  → user's orders
GET    /api/v1/users/123/orders/456 → specific order

❌ Avoid
GET  /getUsers
POST /deleteUser/123
GET  /api/user-list
POST /api/users/create
GET  /api/UpdateUser?id=1&name=Ahmad  (use PATCH instead)

# Filtering, Sorting, Pagination (query strings)
GET /api/v1/users?role=admin&active=true
GET /api/v1/posts?sort=-created_at,title  (- = DESC)
GET /api/v1/posts?page=2&per_page=20
GET /api/v1/posts?search=laravel
GET /api/v1/users?fields=id,name,email  (field selection)
```

## Standard Response Format
```json
// Success — single resource
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Rana Ahmad",
    "email": "ahmad@example.com",
    "created_at": "2026-09-04T08:00:00Z"
  },
  "message": "User retrieved successfully"
}

// Success — collection
{
  "success": true,
  "data": [ /* array of resources */ ],
  "meta": {
    "total": 100,
    "page": 2,
    "per_page": 20,
    "total_pages": 5,
    "has_next": true,
    "has_prev": true
  },
  "links": {
    "self": "/api/v1/users?page=2",
    "next": "/api/v1/users?page=3",
    "prev": "/api/v1/users?page=1"
  }
}

// Validation error — 422
{
  "success": false,
  "message": "Validation failed",
  "errors": {
    "email":    ["The email field is required.", "Must be a valid email."],
    "password": ["Minimum 8 characters."]
  }
}

// General error — 4xx / 5xx
{
  "success": false,
  "message": "User not found",
  "error": {
    "code": "USER_NOT_FOUND",
    "details": "No user with ID 999 exists."
  }
}
```

## JavaScript — Fetch API
```js
// GET
const response = await fetch('/api/v1/users');
if (!response.ok) throw new Error(`HTTP error: ${response.status}`);
const data = await response.json();

// POST
const res = await fetch('/api/v1/users', {
  method:  'POST',
  headers: { 'Content-Type': 'application/json' },
  body:    JSON.stringify({ name: 'Ahmad', email: 'a@b.com' })
});

// With Authorization
const res = await fetch('/api/v1/profile', {
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type':  'application/json'
  }
});

// Reusable API client
const api = {
  baseURL: '/api/v1',
  headers: () => ({
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${localStorage.getItem('token')}`
  }),
  async get(path) {
    const r = await fetch(this.baseURL + path, { headers: this.headers() });
    if (!r.ok) throw await r.json();
    return r.json();
  },
  async post(path, body) {
    const r = await fetch(this.baseURL + path, {
      method: 'POST', headers: this.headers(), body: JSON.stringify(body)
    });
    if (!r.ok) throw await r.json();
    return r.json();
  }
};

const users = await api.get('/users');
```

## Authentication Patterns
```
1. API Key       → X-API-Key: your-key (header)
                   ?api_key=your-key  (query — less secure)

2. Bearer Token  → Authorization: Bearer <token>

3. Basic Auth    → Authorization: Basic base64(username:password)

4. JWT           → Authorization: Bearer <jwt-token>
   → Header.Payload.Signature
   → Store in HttpOnly cookie (not localStorage!)
   → Short expiry (15min) + Refresh tokens

5. OAuth 2.0     → Delegated access
   → Flows: Authorization Code (web), PKCE (SPA/mobile),
             Client Credentials (server-to-server)
```

## API Versioning
```
✅ URL versioning (most common)
GET /api/v1/users
GET /api/v2/users

✅ Header versioning
Accept: application/vnd.myapi.v2+json

✅ Query param
GET /api/users?version=2
```

## Security Checklist
```
✅ Always use HTTPS (TLS)
✅ Validate & sanitize all input
✅ Authenticate every protected route
✅ Authorize by ownership (user can only edit own data)
✅ Rate limit all endpoints (especially auth)
✅ Don't expose sensitive data in errors
✅ Use parameterized queries (no SQL injection)
✅ Set CORS correctly (not *  in production)
✅ Log requests but never log passwords/tokens
✅ Return minimal data (no password hashes)
```

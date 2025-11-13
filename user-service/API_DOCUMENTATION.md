# User Service API Documentation

## Base URL
```
https://user-service-nv6m.onrender.com
```

## Health Check
```
GET https://user-service-nv6m.onrender.com/health
```

---

## Authentication Endpoints

### 1. Register New User
**Endpoint:** `POST /api/v1/users/`

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecurePassword123!",
  "name": "John Doe"
}
```

**Success Response (201 Created):**
```json
{
  "id": "uuid-here",
  "email": "user@example.com",
  "name": "John Doe",
  "tokens": {
    "access": "eyJ0eXAiOiJKV1QiLCJhbGc...",
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGc..."
  }
}
```

**cURL Example:**
```bash
curl -X POST https://user-service-nv6m.onrender.com/api/v1/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePassword123!",
    "name": "John Doe"
  }'
```

---

### 2. Login (Get Tokens)
**Endpoint:** `POST /api/v1/users/login`

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "SecurePassword123!"
}
```

**Success Response (200 OK):**
```json
{
  "access": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "user": {
    "id": "uuid-here",
    "email": "user@example.com",
    "name": "John Doe"
  }
}
```

**cURL Example:**
```bash
curl -X POST https://user-service-nv6m.onrender.com/api/v1/users/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePassword123!"
  }'
```

---

### 3. Refresh Access Token
**Endpoint:** `POST /api/v1/users/token/refresh`

**Request Body:**
```json
{
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGc..."
}
```

**Success Response (200 OK):**
```json
{
  "access": "eyJ0eXAiOiJKV1QiLCJhbGc..."
}
```

**cURL Example:**
```bash
curl -X POST https://user-service-nv6m.onrender.com/api/v1/users/token/refresh \
  -H "Content-Type: application/json" \
  -d '{
    "refresh": "YOUR_REFRESH_TOKEN_HERE"
  }'
```

---

## User Profile Endpoints

### 4. Get User Profile
**Endpoint:** `GET /api/v1/users/profile`

**Headers:**
```
Authorization: Bearer <access_token>
```

**Success Response (200 OK):**
```json
{
  "id": "uuid-here",
  "email": "user@example.com",
  "name": "John Doe",
  "created_at": "2025-11-13T20:30:00Z"
}
```

**cURL Example:**
```bash
curl -X GET https://user-service-nv6m.onrender.com/api/v1/users/profile \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN_HERE"
```

---

### 5. Update User Profile
**Endpoint:** `PUT /api/v1/users/profile`

**Headers:**
```
Authorization: Bearer <access_token>
```

**Request Body (all fields optional):**
```json
{
  "name": "John Updated Doe",
  "email": "newemail@example.com"
}
```

**Success Response (200 OK):**
```json
{
  "id": "uuid-here",
  "email": "newemail@example.com",
  "name": "John Updated Doe",
  "updated_at": "2025-11-13T20:35:00Z"
}
```

**cURL Example:**
```bash
curl -X PUT https://user-service-nv6m.onrender.com/api/v1/users/profile \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Updated Doe"
  }'
```

---

## Notification Preferences

### 6. Get Notification Preferences
**Endpoint:** `GET /api/v1/users/preferences`

**Headers:**
```
Authorization: Bearer <access_token>
```

**Success Response (200 OK):**
```json
{
  "id": "uuid-here",
  "user_id": "user-uuid",
  "email_notifications": true,
  "push_notifications": true,
  "sms_notifications": false
}
```

**cURL Example:**
```bash
curl -X GET https://user-service-nv6m.onrender.com/api/v1/users/preferences \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN_HERE"
```

---

### 7. Update Notification Preferences
**Endpoint:** `PUT /api/v1/users/preferences`

**Headers:**
```
Authorization: Bearer <access_token>
```

**Request Body (all fields optional):**
```json
{
  "email_notifications": false,
  "push_notifications": true,
  "sms_notifications": true
}
```

**Success Response (200 OK):**
```json
{
  "id": "uuid-here",
  "user_id": "user-uuid",
  "email_notifications": false,
  "push_notifications": true,
  "sms_notifications": true,
  "updated_at": "2025-11-13T20:40:00Z"
}
```

**cURL Example:**
```bash
curl -X PUT https://user-service-nv6m.onrender.com/api/v1/users/preferences \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{
    "email_notifications": false,
    "push_notifications": true
  }'
```

---

## Push Token Management

### 8. Register Push Token
**Endpoint:** `POST /api/v1/users/push-token`

**Headers:**
```
Authorization: Bearer <access_token>
```

**Request Body:**
```json
{
  "token": "ExponentPushToken[xxxxxxxxxxxxxxxxxxxxxx]",
  "device_type": "ios",
  "device_id": "unique-device-identifier"
}
```

**Field Descriptions:**
- `token` (required): FCM/APNS/Expo push token
- `device_type` (required): One of: `"ios"`, `"android"`, `"web"`
- `device_id` (optional): Unique device identifier

**Success Response (201 Created):**
```json
{
  "id": "uuid-here",
  "user_id": "user-uuid",
  "token": "ExponentPushToken[xxxxxxxxxxxxxxxxxxxxxx]",
  "device_type": "ios",
  "device_id": "unique-device-identifier",
  "is_active": true,
  "created_at": "2025-11-13T20:45:00Z"
}
```

**cURL Example:**
```bash
curl -X POST https://user-service-nv6m.onrender.com/api/v1/users/push-token \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{
    "token": "ExponentPushToken[xxxxxxxxxxxxxxxxxxxxxx]",
    "device_type": "ios",
    "device_id": "unique-device-identifier"
  }'
```

---

### 9. Get User's Push Tokens
**Endpoint:** `GET /api/v1/users/push-token`

**Headers:**
```
Authorization: Bearer <access_token>
```

**Success Response (200 OK):**
```json
[
  {
    "id": "uuid-1",
    "token": "ExponentPushToken[xxxxxxxxxxxxxxxxxxxxxx]",
    "device_type": "ios",
    "device_id": "device-1",
    "is_active": true,
    "created_at": "2025-11-13T20:45:00Z"
  },
  {
    "id": "uuid-2",
    "token": "ExponentPushToken[yyyyyyyyyyyyyyyyyyyyyy]",
    "device_type": "android",
    "device_id": "device-2",
    "is_active": true,
    "created_at": "2025-11-13T19:30:00Z"
  }
]
```

**cURL Example:**
```bash
curl -X GET https://user-service-nv6m.onrender.com/api/v1/users/push-token \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN_HERE"
```

---

### 10. Delete Push Token
**Endpoint:** `DELETE /api/v1/users/push-token/<token_id>`

**Headers:**
```
Authorization: Bearer <access_token>
```

**Success Response (204 No Content)**

**cURL Example:**
```bash
curl -X DELETE https://user-service-nv6m.onrender.com/api/v1/users/push-token/uuid-here \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN_HERE"
```

---

## Error Responses

### Common Error Formats

**400 Bad Request:**
```json
{
  "error": "Invalid request data",
  "details": {
    "email": ["This field is required"],
    "password": ["Password must be at least 8 characters"]
  }
}
```

**401 Unauthorized:**
```json
{
  "detail": "Authentication credentials were not provided."
}
```

**403 Forbidden:**
```json
{
  "detail": "You do not have permission to perform this action."
}
```

**404 Not Found:**
```json
{
  "detail": "Not found."
}
```

**500 Internal Server Error:**
```json
{
  "error": "Internal server error",
  "message": "An unexpected error occurred"
}
```

---

## Authentication Flow

### Standard Flow:
1. **Register** → Get access + refresh tokens
2. **Use access token** in `Authorization: Bearer <token>` header for all protected endpoints
3. **Refresh token** when access token expires (typically 15-30 minutes)
4. **Logout** → Delete tokens on client side

### Token Lifetime:
- **Access Token:** 15 minutes (can be configured)
- **Refresh Token:** 7 days (can be configured)

---

## Notes for Team

### Important:
1. **All authenticated endpoints require:** `Authorization: Bearer <access_token>` header
2. **Content-Type must be:** `application/json` for POST/PUT requests
3. **Base URL:** `https://user-service-nv6m.onrender.com`
4. **Health Check:** `GET /health` (no auth required)

### Free Tier Limitations:
⚠️ The service runs on Render's free tier:
- Spins down after 15 minutes of inactivity
- First request after sleep may take 30-60 seconds
- Subsequent requests are fast

### Database:
- PostgreSQL (Aiven Cloud)
- Persistent data storage

### Caching:
- Redis enabled for session management and caching
- Improves performance for authenticated requests

---

## Quick Test Script

```bash
# 1. Health Check
curl https://user-service-nv6m.onrender.com/health

# 2. Register
curl -X POST https://user-service-nv6m.onrender.com/api/v1/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "Test123!@#",
    "name": "Test User"
  }'

# 3. Login (save the access token)
curl -X POST https://user-service-nv6m.onrender.com/api/v1/users/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "Test123!@#"
  }'

# 4. Get Profile (replace TOKEN with access token from step 3)
curl -X GET https://user-service-nv6m.onrender.com/api/v1/users/profile \
  -H "Authorization: Bearer TOKEN"

# 5. Update Preferences
curl -X PUT https://user-service-nv6m.onrender.com/api/v1/users/preferences \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "push_notifications": true,
    "email_notifications": false
  }'

# 6. Register Push Token
curl -X POST https://user-service-nv6m.onrender.com/api/v1/users/push-token \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "token": "ExponentPushToken[test123]",
    "device_type": "ios"
  }'
```

---

## Contact & Support

- **Service URL:** https://user-service-nv6m.onrender.com
- **Status:** Live and Running ✅
- **Platform:** Render.com (Free Tier)
- **Database:** Aiven PostgreSQL
- **Cache:** Redis (Leapcell)

**Deployed:** November 13, 2025

---

## Summary for Team Lead

**Service is LIVE at:** `https://user-service-nv6m.onrender.com`

**Available Endpoints:**
- ✅ User Registration
- ✅ User Login
- ✅ Token Refresh
- ✅ User Profile (Get/Update)
- ✅ Notification Preferences (Get/Update)
- ✅ Push Token Management (Register/List/Delete)
- ✅ Health Check

**Authentication:** JWT Bearer tokens
**Response Format:** JSON
**Status Codes:** Standard HTTP (200, 201, 400, 401, 404, 500)

Ready for integration! 🚀

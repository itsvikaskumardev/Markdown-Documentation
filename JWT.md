## 1. What is JWT?

JWT is a token created by the backend after successful login.

It is commonly used to tell the backend:

> **"This user has already authenticated. Here is proof of their identity."**

---
# 2. Complete JWT Authentication Flow

```text
┌──────────────────┐
│     FRONTEND     │
│                  │
│ Email            │
│ Password         │
└────────┬─────────┘
         │
         │ 1. Login
         │ email + password
         ▼
┌──────────────────┐
│     BACKEND      │
│                  │
│ Verify user      │
│ Verify password  │
└────────┬─────────┘
         │
         │ 2. Query user
         ▼
┌──────────────────┐
│    DATABASE      │
│                  │
│ Email            │
│ PasswordHash     │
│ UserId           │
│ Role             │
└────────┬─────────┘
         │
         │ User found
         ▼
┌──────────────────┐
│     BACKEND      │
│                  │
│ Password correct │
│       ↓          │
│ Generate JWT     │
└────────┬─────────┘
         │
         │ 3. JWT
         ▼
┌──────────────────┐
│     FRONTEND     │
│                  │
│ Store JWT        │
└────────┬─────────┘
         │
         │ 4. API request + JWT
         ▼
┌──────────────────┐
│     BACKEND      │
│                  │
│ Validate JWT     │
└────────┬─────────┘
         │
         │ Valid
         ▼
       DATA
```

---

# 3. Step 1 — User Registration

Before login, the user usually creates an account.

User enters:

```text
Email:
vikas@gmail.com

Password:
MyPassword123
```

Frontend sends:

```http
POST /api/auth/register
```

```json
{
  "email": "vikas@gmail.com",
  "password": "MyPassword123"
}
```

---

# 4. Password Is NOT Stored Directly

The backend should **never store**:

```text
MyPassword123
```

Instead:

```text
MyPassword123
       ↓
Password Hashing Algorithm
       ↓
PasswordHash
       ↓
Database
```

Example:

```text
MyPassword123
       ↓
PBKDF2 / bcrypt / Argon2id
       ↓
$2b$12$xxxxxxxxxxxxxxxx
```

Database:

```text
Users
------------------------------------------------
Id    Email              PasswordHash
------------------------------------------------
101   vikas@gmail.com    $2b$12$xxxxxxxx...
```

---

# 5. Password Hashing vs JWT Signing

These are **two different things**.

### Password hashing

Used for:

```text
User password
     ↓
Password hash
     ↓
Database
```

Common algorithms:

```text
PBKDF2
bcrypt
Argon2id
scrypt
```

### JWT signing

Used for:

```text
JWT Header
     +
JWT Payload
     +
Secret Key
     ↓
Signature
```

Common JWT algorithms include:

```text
HS256
RS256
ES256
```

Remember:

```text
PASSWORD
   ↓
Password Hashing
   ↓
Database


JWT
   ↓
JWT Signing
   ↓
Frontend / Client
```

---

# 6. Step 2 — User Login

User enters:

```text
Email:
vikas@gmail.com

Password:
MyPassword123
```

Frontend sends:

```http
POST /api/auth/login
Content-Type: application/json
```

```json
{
  "email": "vikas@gmail.com",
  "password": "MyPassword123"
}
```

Diagram:

```text
┌──────────────┐
│   FRONTEND   │
│              │
│ Email        │
│ Password     │
└──────┬───────┘
       │
       │ POST /login
       │
       ▼
┌──────────────┐
│   BACKEND    │
└──────────────┘
```

---

# 7. Step 3 — Backend Finds User

Backend searches the database using the email:

```sql
SELECT *
FROM Users
WHERE Email = 'vikas@gmail.com';
```

Database returns:

```text
UserId:
101

Email:
vikas@gmail.com

PasswordHash:
$2b$12$xxxxxxxx...

Role:
Buyer
```

---

# 8. Step 4 — Password Verification

The backend now has:

```text
Password entered by user:
MyPassword123

Stored PasswordHash:
$2b$12$xxxxxxxx...
```

The backend does **not simply hash the password again and compare strings**.

Instead, it uses the password-hashing library's verification function.

Conceptually:

```text
User enters
"MyPassword123"
       │
       ▼
┌──────────────────────┐
│ Password Verify      │
│                      │
│ Password             │
│        +             │
│ Stored PasswordHash  │
└──────────┬───────────┘
           │
           ▼
       Match?
       /    \
     YES     NO
      │       │
      ▼       ▼
   Login    Reject
```

For example, ASP.NET Core provides `PasswordHasher<TUser>`.

Conceptually:

```csharp
hasher.VerifyHashedPassword(
    user,
    user.PasswordHash,
    enteredPassword
);
```

Result:

```text
Success
   ↓
Continue login
```

or:

```text
Failure
   ↓
Reject login
```

---

# 9. Step 5 — Generate JWT

If the password is correct:

```text
Email ✅
Password ✅
User exists ✅
```

Backend creates a JWT.

JWT contains three parts:

```text
HEADER.PAYLOAD.SIGNATURE
```


---

# 10. JWT Header

Example:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### `alg`

Means:

```text
Algorithm used to sign JWT
```

Here:

```text
HS256
```

means:

```text
HMAC-SHA256
```

### `typ`

Means:

```text
Token type = JWT
```

---

# 11. JWT Payload

Payload contains **claims**.

Example:

```json
{
  "sub": "101",
  "email": "vikas@gmail.com",
  "role": "Buyer",
  "exp": 1788174000
}
```

Meaning:

```text
sub /Subject
 ↓
User ID = 101

email
 ↓
vikas@gmail.com

role
 ↓
Buyer

exp
 ↓
Expiration time
```

---

# 12. Important: JWT Payload Is Not Secret

The payload is normally **encoded, not encrypted**.

Therefore:

```text
JWT
 ↓
Someone can decode
 ↓
Read Header + Payload
```

Never put:

```text
❌ Password
❌ PasswordHash
❌ Secret Key
❌ Database Password
❌ Other sensitive secrets
```

inside the JWT.

---

# 13. JWT Signature

The signature protects the JWT from modification.

Conceptually:

```text
Header
   +
Payload
   +
Secret Key
   ↓
HMAC-SHA256
   ↓
Signature
```

Final JWT:

```text
Header.Payload.Signature
```

---

# 14. What Is HS256?

HS256 means:

```text
HMAC
  +
SHA-256
```

It is a **symmetric signing algorithm**.

That means the same secret key is used to:

```text
Sign JWT
   +
Verify JWT
```

Diagram:

```text
             SECRET KEY
                 🔐
              /      \
             /        \
            ▼          ▼
        SIGN JWT    VERIFY JWT
```

The secret key must remain on the backend.

```text
┌────────────────────┐
│      BACKEND       │
│                    │
│   SECRET KEY 🔐    │
└────────────────────┘

        ❌

Frontend must NOT know the secret key.
```

---

# 15. How Does the Signature Prevent Modification?

Suppose the original JWT payload is:

```json
{
  "sub": "101",
  "role": "Buyer"
}
```

The server creates:

```text
Header + Payload + Secret Key
             ↓
          HS256
             ↓
         Signature
```

Now an attacker changes:

```text
role = Buyer
```

to:

```text
role = Admin
```

The payload has changed.

The signature no longer matches.

Backend calculates:

```text
Header
+
Modified Payload
+
Secret Key
       ↓
New Signature
```

But the JWT contains the old signature.

Therefore:

```text
New Signature
      ≠
JWT Signature
```

Result:

```text
❌ Token Invalid
```

---

# 16. What Is the Secret Key?

The secret key is a secret value known by the backend.

Example conceptually:

```text
JWT_SECRET_KEY = very-long-random-secret
```

It should be stored securely, such as in:

```text
Environment variables
Secret manager
User Secrets (development)
```

It should **not** be committed to GitHub.

It should **never be sent to the frontend**.

---

# 17. Step 6 — Backend Sends JWT to Frontend

After generating the JWT:

```text
Backend
   ↓
JWT
   ↓
Frontend
```

For example:

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

Or the backend can set the token in a cookie.

---

# 18. Where Is JWT Stored?

There are several choices.

## Option 1 — HttpOnly Cookie

```text
Browser
   │
   └── Cookie
         │
         └── accessToken
```

Cookie can be:

```text
HttpOnly
Secure
SameSite
```

### Advantage

JavaScript cannot directly read an `HttpOnly` cookie.

This reduces exposure to token theft through many XSS scenarios.

### Disadvantage

Cookies are automatically sent with matching requests, so CSRF protection and correct cookie configuration are important.

---

# 19. Option 2 — localStorage

```javascript
localStorage.setItem(
    "accessToken",
    token
);
```

Diagram:

```text
Browser
   │
   └── localStorage
          │
          └── accessToken
```

### Advantage

Easy to use.

### Disadvantage

JavaScript can read it.

If your website has an XSS vulnerability:

```text
Malicious JavaScript
       ↓
localStorage
       ↓
JWT stolen
```

---

# 20. Option 3 — sessionStorage

```javascript
sessionStorage.setItem(
    "accessToken",
    token
);
```

Diagram:

```text
Browser Tab
     │
     └── sessionStorage
             │
             └── accessToken
```

### Advantage

Usually disappears when the tab/session ends.

### Disadvantage

JavaScript can still access it, so XSS remains a concern.

---

# 21. Storage Comparison

| Storage         | JavaScript can read? | Main concern             |
| --------------- | -------------------: | ------------------------ |
| HttpOnly Cookie |                    ❌ | CSRF needs consideration |
| localStorage    |                    ✅ | XSS can steal token      |
| sessionStorage  |                    ✅ | XSS can steal token      |

A common security-focused web architecture uses:

```text
Short-lived Access Token
        +
Secure HttpOnly Cookie
        +
Refresh Token Rotation / Revocation
```

---

# 22. Step 7 — User Calls Protected API

Suppose the user wants their properties:

```http
GET /api/properties/my-properties
```

If using a bearer token:

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
```

Flow:

```text
┌──────────────┐
│   FRONTEND   │
└──────┬───────┘
       │
       │ JWT
       ▼
┌──────────────┐
│   BACKEND    │
└──────────────┘
```

---

# 23. What Does the Backend Do With JWT?

Backend receives:

```text
Authorization: Bearer <JWT>
```

It performs validation.

```text
JWT
 ↓
Extract Header
 ↓
Extract Payload
 ↓
Extract Signature
 ↓
Verify Signature
 ↓
Check Expiration
 ↓
Check Issuer/Audience if configured
 ↓
Authentication successful
```

---

# 24. JWT Signature Verification

Backend already knows:

```text
SECRET KEY 🔐
```

It takes:

```text
Header + Payload
```

and calculates:

```text
HMAC-SHA256(
    Header + Payload,
    Secret Key
)
```

Then:

```text
Calculated Signature
        │
        │ compare
        ▼
JWT Signature
```

If:

```text
Same
 ↓
Signature valid
```

If:

```text
Different
 ↓
Token modified/invalid
```

---

# 25. JWT Expiration — `exp`

JWT can contain:

```json
{
  "sub": "101",
  "role": "Buyer",
  "exp": 1788174000
}
```

`exp` means:

```text
Expiration Time
```

Backend checks:

```text
Current Time
     ↓
Compare with exp
```

If:

```text
Current Time < exp
```

then:

```text
✅ Token has not expired
```

If:

```text
Current Time > exp
```

then:

```text
❌ Token expired
```

---

# 26. Complete JWT Validation

The backend can conceptually perform:

```text
               JWT
                │
                ▼
        ┌───────────────┐
        │ Signature OK? │
        └───────┬───────┘
                │ YES
                ▼
        ┌───────────────┐
        │ Expired?      │
        └───────┬───────┘
                │ NO
                ▼
        ┌───────────────┐
        │ Issuer/Aud OK?│
        └───────┬───────┘
                │ YES
                ▼
       ✅ Authenticated
                │
                ▼
          Execute API
```

---

# 27. Does JWT Need to Be Stored in Database?

### Access JWT

Usually:

```text
❌ Not stored in database
```

Why?

Because the JWT contains its claims and is cryptographically signed.

The server can validate it using the signing key.

```text
JWT
 ↓
Verify signature
 ↓
Check expiration
 ↓
Authenticated
```

No access-token database lookup is required just to validate the JWT.

---

# 28. What Is Stored in Database?

Example:

```text
Users
------------------------------------------------
Id
Email
PasswordHash
Name
Role
------------------------------------------------
101
vikas@gmail.com
$2b$12$xxxxxxxx
Vikas
Buyer
```

For refresh-token/session management, you may also have:

```text
RefreshTokens
------------------------------------------------
Id
UserId
TokenHash
ExpiresAt
RevokedAt
CreatedAt
------------------------------------------------
1
101
abc123...
2026-09-30
NULL
2026-08-31
```

The exact schema depends on the authentication architecture.

---

# 29. What Is Stored in JWT?

Example:

```json
{
  "sub": "101",
  "email": "vikas@gmail.com",
  "role": "Buyer",
  "exp": 1788174000
}
```

So remember:

```text
DATABASE
│
├── UserId
├── Email
├── PasswordHash
├── Name
├── Role
└── Other user data


JWT
│
├── UserId
├── Role
├── Expiration
├── Issuer
├── Audience
└── Other claims
```

---

# 30. Access Token

Access token is used to access protected APIs.

Example:

```text
Access Token
     │
     ├── User ID
     ├── Role
     └── Expiration
```

It should usually have a **short lifetime**.

Example:

```text
Access Token
     ↓
15 minutes
```

---

# 31. Why Not Make Access Token Last 30 Days?

Suppose an attacker steals an access token.

If it lasts:

```text
30 days
```

the attacker could potentially use it for a long time.

Instead:

```text
Access Token
     ↓
Short lifetime
     ↓
15 minutes
```

If stolen, its useful lifetime is reduced.

But users shouldn't have to log in every 15 minutes.

That's why we use refresh tokens.

---

# 32. Refresh Token

Refresh token is used to obtain a new access token.

```text
Access Token
     ↓
Short-lived

Refresh Token
     ↓
Longer-lived
```

Important:

> **A refresh token does not have to be a JWT.**

It can simply be a long random value that the server tracks.

---

# 33. Why Do We Need Refresh Tokens?

Example:

```text
10:00
User logs in
```

Server gives:

```text
Access Token
Expires: 10:15

Refresh Token
Expires: later
```

At:

```text
10:10
```

access token works.

At:

```text
10:20
```

access token expires.

Without refresh token:

```text
❌ User must login again
```

With refresh token:

```text
Expired Access Token
        ↓
Refresh Token
        ↓
New Access Token
        ↓
Continue using application
```

---

# 34. Complete Refresh Flow

```text
┌──────────────┐
│   FRONTEND   │
└──────┬───────┘
       │
       │ API + expired access token
       ▼
┌──────────────┐
│   BACKEND    │
└──────┬───────┘
       │
       │ 401 Unauthorized
       ▼
┌──────────────┐
│   FRONTEND   │
└──────┬───────┘
       │
       │ Refresh request
       │ + refresh token
       ▼
┌──────────────┐
│   BACKEND    │
│              │
│ Validate     │
│ refresh token│
└──────┬───────┘
       │
       │ Valid
       ▼
┌──────────────┐
│ New Access   │
│ Token        │
└──────┬───────┘
       │
       ▼
   FRONTEND
       │
       │ Retry API
       ▼
   BACKEND
       │
       ▼
      DATA
```

---

# 35. What If the Access Token Is Invalid?

Examples:

```text
❌ Wrong signature
❌ Token modified
❌ Malformed JWT
❌ Expired token
❌ Invalid issuer
❌ Invalid audience
```

Backend normally returns:

```http
401 Unauthorized
```

---

# 36. What If the User Doesn't Have Permission?

Authentication and authorization are different.

### Authentication

```text
Who are you?
```

JWT can establish:

```text
User ID = 101
```

### Authorization

```text
Are you allowed to do this?
```

Example:

```text
JWT says:

role = Buyer
```

But API requires:

```text
role = Admin
```

Result:

```text
❌ 403 Forbidden
```

Simple difference:

```text
401 = Not authenticated
403 = Authenticated but not allowed
```

---




---



# 39. Token Gets Tampered With

Original:

```text
role = Buyer
```

Attacker changes:

```text
role = Admin
```

But doesn't know:

```text
Secret Key 🔐
```

Therefore:

```text
Modified Payload
       ↓
Signature doesn't match
       ↓
❌ 401 Unauthorized
```

---

# 40. Token Expires

```text
Access Token
     ↓
15 minutes
     ↓
Expired
```

Frontend calls API:

```text
JWT expired
     ↓
Backend
     ↓
401 Unauthorized
```

Then:

```text
Frontend
     ↓
Refresh request
     ↓
Backend validates refresh token
     ↓
New Access Token
     ↓
Frontend
     ↓
Retry API
```

---

# 41. The Entire JWT Flow in One Diagram

```text
                         LOGIN
                           │
                           ▼
                  ┌─────────────────┐
                  │    FRONTEND     │
                  │                 │
                  │ Email           │
                  │ Password        │
                  └────────┬────────┘
                           │
                           │ POST /login
                           ▼
                  ┌─────────────────┐
                  │     BACKEND     │
                  │                 │
                  │ Find User       │
                  │ Verify Password │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │    DATABASE     │
                  │                 │
                  │ UserId          │
                  │ Email           │
                  │ PasswordHash    │
                  │ Role            │
                  └────────┬────────┘
                           │
                           │ Password correct
                           ▼
                  ┌─────────────────┐
                  │     BACKEND     │
                  │                 │
                  │ Generate JWT    │
                  │                 │
                  │ Header          │
                  │ Payload         │
                  │ Signature       │
                  └────────┬────────┘
                           │
                           │ JWT
                           ▼
                  ┌─────────────────┐
                  │    FRONTEND     │
                  │                 │
                  │ Store JWT       │
                  └────────┬────────┘
                           │
                           │ API + JWT
                           ▼
                  ┌─────────────────┐
                  │     BACKEND     │
                  │                 │
                  │ Verify JWT      │
                  │                 │
                  │ Signature ✓     │
                  │ exp ✓           │
                  │ Claims ✓        │
                  └────────┬────────┘
                           │
                           │ Valid
                           ▼
                         DATA
```

---

# 42. JWT + Database — Final Mental Model

Keep this picture in your mind:

```text
                 ┌───────────────┐
                 │   FRONTEND    │
                 │               │
                 │ Email         │
                 │ Password      │
                 │               │
                 │ JWT           │
                 └───────┬───────┘
                         │
                         │ HTTP
                         ▼
                 ┌───────────────┐
                 │    BACKEND    │
                 │               │
                 │ Password      │
                 │ Verification  │
                 │               │
                 │ JWT Creation  │
                 │               │
                 │ JWT Validation│
                 │      🔐       │
                 │ Secret Key    │
                 └───────┬───────┘
                         │
                         │ SQL
                         ▼
                 ┌───────────────┐
                 │   DATABASE    │
                 │               │
                 │ UserId        │
                 │ Email         │
                 │ PasswordHash  │
                 │ Role          │
                 │               │
                 │ Refresh Token │
                 │   State       │
                 └───────────────┘
```

---

# 43. Final Things to Remember

### Password

```text
Plain Password
      ↓
Password Hashing
      ↓
PasswordHash
      ↓
Database
```

### Login

```text
Email + Password
       ↓
Backend
       ↓
Find User
       ↓
Verify Password
       ↓
Correct?
```

### JWT creation

```text
Header
   +
Payload
   +
Secret Key
   ↓
HS256
   ↓
Signature
   ↓
JWT
```

### JWT request

```text
Frontend
   ↓
JWT
   ↓
Backend
   ↓
Verify Signature
   ↓
Check exp
   ↓
Read Claims
   ↓
Authenticated
```

### Expiration

```text
exp reached
    ↓
Access Token expired
    ↓
401
    ↓
Refresh Token
    ↓
New Access Token
```

### Database vs JWT

```text
DATABASE
├── User information
├── PasswordHash
├── Role
└── Refresh-token/session state (if used)


JWT
├── User ID
├── Role
├── Expiration
├── Issuer/Audience
└── Other claims


SECRET KEY
└── Backend ONLY 🔐
```



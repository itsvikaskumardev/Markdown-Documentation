# 1. What is the Complete JWT Authentication Flow?

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

# 2. What happens during Step 1 - User Registration?

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

# 3. Why is the Password NOT Stored Directly?

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


# 4. What happens during Step 2 - User Login?

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

# 5. How does the Backend Find the User (Step 3)?

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

# 6. How does Password Verification work (Step 4)?

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

----
# 7. How is the JWT Generated?

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

# 8. What is the JWT Header?

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

# 9. What is the JWT Payload?

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

# 10. Why is it Important that the JWT Payload Is Not Secret?

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

# 11. What is the JWT Signature?

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

# 12. What Is HS256?

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

# 13. How Does the Signature Prevent Modification?

If someone changes the payload, the backend will calculate a new signature that won't match the one stored in the token. 

Here is a simplified code example demonstrating the process:

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

# 14. What Is the Secret Key?

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

# 15. How does the Backend Send the JWT to the Frontend (Step 6)?

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

# 16. Where Is JWT Stored?

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

# 17. How does storing JWT in localStorage work (Option 2)?

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

# 18. How does storing JWT in sessionStorage work (Option 3)?

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

# 19. How do different storage options compare?

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

# 20. What happens when the User Calls a Protected API (Step 7)?

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

# 21. What Does the Backend Do With JWT?

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

# 22. How does JWT Signature Verification work?

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

# 23. What is JWT Expiration (`exp`)?

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

# 24. What are the steps for Complete JWT Validation?

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

# 25. Does JWT Need to Be Stored in Database?

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

# 26. What Is Stored in Database?

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

# 27. What Is Stored in JWT?

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

# 28. What is an Access Token?

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

# 29. Why Not Make Access Token Last 30 Days?

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

# 30. What is a Refresh Token?

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

# 31. Why Do We Need Refresh Tokens?

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

# 32. What is the Complete Refresh Flow?

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

# 33. What If the Access Token Is Invalid?

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

# 34. What If the User Doesn't Have Permission?

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



# 35. What happens if the Token Gets Tampered With?

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

# 36. What happens if the Token Expires?

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
---
Yes — this is the part that usually confuses people. Let's forget the complicated terms for a moment.

The easiest way to understand it is:

> **SIGN JWT = Backend creates a special fingerprint for the JWT using the secret key.**
> **VERIFY JWT = Backend creates that fingerprint again later and checks whether it matches.**

---

# 38. First: What are we signing?

Suppose your backend creates this JWT:

```text
HEADER.PAYLOAD
```

For example:

```json
Header:
{
  "alg": "HS256",
  "typ": "JWT"
}
```

```json
Payload:
{
  "sub": "101",
  "role": "Buyer"
}
```

The backend has a secret key:

```text
SECRET KEY 🔐
my-super-secret-key
```

Now the backend does:

```text
Header + Payload + Secret Key
              ↓
           HS256
              ↓
          Signature
```

This is called **SIGNING the JWT**.

---

# 39. What Does "SIGN" Mean?

"Sign" does **not** mean putting a normal signature like your handwritten signature.

It means:

> **Use the secret key and algorithm to generate a cryptographic signature for the JWT.**

Think of it like putting a special security seal on a package.

```text
                    SECRET KEY 🔐
                         │
                         ▼
JWT Header + Payload ──→ HS256
                         │
                         ▼
                    SIGNATURE
```

Then the JWT becomes:

```text
HEADER.PAYLOAD.SIGNATURE
```

So:

```text
SIGN JWT
    ↓
Generate the Signature
    ↓
Using Secret Key + HS256
```

---

# 40. Why Do We Sign It?

Because later the backend needs to know:

> **"Did someone modify this JWT?"**

Suppose the original JWT says:

```json
{
  "sub": "101",
  "role": "Buyer"
}
```

The server signs it.

```text
Payload
   ↓
SIGN
   ↓
Signature = ABC123
```

So the JWT contains:

```text
HEADER.PAYLOAD.ABC123
```

---

# 41. What happens when the Frontend Gets the JWT?

Backend sends:

```text
JWT
 ↓
Frontend
```

The frontend doesn't need to understand the signature.

It simply sends the JWT back when calling protected APIs.

```text
Frontend
    │
    │ JWT
    ▼
Backend
```

---

# 42. Now What Is "VERIFY JWT"?

This happens when the backend receives the JWT later.

The backend receives:

```text
HEADER.PAYLOAD.ABC123
```

The backend already knows the secret key:

```text
SECRET KEY 🔐
my-super-secret-key
```

So the backend performs the same signing calculation again:

```text
Received Header + Received Payload
                  +
             Secret Key
                  ↓
                HS256
                  ↓
          Calculated Signature
```

Suppose it calculates:

```text
Calculated Signature = ABC123
```

The JWT contains:

```text
JWT Signature = ABC123
```

Compare:

```text
Calculated Signature
        =
JWT Signature

       ✅
```

Therefore:

```text
JWT is valid
```

This is called **VERIFYING the JWT**.

---

# 43. What is a Very Simple Diagram of this?

### When creating JWT

```text
              SECRET KEY 🔐
                   │
                   │
                   ▼
Header + Payload ──→ HS256
                   │
                   ▼
               SIGNATURE
                   │
                   ▼
          HEADER.PAYLOAD.SIGNATURE
```

This is:

> **SIGN JWT**

---

### When receiving JWT later

```text
        JWT received
             │
             ▼
     Header + Payload
             │
             │ + Secret Key 🔐
             ▼
           HS256
             │
             ▼
    Calculated Signature
             │
             ▼
       Compare with
       JWT Signature
          /      \
        SAME    DIFFERENT
         │          │
         ▼          ▼
        ✅          ❌
      VALID       INVALID
```

This is:

> **VERIFY JWT**

---

# 44. What If Someone Changes the JWT?

This is where signing becomes useful.

Original:

```json
{
  "sub": "101",
  "role": "Buyer"
}
```

Original signature:

```text
ABC123
```

JWT:

```text
HEADER.PAYLOAD.ABC123
```

Now imagine an attacker changes:

```json
{
  "sub": "101",
  "role": "Admin"
}
```

But they don't know the secret key.

So they cannot create the correct signature for this modified payload.

The JWT might become:

```text
HEADER.MODIFIED_PAYLOAD.ABC123
```

Backend receives it.

It calculates:

```text
Modified Header + Modified Payload
              +
          Secret Key
              ↓
             HS256
              ↓
        XYZ999
```

But the JWT says:

```text
ABC123
```

Compare:

```text
XYZ999 ≠ ABC123
```

Therefore:

```text
❌ INVALID JWT
```

---

# 45. What is The Important Point here?

The secret key is **NOT sent with the JWT**.

You have:

```text
BACKEND

Secret Key 🔐
     │
     ├──────────→ Sign JWT
     │
     └──────────→ Verify JWT
```

Frontend only gets:

```text
JWT
```

Frontend does **not** get:

```text
Secret Key ❌
```

---

# 46. Why should we Think of It Like a Stamp?

Imagine your backend has a secret stamp:

```text
       🔐 SECRET STAMP
             │
             ▼
      ┌──────────────┐
      │ JWT          │
      │ User = 101   │
      └──────────────┘
             │
             ▼
      Special seal
```

That's **SIGN**.

Later:

```text
JWT comes back
      ↓
Backend checks the seal
      ↓
Seal correct?
   /       \
 YES       NO
  ↓         ↓
✅         ❌
```

That's **VERIFY**.

The cryptographic process is much more sophisticated than a physical stamp, but this analogy is useful for understanding the purpose.

---

# 47. How would you summarize this In One Sentence?

Remember this:

> **SIGN = When the backend creates the JWT, it uses the secret key to create the signature.**

> **VERIFY = When the backend receives the JWT later, it uses the same secret key to calculate the signature again and checks whether it matches the signature inside the JWT.**

So your original diagram:

```text
SECRET KEY 🔐
     │
     ├──────────────→ SIGN JWT
     │                    │
     │                    ↓
     │                  JWT
     │
     └──────────────→ VERIFY JWT
                          │
                          ↓
                    Valid / Invalid
```

means simply:

```text
CREATE JWT
    ↓
Secret Key + Header + Payload
    ↓
Generate Signature
    ↓
JWT


LATER...

Receive JWT
    ↓
Secret Key + Header + Payload
    ↓
Generate Signature Again
    ↓
Compare Signatures
    ↓
Same? → ✅ Valid
Different? → ❌ Invalid
```

**That's what "sign" and "verify" mean.**



# HTTP vs HTTPS — Simple Notes

The easiest way to understand **HTTP vs HTTPS** is:

> **HTTP and HTTPS both use HTTP for sending requests and responses. The main difference is what happens to the HTTP data while it travels between the browser and server.**

---

# 1. What is HTTP?

**HTTP = HyperText Transfer Protocol**

HTTP is a set of rules that tells a **browser and server how to communicate**.

For example, when you open:

```text
http://example.com/users
```

the browser can send an HTTP request:

```text
Browser
   │
   │  HTTP Request
   │  GET /users
   ▼
Server
   │
   │  HTTP Response
   │  200 OK + data
   ▼
Browser
```

### Simple example

Browser asks:

```http
GET /users
Host: example.com
```

Server replies:

```http
HTTP/1.1 200 OK

[
  { "name": "Vikas" }
]
```

HTTP defines things like:

* `GET`
* `POST`
* `PUT`
* `PATCH`
* `DELETE`
* Request headers
* Response headers
* Status codes like `200`, `404`, `500`
* Request/response format

---

# 2. What is HTTPS?

**HTTPS = HTTP Secure**

HTTPS still uses **HTTP**.

The difference is that HTTPS adds **TLS** around the communication.

```text
HTTP
  +
TLS
  ↓
HTTPS
```

TLS stands for **Transport Layer Security**.

So conceptually:

```text
HTTP
 ↓
TLS protection
 ↓
HTTPS
```

For example:

```text
https://example.com/users
```

The browser still sends an HTTP request, but that HTTP data is sent through a **TLS-protected connection**.

---

# 3. What exactly changes when HTTP becomes HTTPS?

This is the most important point.

### HTTP

```text
HTTP Request
     ↓
Internet
     ↓
Server
```

The HTTP data is sent directly over the connection.

### HTTPS

```text
HTTP Request
     ↓
TLS
     ↓
Encrypted data
     ↓
Internet
     ↓
TLS
     ↓
HTTP Request
     ↓
Server
```

So **your API endpoints, HTTP methods, headers, request body, and response structure don't fundamentally change because you switched from HTTP to HTTPS.**

For example:

```http
POST /api/login
```

is still:

```http
POST /api/login
```

with either HTTP or HTTPS.

The important difference is **how the data is transported**.

---

# 4. HTTP vs HTTPS communication

## HTTP flow

```text
Browser
   │
   │ HTTP Request
   │
   ▼
Internet
   │
   ▼
Server
   │
   │ HTTP Response
   ▼
Browser
```

The HTTP request contains things such as:

```text
Method
URL
Headers
Body
```

Example:

```http
POST /api/login

{
  "email": "vikas@example.com",
  "password": "MyPassword123"
}
```

With plain HTTP, the communication is not protected by TLS.

---

# 5. HTTPS flow

HTTPS adds a TLS connection.

```text
Browser
   │
   │ 1. Connect to server
   ▼
Server
   │
   │ 2. TLS handshake
   ▼
Browser
   │
   │ 3. Secure TLS connection established
   ▼
Browser
   │
   │ 4. HTTP Request
   │    ↓
   │    TLS encrypts/protects it
   ▼
Internet
   │
   │ Protected data
   ▼
Server
   │
   │ TLS processes it
   │
   │ HTTP Request
   ▼
Server
```

Then the response goes back through the same protected TLS connection.

---

# 6. What happens when Browser sends HTTP?

Suppose you visit:

```text
http://example.com
```

### Step 1 — Browser connects to server

```text
Browser ─────────────→ Server
```

### Step 2 — Browser sends HTTP request

```http
GET / HTTP/1.1
Host: example.com
```

### Step 3 — Server processes it

```text
Server
  ↓
Find requested resource
  ↓
Generate response
```

### Step 4 — Server sends HTTP response

```http
HTTP/1.1 200 OK

<html>...</html>
```

### Overall

```text
Browser
   │
   │ HTTP Request
   ▼
Server
   │
   │ HTTP Response
   ▼
Browser
```

There is **no TLS handshake**.

---

# 7. What happens when Browser sends HTTPS?

Suppose you visit:

```text
https://example.com
```

There are some additional steps.

## Step 1 — Browser connects to server

```text
Browser ─────────────→ Server
```

## Step 2 — TLS handshake happens

The browser and server establish the TLS connection.

Very simplified:

```text
Browser
   │
   │ "I want a secure TLS connection"
   ▼
Server
   │
   │ "Here is my certificate"
   ▼
Browser
   │
   │ Verify certificate
   ▼
TLS connection established
```

The real TLS handshake is more detailed, but this is the basic idea.

---

# 8. What is the certificate?

The server has a **TLS certificate**.

For example:

```text
example.com
     ↓
TLS Certificate
     ↓
Issued by a trusted Certificate Authority
```

The browser uses the certificate to help verify:

> "Am I really communicating with the server for this domain?"

For example, if you visit:

```text
https://bank.com
```

the browser checks that the certificate is valid for the relevant domain and trusted.

---

# 9. After TLS is established

Now the browser can send the HTTP request through the TLS connection.

Conceptually:

```text
Browser
   │
   │ HTTP Request
   ▼
   TLS
   │
   │ Protected/Encrypted
   ▼
Internet
   │
   ▼
Server
   │
   │ TLS
   ▼
HTTP Request
```

The server processes the HTTP request normally.

For example:

```http
POST /api/login
```

with:

```json
{
  "email": "vikas@example.com",
  "password": "MyPassword123"
}
```

The server receives the HTTP request after TLS processing.

---

# 10. Very important: HTTPS does NOT replace HTTP

A common misunderstanding is:

```text
HTTP ❌
HTTPS ✅
```

It's better to think:

```text
             HTTPS
               │
        ┌──────┴──────┐
        │             │
       HTTP           TLS
```

HTTPS essentially means:

> **HTTP carried through a TLS-protected connection.**

So:

```text
HTTP = application protocol

TLS = protection layer

HTTPS = HTTP + TLS
```

---

# 11. What does TLS actually provide?

TLS mainly provides three important properties.

## 1. Encryption

Data sent between browser and server is encrypted.

Conceptually:

```text
Browser

"password123"
      │
      ▼
    TLS
      │
      ▼
"8fj3k9...encrypted data..."
      │
      ▼
   Internet
      │
      ▼
    Server
      │
      ▼
    TLS
      │
      ▼
"password123"
```

Someone observing the network should not be able to simply read the HTTP contents.

---

## 2. Integrity

TLS helps detect if transmitted data has been modified.

Conceptually:

```text
Browser
   │
   │ Original data
   ▼
Internet
   │
   │ Someone changes data?
   ▼
TLS detects the problem
```

So the receiver can detect tampering with the protected communication.

---

## 3. Authentication

The certificate helps the browser verify the server's identity.

```text
Browser
   │
   │ "Who are you?"
   ▼
Server
   │
   │ Certificate
   ▼
Browser
   │
   │ Verify certificate
   ▼
Server identity verified
```

This is why HTTPS is especially important for websites handling logins, payments, personal information, APIs, etc.

---

# 12. Simple HTTP vs HTTPS diagram

### HTTP

```text
┌─────────┐
│ Browser │
└────┬────┘
     │
     │ HTTP Request
     ▼
  Internet
     │
     │ HTTP data
     ▼
┌─────────┐
│ Server  │
└────┬────┘
     │
     │ HTTP Response
     ▼
┌─────────┐
│ Browser │
└─────────┘
```

### HTTPS

```text
┌─────────┐
│ Browser │
└────┬────┘
     │
     │ HTTP Request
     ▼
┌─────────┐
│   TLS   │
└────┬────┘
     │
     │ Protected/Encrypted
     ▼
  Internet
     │
     ▼
┌─────────┐
│   TLS   │
└────┬────┘
     │
     │ HTTP Request
     ▼
┌─────────┐
│ Server  │
└─────────┘
```

---

# 13. HTTP and HTTPS ports

This is another important difference.

### HTTP

Normally uses:

```text
Port 80
```

Example:

```text
http://example.com:80
```

Usually you don't write `:80` because it is the default.

### HTTPS

Normally uses:

```text
Port 443
```

Example:

```text
https://example.com:443
```

Again, you normally don't need to write `:443`.

So:

```text
HTTP  → Port 80
HTTPS → Port 443
```

---

# 14. URL difference

### HTTP

```text
http://example.com
```

### HTTPS

```text
https://example.com
```

The protocol at the beginning changes:

```text
http://
   ↓
https://
```

---

# 15. Important comparison table

| Feature                                   | HTTP                         | HTTPS       |
| ----------------------------------------- | ---------------------------- | ----------- |
| Full name                                 | HyperText Transfer Protocol  | HTTP Secure |
| Uses HTTP                                 | ✅                            | ✅           |
| Uses TLS                                  | ❌                            | ✅           |
| TLS handshake                             | ❌                            | ✅           |
| Data protected by TLS                     | ❌                            | ✅           |
| Encryption                                | ❌                            | ✅           |
| Data integrity protection                 | ❌                            | ✅           |
| Server authentication through certificate | ❌                            | ✅           |
| Default port                              | **80**                       | **443**     |
| URL                                       | `http://`                    | `https://`  |
| HTTP methods                              | GET, POST, PUT, DELETE, etc. | Same        |
| HTTP status codes                         | 200, 404, 500, etc.          | Same        |
| Request/response concept                  | Same                         | Same        |
| Typical modern website                    | Rarely appropriate           | **Yes**     |

---

# 16. What does NOT change?

This is also important.

Switching:

```text
http://api.example.com
```

to:

```text
https://api.example.com
```

doesn't change the basic HTTP concepts.

You still have:

### Methods

```text
GET
POST
PUT
PATCH
DELETE
```

### Status codes

```text
200 OK
201 Created
400 Bad Request
401 Unauthorized
404 Not Found
500 Internal Server Error
```

### Headers

```text
Authorization
Content-Type
Accept
User-Agent
```

### Request body

```json
{
  "name": "Vikas"
}
```

### Response body

```json
{
  "message": "User created"
}
```

These are still HTTP concepts.

---

# 17. Real-world example — HTTP

Imagine a simple website:

```text
http://example.com
```

Browser:

```text
GET /
```

Server:

```text
200 OK
<html>...</html>
```

The HTTP protocol handles this request and response.

However, using plain HTTP for modern public websites is generally inappropriate because the connection doesn't get TLS protection.

---

# 18. Real-world example — HTTPS

Imagine your banking website:

```text
https://bank.example
```

You log in:

```text
Browser
   │
   │ POST /login
   │
   │ email + password
   ▼
   TLS
   │
   │ Protected communication
   ▼
 Internet
   │
   ▼
   TLS
   │
   ▼
 Server
```

The server receives the HTTP request after the TLS layer processes the protected connection.

The same applies to:

* Banking websites
* E-commerce websites
* Social media
* Login systems
* REST APIs
* Admin dashboards
* Payment systems

---

# 19. HTTP vs HTTPS in an API

Suppose your backend has:

```text
GET /api/users
```

### HTTP API

```text
Frontend
   │
   │ HTTP
   │
   │ GET /api/users
   ▼
Backend
```

URL:

```text
http://api.example.com/api/users
```

### HTTPS API

```text
Frontend
   │
   │ HTTPS
   │
   │ HTTP request
   │      ↓
   │     TLS
   ▼
Internet
   │
   ▼
Backend
```

URL:

```text
https://api.example.com/api/users
```

Your backend endpoint can still be:

```text
GET /api/users
```

The important change is the **transport connection**, not the HTTP endpoint itself.

---

# 20. HTTP vs HTTPS — easiest way to remember

Think of sending a letter.

### HTTP

```text
You
 │
 │ Letter
 ▼
Open transportation
 │
 ▼
Server
```

The HTTP message is traveling without TLS protection.

### HTTPS

```text
You
 │
 │ Letter
 ▼
Locked/protected transport
 │
 ▼
Internet
 │
 ▼
Server
```

TLS provides the protected transport.

---

# 21. When should I use HTTP?

For modern applications, **normally use HTTPS**.

HTTP may still be encountered for:

* Local development
* Internal/testing environments
* Temporary local services
* Redirecting HTTP traffic to HTTPS

For example, during development:

```text
http://localhost:5000
```

is very common.

---

# 22. When should I use HTTPS?

Use HTTPS for essentially every **production website/API**.

Especially when handling:

```text
Login
Passwords
JWT tokens
Cookies
Personal information
Payments
Banking information
User data
Admin APIs
```

For example:

```text
Frontend
https://myapp.com

        ↓ HTTPS

Backend
https://api.myapp.com
```

This is the normal production setup.

---

# 23. One important point about JWT

Since you're learning backend authentication, remember this:

```text
HTTPS ≠ Authentication
HTTPS ≠ Authorization
```

They solve different problems.

### HTTPS

Protects communication between:

```text
Browser
   ↕
Server
```

### JWT

Can be used by the application to identify/authenticate the user.

```text
Client
   │
   │ Authorization: Bearer <JWT>
   ▼
Backend
   │
   │ Verify JWT
   ▼
User identified
```

So you might have:

```text
             HTTPS
Browser ───────────────── Server
             │
             │
             ▼
            JWT
             │
             ▼
     Authentication
```

**HTTPS protects the communication. JWT is an application-level authentication mechanism.**

---

# 24. Final mental model

Remember this diagram:

```text
HTTP
 │
 │ Application protocol
 │
 ├── GET
 ├── POST
 ├── PUT
 ├── DELETE
 ├── Headers
 ├── Body
 └── Status codes


HTTPS
 │
 ├── HTTP
 │
 └── TLS
      ├── Encryption
      ├── Integrity
      └── Server authentication
```

### In one sentence:

> **HTTP defines how the browser and server communicate, while HTTPS uses HTTP over a TLS-protected connection so that the communication is encrypted, integrity-protected, and the server can be authenticated.**

### Quick reference

```text
HTTP
↓
Port 80
↓
HTTP communication
↓
No TLS


HTTPS
↓
Port 443
↓
TLS handshake
↓
TLS-protected connection
↓
HTTP communication
```

So when you see:

```text
https://api.example.com/users
```

think:

```text
HTTPS
  ↓
TLS connection
  ↓
HTTP request
  ↓
GET /users
  ↓
Server
  ↓
HTTP response
  ↓
TLS protection
  ↓
Browser
```
----
Yes — **that's basically correct**, but let's make it very clear.

### HTTP → data is sent without encryption

Suppose you send:

```json
{
  "email": "vikas@gmail.com",
  "password": "12345"
}
```

With **HTTP**, the data travels without TLS encryption:

```text
Browser
   │
   │  email = vikas@gmail.com
   │  password = 12345
   ▼
Internet
   │
   ▼
Server
```

So the data is essentially in its **original/readable form** while being transmitted.

---

### HTTPS → data is encrypted during transmission

With HTTPS:

```text
Browser
   │
   │ Original data
   │
   │ "password": "12345"
   ▼
   TLS
   │
   │ Encrypted data
   │
   │ "x8#kP2@9......"
   ▼
Internet
   │
   ▼
   TLS
   │
   │ Decrypted for the server
   ▼
Server
```

The important thing is:

```text
Your data
   ↓
TLS encrypts it
   ↓
Encrypted data travels over the network
   ↓
Server receives it
   ↓
TLS decrypts/processes it
   ↓
Server gets the original data
```

### Simple example

Think of it like this:

**HTTP:**

```text
"Hello Server"
      ↓
"Hello Server"
      ↓
Server
```

**HTTPS:**

```text
"Hello Server"
      ↓
   Encryption
      ↓
"8fj#K29$x..."
      ↓
   Internet
      ↓
   Decryption
      ↓
"Hello Server"
      ↓
Server
```

So your main note should be:

> **HTTP sends HTTP data without TLS protection. HTTPS sends HTTP data through TLS, which encrypts the data while it travels between the client and server.**

One small correction: **"plain text" doesn't mean the data is necessarily literally transmitted as readable words in every HTTP implementation**; it means there is no TLS encryption protecting the HTTP communication.

----------------
Exactly — **decryption happens at the server side**, not somewhere in the middle of the Internet.

Think of the flow like this:

```text
        ENCRYPTION                         DECRYPTION
Browser ───────────────→ Internet ───────────────→ Server
          🔒                              🔓
      encrypted data                  original data
```

### Step by step

Suppose your browser wants to send:

```json
{
  "password": "12345"
}
```

**1. Browser has the original data**

```text
Browser
   ↓
"password": "12345"
```

**2. TLS encrypts the data**

```text
"password": "12345"
        ↓
     🔒 Encrypt
        ↓
"8f#K29$xP..."
```

**3. Encrypted data travels through the Internet**

```text
Browser
   ↓
🔒 "8f#K29$xP..."
   ↓
Internet
   ↓
🔒 "8f#K29$xP..."
   ↓
Server
```

The Internet/network **does not decrypt it**.

**4. Server receives the encrypted TLS data**

```text
Server
   ↓
🔒 "8f#K29$xP..."
   ↓
TLS decrypts/processes it
   ↓
"password": "12345"
```

**5. Your application/backend gets the original HTTP request**

```text
TLS
 ↓
HTTP Request
 ↓
Backend application
```

So remember:

```text
Browser
   │
   │ Encrypt 🔒
   ▼
Internet
   │
   │ Encrypted data
   ▼
Server
   │
   │ Decrypt 🔓
   ▼
Backend application
```

### ⭐ Most important point

**The Internet is just carrying the encrypted data. It does NOT decrypt it.**

The **TLS layer on the server** handles the protected connection and makes the HTTP data available to the server/application.

And the reverse happens for the response:

```text
Server
   │
   │ Original HTTP response
   ▼
TLS encrypts 🔒
   │
   ▼
Internet
   │
   ▼
Browser
   │
   │ TLS decrypts 🔓
   ▼
Browser gets HTTP response
```

So your note can be:

> **HTTPS: Browser encrypts → encrypted data travels through Internet → server receives it → server's TLS layer decrypts/processes it → backend gets the HTTP data.**

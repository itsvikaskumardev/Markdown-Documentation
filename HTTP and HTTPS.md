# HTTP vs HTTPS — Simple Notes

The easiest way to understand **HTTP vs HTTPS** is:

> **HTTP and HTTPS both use HTTP for sending requests and responses. The main difference is what happens to the HTTP data while it travels between the browser and server.**

---

# 1. What is HTTP?

**HTTP = HyperText Transfer Protocol**

HTTP is a set of rules that tells a **browser and server how to communicate**.
It defines things like:
* Methods (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`)
* Request/Response headers and body formats
* Status codes (e.g., `200`, `404`, `500`)

Example of an HTTP Request:
```http
POST /api/login
Host: example.com

{
  "email": "vikas@example.com",
  "password": "MyPassword123"
}
```

---

# 2. What is HTTPS?

**HTTPS = HTTP Secure**

HTTPS still uses **HTTP**. The difference is that HTTPS adds **TLS (Transport Layer Security)** protection around the communication.

```text
HTTP
  +
TLS
  ↓
HTTPS
```

The browser still sends an HTTP request, but that HTTP data is sent through a **TLS-protected connection**.

It's better to think:
```text
             HTTPS
               │
        ┌──────┴──────┐
        │             │
       HTTP           TLS
(App Protocol)  (Protection Layer)
```

---

# 3. What exactly changes when HTTP becomes HTTPS?

**Your API endpoints, HTTP methods, headers, request body, and response structure don't fundamentally change because you switched from HTTP to HTTPS.** 

For example, a `POST /api/login` request is exactly the same conceptually in both. The important difference is **how the data is transported**.

---

# 4. The Communication Flow: HTTP vs HTTPS

### HTTP Flow (No Protection)
With plain HTTP, the communication is not protected by TLS. The data is sent directly over the connection in its original, readable form.

```text
Browser
   │  Original data ("password": "MyPassword123")
   ▼
Internet
   │  Anyone observing can read this data!
   ▼
Server
   │  Receives original data
```

### HTTPS Flow (TLS Protection)
HTTPS adds a TLS handshake to establish a secure connection before sending HTTP data. The data is **encrypted** by the sender and **decrypted** only by the receiver.

```text
        ENCRYPTION                         DECRYPTION
Browser ───────────────→ Internet ───────────────→ Server
          🔒                              🔓
      encrypted data                  original data
```

**Step-by-step HTTPS Flow:**
1. **Connect:** Browser connects to server.
2. **TLS Handshake:** Browser and server verify identity and establish a secure TLS connection.
3. **Encrypt & Send:** Browser sends the HTTP request. TLS encrypts it (e.g., `"password": "123"` becomes `"8f#K29$xP..."`).
4. **Transport:** The encrypted data travels through the internet. **The internet does NOT decrypt it.**
5. **Decrypt & Process:** Server receives the encrypted data. The server's TLS layer decrypts it, and the backend application processes the original HTTP request.

**Easiest way to remember:**
* **HTTP:** Sending a postcard (open transportation).
* **HTTPS:** Sending a sealed letter (locked/protected transport).

---

# 5. What is the certificate?

The server has a **TLS certificate** issued by a trusted Certificate Authority. 
The browser uses this certificate during the TLS handshake to verify: *"Am I really communicating with the real server for this domain?"*

For example, if you visit `https://bank.com`, the browser checks that the certificate is valid for the relevant domain and trusted.

---

# 6. What does TLS actually provide?

1. **Encryption:** Data sent between browser and server is encrypted so observers cannot read the HTTP contents.
2. **Integrity:** TLS helps detect if transmitted data has been modified or tampered with in transit.
3. **Authentication:** The certificate helps the browser verify the server's identity.

This is why HTTPS is especially important for websites handling logins, payments, personal information, and APIs.

---

# 7. Key Differences Summary

### URL and Ports
* **HTTP:** `http://` (Default Port: 80)
* **HTTPS:** `https://` (Default Port: 443)

### Comparison Table

| Feature                                   | HTTP                         | HTTPS       |
| ----------------------------------------- | ---------------------------- | ----------- |
| Uses HTTP concepts (methods, codes, etc.) | ✅                            | ✅           |
| Uses TLS                                  | ❌                            | ✅           |
| TLS handshake                             | ❌                            | ✅           |
| Data encrypted in transit                 | ❌                            | ✅           |
| Server authentication (certificate)       | ❌                            | ✅           |
| Default port                              | **80**                       | **443**     |
| URL                                       | `http://`                    | `https://`  |
| Best used for                             | Local development/testing    | **Production** websites & APIs |

---

# 8. Important: HTTPS vs JWT

Since you're learning backend authentication, remember:
* **HTTPS ≠ Authentication / Authorization**

They solve different problems.

* **HTTPS** protects the communication channel between the client and the server.
* **JWT** is an application-level mechanism used by your backend to identify/authenticate a specific user.

```text
             HTTPS
Browser ───────────────── Server
             │
             ▼
            JWT
             │
             ▼
     Authentication
```

**HTTPS protects the communication. JWT is an application-level authentication mechanism sent within that protected communication.**

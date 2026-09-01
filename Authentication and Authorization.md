# Authentication vs Authorization 

# 1. What is Authentication?

**Authentication** means checking and confirming **who a user is**.


### Simple diagram

```text
User
  ↓
"Who are you?"
  ↓
Identity verification
  ↓
Is the identity valid?
  ↓
 ┌───────────────┐
 │               │
 YES             NO
 ↓               ↓
User identified  Access denied
```

# 2. What is Authorization?

**Authorization** happens after the system knows who the user is.

It determines **what that user is allowed to do**.


### Simple diagram

```text
User identified
      ↓
"What are you allowed to do?"
      ↓
Check permissions / role / access rules
      ↓
 ┌───────────────┐
 │               │
 ALLOWED        NOT ALLOWED
 ↓               ↓
Allow            Deny
```
---


# 3. Complete Authentication + Authorization Flow

This is one of the most useful diagrams to remember.

```text
                    USER
                      ↓
              Accesses Application
                      ↓
              ┌───────────────┐
              │ Authentication│
              └───────────────┘
                      ↓
                "Who are you?"
                      ↓
              Identity verified?
                 ↙         ↘
               NO           YES
               ↓             ↓
             401         User identified
                             ↓
                     ┌────────────────┐
                     │ Authorization  │
                     └────────────────┘
                             ↓
                    "What can you do?"
                             ↓
                     Check permissions
                             ↓
                       Allowed?
                      ↙          ↘
                    NO            YES
                    ↓              ↓
                   403          Request
                                allowed
```

---

# 4. What if the User is Not Authenticated?

Suppose someone tries to access a protected resource without being identified.

```text
User
 ↓
Request
 ↓
Server
 ↓
"Who are you?"
 ↓
Cannot establish identity
 ↓
❌ Access denied
```

The server essentially says:

> "I don't know who you are, so I cannot let you access this protected resource."

This is generally represented by:

**401 Unauthorized**

---

# 5. What if the User is Authenticated but Doesn't Have Permission?

Now consider a different situation.

The server knows:

```text
User = Vikas
```

So authentication succeeded.

But Vikas tries to perform an action he isn't allowed to perform.

```text
User
 ↓
Authenticated
 ↓
Vikas
 ↓
Tries to delete users
 ↓
Authorization check
 ↓
Does Vikas have permission?
 ↓
NO
 ↓
❌ Access denied
```

This is generally represented by:

**403 Forbidden**

---

# 6. How does the Backend Check Authentication?

## Step 1: User logs in

```text
Frontend
   ↓
"Login"
   ↓
Backend
   ↓
Checks user's login information
   ↓
Identity verified
   ↓
Backend creates JWT
   ↓
Frontend receives JWT
```

The JWT basically represents:

> "This request belongs to this authenticated user."

---

## Step 2: Frontend sends the JWT with later requests

For example, the user wants to view orders.

```text
Frontend
   ↓
GET /orders
   ↓
JWT attached to request
   ↓
Backend
```

Commonly it is sent in the `Authorization` header:

```text
Authorization: Bearer <JWT>
```

So yes, **the frontend sends the token to the backend with the request**.

---

## Step 3: Backend checks Authentication

The backend receives:

```text
Request
   +
JWT
   ↓
Backend
```

It verifies that the JWT is valid.

Conceptually:

```text
JWT
 ↓
Is it valid?
 ↓
YES
 ↓
Who does it represent?
 ↓
User = Vikas
```

Now the backend knows:

> **"This request is from Vikas."**

That's **Authentication**.

```text
Authentication
      ↓
JWT verified
      ↓
User identified
      ↓
Vikas
```

---

# 7. How does the Backend Check Authorization?

## Step 4: Backend checks Authorization

Now suppose Vikas requests:

```text
DELETE /users/123
```

The backend already knows:

```text
User = Vikas
```

Now it asks:

> "Is Vikas allowed to delete users?"

For example:

```text
Vikas
  ↓
Role = Employee
  ↓
Employee permissions
  ↓
❌ Cannot delete users
  ↓
403 Forbidden
```

But if:

```text
Vikas
  ↓
Role = Admin
  ↓
Admin permissions
  ↓
✅ Can delete users
  ↓
Request allowed
```







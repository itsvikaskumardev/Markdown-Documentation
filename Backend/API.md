# API and REST API — Complete Beginner-Friendly Notes

The easiest way to understand all of this is to see the **connection between the concepts**:

> **Frontend/Client → HTTP Request → API → Backend → Database → Backend → API → HTTP Response → Frontend/Client**

Once this flow is clear, **API, HTTP, REST API, endpoint, JSON, JWT, headers, CRUD, etc.** become much easier to understand.

---

# 1. What is an API?

## Simple definition

**API = Application Programming Interface**

An API is a **way for two software applications to communicate with each other**.

In simple words:

> **API is a set of rules that allows one application to ask another application for data or to perform some operation.**

For example, imagine you have a hospital application.

The frontend needs to show a list of doctors.

The frontend can ask the backend:

```text
"Give me all doctors"
```

The backend processes the request and sends back:

```json
[
  {
    "id": 1,
    "name": "Dr. Rahul",
    "specialization": "Cardiologist"
  },
  {
    "id": 2,
    "name": "Dr. Priya",
    "specialization": "Dentist"
  }
]
```

The frontend then displays these doctors.

That communication happens through an **API**.

---

# 2. Why do we need APIs?

Imagine you have:

```text
Frontend
    ↓
Database
```

It might seem simple to let the frontend directly access the database.

But this is a **bad idea**.

Why?

Because the database contains important things such as:

* User data
* Passwords
* Orders
* Payments
* Internal business data

You don't want users' browsers directly accessing your database.

Instead, we put a backend/API between them:

```text
Frontend
   ↓
 API / Backend
   ↓
Database
```

Now the backend controls:

* Who can access data
* What data can be accessed
* What operations are allowed
* Validation
* Authentication
* Authorization
* Business logic
* Database operations

So an API acts as a **controlled communication layer**.

---

# 3. Real-world example of an API

Think about a restaurant.

You are sitting at a table.

You don't go directly into the kitchen and make your food.

Instead:

```text
You
 ↓
Waiter
 ↓
Kitchen
```

You tell the waiter:

> "I want a pizza."

The waiter gives the order to the kitchen.

The kitchen prepares it.

The waiter brings the pizza back.

```text
You
 ↓
Request: Pizza
 ↓
Waiter
 ↓
Kitchen
 ↓
Response: Pizza
 ↓
Waiter
 ↓
You
```

The **waiter is similar to an API**.

You don't need to know:

* How the kitchen works
* Where ingredients are stored
* How the pizza is prepared

You only need to know:

> "How do I ask for a pizza?"

Similarly, a frontend doesn't need to know exactly how the backend works.

It only needs to know:

> "Which API should I call and what should I send?"

---

# 4. How an API works

Suppose your frontend wants user number `10`.

It sends a request:

```text
GET /api/users/10
```

The backend receives it.

The backend may do:

```text
Receive request
      ↓
Check authentication
      ↓
Validate request
      ↓
Find user 10
      ↓
Query database
      ↓
Get user data
      ↓
Create response
      ↓
Send response
```

The response might be:

```json
{
  "id": 10,
  "name": "Vikas",
  "email": "vikas@example.com"
}
```

The frontend receives this data and displays it.

---

# 5. Client and Server

This is one of the most important concepts.

## Client

A **client** is the application that makes a request.

Examples:

* Web browser
* React application
* Mobile application
* Desktop application
* Another backend service
* Postman

For example:

```text
React Frontend
```

can be a client.

---

# 6. Server

A **server** is a computer/application that receives requests and provides responses.

For example:

```text
.NET Backend
```

can act as a server.

The server may contain:

* API endpoints
* Business logic
* Authentication
* Database logic
* Validation

---

# 7. Client → Server communication

The basic flow is:

```text
       REQUEST
Client -----------> Server
Client <----------- Server
       RESPONSE
```

More clearly:

```text
┌──────────────┐
│    Client    │
│   React App  │
└──────┬───────┘
       │
       │ HTTP Request
       ↓
┌──────────────┐
│ API / Server │
│  .NET Backend│
└──────┬───────┘
       │
       │ Process request
       ↓
┌──────────────┐
│   Database   │
└──────┬───────┘
       │
       │ Data
       ↓
┌──────────────┐
│ API / Server │
└──────┬───────┘
       │
       │ HTTP Response
       ↓
┌──────────────┐
│    Client    │
│   React App  │
└──────────────┘
```

---

# 8. What is HTTP?

**HTTP = HyperText Transfer Protocol**

HTTP is a **communication protocol** used for communication over the web.

It defines how a client and server communicate.

For example:

```text
Client
  ↓
HTTP Request
  ↓
Server
  ↓
HTTP Response
  ↓
Client
```

APIs commonly use HTTP because HTTP already provides rules for:

* Sending requests
* Receiving responses
* Methods
* Status codes
* Headers
* Data transfer

---

# 9. What is an HTTP Request?

An HTTP request is a message sent by the client to the server.

For example:

```http
GET /api/users/10 HTTP/1.1
Host: example.com
Accept: application/json
```

It tells the server:

> "Please give me user 10."

An HTTP request can contain:

```text
HTTP Request
│
├── Method
├── URL
├── Headers
├── Query Parameters
├── Path Parameters
└── Body
```

---

# 10. What is an HTTP Response?

The server sends an HTTP response back.

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "id": 10,
  "name": "Vikas"
}
```

A response generally contains:

```text
HTTP Response
│
├── Status Code
├── Headers
└── Body
```

---

# 11. HTTP Methods

HTTP provides different methods to tell the server **what operation we want to perform**.

The most important ones are:

| Method | Common meaning        |
| ------ | --------------------- |
| GET    | Get/read data         |
| POST   | Create data           |
| PUT    | Replace/update data   |
| PATCH  | Partially update data |
| DELETE | Delete data           |

These are commonly mapped to **CRUD** operations.

```text
CRUD

Create → POST
Read   → GET
Update → PUT / PATCH
Delete → DELETE
```

---

# 12. GET

Used to retrieve data.

Example:

```http
GET /api/users
```

Meaning:

> Give me users.

Another example:

```http
GET /api/users/10
```

Meaning:

> Give me user 10.

Usually, GET does not contain a request body.

---

# 13. POST

Used to create something.

For example:

```http
POST /api/users
```

Body:

```json
{
  "name": "Vikas",
  "email": "vikas@example.com"
}
```

The backend creates a new user.

---

# 14. PUT

PUT is generally used to **replace/update the complete resource**.

Example:

```http
PUT /api/users/10
```

Body:

```json
{
  "name": "Vikas Kumar",
  "email": "vikas@example.com"
}
```

Think:

> "Here is the new complete version of user 10."

---

# 15. PATCH

PATCH is generally used for a **partial update**.

For example, you only want to change the name.

```http
PATCH /api/users/10
```

Body:

```json
{
  "name": "Vikas Kumar"
}
```

You don't need to send every user field.

---

# 16. DELETE

Used to delete a resource.

```http
DELETE /api/users/10
```

Meaning:

> Delete user 10.

---

# 17. HTTP Status Codes

The server uses status codes to tell the client what happened.

## 200 — OK

Request succeeded.

```text
GET /api/users/10

200 OK
```

---

## 201 — Created

Something was successfully created.

Example:

```text
POST /api/users

201 Created
```

---

## 400 — Bad Request

The request sent by the client is invalid.

Example:

```json
{
  "email": "not-an-email"
}
```

The backend may return:

```text
400 Bad Request
```

Meaning:

> "Your request is incorrect."

---

## 401 — Unauthorized

The client is not properly authenticated.

For example:

```text
No valid JWT token
```

Response:

```text
401 Unauthorized
```

Think:

> "I don't know who you are."

---

## 403 — Forbidden

The user is authenticated but does not have permission.

Example:

```text
User is logged in
        ↓
Trying to access Admin API
        ↓
No admin permission
        ↓
403 Forbidden
```

Think:

> "I know who you are, but you are not allowed to do this."

---

## 404 — Not Found

The requested resource does not exist.

Example:

```text
GET /api/users/999999
```

If user doesn't exist:

```text
404 Not Found
```

---

## 500 — Internal Server Error

Something went wrong on the server.

For example:

```text
API
 ↓
Backend error
 ↓
Database exception
 ↓
500 Internal Server Error
```

---

# 18. What is REST?

**REST = Representational State Transfer**

REST is an **architectural style** for designing APIs.

This is important:

> REST is not a programming language.

It is not:

```text
REST = framework
```

It is a set of principles/guidelines for designing web APIs.

---

# 19. What is a REST API?

A REST API is an API designed according to REST principles, commonly using HTTP.

For example:

```http
GET    /api/products
GET    /api/products/10
POST   /api/products
PUT    /api/products/10
PATCH  /api/products/10
DELETE /api/products/10
```

Here:

```text
/products
```

represents the **product resource**.

The HTTP method tells us what we want to do with that resource.

---

# 20. API vs HTTP API vs REST API

This is where many beginners get confused.

Think of them as increasingly specific concepts.

```text
API
 │
 ├── Can use different communication methods
 │
 └── HTTP API
       │
       └── Uses HTTP
             │
             └── REST API
                   │
                   └── Follows REST principles
```

### API

Broad concept.

> A way for software components to communicate.

### HTTP API

An API that communicates using HTTP.

```text
Client
  ↓ HTTP
API
  ↓
Server
```

### REST API

An HTTP-based API designed according to REST principles.

So:

> **Every REST API is an API.**

And commonly:

> **A REST API is an HTTP API.**

But:

> **Not every API is a REST API.**

And:

> **Not every HTTP API is necessarily RESTful.**

---

# 21. What is an Endpoint?

An **endpoint** is a specific URL through which an API provides a particular operation.

Example:

```http
GET /api/users
```

This can be an endpoint.

Another:

```http
GET /api/users/10
```

Another:

```http
POST /api/users
```

Think:

> **Endpoint = a specific address where an API operation is available.**

---

# 22. URL

URL tells the client **where to send the request**.

Example:

```text
https://example.com/api/users/10
```

Break it down:

```text
https://
   ↓
Protocol

example.com
   ↓
Server/domain

/api/users/10
   ↓
API path
```

---

# 23. Path / Route Parameters

Consider:

```http
GET /api/users/10
```

Here:

```text
10
```

is a **path parameter**.

The route could be:

```text
/api/users/{id}
```

And:

```text
/api/users/10
```

means:

```text
id = 10
```

Another example:

```http
GET /api/orders/500
```

means:

```text
orderId = 500
```

---

# 24. Query Parameters

Query parameters are used to provide additional information in the URL.

Example:

```http
GET /api/users?page=2&limit=10
```

Here:

```text
page = 2
limit = 10
```

are query parameters.

Another example:

```http
GET /api/products?category=mobile
```

Meaning:

> Give me products from the mobile category.

---

# 25. Headers

Headers contain **additional information/metadata** about the request or response.

Example:

```http
Authorization: Bearer eyJ...
Content-Type: application/json
Accept: application/json
```

Common headers include:

### Authorization

Used to send authentication information.

```http
Authorization: Bearer <JWT>
```

### Content-Type

Tells the server what type of data is being sent.

```http
Content-Type: application/json
```

### Accept

Tells the server what response format the client can accept.

```http
Accept: application/json
```

---

# 26. Request Body

The request body contains data sent to the server.

For example, when creating a user:

```http
POST /api/users
Content-Type: application/json
```

Body:

```json
{
  "name": "Vikas",
  "email": "vikas@example.com",
  "password": "123456"
}
```

The backend reads this data.

---

# 27. What is JSON?

**JSON = JavaScript Object Notation**

JSON is a common format used for exchanging data between frontend and backend.

Example:

```json
{
  "id": 10,
  "name": "Vikas",
  "age": 22
}
```

It represents data in a simple structure:

```text
key → value
```

For example:

```text
name → Vikas
age  → 22
```

JSON can contain:

* Strings
* Numbers
* Boolean
* Arrays
* Objects
* null

---

# 28. Complete API Request Example

Suppose the frontend wants user `10`.

It sends:

```http
GET /api/users/10 HTTP/1.1
Host: example.com
Accept: application/json
Authorization: Bearer <JWT>
```

Let's understand it:

```text
GET
 ↓
What operation?

/api/users/10
 ↓
Which resource?

Authorization
 ↓
Who is making the request?

Accept
 ↓
What response format do I want?
```

The server responds:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

Body:

```json
{
  "id": 10,
  "name": "Vikas",
  "email": "vikas@example.com"
}
```

---

# 29. Complete Real Application Flow

Now let's connect everything.

Suppose you have:

```text
React Frontend
.NET Web API
PostgreSQL Database
```

The user opens:

```text
Doctor Dashboard
```

The frontend needs doctor information.

---

## Step 1 — Frontend

React calls:

```http
GET /api/doctors/10
```

---

## Step 2 — HTTP Request

The browser sends:

```text
HTTP Request
     ↓
.NET API
```

The request might contain:

```http
GET /api/doctors/10
Authorization: Bearer <JWT>
Accept: application/json
```

---

## Step 3 — API Endpoint

.NET receives the request.

For example:

```text
GET /api/doctors/{id}
```

The API extracts:

```text
id = 10
```

---

## Step 4 — Authentication

The backend checks the JWT.

```text
Is JWT valid?
       ↓
      YES
       ↓
Continue
```

---

## Step 5 — Authorization

The backend may check:

```text
Is this user allowed to access this resource?
```

If yes:

```text
Continue
```

Otherwise:

```text
403 Forbidden
```

---

# 30. Backend talks to Database

The backend now needs doctor information.

It may execute a database query such as:

```sql
SELECT *
FROM Doctors
WHERE Id = 10;
```

Database returns:

```text
Doctor #10
Name: Rahul
Specialization: Cardiologist
```

---

# 31. Backend creates the response

The .NET backend converts the data into JSON.

```json
{
  "id": 10,
  "name": "Rahul",
  "specialization": "Cardiologist"
}
```

Then sends:

```text
HTTP 200 OK
```

---

# 32. Frontend receives response

React receives:

```json
{
  "id": 10,
  "name": "Rahul",
  "specialization": "Cardiologist"
}
```

React uses the data to display:

```text
Dr. Rahul
Cardiologist
```

---

# 33. Complete End-to-End Diagram

This is the most important diagram to remember:

```text
┌───────────────┐
│    USER       │
│    Browser    │
└───────┬───────┘
        │
        │ 1. User opens page
        ↓
┌───────────────┐
│    REACT      │
│   FRONTEND    │
└───────┬───────┘
        │
        │ 2. HTTP Request
        │ GET /api/doctors/10
        ↓
┌───────────────────┐
│    .NET WEB API   │
│      BACKEND      │
└────────┬──────────┘
         │
         │ 3. Authentication
         │ 4. Authorization
         │ 5. Business Logic
         ↓
┌───────────────────┐
│     DATABASE      │
│    PostgreSQL     │
└────────┬──────────┘
         │
         │ 6. Data
         ↓
┌───────────────────┐
│    .NET BACKEND   │
└────────┬──────────┘
         │
         │ 7. JSON Response
         │    200 OK
         ↓
┌───────────────┐
│    REACT      │
│   FRONTEND    │
└───────┬───────┘
        │
        │ 8. Display data
        ↓
┌───────────────┐
│     USER      │
└───────────────┘
```

---

# 34. Where does REST fit into this?

Now add REST.

```text
User
 ↓
React Frontend
 ↓
HTTP Request
 ↓
REST API
 ↓
.NET Backend
 ↓
PostgreSQL
```

For example:

```http
GET /api/doctors/10
```

The API follows REST-style principles:

```text
GET
 ↓
Read

/api/doctors/10
 ↓
Doctor resource with ID 10
```

Another:

```http
POST /api/doctors
```

means:

```text
POST
 ↓
Create

/api/doctors
 ↓
Doctor resource
```

Another:

```http
DELETE /api/doctors/10
```

means:

```text
DELETE
 ↓
Delete

/api/doctors/10
 ↓
Doctor #10
```

---

# 35. Resource

A **resource** is the thing your API is working with.

Examples:

```text
Users
Doctors
Patients
Orders
Products
Appointments
Services
```

For example:

```text
/api/users
```

represents the **users resource**.

```text
/api/doctors
```

represents the **doctors resource**.

```text
/api/orders
```

represents the **orders resource**.

---

# 36. CRUD

CRUD means:

```text
C = Create
R = Read
U = Update
D = Delete
```

Example for users:

```text
CREATE
POST /api/users

READ
GET /api/users
GET /api/users/10

UPDATE
PUT /api/users/10
PATCH /api/users/10

DELETE
DELETE /api/users/10
```

So REST APIs commonly map HTTP methods to CRUD operations.

---

# 37. Authentication vs Authorization

These two are extremely important.

## Authentication

Authentication means:

> **Who are you?**

Example:

```text
Login
 ↓
Email + Password
 ↓
Backend verifies
 ↓
JWT generated
```

---

## Authorization

Authorization means:

> **What are you allowed to do?**

Example:

```text
User logged in
       ↓
Is user an Admin?
       ↓
YES → Allow
NO  → 403 Forbidden
```

Remember:

```text
Authentication = Who are you?

Authorization = What can you do?
```

---

# 38. What is JWT?

**JWT = JSON Web Token**

JWT is commonly used for authentication in APIs.

After login:

```text
Frontend
   ↓
POST /api/login
   ↓
Backend
   ↓
Check username/password
   ↓
Create JWT
   ↓
Frontend receives JWT
```

Then future requests can contain:

```http
Authorization: Bearer <JWT>
```

For example:

```text
React
  ↓
GET /api/orders
Authorization: Bearer eyJ...
  ↓
.NET API
```

The backend validates the token.

---

# 39. Statelessness

REST APIs are commonly designed to be **stateless**.

Simple meaning:

> The server should not depend on remembering previous client requests in order to understand the current request.

For example:

```text
Request 1:
GET /api/orders
Authorization: Bearer <JWT>

Request 2:
GET /api/orders/10
Authorization: Bearer <JWT>
```

Each request contains the information needed to process it, such as the authentication token.

The server does not need to think:

> "What did this client tell me 5 minutes ago?"

This makes APIs easier to scale.

---

# 40. API Versioning

APIs can change over time.

Suppose you have:

```text
/api/v1/users
```

Later you make major changes.

Instead of breaking existing clients, you might introduce:

```text
/api/v2/users
```

So:

```text
v1 → Old API
v2 → New API
```

This is called **API versioning**.

---

# 41. API Documentation

API documentation explains how developers should use the API.

It may explain:

```text
Endpoint
HTTP method
Parameters
Headers
Request body
Response
Status codes
Authentication
```

For example:

```text
GET /api/users/{id}
```

Documentation might say:

```text
Method:
GET

Endpoint:
/api/users/{id}

Path parameter:
id

Response:
200 OK

Example:
{
   "id": 10,
   "name": "Vikas"
}
```

Tools such as Swagger/OpenAPI are commonly used to document APIs.

---

# 42. E-commerce Example

Imagine an e-commerce application.

```text
React
  ↓
.NET REST API
  ↓
Database
```

The user opens the product page.

Frontend calls:

```http
GET /api/products/10
```

---

## Request

```http
GET /api/products/10
Accept: application/json
```

---

## Backend

.NET receives:

```text
productId = 10
```

It queries the database:

```sql
SELECT *
FROM Products
WHERE Id = 10;
```

---

## Database

Returns:

```text
Id: 10
Name: iPhone
Price: 70000
Stock: 5
```

---

## Backend

Converts it into JSON:

```json
{
  "id": 10,
  "name": "iPhone",
  "price": 70000,
  "stock": 5
}
```

---

## Response

```http
200 OK
Content-Type: application/json
```

```json
{
  "id": 10,
  "name": "iPhone",
  "price": 70000,
  "stock": 5
}
```

---

## Frontend

React receives the response and displays:

```text
iPhone

₹70,000

5 available

[Buy Now]
```

The complete flow is:

```text
User
 ↓
React
 ↓
GET /api/products/10
 ↓
.NET REST API
 ↓
Business Logic
 ↓
PostgreSQL
 ↓
Product Data
 ↓
.NET REST API
 ↓
JSON Response
 ↓
React
 ↓
Product displayed
```

---

# 43. Practical .NET Example

Suppose you are building a .NET backend.

You might have an endpoint like:

```csharp
app.MapGet("/api/products/{id}", async (int id, AppDbContext db) =>
{
    var product = await db.Products.FindAsync(id);

    if (product == null)
        return Results.NotFound();

    return Results.Ok(product);
});
```

Now understand what happens.

Frontend sends:

```http
GET /api/products/10
```

.NET receives:

```text
id = 10
```

Then:

```csharp
db.Products.FindAsync(id)
```

asks the database for the product.

If it doesn't exist:

```text
404 Not Found
```

If it exists:

```text
200 OK
```

with JSON data.

So the code connects everything:

```text
React
  ↓
HTTP GET
  ↓
/api/products/10
  ↓
.NET Minimal API
  ↓
EF Core
  ↓
PostgreSQL
  ↓
Product
  ↓
JSON
  ↓
React
```

---

# 44. One Important Distinction: API ≠ Backend

Beginners often use these words as if they mean exactly the same thing.

They are related, but not identical.

```text
Backend
│
├── API endpoints
├── Business logic
├── Authentication
├── Authorization
├── Database access
├── Validation
└── Other server-side code
```

The **API is the interface exposed by the backend**.

Think:

```text
Backend = Entire kitchen

API = Counter/window through which orders are accepted
```

The client communicates through the API.

---

# 45. One Important Distinction: REST ≠ HTTP

HTTP is a **protocol**.

REST is an **architectural style**.

```text
HTTP
 ↓
Rules for communication

REST
 ↓
Guidelines for designing APIs
```

REST APIs commonly use HTTP methods:

```text
GET
POST
PUT
PATCH
DELETE
```

But REST itself is not the HTTP protocol.

---

# 46. Everything Connected Together

Now let's put all the concepts into one picture:

```text
                         INTERNET
                            │
                            │ HTTP
                            ↓
┌──────────────────────────────────────────┐
│                CLIENT                    │
│                                          │
│             React Frontend               │
└──────────────────┬───────────────────────┘
                   │
                   │ HTTP Request
                   │
                   │ GET /api/products/10
                   │ Headers
                   │ JWT
                   │ Query/Path Parameters
                   │ Body (when required)
                   ↓
┌──────────────────────────────────────────┐
│               REST API                   │
│                                          │
│        .NET ASP.NET Core Web API         │
│                                          │
│  Endpoint                                │
│  Authentication                          │
│  Authorization                           │
│  Validation                              │
│  Business Logic                          │
└──────────────────┬───────────────────────┘
                   │
                   │ Database Query
                   ↓
┌──────────────────────────────────────────┐
│                DATABASE                  │
│                                          │
│               PostgreSQL                 │
│                                          │
│       Users / Products / Orders          │
└──────────────────┬───────────────────────┘
                   │
                   │ Data
                   ↓
┌──────────────────────────────────────────┐
│               .NET API                   │
│                                          │
│       Creates HTTP Response              │
└──────────────────┬───────────────────────┘
                   │
                   │ HTTP Response
                   │
                   │ 200 OK
                   │ application/json
                   ↓
┌──────────────────────────────────────────┐
│             React Frontend               │
│                                          │
│          Displays the data               │
└──────────────────────────────────────────┘
```

---

# 47. The Complete Mental Model

Remember this chain:

```text
API
 ↓
A way for software to communicate

HTTP API
 ↓
API communicating using HTTP

REST API
 ↓
HTTP API designed using REST principles

Endpoint
 ↓
Specific API address/operation

HTTP Method
 ↓
What operation we want

Headers
 ↓
Extra information about request/response

Body
 ↓
Data being sent

JSON
 ↓
Common format for that data

Backend
 ↓
Processes the request

Database
 ↓
Stores/retrieves data

Response
 ↓
Result sent back to client
```

---

# 48. Most Important Diagram to Remember

If you remember only one diagram, remember this:

```text
             REQUEST
Frontend ──────────────────→ API
                              │
                              ↓
                          Backend
                              │
                              ↓
                          Database
                              │
                              ↓
                          Backend
                              │
                              ↓
API ←─────────────────────────┘
 │
 │ RESPONSE
 ↓
Frontend
```

Or even simpler:

```text
Client
  │
  │ HTTP Request
  ↓
API
  │
  ↓
Backend
  │
  │ Query
  ↓
Database
  │
  │ Data
  ↓
Backend
  │
  │ JSON Response
  ↓
Client
```

---

# 49. Quick Revision Notes

### API

> A way for software applications to communicate with each other.

### Client

> The application that sends the request.

### Server

> The application that receives the request and sends a response.

### HTTP

> A protocol used for communication between client and server on the web.

### HTTP Request

> Message sent from client to server.

### HTTP Response

> Message sent from server back to client.

### REST

> An architectural style for designing web APIs.

### REST API

> An API that commonly uses HTTP and follows REST principles.

### Endpoint

> A specific API URL used for an operation.

### JSON

> A common data format used to exchange information.

### Header

> Extra information attached to a request or response.

### Authentication

> "Who are you?"

### Authorization

> "What are you allowed to do?"

### JWT

> A token commonly used to carry authentication information.

### CRUD

```text
Create → POST
Read   → GET
Update → PUT/PATCH
Delete → DELETE
```

---

# 50. Final Connection — Don't Memorize These as Separate Topics

The biggest thing to understand is that these are **not separate concepts**.

They fit together like this:

```text
                    API
                     │
                     │
             Allows communication
                     │
                     ↓
                  HTTP API
                     │
                     │ Uses HTTP
                     ↓
                 REST API
                     │
                     │ Has endpoints
                     ↓
             /api/products/10
                     │
                     │ HTTP Method
                     ↓
                    GET
                     │
                     │ Request
                     ↓
             ┌───────────────┐
             │    Headers    │
             │    JWT        │
             │    Parameters  │
             │    Body        │
             └───────┬───────┘
                     │
                     ↓
               .NET Backend
                     │
              Authentication
                     │
              Authorization
                     │
               Business Logic
                     │
                     ↓
                 Database
                     │
                     ↓
                  Data
                     │
                     ↓
               JSON Response
                     │
                 200 OK
                     │
                     ↓
                  Frontend
                     │
                     ↓
               Display to User
```

### In one sentence:

> **A frontend/client uses an API endpoint to send an HTTP request to a backend server; a REST API is a common way of designing those HTTP APIs using resources and HTTP methods; the backend processes the request, communicates with the database, and sends an HTTP response—often containing JSON—back to the client.**

That single flow is the foundation for understanding **ASP.NET Core Web API, React API calls, JWT authentication, CRUD, Swagger, databases, Postman, microservices, and eventually system design**.

# CQRS, Design Patterns, MediatR, and Databases — Beginner-Friendly Guide

The easiest way to understand all of this is to build the picture step by step:

```text
Design Patterns
      ↓
Ways to solve common software problems
      ↓
CQRS
      ↓
Separate "writing data" from "reading data"
      ↓
Command / Query
      ↓
Handler
      ↓
MediatR can route the request to the correct Handler
      ↓
Database
      ↓
One DB OR separate Read/Write DBs
      ↓
If separate DBs
      ↓
Events + Message Broker
      ↓
Eventual Consistency
```

---

# 1. What is a Design Pattern?

A **design pattern** is a commonly used solution to a common software-design problem.

Think about building houses.

If many architects need to solve the same problem, they don't have to invent a completely new solution every time. They can follow a proven design.

Software design patterns work similarly.

### Simple example

Suppose many parts of your application need to create objects.

Instead of writing object-creation logic everywhere:

```text
Code A → creates object
Code B → creates object
Code C → creates object
Code D → creates object
```

you can use a **Factory Pattern**:

```text
          Request
             ↓
          Factory
         /       \
        ↓         ↓
   Object A    Object B
```

The Factory provides a standard way of creating objects.

### Important

A design pattern is **not**:

* a framework
* a library
* a NuGet package
* a piece of code you must copy

It is mainly a **design idea/solution**.

---

## Design Pattern vs Architecture vs Coding Style

| Concept        | Simple meaning                               | Example                           |
| -------------- | -------------------------------------------- | --------------------------------- |
| Design Pattern | Reusable solution to a common design problem | Factory, Strategy, Mediator       |
| Architecture   | Overall structure of the application         | Clean Architecture, Microservices |
| Coding Style   | How you write/organize code                  | Naming, formatting, conventions   |

Think of a house:

```text
Architecture
    ↓
Overall house structure

Design Pattern
    ↓
Specific solution used inside the house

Coding Style
    ↓
How things are written/presented
```

---

# 2. What is CQRS?

CQRS stands for:

> **Command Query Responsibility Segregation**

The name looks complicated, but the main idea is simple:

> **Separate operations that change data from operations that read data.**

There are two types of operations:

```text
                Application
                     |
              ┌──────┴──────┐
              ↓             ↓
          COMMAND         QUERY
              ↓             ↓
         Change Data      Read Data
```

### Command

A **Command** says:

> "Please change something."

Examples:

```text
CreateOrder
UpdateOrder
CancelOrder
DeleteOrder
```

### Query

A **Query** says:

> "Please give me some data."

Examples:

```text
GetOrderById
GetAllOrders
GetCustomerOrders
```

So:

```text
COMMAND → Changes state/data

QUERY   → Reads state/data
```

---

# 3. Why Was CQRS Introduced?

Imagine a normal application:

```text
Controller
    ↓
OrderService
    ↓
OrderRepository
    ↓
Database
```

The same service/repository may handle:

```text
Create Order
Update Order
Cancel Order
Get Order
Get Orders
Search Orders
Reports
Dashboards
```

As the application becomes larger, this can become complicated.

CQRS says:

> Separate the responsibilities.

```text
                 API
                  |
          ┌───────┴────────┐
          ↓                ↓
       Commands          Queries
          ↓                ↓
    Command Handlers   Query Handlers
          ↓                ↓
       Write Logic      Read Logic
```

The important point is:

### CQRS separates responsibility.

It does **not necessarily separate databases**.

---

# 4. Normal Application vs CQRS

## Normal approach

A simple application may look like:

```text
Client
  ↓
Controller
  ↓
Service
  ↓
Repository
  ↓
Database
```

For example:

```text
POST /orders
     ↓
OrderController
     ↓
OrderService
     ↓
OrderRepository
     ↓
PostgreSQL
```

And:

```text
GET /orders/123
     ↓
OrderController
     ↓
OrderService
     ↓
OrderRepository
     ↓
PostgreSQL
```

Both operations may use the same service structure.

---

# 5. CQRS Approach

With CQRS:

```text
                    API
                     |
             ┌───────┴───────┐
             ↓               ↓
          Command           Query
             ↓               ↓
       CommandHandler    QueryHandler
             ↓               ↓
          Write Logic     Read Logic
             ↓               ↓
          Database         Database
```

For example:

### Create

```text
POST /orders
     ↓
CreateOrderCommand
     ↓
CreateOrderCommandHandler
     ↓
Database
```

### Get

```text
GET /orders/123
     ↓
GetOrderByIdQuery
     ↓
GetOrderByIdQueryHandler
     ↓
Database
```

---

# 6. Is CQRS Mandatory?

**No.**

CQRS is an architectural/design approach.

You don't need CQRS for every application.

### Simple CRUD application

For something like:

```text
Employee CRUD
Product CRUD
Simple Admin Panel
Small Internal Tool
```

normal architecture may be enough:

```text
Controller
   ↓
Service
   ↓
Repository
   ↓
DB
```

Using CQRS here could create unnecessary classes and complexity.

### Complex application

CQRS can be useful when you have:

```text
Complex business rules
Many different types of reads
Heavy read traffic
Complex write operations
Different read/write requirements
Large systems
Event-driven architecture
```

---

# 7. Does CQRS Automatically Make an Application Better?

**No.**

CQRS is a tool.

It solves particular problems.

Using CQRS everywhere can actually make a simple application worse because you may end up with:

```text
CreateProductCommand.cs
CreateProductCommandHandler.cs
GetProductQuery.cs
GetProductQueryHandler.cs
UpdateProductCommand.cs
UpdateProductCommandHandler.cs
...
```

For a simple CRUD application, that can be unnecessary.

So remember:

> **CQRS is not automatically better. It is better when its separation solves a real problem.**

---

# 8. Command

A Command represents an operation that changes data.

Example:

```csharp
public record CreateOrderCommand(
    int CustomerId,
    decimal Amount
);
```

It means:

> "Create an order for this customer."

Flow:

```text
CreateOrderCommand
        ↓
     Handler
        ↓
   Validate data
        ↓
 Business rules
        ↓
   Save to DB
```

Other commands:

```text
CreateOrderCommand
UpdateOrderCommand
CancelOrderCommand
DeleteOrderCommand
DispatchOrderCommand
```

---

# 9. Query

A Query represents an operation that reads data.

Example:

```csharp
public record GetOrderByIdQuery(int OrderId);
```

It means:

> "Give me order 123."

Flow:

```text
GetOrderByIdQuery
        ↓
      Handler
        ↓
      Database
        ↓
    Order Data
```

Examples:

```text
GetOrderByIdQuery
GetAllOrdersQuery
GetCustomerOrdersQuery
SearchOrdersQuery
GetOrderDashboardQuery
```

---

# 10. Command vs Query

| Command                             | Query                  |
| ----------------------------------- | ---------------------- |
| Changes data                        | Reads data             |
| Create                              | Get                    |
| Update                              | Search                 |
| Delete                              | List                   |
| Cancel                              | Report                 |
| Usually returns result/ID/status    | Usually returns data   |
| Should not be used just for reading | Should not change data |

Simple rule:

```text
Does it CHANGE data?
       ↓
      YES
       ↓
    COMMAND

Does it READ data?
       ↓
      YES
       ↓
     QUERY
```

---

# 11. CQRS in .NET

A common structure is:

```text
               ASP.NET Core API
                      |
             ┌────────┴────────┐
             ↓                 ↓
          Command            Query
             ↓                 ↓
       CommandHandler     QueryHandler
             ↓                 ↓
       Business Logic      Read Logic
             ↓                 ↓
          DbContext         DbContext
             ↓                 ↓
              PostgreSQL
```

Each part has a job.

### API

Receives HTTP request.

```text
POST /orders
```

### Command

Describes what should happen.

```text
CreateOrderCommand
```

### Handler

Actually handles the command.

```text
CreateOrderCommandHandler
```

### DbContext

Communicates with the database through EF Core.

### Database

Stores the data.

---

# 12. What is MediatR?

Now we reach an important distinction.

**CQRS and MediatR are not the same thing.**

CQRS is a **design/architectural approach**.

MediatR is a **.NET library that implements the Mediator pattern** and can be used to help build CQRS-style applications.

Think:

```text
CQRS
 ↓
"Separate commands and queries"

MediatR
 ↓
"Help route those commands/queries to their handlers"
```

---

# 13. What is the Mediator Pattern?

A **Mediator** is like a middle person.

Imagine an office.

Without a mediator:

```text
Person A ─────→ Person B
Person A ─────→ Person C
Person A ─────→ Person D
Person B ─────→ Person C
Person C ─────→ Person D
```

Everyone communicates directly with everyone.

This becomes messy.

With a mediator:

```text
Person A ──→
Person B ──→  MEDIATOR
Person C ──→
Person D ──→
```

The mediator coordinates communication.

---

# 14. MediatR in ASP.NET Core

Instead of the endpoint directly calling a handler:

```text
API
 ↓
Handler
```

we can do:

```text
API
 ↓
MediatR
 ↓
Handler
```

Example:

```csharp
app.MapPost("/orders",
    async (
        CreateOrderCommand command,
        ISender sender) =>
    {
        var result = await sender.Send(command);

        return Results.Ok(result);
    });
```

The endpoint doesn't need to manually create:

```csharp
new CreateOrderCommandHandler(...)
```

MediatR handles the routing.

---

# 15. What Happens Inside?

Suppose we have:

```csharp
public record CreateOrderCommand(
    int CustomerId,
    decimal Amount
) : IRequest<int>;
```

And:

```csharp
public class CreateOrderCommandHandler
    : IRequestHandler<CreateOrderCommand, int>
{
    public async Task<int> Handle(
        CreateOrderCommand request,
        CancellationToken cancellationToken)
    {
        // business logic

        // save order

        return 123;
    }
}
```

Then:

```text
API
 ↓
CreateOrderCommand
 ↓
sender.Send(command)
 ↓
MediatR
 ↓
Find handler for CreateOrderCommand
 ↓
CreateOrderCommandHandler
 ↓
Handle()
 ↓
Database
```

---

# 16. How Does MediatR Find the Correct Handler?

This is one of the most important concepts.

We have:

```csharp
CreateOrderCommand
```

which implements:

```csharp
IRequest<int>
```

And the handler implements:

```csharp
IRequestHandler<CreateOrderCommand, int>
```

So MediatR knows:

```text
CreateOrderCommand
        ↓
IRequestHandler<CreateOrderCommand, int>
        ↓
CreateOrderCommandHandler
```

Conceptually:

```text
"Someone sent CreateOrderCommand."

             ↓

MediatR searches registered handlers

             ↓

Finds:
CreateOrderCommandHandler

             ↓

Calls:
Handle()
```

---

# 17. IRequest

`IRequest<T>` represents a request that expects a response.

Example:

```csharp
public record CreateOrderCommand(
    int CustomerId
) : IRequest<int>;
```

Here:

```text
IRequest<int>
```

means:

> "This request will eventually return an int."

For example:

```text
Create Order
     ↓
Order ID = 123
```

So:

```csharp
IRequest<int>
```

---

# 18. IRequestHandler

`IRequestHandler` handles the request.

```csharp
public class CreateOrderCommandHandler
    : IRequestHandler<CreateOrderCommand, int>
```

Meaning:

```text
I handle:
CreateOrderCommand

I return:
int
```

Its main method is:

```csharp
Handle()
```

---

# 19. ISender

`ISender` is used when you want to send a request.

For example:

```csharp
await sender.Send(command);
```

Think:

```text
ISender
   ↓
Send request
   ↓
MediatR
   ↓
Handler
```

For your API endpoints, `ISender` is often enough.

---

# 20. IMediator

`IMediator` provides mediator functionality, including sending requests and publishing notifications.

You can think of:

```text
ISender
 ↓
Mainly sending requests

IMediator
 ↓
Broader mediator interface
```

Modern MediatR usage often prefers the smaller interface when that's all the caller needs:

```csharp
ISender sender
```

This follows a simple idea:

> Depend only on what you need.

---

# 21. Command + Query + Handler + MediatR

Now connect everything:

```text
                 API
                  |
          ┌───────┴───────┐
          ↓               ↓
       Command           Query
          ↓               ↓
       MediatR           MediatR
          ↓               ↓
  Command Handler    Query Handler
          ↓               ↓
     Business Logic    Read Logic
          ↓               ↓
       Database         Database
```

### Command

> What do I want to change?

### Query

> What data do I want to read?

### Handler

> How should this request be executed?

### MediatR

> Which handler should receive this request?

---

# 22. Complete Order Creation Example

Suppose:

```text
Customer wants to create Order #1001
```

The client sends:

```http
POST /orders
```

with:

```json
{
  "customerId": 10,
  "amount": 500
}
```

Flow:

```text
                    CLIENT
                       |
                       ↓
              POST /orders
                       |
                       ↓
                API Endpoint
                       |
                       ↓
           CreateOrderCommand
                       |
                       ↓
                    MediatR
                       |
                       ↓
         CreateOrderCommandHandler
                       |
                       ↓
             Business Validation
                       |
                       ↓
                  DbContext
                       |
                       ↓
                 PostgreSQL
                       |
                       ↓
                Order Created
                       |
                       ↓
                  Order ID
                       |
                       ↓
                  API Response
```

---

# 23. What Does the Handler Do?

For example:

```csharp
public class CreateOrderCommandHandler
    : IRequestHandler<CreateOrderCommand, int>
{
    private readonly AppDbContext _db;

    public CreateOrderCommandHandler(AppDbContext db)
    {
        _db = db;
    }

    public async Task<int> Handle(
        CreateOrderCommand request,
        CancellationToken cancellationToken)
    {
        var order = new Order
        {
            CustomerId = request.CustomerId,
            Amount = request.Amount
        };

        _db.Orders.Add(order);

        await _db.SaveChangesAsync(cancellationToken);

        return order.Id;
    }
}
```

Simple meaning:

```text
Receive command
      ↓
Create Order object
      ↓
Add it to EF Core
      ↓
Save to PostgreSQL
      ↓
Return Order ID
```

---

# 24. Fetching an Order

Now suppose the client requests:

```http
GET /orders/123
```

Flow:

```text
CLIENT
  ↓
GET /orders/123
  ↓
API Endpoint
  ↓
GetOrderByIdQuery
  ↓
MediatR
  ↓
GetOrderByIdQueryHandler
  ↓
PostgreSQL
  ↓
Order Data
  ↓
API Response
```

Example:

```csharp
public record GetOrderByIdQuery(int OrderId)
    : IRequest<OrderDto?>;
```

Handler:

```csharp
public class GetOrderByIdQueryHandler
    : IRequestHandler<GetOrderByIdQuery, OrderDto?>
{
    public async Task<OrderDto?> Handle(
        GetOrderByIdQuery request,
        CancellationToken cancellationToken)
    {
        return await _db.Orders
            .AsNoTracking()
            .Where(x => x.Id == request.OrderId)
            .Select(x => new OrderDto
            {
                Id = x.Id,
                Amount = x.Amount
            })
            .FirstOrDefaultAsync(cancellationToken);
    }
}
```

Notice:

```text
Query Handler
     ↓
READ
     ↓
Doesn't modify order
```

---

# 25. CQRS and Database

This is the most important part.

A common misunderstanding is:

> "CQRS means two databases."

❌ **Incorrect.**

The correct idea is:

> **CQRS separates read and write responsibilities.**

It does **not require two databases**.

There are two common approaches.

---

# 26. Approach 1 — One Database

This is completely valid CQRS.

```text
                 CQRS
                /    \
               /      \
          Command      Query
             ↓           ↓
      Command Handler  Query Handler
             ↓           ↓
             └─────┬─────┘
                   ↓
             PostgreSQL
```

Both handlers use the same database.

For example:

```text
CreateOrderCommandHandler
          ↓
      PostgreSQL
```

and:

```text
GetOrderQueryHandler
          ↓
      PostgreSQL
```

Same database.

Different responsibilities.

---

# 27. Why Use One Database?

Because it is much simpler.

You get:

```text
One database
One connection
One source of truth
No synchronization system
No message broker
No eventual consistency problem
```

This is often a good choice when you're introducing CQRS into a normal .NET application.

For example:

```text
ASP.NET Core
     ↓
   CQRS
     ↓
  MediatR
     ↓
 PostgreSQL
```

Very reasonable architecture.

---

# 28. Approach 2 — Two Databases

For larger systems, you can separate read and write storage.

```text
                   API
                    |
             ┌──────┴──────┐
             ↓             ↓
          Command         Query
             ↓             ↓
          Handler         Handler
             ↓             ↓
        WRITE DB          READ DB
```

For example:

```text
PostgreSQL
   ↓
Write database

Elasticsearch / PostgreSQL / SQL Server / etc.
   ↓
Read database
```

The exact technology depends on the application.

---

# 29. Why Would We Want Two Databases?

Imagine an e-commerce system.

Millions of users constantly perform:

```text
Search products
View products
View orders
View dashboards
Generate reports
```

But writes may be comparatively smaller:

```text
Create Order
Cancel Order
Update Order
```

Read and write workloads may have very different requirements.

You might want:

```text
WRITE SIDE
    ↓
Optimized for transactions
```

and:

```text
READ SIDE
    ↓
Optimized for searching/reporting
```

This is where more advanced CQRS becomes useful.

---

# 30. The Big Question: How Do Two Databases Stay in Sync?

Suppose:

```text
User creates Order
       ↓
Write DB
```

How does the Read DB know about it?

One common architecture is:

```text
              Create Order
                    ↓
               Command Handler
                    ↓
               Write Database
                    ↓
              OrderCreated Event
                    ↓
              Message Broker
                    ↓
            Read-side Consumer
                    ↓
                Read DB
```

Now we need to understand several terms.

---

# 31. What is an Event?

An event is a message saying:

> "Something has already happened."

For example:

```text
OrderCreated
```

means:

> "An order was created."

It is different from a command.

### Command

```text
CreateOrder
```

means:

> "Please create an order."

### Event

```text
OrderCreated
```

means:

> "The order has been created."

Remember:

```text
COMMAND
"Do this."

EVENT
"This happened."
```

---

# 32. Event-Driven Architecture

In event-driven architecture, one component performs an action and publishes an event.

```text
Order Service
     |
     | OrderCreated
     ↓
Message Broker
     |
     ├────────→ Inventory Service
     |
     ├────────→ Notification Service
     |
     ├────────→ Analytics Service
     |
     └────────→ Read Model
```

The services don't necessarily call each other directly.

They can communicate through events.

---

# 33. What is a Message Broker?

A **message broker** is software that receives messages/events and delivers them to consumers.

Examples include:

* Kafka
* RabbitMQ
* Azure Service Bus

Think of it like a post office:

```text
Producer
   ↓
Message
   ↓
Post Office / Broker
   ↓
Consumer
```

For example:

```text
Order Service
      ↓
"OrderCreated"
      ↓
    Kafka
      ↓
Read Model Consumer
      ↓
Read Database
```

---

# 34. What is a Consumer?

A consumer is a component that receives and processes messages.

Example:

```text
Kafka
  ↓
OrderCreated
  ↓
ReadDatabaseConsumer
  ↓
Insert order into Read DB
```

So:

```text
Producer → creates event

Broker → transports/stores event

Consumer → processes event
```

---

# 35. Domain Event vs Integration Event

These terms can be confusing.

### Domain Event

Usually represents something important that happened inside your domain/business logic.

Example:

```text
OrderCreated
OrderCancelled
PaymentCompleted
```

### Integration Event

Usually represents an event intended to communicate between separate components/services.

For example:

```text
Order Service
      ↓
OrderCreatedIntegrationEvent
      ↓
Message Broker
      ↓
Inventory Service
```

Simple way to remember:

```text
Domain Event
    ↓
Important business event

Integration Event
    ↓
Event used to communicate across boundaries
```

The exact terminology varies between architectures.

---

# 36. Complete Two-Database Flow

Let's create an order.

```text
                         CLIENT
                            |
                            ↓
                     POST /orders
                            |
                            ↓
                   CreateOrderCommand
                            |
                            ↓
                         MediatR
                            |
                            ↓
                CreateOrderCommandHandler
                            |
                            ↓
                     WRITE DATABASE
                            |
                            ↓
                    Order Successfully
                       Created
                            |
                            ↓
                     OrderCreated Event
                            |
                            ↓
                    MESSAGE BROKER
                            |
                            ↓
                 Read Model Consumer
                            |
                            ↓
                     READ DATABASE
```

This is the important picture.

---

# 37. Are Both Databases Updated at Exactly the Same Time?

Usually:

**No.**

With this architecture:

```text
Write DB
   ↓
Event
   ↓
Broker
   ↓
Consumer
   ↓
Read DB
```

there can be a small delay.

For example:

```text
10:00:00.000
Order saved in Write DB

10:00:00.020
Event published

10:00:00.050
Consumer receives event

10:00:00.080
Read DB updated
```

The exact timings depend on the system.

This is called:

# Eventual Consistency

---

# 38. What is Eventual Consistency?

It means:

> The data may not be immediately the same everywhere, but the system expects the different copies to become consistent eventually.

Example:

```text
WRITE DB
Order #1001 exists
       ↓
       ↓ small delay
       ↓
READ DB
Order #1001 appears
```

During that small period:

```text
Write DB → Order exists

Read DB → Order not yet visible
```

Eventually:

```text
Write DB → Order exists
Read DB  → Order exists
```

---

# 39. Strong Consistency vs Eventual Consistency

### Strong consistency

After a successful write, subsequent reads see the new value immediately according to the consistency guarantees of the system.

Conceptually:

```text
WRITE
  ↓
DATABASE
  ↓
READ
  ↓
New data
```

### Eventual consistency

The new data may take some time to appear in another read model.

```text
WRITE DB
   ↓
Event
   ↓
Broker
   ↓
Consumer
   ↓
READ DB
```

So:

```text
Strong Consistency
→ immediately consistent within the relevant system/transaction

Eventual Consistency
→ consistent after some time
```

---

# 40. What If the Read Database Is Down?

This is one of the advantages of using a message broker.

Imagine:

```text
Write DB
   ↓
OrderCreated
   ↓
Kafka
   X
Read DB temporarily unavailable
```

Depending on the broker/configuration and consumer design, the message can remain available for later processing.

When the Read DB becomes available:

```text
Kafka
  ↓
Consumer retries/processes event
  ↓
Read DB
  ↓
Order appears
```

This is one reason reliable messaging and retry mechanisms are important in distributed systems.

The exact reliability depends on how the broker and application are configured.

---

# 41. Important: What Happens If Event Publishing Fails?

This is a real-world problem.

Suppose:

```text
Save Order → SUCCESS
Publish Event → FAILURE
```

Then:

```text
Write DB → Order exists
Read DB   → Doesn't know about it
```

This is one reason distributed CQRS can become complicated.

Patterns such as the **Transactional Outbox Pattern** are commonly used to help solve this problem.

Conceptually:

```text
             Database Transaction
                    |
          ┌─────────┴─────────┐
          ↓                   ↓
      Save Order          Save Event
          ↓                   ↓
          └─────────┬─────────┘
                    ↓
               Outbox Table
                    ↓
              Event Publisher
                    ↓
              Message Broker
```

The important idea is that the order and the outgoing event are recorded reliably together.

---

# 42. CQRS Does NOT Mean Two Databases

Memorize this:

```text
             CQRS
              |
      Separate responsibilities
              |
       ┌──────┴──────┐
       ↓             ↓
    COMMAND         QUERY
       ↓             ↓
    WRITE           READ
```

It can use:

### One database

```text
Command ──→ PostgreSQL ←── Query
```

or:

### Two databases

```text
Command ──→ Write DB

Query ──→ Read DB
```

Therefore:

```text
CQRS ≠ Two Databases
```

Instead:

```text
CQRS = Separate Read and Write Responsibilities
```

---

# 43. CQRS vs MediatR vs Other Concepts

This distinction is extremely important.

| Concept              | What is it?                                     | Main purpose                                           |
| -------------------- | ----------------------------------------------- | ------------------------------------------------------ |
| CQRS                 | Architectural/design approach                   | Separate commands and queries                          |
| Mediator Pattern     | Design pattern                                  | Reduce direct communication between components         |
| MediatR              | .NET library                                    | Implements mediator-style request handling             |
| Repository Pattern   | Design pattern/abstraction                      | Encapsulate data-access operations                     |
| Service Layer        | Application design approach                     | Organize business/application logic                    |
| Clean Architecture   | Architectural approach                          | Separate business logic from infrastructure            |
| Dependency Injection | Design technique / framework-supported approach | Provide dependencies instead of creating them manually |

Think:

```text
CQRS
 ↓
How we organize READ vs WRITE

MediatR
 ↓
How requests can be routed to handlers

Repository
 ↓
How data-access logic can be abstracted

Clean Architecture
 ↓
How the whole application can be organized
```

These can be used together, but they are **not the same thing**.

---

# 44. Common Design Patterns in .NET

There are many design patterns. A common classification is the **Gang of Four (GoF)** patterns.

## Creational Patterns

Concerned mainly with **creating objects**.

### Singleton

One shared instance.

```text
Application
    |
    ↓
 Singleton
    |
    ↓
 One instance
```

Useful when exactly one shared instance is appropriate.

However, don't use Singleton just because you can. ASP.NET Core already provides dependency-injection lifetimes such as Singleton.

---

### Factory

Moves object creation into a dedicated place.

```text
Request
   ↓
Factory
   ↓
Correct Object
```

Useful when object creation is complicated or depends on conditions.

---

### Builder

Builds a complicated object step by step.

```text
Builder
  ↓
Set A
  ↓
Set B
  ↓
Set C
  ↓
Final Object
```

Useful when an object has many optional/configurable parts.

---

# 45. Structural Patterns

Concerned with **how objects/classes are connected**.

### Adapter

Makes incompatible interfaces work together.

```text
Your Code
   ↓
Adapter
   ↓
Third-party API
```

---

### Decorator

Adds behavior around an existing object without changing its core implementation.

```text
Request
   ↓
Logging Decorator
   ↓
Authorization Decorator
   ↓
Actual Service
```

---

### Facade

Provides a simple interface over a complicated system.

```text
Client
  ↓
Facade
  ↓
 ┌──────┬──────┬──────┐
 ↓      ↓      ↓
 A      B      C
```

---

# 46. Behavioral Patterns

Concerned with **how objects communicate and behave**.

### Strategy

Allows you to choose between different algorithms/behaviors.

```text
Payment
   ↓
Strategy
 /    \
↓      ↓
Card   UPI
```

---

### Observer

One object notifies interested objects when something happens.

```text
Publisher
   ↓
Event
 /   |   \
↓    ↓    ↓
A    B    C
```

---

### Mediator

Centralizes communication.

```text
A ──┐
B ──┼──→ Mediator
C ──┘
```

---

### Chain of Responsibility

Passes a request through a chain of handlers.

```text
Request
   ↓
Handler 1
   ↓
Handler 2
   ↓
Handler 3
   ↓
Result
```

ASP.NET Core middleware is a good practical example of a chain-like processing model.

---

# 47. Repository Pattern

Repository provides an abstraction over data access.

Instead of putting database operations everywhere:

```text
Handler
   ↓
EF Core
   ↓
Database
```

you might have:

```text
Handler
   ↓
IOrderRepository
   ↓
OrderRepository
   ↓
EF Core
   ↓
Database
```

For example:

```csharp
public interface IOrderRepository
{
    Task<Order?> GetByIdAsync(int id);
    Task AddAsync(Order order);
}
```

Important:

> EF Core's `DbContext` already provides many repository/unit-of-work-like capabilities, so adding a repository layer is a design choice, not a requirement.

---

# 48. Unit of Work

A Unit of Work groups related database changes into one logical operation/transaction.

Conceptually:

```text
Unit of Work
     |
 ┌───┼────┐
 ↓   ↓    ↓
Order Item Payment
     |
     ↓
Commit
```

Either the transaction succeeds or, where transactional guarantees apply, changes can be rolled back together.

Again, EF Core's `DbContext` already behaves like a unit-of-work abstraction in many applications.

---

# 49. Dependency Injection

Dependency Injection means:

> Instead of a class creating its dependencies itself, those dependencies are provided to it.

Without DI:

```csharp
var service = new OrderService();
```

With DI:

```csharp
public OrderHandler(IOrderService service)
{
    _service = service;
}
```

ASP.NET Core's built-in DI container creates/provides the dependency.

```text
DI Container
    |
    ├── OrderService
    ├── DbContext
    ├── Repository
    └── Handler
```

---

# 50. CQRS with Clean Architecture

A common Clean Architecture structure might look like:

```text
                 API
                  ↓
             Application
                  ↓
               Domain
                  ↑
           Infrastructure
                  ↓
              Database
```

One possible project structure:

```text
MyApp
│
├── API
│   └── Endpoints
│
├── Application
│   ├── Commands
│   │   ├── CreateOrder
│   │   │   ├── CreateOrderCommand.cs
│   │   │   └── CreateOrderCommandHandler.cs
│   │   │
│   │   └── CancelOrder
│   │
│   ├── Queries
│   │   ├── GetOrderById
│   │   │   ├── GetOrderByIdQuery.cs
│   │   │   └── GetOrderByIdQueryHandler.cs
│   │
│   └── Interfaces
│
├── Domain
│   ├── Entities
│   └── Domain Events
│
└── Infrastructure
    ├── Persistence
    │   └── AppDbContext.cs
    └── Repositories
```

This is only one possible organization.

---

# 51. Where Does Everything Go?

A simplified view:

```text
API
 │
 │ receives HTTP request
 ↓
Application
 │
 ├── Commands
 ├── Queries
 ├── Handlers
 └── Interfaces
 │
 ↓
Domain
 │
 ├── Entities
 ├── Business Rules
 └── Domain Concepts
 │
 ↑
Infrastructure
 │
 ├── EF Core
 ├── DbContext
 ├── Repositories
 └── External Services
 │
 ↓
Database
```

The exact project boundaries vary by architecture.

---

# 52. One-Database CQRS + Clean Architecture

A realistic architecture could be:

```text
                        CLIENT
                           ↓
                    ASP.NET Core API
                           ↓
                    ┌──────┴──────┐
                    ↓             ↓
                 COMMAND         QUERY
                    ↓             ↓
                 MediatR        MediatR
                    ↓             ↓
               Command         Query
               Handler         Handler
                    ↓             ↓
                    └──────┬──────┘
                           ↓
                       DbContext
                           ↓
                       PostgreSQL
```

This is a perfectly valid architecture.

You don't need Kafka or two databases just because you use CQRS.

---

# 53. Evolving to Two Databases

Later, suppose your application becomes much larger.

You might evolve it:

```text
                         CLIENT
                            ↓
                           API
                            |
                  ┌─────────┴─────────┐
                  ↓                   ↓
               COMMAND               QUERY
                  ↓                   ↓
               Handler              Handler
                  ↓                   ↓
              WRITE DB             READ DB
                  |
                  ↓
              Domain Event
                  |
                  ↓
            Message Broker
                  |
                  ↓
           Read Model Consumer
                  |
                  ↓
               READ DB
```

Now the architecture has more moving parts.

That can give you scalability and specialized read models, but it also introduces more complexity.

---

# 54. Read Model / Projection

These two terms are important.

## Read Model

A model designed specifically for reading.

For example, your write database might have:

```text
Orders
Customers
OrderItems
Products
Payments
```

To display an order dashboard, you might need data from all of them.

Instead of performing a complicated query every time, you could create a read model:

```text
OrderDashboardReadModel
--------------------------------
OrderId
CustomerName
TotalAmount
PaymentStatus
OrderStatus
ItemCount
WarehouseName
```

Then:

```text
Query
 ↓
Read Model
 ↓
Fast response
```

---

# 55. Projection

A **projection** is the process of building/updating a read model from events.

Example:

```text
OrderCreated
     ↓
Projection
     ↓
Create row in OrderReadModel
```

Then:

```text
OrderStatusChanged
     ↓
Projection
     ↓
Update OrderReadModel
```

So:

```text
Events
  ↓
Projection
  ↓
Read Model
```

---

# 56. Complete Real-World Order Management System

Let's put everything together.

## Version 1 — Simple One Database

Technology:

```text
ASP.NET Core
CQRS
MediatR
EF Core
PostgreSQL
```

Architecture:

```text
                         CLIENT
                            |
                            ↓
                     ASP.NET CORE API
                            |
                ┌───────────┴───────────┐
                ↓                       ↓
             COMMAND                  QUERY
                ↓                       ↓
             MediatR                 MediatR
                ↓                       ↓
        Command Handler          Query Handler
                ↓                       ↓
                └──────────┬────────────┘
                           ↓
                       EF Core
                           ↓
                       PostgreSQL
```

### Create Order

```text
POST /orders
     ↓
CreateOrderCommand
     ↓
MediatR
     ↓
CreateOrderCommandHandler
     ↓
Business Rules
     ↓
DbContext
     ↓
PostgreSQL
     ↓
Order Created
```

### Get Order

```text
GET /orders/1001
     ↓
GetOrderByIdQuery
     ↓
MediatR
     ↓
GetOrderByIdQueryHandler
     ↓
EF Core
     ↓
PostgreSQL
     ↓
OrderDto
     ↓
Client
```

This is already CQRS.

**Only one database is being used.**

---

# 57. Version 2 — Two Databases

As the system grows:

```text
                           CLIENT
                              |
                              ↓
                            API
                              |
                   ┌──────────┴──────────┐
                   ↓                     ↓
                COMMAND                 QUERY
                   ↓                     ↓
                MediatR                MediatR
                   ↓                     ↓
             Command Handler       Query Handler
                   ↓                     ↓
               WRITE DB             READ DB
                   |
                   ↓
             OrderCreated
                   |
                   ↓
             Message Broker
                   |
                   ↓
             Event Consumer
                   |
                   ↓
                READ DB
```

Now:

```text
WRITE SIDE
    ↓
Transaction/business rules
    ↓
Write Database
```

and:

```text
READ SIDE
    ↓
Optimized queries
    ↓
Read Database
```

---

# 58. Example: Order Creation With Two Databases

Let's follow the request.

### Step 1 — Client

```http
POST /orders
```

### Step 2 — API

```text
API
 ↓
CreateOrderCommand
```

### Step 3 — MediatR

```text
CreateOrderCommand
 ↓
MediatR
 ↓
CreateOrderCommandHandler
```

### Step 4 — Handler

Handler validates:

```text
Customer exists?
Products available?
Price correct?
Order valid?
```

### Step 5 — Write DB

```text
Write DB
 ↓
Order saved
```

### Step 6 — Event

```text
OrderCreated
```

### Step 7 — Broker

```text
OrderCreated
     ↓
Kafka/RabbitMQ/Azure Service Bus
```

### Step 8 — Consumer

```text
Consumer receives OrderCreated
     ↓
Build/update Read Model
```

### Step 9 — Read DB

```text
Read DB
 ↓
Order #1001 available
```

---

# 59. What If the User Immediately Requests the Order?

Imagine:

```text
t = 0
Order created in Write DB

t = 10 ms
User calls GET /orders/1001

t = 20 ms
Read DB hasn't received event yet
```

The query may temporarily return:

```text
Order not found
```

even though:

```text
Write DB
Order exists
```

This is **eventual consistency**.

This is an important trade-off of a distributed read/write architecture.

---

# 60. CQRS Advantages

### 1. Separation of responsibilities

```text
Commands → Write
Queries  → Read
```

Makes the application easier to reason about when complexity grows.

### 2. Complex business logic

Commands can contain complicated business rules without mixing them with read/reporting logic.

### 3. Read optimization

Read models can be designed specifically for queries.

### 4. Write optimization

The write model can focus on transactional/business requirements.

### 5. Scalability

Read and write workloads can potentially scale independently.

```text
           API
            |
      ┌─────┴─────┐
      ↓           ↓
   WRITE        READ
    x2            x20
```

For read-heavy systems, this can be valuable.

### 6. Different data models

The write model doesn't have to be identical to the read model.

---

# 61. CQRS Disadvantages

### More code

Instead of:

```text
OrderService
```

you may have:

```text
CreateOrderCommand
CreateOrderCommandHandler

UpdateOrderCommand
UpdateOrderCommandHandler

GetOrderQuery
GetOrderQueryHandler
```

### More classes

This increases project size.

### More complexity

Especially when you introduce:

```text
Events
Message Broker
Read DB
Consumers
Retries
Dead-letter queues
```

### Eventual consistency

Two databases can temporarily disagree.

### Debugging becomes harder

A request may travel through:

```text
API
 ↓
Handler
 ↓
Write DB
 ↓
Event
 ↓
Broker
 ↓
Consumer
 ↓
Read DB
```

### More infrastructure

You may need:

```text
Kafka/RabbitMQ
Monitoring
Retry mechanisms
Dead-letter handling
Event processing
```

---

# 62. When is CQRS Overengineering?

Suppose you have:

```text
Small Employee CRUD API
```

Requirements:

```text
Create employee
Get employee
Update employee
Delete employee
```

Architecture:

```text
API
 ↓
CQRS
 ↓
MediatR
 ↓
20 Handlers
 ↓
Events
 ↓
Kafka
 ↓
Read DB
```

That's probably excessive.

A simpler architecture might be:

```text
API
 ↓
Service
 ↓
EF Core
 ↓
PostgreSQL
```

The principle is:

> **Don't add architecture because it sounds advanced. Add it because it solves a real problem.**

---

# 63. A Very Important Mental Model

Think of a restaurant.

### Command

Customer says:

> "I want to order a pizza."

```text
Customer
   ↓
Order Command
   ↓
Kitchen/Handler
   ↓
Make pizza
```

### Query

Customer says:

> "What is the status of my order?"

```text
Customer
   ↓
Query
   ↓
Read system
   ↓
"Preparing"
```

### MediatR

The waiter acts as the mediator:

```text
Customer
   ↓
Waiter / Mediator
   ↓
Correct person
```

### Database

The restaurant's records contain information.

### Two databases

Imagine:

```text
Operational system
     ↓
Write DB
```

and:

```text
Reporting/dashboard system
     ↓
Read DB
```

### Event

Kitchen says:

> "Order 1001 has been prepared."

```text
OrderPrepared
```

### Message Broker

The restaurant's internal communication system distributes that information.

---

# 64. The Complete Picture

Now connect everything.

```text
                    DESIGN PATTERNS
                          |
             Reusable solutions to
              common design problems
                          |
             ┌────────────┴────────────┐
             ↓                         ↓
         Mediator                   Other Patterns
             |
          MediatR
       (.NET library)
             |
             ↓
            CQRS
             |
      Separate responsibilities
             |
       ┌─────┴─────┐
       ↓           ↓
    COMMAND       QUERY
       ↓           ↓
    Handler      Handler
       ↓           ↓
      WRITE       READ
       ↓           ↓
   ┌────────┐   ┌────────┐
   │ One DB │   │ One DB │
   └────────┘   └────────┘

          OR

       WRITE DB
           ↓
         EVENT
           ↓
    MESSAGE BROKER
           ↓
       CONSUMER
           ↓
        READ DB
           ↓
   EVENTUAL CONSISTENCY
```

---

# 65. The Most Important Distinction

Don't mix these concepts together.

### Design Pattern

```text
A reusable design solution
```

Example:

```text
Mediator Pattern
```

### MediatR

```text
A .NET library
```

that helps implement mediator-style communication.

### CQRS

```text
An architectural/design approach
```

that separates:

```text
Commands
and
Queries
```

### Handler

```text
Code responsible for processing a request
```

### Database

```text
Where data is stored
```

### Event

```text
A message saying something happened
```

### Message Broker

```text
System that transports/stores messages between producers and consumers
```

### Eventual Consistency

```text
Different data stores become consistent after some delay
```

---

# 66. One Final Diagram to Memorize

If you remember only one diagram, remember this:

```text
                         CLIENT
                            |
                            ↓
                     ASP.NET CORE API
                            |
                  ┌─────────┴─────────┐
                  ↓                   ↓
              COMMAND               QUERY
          "Change something"    "Give me data"
                  ↓                   ↓
               MediatR              MediatR
                  ↓                   ↓
          Command Handler      Query Handler
                  ↓                   ↓
              WRITE SIDE          READ SIDE
                  ↓                   ↓
              WRITE DB            READ DB
                  |
                  ↓
                 EVENT
                  |
                  ↓
            MESSAGE BROKER
                  |
                  ↓
               CONSUMER
                  |
                  ↓
               READ DB
```

But remember:

```text
                 CQRS
                  |
          ┌───────┴───────┐
          ↓               ↓
       Command           Query
          ↓               ↓
       Handler           Handler
          ↓               ↓
       Database          Database
```

The two databases are **optional**.

---

# 67. Key Points to Remember

### Design Pattern

> A reusable solution to a common software design problem.

### CQRS

> Separate **commands** from **queries**.

```text
Command → Change
Query   → Read
```

### Command

```text
Create
Update
Delete
Cancel
```

### Query

```text
Get
Search
List
Report
```

### Handler

> Contains the code that processes a command/query.

### MediatR

> A .NET library that can route requests to their appropriate handlers using the Mediator pattern.

### CQRS ≠ MediatR

```text
CQRS  → Design approach
MediatR → .NET library
```

### CQRS ≠ Two Databases

```text
CQRS
 ↓
Separate Read/Write Responsibilities
```

You can have:

```text
CQRS + One DB
```

or:

```text
CQRS + Two DBs
```

### Two databases

Usually:

```text
Write DB
   ↓
Event
   ↓
Message Broker
   ↓
Consumer
   ↓
Read DB
```

### Event

> "Something happened."

```text
OrderCreated
OrderCancelled
PaymentCompleted
```

### Message Broker

Examples:

```text
Kafka
RabbitMQ
Azure Service Bus
```

### Eventual Consistency

> Read DB may take some time to reflect the latest Write DB state.

### Clean Architecture

Provides a way to organize the larger application into boundaries such as:

```text
API
 ↓
Application
 ↓
Domain
 ↑
Infrastructure
 ↓
Database
```

### Most important rule

```text
Small/simple application
        ↓
Keep architecture simple

Complex application
        ↓
Consider CQRS

Complex + high-scale read/write requirements
        ↓
Consider separate read/write models

Distributed system
        ↓
Events + Message Broker
        ↓
Eventual Consistency
```

**The complete mental chain is:**

```text
Design Pattern
      ↓
Mediator Pattern
      ↓
MediatR (optional .NET library)
      ↓
CQRS
      ↓
Command / Query
      ↓
Handler
      ↓
Database
      ↓
One DB OR separate Read/Write DB
      ↓
If separate DBs:
      ↓
Events
      ↓
Message Broker
      ↓
Consumer
      ↓
Read Model / Projection
      ↓
Eventual Consistency
```

That is the **complete picture of how these concepts connect**.

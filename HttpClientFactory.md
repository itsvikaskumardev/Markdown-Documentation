# HttpClient and IHttpClientFactory in .NET

Let's understand this from **zero**, using a microservices example.

The most important idea to remember is:

> **`HttpClient` actually sends HTTP requests. `IHttpClientFactory` creates/manages `HttpClient` instances for you in a safer and cleaner way.**

---

# 1. What is `HttpClient`?

`HttpClient` is a .NET class used to **send HTTP requests to another API/server**.

For example, suppose we have:

```text
Order Service
     |
     | HTTP GET
     ↓
Routing Service
     |
     | Response
     ↓
Order Service
```

The Order Service might call:

```text
GET https://routing-service/routes/123
```

In C#, `HttpClient` is the object that makes this call.

### Simple example

```csharp
using System.Net.Http.Json;

var client = new HttpClient();

var response = await client.GetAsync(
    "https://example.com/api/products");

var products = await response.Content
    .ReadFromJsonAsync<List<Product>>();
```

Here:

```text
HttpClient
   |
   └── sends HTTP request
           |
           ↓
      Other API
           |
           ↓
      HTTP response
```

So you can think of `HttpClient` as a **messenger** between two applications.

---

# 2. What is `IHttpClientFactory`?

`IHttpClientFactory` is a .NET service that **creates `HttpClient` objects for you**.

Instead of doing this everywhere:

```csharp
var client = new HttpClient();
```

you can do:

```csharp
var client = httpClientFactory.CreateClient();
```

The factory manages the underlying HTTP infrastructure and lets you configure clients centrally.

For example:

```csharp
public class RoutingService
{
    private readonly IHttpClientFactory _httpClientFactory;

    public RoutingService(IHttpClientFactory httpClientFactory)
    {
        _httpClientFactory = httpClientFactory;
    }

    public async Task CallRoutingApi()
    {
        var client = _httpClientFactory.CreateClient();

        var response = await client.GetAsync(
            "https://routing-service/routes");
    }
}
```

---

# 3. `HttpClient` vs `IHttpClientFactory`

This is one of the most important differences.

| `HttpClient`               | `IHttpClientFactory`                            |
| -------------------------- | ----------------------------------------------- |
| Actual HTTP client         | Factory that creates `HttpClient`               |
| Sends HTTP requests        | Creates/configures clients                      |
| Can be created with `new`  | Usually obtained through Dependency Injection   |
| Handles HTTP communication | Helps manage HTTP client configuration/lifetime |
| Can be used directly       | Produces `HttpClient` instances                 |

Think of it like this:

```text
IHttpClientFactory
        |
        | creates
        ↓
   HttpClient
        |
        | sends request
        ↓
    Other API
```

So **they aren't competitors**.

You commonly use them **together**.

---

# 4. Why do we use them in .NET?

Imagine you have these microservices:

```text
                   ┌──────────────────┐
                   │   Order Service  │
                   └────────┬─────────┘
                            │
             ┌──────────────┼──────────────┐
             ↓              ↓              ↓
      Routing Service   Payment Service   User Service
```

Order Service needs to communicate with other services.

HTTP is commonly used:

```text
Order Service
     |
     | HTTP
     ↓
Routing Service
```

`HttpClient` allows this communication.

But if you create clients incorrectly:

```csharp
new HttpClient()
new HttpClient()
new HttpClient()
new HttpClient()
```

throughout your application, you can run into connection-management and configuration problems.

`IHttpClientFactory` gives you a standard way to create and configure clients.

---

# 5. Microservice A calling Microservice B

Yes.

This is a very common use case.

Suppose:

```text
Microservice A
Order Service

Microservice B
Routing Service
```

Routing Service exposes:

```http
GET /routes/{orderId}
```

Order Service wants route information.

The flow is:

```text
┌────────────────────┐
│   Order Service    │
│   Microservice A   │
└─────────┬──────────┘
          │
          │ HTTP GET
          │
          ↓
┌────────────────────┐
│  Routing Service   │
│   Microservice B   │
└─────────┬──────────┘
          │
          │ JSON response
          ↓
┌────────────────────┐
│   Order Service    │
└────────────────────┘
```

The Order Service uses `HttpClient`.

And preferably, `HttpClient` is obtained through `IHttpClientFactory`.

---

# 6. How communication works

Suppose Order Service wants:

```text
GET /routes/ORDER123
```

The code might be:

```csharp
var client = _httpClientFactory.CreateClient("RoutingApi");

var response = await client.GetAsync(
    $"/routes/{orderId}",
    cancellationToken);
```

The request travels over the network:

```text
Order Service
     |
     | HTTP GET
     | /routes/ORDER123
     ↓
Routing Service
     |
     | Finds route
     |
     | HTTP 200
     | JSON
     ↓
Order Service
```

For example, Routing Service might return:

```json
{
    "orderId": "ORDER123",
    "routeId": "ROUTE456"
}
```

Then Order Service can deserialize it into a C# object.

---

# 7. Why prefer `IHttpClientFactory` over `new HttpClient()` everywhere?

You may see code like:

```csharp
var client = new HttpClient();
```

It works.

The problem is **how you manage clients at scale**.

A common mistake is creating lots of clients repeatedly:

```csharp
public async Task Method1()
{
    using var client = new HttpClient();
    ...
}

public async Task Method2()
{
    using var client = new HttpClient();
    ...
}
```

This can lead to inefficient connection/resource management and, under load, problems such as socket exhaustion.

`IHttpClientFactory` provides centralized client configuration and manages the underlying handlers appropriately.

For example:

```csharp
builder.Services.AddHttpClient();
```

Then:

```csharp
public class OrderService
{
    private readonly IHttpClientFactory _factory;

    public OrderService(IHttpClientFactory factory)
    {
        _factory = factory;
    }

    public async Task CallApi()
    {
        var client = _factory.CreateClient();

        await client.GetAsync("...");
    }
}
```

### Another major advantage: configuration

Instead of repeating:

```csharp
client.BaseAddress = new Uri(...);
client.Timeout = ...;
client.DefaultRequestHeaders...
```

you can configure it once:

```csharp
builder.Services.AddHttpClient("RoutingApi", client =>
{
    client.BaseAddress = new Uri(
        "https://routing-service.example.com");

    client.Timeout = TimeSpan.FromSeconds(10);
});
```

Then:

```csharp
var client = factory.CreateClient("RoutingApi");
```

The client already has the configuration.

---

# 8. Three common ways to use `HttpClient`

There are three patterns you should understand.

## A. Basic/default client

Register:

```csharp
builder.Services.AddHttpClient();
```

Then:

```csharp
public class OrderService
{
    private readonly IHttpClientFactory _factory;

    public OrderService(IHttpClientFactory factory)
    {
        _factory = factory;
    }

    public async Task CallApi()
    {
        var client = _factory.CreateClient();

        var response = await client.GetAsync(
            "https://example.com/api/orders");
    }
}
```

Use this when you don't need special configuration for different APIs.

---

# 9. Named clients

Suppose you have multiple external APIs:

```text
Order Service
     |
     ├── Routing API
     |
     ├── Payment API
     |
     └── User API
```

You can create named clients.

### Program.cs

```csharp
builder.Services.AddHttpClient("RoutingApi", client =>
{
    client.BaseAddress = new Uri(
        "https://routing-service.example.com");
});

builder.Services.AddHttpClient("PaymentApi", client =>
{
    client.BaseAddress = new Uri(
        "https://payment-service.example.com");
});
```

Then:

```csharp
var routingClient =
    _factory.CreateClient("RoutingApi");
```

And:

```csharp
var paymentClient =
    _factory.CreateClient("PaymentApi");
```

Think:

```text
IHttpClientFactory
       |
       ├── "RoutingApi" → Routing configuration
       |
       ├── "PaymentApi" → Payment configuration
       |
       └── "UserApi"    → User configuration
```

Named clients are useful when you have **multiple APIs with different configurations**.

---

# 10. Typed clients

Typed clients are often very clean for application code.

Instead of doing:

```csharp
_factory.CreateClient("RoutingApi");
```

everywhere, you create a class specifically for the Routing API.

For example:

```csharp
public class RoutingApiClient
{
    private readonly HttpClient _httpClient;

    public RoutingApiClient(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }

    public async Task<RouteDto?> GetRouteAsync(
        string orderId,
        CancellationToken cancellationToken)
    {
        return await _httpClient.GetFromJsonAsync<RouteDto>(
            $"/routes/{orderId}",
            cancellationToken);
    }
}
```

Register it:

```csharp
builder.Services.AddHttpClient<RoutingApiClient>(client =>
{
    client.BaseAddress = new Uri(
        "https://routing-service.example.com");
});
```

Now your application can simply inject:

```csharp
RoutingApiClient
```

instead of dealing with `IHttpClientFactory` directly.

```text
Application
     |
     ↓
RoutingApiClient
     |
     ↓
HttpClient
     |
     ↓
Routing Service
```

### Why typed clients are nice

Your application code becomes:

```csharp
var route = await _routingApiClient
    .GetRouteAsync(orderId, cancellationToken);
```

instead of:

```csharp
var client = _factory.CreateClient("RoutingApi");

var response = await client.GetAsync(...);

var route = await response.Content
    .ReadFromJsonAsync<RouteDto>();
```

The API-specific HTTP code is kept inside `RoutingApiClient`.

---

# 11. Configure `IHttpClientFactory` in `Program.cs`

## Basic

```csharp
builder.Services.AddHttpClient();
```

That's enough to register `IHttpClientFactory`.

Then:

```csharp
public class MyService
{
    private readonly IHttpClientFactory _factory;

    public MyService(IHttpClientFactory factory)
    {
        _factory = factory;
    }
}
```

---

## Named

```csharp
builder.Services.AddHttpClient("RoutingApi", client =>
{
    client.BaseAddress = new Uri(
        "https://routing-service.example.com");

    client.Timeout = TimeSpan.FromSeconds(10);
});
```

---

## Typed

```csharp
builder.Services.AddHttpClient<RoutingApiClient>(client =>
{
    client.BaseAddress = new Uri(
        "https://routing-service.example.com");
});
```

---

# 12. Complete Microservice Example

Let's create two simple services.

```text
┌─────────────────────────┐
│     Order Service       │
│   Microservice A        │
│                         │
│   GET /orders/{id}      │
└────────────┬────────────┘
             │
             │ HTTP GET
             ↓
┌─────────────────────────┐
│     Routing Service     │
│   Microservice B        │
│                         │
│   GET /routes/{id}      │
└─────────────────────────┘
```

---

## Microservice B — Routing Service

It exposes:

```http
GET /routes/{orderId}
```

Example:

```csharp
app.MapGet("/routes/{orderId}", (string orderId) =>
{
    var route = new
    {
        OrderId = orderId,
        RouteId = "ROUTE-100"
    };

    return Results.Ok(route);
});
```

Request:

```http
GET /routes/ORDER-123
```

Response:

```json
{
    "orderId": "ORDER-123",
    "routeId": "ROUTE-100"
}
```

---

# 13. Microservice A — Order Service

Create a DTO:

```csharp
public class RouteDto
{
    public string OrderId { get; set; } = "";
    public string RouteId { get; set; } = "";
}
```

Configure the client:

```csharp
builder.Services.AddHttpClient("RoutingApi", client =>
{
    client.BaseAddress = new Uri(
        "https://localhost:7002");
});
```

Create service:

```csharp
public class RoutingService
{
    private readonly IHttpClientFactory _factory;

    public RoutingService(IHttpClientFactory factory)
    {
        _factory = factory;
    }

    public async Task<RouteDto?> GetRouteAsync(
        string orderId,
        CancellationToken cancellationToken)
    {
        var client = _factory.CreateClient("RoutingApi");

        return await client.GetFromJsonAsync<RouteDto>(
            $"/routes/{orderId}",
            cancellationToken);
    }
}
```

Register:

```csharp
builder.Services.AddScoped<RoutingService>();
```

Then endpoint:

```csharp
app.MapGet("/orders/{orderId}/route",
    async (
        string orderId,
        RoutingService routingService,
        CancellationToken cancellationToken) =>
    {
        var route = await routingService.GetRouteAsync(
            orderId,
            cancellationToken);

        return Results.Ok(route);
    });
```

---

# 14. Complete Request Flow

Suppose the frontend calls:

```http
GET /orders/ORDER-123/route
```

The flow becomes:

```text
              Browser / Frontend
                      |
                      | GET
                      ↓
              ┌───────────────┐
              │ Order Service │
              └───────┬───────┘
                      |
                      | IHttpClientFactory
                      ↓
                  HttpClient
                      |
                      | GET /routes/ORDER-123
                      ↓
              ┌─────────────────┐
              │ Routing Service │
              └────────┬────────┘
                       |
                       | JSON
                       ↓
              { "routeId": "ROUTE-100" }
                       |
                       ↓
                  Order Service
                       |
                       ↓
                    Frontend
```

The important thing is:

**`IHttpClientFactory` doesn't magically communicate with the other service.**

It gives you an `HttpClient`.

**`HttpClient` sends the HTTP request.**

---

# 15. What is `BaseAddress`?

Suppose the API URL is:

```text
https://routing-service.example.com
```

You can configure:

```csharp
client.BaseAddress = new Uri(
    "https://routing-service.example.com");
```

Then:

```csharp
await client.GetAsync("/routes/123");
```

becomes:

```text
https://routing-service.example.com/routes/123
```

Without `BaseAddress`, you'd have to write:

```csharp
await client.GetAsync(
    "https://routing-service.example.com/routes/123");
```

every time.

---

# 16. HTTP GET

Used to **retrieve data**.

```csharp
var response = await client.GetAsync(
    "/routes/123",
    cancellationToken);
```

Or:

```csharp
var route = await client.GetFromJsonAsync<RouteDto>(
    "/routes/123",
    cancellationToken);
```

---

# 17. HTTP POST

Used to **create/send data**.

Suppose Routing Service has:

```http
POST /routes
```

You can do:

```csharp
var request = new
{
    OrderId = "ORDER-123",
    DriverId = "DRIVER-10"
};

var response = await client.PostAsJsonAsync(
    "/routes",
    request,
    cancellationToken);
```

JSON sent:

```json
{
    "orderId": "ORDER-123",
    "driverId": "DRIVER-10"
}
```

---

# 18. HTTP PUT

Used to update an existing resource.

```csharp
var request = new
{
    DriverId = "DRIVER-20"
};

var response = await client.PutAsJsonAsync(
    "/routes/123",
    request,
    cancellationToken);
```

---

# 19. HTTP DELETE

Used to delete something.

```csharp
var response = await client.DeleteAsync(
    "/routes/123",
    cancellationToken);
```

---

# 20. JSON Serialization

Microservices commonly communicate using JSON.

For example:

```csharp
var order = new
{
    Id = "ORDER-123",
    Amount = 500
};
```

When using:

```csharp
await client.PostAsJsonAsync(
    "/orders",
    order,
    cancellationToken);
```

.NET converts the object into JSON:

```json
{
    "id": "ORDER-123",
    "amount": 500
}
```

Similarly, when receiving JSON:

```json
{
    "orderId": "ORDER-123",
    "routeId": "ROUTE-100"
}
```

you can convert it into:

```csharp
RouteDto
```

using:

```csharp
ReadFromJsonAsync<RouteDto>()
```

---

# 21. Headers

HTTP requests can contain headers.

For example:

```text
Authorization
Content-Type
Accept
```

You can add a header:

```csharp
client.DefaultRequestHeaders.Add(
    "X-Service-Name",
    "OrderService");
```

For an authorization token:

```csharp
client.DefaultRequestHeaders.Authorization =
    new AuthenticationHeaderValue(
        "Bearer",
        token);
```

Request:

```text
GET /routes/123

Authorization: Bearer eyJ...
```

This is commonly used when Microservice A needs to authenticate with Microservice B.

---

# 22. `CancellationToken`

You'll often see:

```csharp
CancellationToken cancellationToken
```

Example:

```csharp
var response = await client.GetAsync(
    "/routes/123",
    cancellationToken);
```

It allows the operation to be cancelled.

For example:

```text
User request
     |
     ↓
Order Service
     |
     | calls Routing Service
     |
     ↓
Routing Service
```

If the original request is cancelled, the cancellation token can propagate through your application and cancel the HTTP operation.

This is particularly useful for long-running requests and helps avoid doing unnecessary work.

---

# 23. What happens if Microservice B is down?

Suppose:

```text
Order Service
      |
      | HTTP
      X
Routing Service DOWN
```

The HTTP request can fail with an exception such as:

```text
HttpRequestException
```

You should handle failures appropriately.

Example:

```csharp
try
{
    var response = await client.GetAsync(
        "/routes/123",
        cancellationToken);

    response.EnsureSuccessStatusCode();

    var route = await response.Content
        .ReadFromJsonAsync<RouteDto>(
            cancellationToken);

    return route;
}
catch (HttpRequestException ex)
{
    // Log the failure
    throw;
}
```

---

# 24. What about HTTP 400, 401, 404, 500?

These are **HTTP responses**, not necessarily network exceptions.

For example:

```text
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
500 Internal Server Error
```

You can inspect:

```csharp
var response = await client.GetAsync(
    "/routes/123",
    cancellationToken);

if (response.IsSuccessStatusCode)
{
    // 2xx
}
else
{
    // Error response
}
```

Or:

```csharp
response.EnsureSuccessStatusCode();
```

`EnsureSuccessStatusCode()` throws when the response isn't a successful HTTP status.

For example:

```text
200 → success
201 → success
204 → success

400 → exception
401 → exception
404 → exception
500 → exception
```

when using `EnsureSuccessStatusCode()`.

---

# 25. Important: Network failure vs HTTP error

These are different.

### Case 1 — Routing Service is down

```text
Order Service
      |
      X
 Routing Service
     DOWN
```

You may get:

```text
HttpRequestException
```

because a response could not be obtained.

### Case 2 — Routing Service is running but returns 404

```text
Order Service
      |
      | GET /routes/999
      ↓
Routing Service
      |
      ↓
404 Not Found
```

You **did receive an HTTP response**.

It's just an unsuccessful status.

This distinction is important.

---

# 26. Handling specific status codes

You can do:

```csharp
var response = await client.GetAsync(
    $"/routes/{orderId}",
    cancellationToken);

if (response.StatusCode == HttpStatusCode.NotFound)
{
    return null;
}

if (response.StatusCode == HttpStatusCode.Unauthorized)
{
    throw new UnauthorizedAccessException();
}

if (response.StatusCode == HttpStatusCode.BadRequest)
{
    // Handle bad request
}

if (response.StatusCode == HttpStatusCode.InternalServerError)
{
    // Handle server error
}
```

Or generally:

```csharp
if (!response.IsSuccessStatusCode)
{
    // Handle error
}
```

---

# 27. What if Microservice B is temporarily unavailable?

This is where production microservice architecture becomes more interesting.

You may use:

```text
Timeout
Retry
Circuit Breaker
Fallback
Logging
```

For example:

```text
Order Service
     |
     ↓
Routing Service
     |
     X
     DOWN
```

Instead of immediately failing every request, a resilience policy can sometimes retry transient failures.

Modern .NET applications can integrate resilience handling with `HttpClient` using the .NET resilience tooling.

Conceptually:

```text
HTTP Request
     |
     ↓
Retry / Timeout / Circuit Breaker
     |
     ↓
HttpClient
     |
     ↓
Routing Service
```

But **don't blindly retry every error**.

For example, retrying a `POST` that creates an order can potentially create duplicate operations if the operation isn't designed to be idempotent.

---

# 28. Named Client vs Typed Client

Here's the easiest way to remember it.

### Named Client

You identify the configuration by a string:

```csharp
var client = factory.CreateClient("RoutingApi");
```

Configuration:

```csharp
builder.Services.AddHttpClient("RoutingApi", client =>
{
    client.BaseAddress = new Uri(
        "https://routing-service");
});
```

Think:

```text
"RoutingApi"
     ↓
HttpClient
```

---

### Typed Client

You create a class:

```csharp
public class RoutingApiClient
{
    private readonly HttpClient _client;

    public RoutingApiClient(HttpClient client)
    {
        _client = client;
    }
}
```

Configuration:

```csharp
builder.Services.AddHttpClient<RoutingApiClient>(client =>
{
    client.BaseAddress = new Uri(
        "https://routing-service");
});
```

Then:

```text
Order Service
      |
      ↓
RoutingApiClient
      |
      ↓
HttpClient
      |
      ↓
Routing Service
```

Typed clients are particularly useful when you want to encapsulate the API-specific operations.

---

# 29. Your real-world code pattern

The code you've been working with is similar to this:

```csharp
public class RoutingService(
    IHttpClientFactory httpClientFactory,
    IOptions<RoutingApiOptions> options,
    ILogger<RoutingService> logger)
    : IRoutingService
{
    public async Task<Dictionary<string, string>>
        GetClusterMappingsAsync(
            IReadOnlyList<string> uniqueOrderIds,
            CancellationToken ct)
    {
        var client =
            httpClientFactory.CreateClient("RoutingApi");

        // Call Routing API
    }
}
```

Here:

```text
IHttpClientFactory
        |
        | CreateClient("RoutingApi")
        ↓
   HttpClient
        |
        | HTTP request
        ↓
   Routing API
```

And:

```csharp
IOptions<RoutingApiOptions>
```

is being used for configuration, such as the routing API URL.

`ILogger` is for logging.

`CancellationToken` is for cancellation.

`IRoutingService` is your application abstraction.

So these things have different responsibilities:

```text
IRoutingService
      |
      ↓
RoutingService
      |
      ├── IHttpClientFactory
      |       |
      |       ↓
      |    HttpClient
      |       |
      |       ↓
      |    Routing API
      |
      ├── IOptions
      |       ↓
      |   Configuration
      |
      └── ILogger
              ↓
            Logs
```

---

# 30. Common mistakes

## Mistake 1 — Creating `new HttpClient()` everywhere

Avoid:

```csharp
var client = new HttpClient();
```

throughout your application.

Prefer:

```csharp
var client = _factory.CreateClient("RoutingApi");
```

or a typed client.

---

## Mistake 2 — Hardcoding URLs everywhere

Avoid:

```csharp
await client.GetAsync(
    "https://my-routing-service.com/routes/123");
```

everywhere.

Prefer configuration:

```csharp
builder.Services.AddHttpClient("RoutingApi", client =>
{
    client.BaseAddress = new Uri(
        configuration["RoutingApi:BaseUrl"]!);
});
```

Then:

```csharp
await client.GetAsync("/routes/123");
```

---

## Mistake 3 — Ignoring HTTP status codes

Don't assume:

```csharp
var response = await client.GetAsync(...);
```

means success.

Always consider:

```csharp
response.IsSuccessStatusCode
```

or:

```csharp
response.EnsureSuccessStatusCode();
```

---

## Mistake 4 — No timeout/resilience strategy

If another service becomes slow:

```text
Order Service
      |
      ↓
Routing Service
      |
      |
      | hangs...
      |
      ↓
Order Service waits...
```

Configure sensible timeouts and appropriate resilience policies.

---

## Mistake 5 — Not passing `CancellationToken`

Prefer:

```csharp
await client.GetAsync(
    "/routes/123",
    cancellationToken);
```

rather than ignoring cancellation.

---

# 31. Best practices

For microservices, a good general structure is:

```text
                 Order Service
                      |
                      ↓
              RoutingApiClient
                      |
                      ↓
                 HttpClient
                      |
                      ↓
              Routing Service
```

And configure the client centrally:

```csharp
builder.Services.AddHttpClient<RoutingApiClient>(client =>
{
    client.BaseAddress = new Uri(
        configuration["RoutingApi:BaseUrl"]!);

    client.Timeout = TimeSpan.FromSeconds(10);
});
```

Then keep API-specific code inside the client:

```csharp
public class RoutingApiClient
{
    private readonly HttpClient _client;

    public RoutingApiClient(HttpClient client)
    {
        _client = client;
    }

    public async Task<RouteDto?> GetRouteAsync(
        string orderId,
        CancellationToken ct)
    {
        return await _client.GetFromJsonAsync<RouteDto>(
            $"/routes/{orderId}",
            ct);
    }
}
```

---

# 32. The most important mental model

Remember these four levels:

```text
┌─────────────────────────────────────┐
│        Your Application Code        │
│                                     │
│  "I need route information"         │
└──────────────────┬──────────────────┘
                   ↓
┌─────────────────────────────────────┐
│       RoutingApiClient / Service    │
│                                     │
│   GetRouteAsync(orderId)            │
└──────────────────┬──────────────────┘
                   ↓
┌─────────────────────────────────────┐
│             HttpClient              │
│                                     │
│       Sends HTTP request             │
└──────────────────┬──────────────────┘
                   ↓
┌─────────────────────────────────────┐
│         Routing Microservice        │
│                                     │
│       GET /routes/{orderId}         │
└─────────────────────────────────────┘
```

And `IHttpClientFactory` sits behind the client creation/configuration:

```text
                 IHttpClientFactory
                         |
                         | creates
                         ↓
                    HttpClient
                         |
                         | HTTP
                         ↓
                 Other Microservice
```

---

# 33. Final summary

If you remember only this, remember:

### `HttpClient`

**Does the actual HTTP communication.**

```csharp
await client.GetAsync("/routes/123");
```

### `IHttpClientFactory`

**Creates and manages/configures `HttpClient` instances.**

```csharp
var client = factory.CreateClient("RoutingApi");
```

### In microservices

If:

```text
Order Service → Routing Service
```

you commonly use:

```text
IHttpClientFactory
        ↓
    HttpClient
        ↓
HTTP Request
        ↓
Routing Service
```

### Three common approaches

```text
Basic
AddHttpClient()
      ↓
CreateClient()

Named
AddHttpClient("RoutingApi")
      ↓
CreateClient("RoutingApi")

Typed
AddHttpClient<RoutingApiClient>()
      ↓
Inject RoutingApiClient
```

For a real microservice application, **typed clients or well-organized named clients + `IHttpClientFactory`** are generally preferable to scattering `new HttpClient()` throughout your code.

The key sentence for your study notes is:

> **`HttpClient` is responsible for sending HTTP requests, while `IHttpClientFactory` provides a managed and configurable way to create `HttpClient` instances. In microservices, Service A can use an `HttpClient` created by `IHttpClientFactory` to communicate with Service B over HTTP.**

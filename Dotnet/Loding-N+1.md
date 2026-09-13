**Lazy Loading** ka simple meaning hai:

> **Kisi cheez ko immediately load/create/fetch mat karo. Jab actually uski need ho tab load karo.**

Iska opposite hai **Eager Loading**:

> **Start mein hi sab kuch load kar do, chahe abhi need ho ya nahi.**

### Real technical example

Suppose database mein:

```text
User
 ├── Id
 ├── Name
 └── Orders
       ├── Order 1
       ├── Order 2
       └── Order 3
```

Aap code likhte ho:

```csharp
var user = db.Users.First();
```

Agar `Orders` **lazy-loaded** hain, initially sirf:

```text
User
 ↓
Database
 ↓
User data
```

load hoga.

`Orders` database se **abhi nahi aayenge**.

Baad mein jab code actually access kare:

```csharp
var orders = user.Orders;
```

tab ORM, for example EF Core, ko pata chalega:

> "Oh, `Orders` ki zarurat ab hui."

Then database query execute ho sakti hai:

```sql
SELECT *
FROM Orders
WHERE UserId = 1;
```

So:

```text
Application starts
       │
       ▼
Get User
       │
       ▼
User loaded
Orders NOT loaded
       │
       │
       │  user.Orders
       ▼
Orders needed
       │
       ▼
Database query
       │
       ▼
Orders loaded
```

### Why is it called "Lazy"?

Because the object is basically saying:

> **"Main abhi kaam nahi karunga. Jab tum mujhe actually access karoge tab karunga."**

---

## Important: Lazy Loading ≠ Async

Ye confusion bahut common hai.

```csharp
await db.Users.ToListAsync();
```

`async/await` ka relation **waiting/thread execution** se hai.

Lazy loading ka relation **WHEN something is loaded** se hai.

For example:

```text
Lazy Loading
    ↓
"When should I fetch/create this?"

Async/Await
    ↓
"How should I wait while an operation is running?"
```

They can be used together, but they are **different concepts**.

---

## EF Core Implementation

EF Core mein lazy loading commonly navigation property access karne par related data load kar sakta hai.

### Database Models Setup

To understand this deeply, let's set up a complete example. Suppose we have the following entity structure in our application:

```csharp
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Order> Orders { get; set; }
        = new List<Order>();
}

public class Order
{
    public int Id { get; set; }
    public decimal Amount { get; set; }

    public int UserId { get; set; }
    public virtual User User { get; set; }
}
```

### Lazy Loading vs Eager Loading in Practice

Without lazy loading enabled, if you just query the user:

```csharp
var user = await db.Users
    .FirstAsync();

// Accessing the navigation property
var userOrders = user.Orders;
```

`Orders` automatically load nahi honge. 

However, with **Lazy Loading** enabled (usually via proxies), writing `user.Orders` can cause another database query to execute at that exact moment.

Aap explicitly **Eager Loading** bhi kar sakte ho, jisme start mein hi related data load ho jata hai:

```csharp
var user = await db.Users
    .Include(x => x.Orders)
    .FirstAsync();
```

```text
Eager Loading Flow:
Query User + Orders
       ↓
Everything loaded immediately


Lazy Loading Flow:
Query User
   ↓
User loaded
   ↓
Later: access user.Orders
   ↓
Orders query generated & executed
```

---

## The Famous N+1 Query Problem

Agar aap lazy loading ko loop mein use karte ho, to ek major performance issue create hota hai jise **N+1 Query Problem** kehte hain.

**N+1 Query** ka matlab hai:

> **Pehle 1 query se main data lao, phir har ek item ke liye 1 extra query chalao.**
> Isliye total queries = **1 + N**.

### Detailed EF Core Example

Suppose database mein **100 users** hain. Aapne following code likha:

```csharp
var users = await db.Users.ToListAsync();

foreach (var user in users)
{
    Console.WriteLine(user.Orders.Count);
}
```

Agar `Orders` navigation property **lazy-loaded** hai, toh yahan kya hota hai:

**Step 1:** Initial Query
```text
Query 1:
SELECT * FROM Users;
```
Aapko 100 users mil gaye. (This is the "1" query).

**Step 2:** The Loop
Jab loop chalta hai, har iteration mein `user.Orders` access hota hai:
```text
User 1 → SELECT * FROM Orders WHERE UserId = 1
User 2 → SELECT * FROM Orders WHERE UserId = 2
User 3 → SELECT * FROM Orders WHERE UserId = 3
...
User 100 → SELECT * FROM Orders WHERE UserId = 100
```
This triggers 100 separate queries. (This is the "N" queries).

So visually, the execution looks like this:

```text
ToListAsync()
     │
     ▼
┌─────────────────────┐
│ SELECT Users        │
└─────────────────────┘
     │
     ▼
  100 Users
     │
     ├── User 1 → user.Orders → SQL query
     ├── User 2 → user.Orders → SQL query
     ├── User 3 → user.Orders → SQL query
     │
     └── User 100 → user.Orders → SQL query

1 (Users) + 100 (Orders) = 101 total queries
```

---

## Why is N+1 Bad?

Imagine the scaling impact:

```text
N = 10 users
→ 11 queries       😐

N = 1,000 users
→ 1,001 queries     😐

N = 100,000 users
→ 100,001 queries   💀
```

Every database query has overhead:

```text
Application
    │
    │ Network/request
    ▼
Database
    │
    │ Execute query
    ▼
Database result
    │
    ▼
Application
```

Doing this thousands of times can make an API **very slow** and put unnecessary load on the database.

---

## How do we avoid the N+1 Problem?

We should avoid triggering queries inside loops. There are two primary solutions: **Eager Loading** and **Projection**.

### Solution 1: Eager Loading using `Include()`

Usually, use **Eager Loading** with `Include()` when you know you need the related entity data:

```csharp
var users = await db.Users
    .Include(u => u.Orders)
    .ToListAsync();

foreach (var user in users)
{
    // Orders are already loaded in memory, no extra query is made!
    Console.WriteLine(user.Orders.Count);
}
```

Conceptually:

```text
BAD (Lazy inside Loop)          GOOD (Eager)
────────────────────────        ────────────────────────
Users                           Users + Orders
  ↓                                   ↓
1 query                           database
                                      ↓
User 1 → Orders → query         required data loaded
User 2 → Orders → query         No query per user
User 3 → Orders → query
...
User N → Orders → query

Total = N + 1 queries           Total = 1 query
```

EF Core can fetch the related data efficiently rather than triggering one query for every user.

### Solution 2: Projection using `Select()` (Better API Approach)

In real ASP.NET Core APIs, you often **don't actually need every full `Order` object**—you might only need the `OrderCount`, or specific fields. 

Using projection (`Select`) is highly efficient:

```csharp
var users = await db.Users
    .Select(u => new UserDto
    {
        Id = u.Id,
        Name = u.Name,
        OrderCount = u.Orders.Count()
    })
    .ToListAsync();
```

This translates directly into an optimized SQL query (like using a `COUNT` aggregate), completely avoiding N+1 and keeping memory usage low because full entities aren't tracked.

---

## Summary Flow

To build efficient applications, remember how this loading chain works and how to break it:

```text
Database
   ↓
EF Core
   ↓
Navigation Property
   ↓
Lazy Loading
   ↓
Access navigation property
   ↓
Automatic SQL query
   ↓
Loop
   ↓
N+1 Problem
   ↓
Solution 1: Include()
   ↓
Solution 2: Projection (Select)
   ↓
Efficient API
```

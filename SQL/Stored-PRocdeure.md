

# Stored Procedure kya hoti hai?

**Stored Procedure ek SQL statements ka saved/reusable group hota hai jo database ke andar store hota hai.**

Simple words mein:

> Agar koi SQL ka kaam baar-baar karna hai, to us SQL logic ko database mein ek naam dekar save kar sakte ho. Us saved SQL program ko **Stored Procedure** kehte hain.

Normal query:

```sql
SELECT *
FROM "Orders"
WHERE "Status" = 'Delivered';
```

Ye query tum baar-baar manually likh sakte ho.

Lekin agar tum is logic ko database mein save kar do:

```text
GetDeliveredOrders
```

to baad mein simply procedure call kar sakte ho:

```sql
CALL GetDeliveredOrders();
```

---

# 1. Real-life example

Maan lo tumhare paas `Orders` table hai:

| OrderId | CustomerId | Status    | Amount |
| ------: | ---------: | --------- | -----: |
|     101 |          1 | Delivered |   5000 |
|     102 |          2 | Pending   |   3000 |
|     103 |          1 | Delivered |   7000 |
|     104 |          3 | Cancelled |   2000 |

Tumhe frequently **delivered orders** nikalne hain.

Normally:

```sql
SELECT *
FROM "Orders"
WHERE "Status" = 'Delivered';
```

Agar ye logic 20 different places se use ho raha hai, to same SQL baar-baar likhna unnecessary ho sakta hai.

Instead database mein procedure bana sakte ho:

```sql
CREATE PROCEDURE GetDeliveredOrders()
LANGUAGE SQL
AS $$
    SELECT *
    FROM "Orders"
    WHERE "Status" = 'Delivered';
$$;
```

Then:

```sql
CALL GetDeliveredOrders();
```

---

# 2. Stored Procedure ka basic syntax

PostgreSQL mein basic syntax:

```sql
CREATE PROCEDURE procedure_name(parameters)
LANGUAGE SQL
AS $$
    SQL statements;
$$;
```

Example:

```sql
CREATE PROCEDURE GetDeliveredOrders()
LANGUAGE SQL
AS $$
    SELECT *
    FROM "Orders"
    WHERE "Status" = 'Delivered';
$$;
```

Yahan:

```text
CREATE PROCEDURE
       ↓
GetDeliveredOrders
       ↓
()
       ↓
LANGUAGE SQL
       ↓
AS $$
   SQL logic
$$
```

---

# 3. Procedure ko execute kaise karte hain?

Procedure create karne ke baad:

```sql
CALL GetDeliveredOrders();
```

`CALL` ka matlab:

> Is stored procedure ko execute/run karo.

---

# 4. Parameters kyun chahiye?

Ab maan lo tum sirf Delivered orders nahi, **kisi bhi status ke orders** nikalna chahte ho.

Instead of creating:

```text
GetDeliveredOrders
GetPendingOrders
GetCancelledOrders
```

Hum ek parameter pass kar sakte hain.

Example:

```sql
CREATE PROCEDURE GetOrdersByStatus(order_status VARCHAR)
LANGUAGE SQL
AS $$
    SELECT *
    FROM "Orders"
    WHERE "Status" = order_status;
$$;
```

Ab:

```sql
CALL GetOrdersByStatus('Delivered');
```

Result:

| OrderId | CustomerId | Status    | Amount |
| ------: | ---------: | --------- | -----: |
|     101 |          1 | Delivered |   5000 |
|     103 |          1 | Delivered |   7000 |

Aur:

```sql
CALL GetOrdersByStatus('Pending');
```

Result:

| OrderId | CustomerId | Status  | Amount |
| ------: | ---------: | ------- | -----: |
|     102 |          2 | Pending |   3000 |

So procedure **reusable** ho gayi.

---

# 5. Stored Procedure mein multiple SQL statements bhi ho sakti hain

Ye important point hai.

Stored procedure sirf ek `SELECT` ke liye nahi hoti.

Ek procedure ke andar multiple operations ho sakte hain.

For example:

```text
Create Order
      ↓
Insert Order
      ↓
Insert OrderItems
      ↓
Update something
      ↓
Commit / transaction logic
```

Example conceptually:

```sql
CREATE PROCEDURE CreateOrder(...)
LANGUAGE plpgsql
AS $$
BEGIN

    INSERT INTO "Orders" (...);

    INSERT INTO "OrderItems" (...);

    UPDATE ...;

END;
$$;
```

Yaani procedure ke andar **business/database logic ka complete sequence** ho sakta hai.

---

# 6. SQL vs Stored Procedure

Ye difference samajhna important hai.

### Normal SQL

```sql
SELECT *
FROM "Orders"
WHERE "CustomerId" = 1;
```

Database ko query bheji:

```text
Application
     ↓
SQL Query
     ↓
Database
     ↓
Result
```

### Stored Procedure

Pehle database mein logic save:

```text
Database
   │
   └── GetOrdersByCustomer()
```

Application:

```sql
CALL GetOrdersByCustomer(1);
```

Flow:

```text
Application
     ↓
CALL Procedure
     ↓
Database
     ↓
Stored Procedure executes
     ↓
SQL statements execute
     ↓
Result
```

---

# 7. Stored Procedure kyun use karte hain?

Main reasons:

## 1. Reusability

Same logic baar-baar use karna ho.

Instead of:

```sql
SELECT ...
WHERE ...
JOIN ...
GROUP BY ...
HAVING ...
```

har jagah likhne ke:

```sql
CALL GetCustomerOrderSummary(1);
```

---

## 2. Complex database logic

Kabhi database operation simple nahi hota.

Example:

```text
Create Order
    ↓
Check Customer
    ↓
Insert Order
    ↓
Insert Items
    ↓
Calculate total
    ↓
Update stock
    ↓
Create transaction record
```

Aise database-heavy logic ko procedure mein encapsulate kiya ja sakta hai.

---

## 3. Centralized logic

Suppose 3 applications database use kar rahi hain:

```text
Web API
   \
Mobile App ----> Database
   /
Admin App
```

Agar important database logic procedure mein hai:

```text
              Database
                  │
          ┌───────┴────────┐
          │ Stored Procedure│
          └───────┬────────┘
                  │
            Common Logic
```

To different applications same database logic call kar sakti hain.

---

## 4. Security

Stored procedures database permissions ke saath useful ho sakti hain.

For example, application ko directly table modification permission dene ke bajaye controlled procedure execution permission diya ja sakta hai.

Concept:

```text
Application
     │
     │ CALL CreateOrder()
     ↓
Stored Procedure
     │
     ↓
Orders / OrderItems
```

Application ko direct tables par unrestricted access dena zaroori nahi.

**Lekin:** security automatically guaranteed nahi hoti. Proper permissions aur procedure design karna zaroori hai.

---

# 8. Stored Procedure aur Function same hain?

**Nahi.**

PostgreSQL mein dono alag concepts hain.

### Function

Generally value/result return karne ke liye use hoti hai.

```sql
CREATE FUNCTION ...
```

Call:

```sql
SELECT function_name(...);
```

### Procedure

Database operation/process execute karne ke liye.

```sql
CREATE PROCEDURE ...
```

Call:

```sql
CALL procedure_name(...);
```

Simple interview difference:

| Function                                          | Procedure                                        |
| ------------------------------------------------- | ------------------------------------------------ |
| Usually value/result return karti hai             | Procedure ek operation/process perform karti hai |
| `SELECT` se call kar sakte hain                   | `CALL` se execute karte hain                     |
| PostgreSQL mein `CREATE FUNCTION`                 | PostgreSQL mein `CREATE PROCEDURE`               |
| Functions expressions/query ke part ho sakti hain | Procedures standalone call hoti hain             |
| Return value/function result important            | Side-effecting operations commonly important     |

**Note:** Exact capabilities database system ke according different hoti hain.

---

# 9. Procedure mein `IF`, variables etc. bhi ho sakte hain

Agar `PL/pgSQL` use karo to procedure programming-language jaisi capabilities bhi de sakti hai.

Example:

```sql
CREATE PROCEDURE CheckOrderStatus(order_id INT)
LANGUAGE plpgsql
AS $$
DECLARE
    order_status VARCHAR;
BEGIN

    SELECT "Status"
    INTO order_status
    FROM "Orders"
    WHERE "OrderId" = order_id;

    IF order_status = 'Delivered' THEN
        RAISE NOTICE 'Order is delivered';
    ELSE
        RAISE NOTICE 'Order is not delivered';
    END IF;

END;
$$;
```

Call:

```sql
CALL CheckOrderStatus(101);
```

Yahan:

```text
Get Order Status
       ↓
Store in variable
       ↓
IF condition
       ↓
Check status
       ↓
Perform action
```

Isliye Stored Procedure ko tum **database ke andar ek small program** ki tarah samajh sakte ho.

---

# 10. `Orders` + `OrderItems` ka practical example

Suppose:

### Orders

| OrderId | CustomerId | Status    |
| ------: | ---------: | --------- |
|     101 |          1 | Delivered |
|     102 |          2 | Pending   |
|     103 |          1 | Delivered |

### OrderItems

| OrderItemId | OrderId | Product  | Quantity | Price |
| ----------: | ------: | -------- | -------: | ----: |
|           1 |     101 | Laptop   |        1 | 50000 |
|           2 |     101 | Mouse    |        2 |  1000 |
|           3 |     102 | Keyboard |        1 |  2000 |
|           4 |     103 | Laptop   |        1 | 50000 |

Suppose requirement:

> Customer ke orders ka total amount chahiye.

Normal query:

```sql
SELECT
    o."CustomerId",
    SUM(oi."Quantity" * oi."Price") AS "TotalAmount"
FROM "Orders" o
JOIN "OrderItems" oi
    ON o."OrderId" = oi."OrderId"
WHERE o."CustomerId" = 1
GROUP BY o."CustomerId";
```

Ab agar ye calculation frequently use hoti hai, to procedure/function approach consider kar sakte ho depending on requirement.

---

# 11. Important: Stored Procedure ko `.NET` code mein add kar sakte hain?

**Haan.**

Ye tumhare question ka important part hai.

Suppose database mein procedure hai:

```sql
CREATE PROCEDURE GetOrdersByStatus(order_status VARCHAR)
LANGUAGE SQL
AS $$
    SELECT *
    FROM "Orders"
    WHERE "Status" = order_status;
$$;
```

.NET application se database ko call karna padega.

Concept:

```text
.NET API
   │
   │ CALL GetOrdersByStatus('Delivered')
   ↓
PostgreSQL
   │
   ↓
Stored Procedure
   │
   ↓
Orders table
   │
   ↓
Result
   ↓
.NET API
```

EF Core / ADO.NET se stored procedure call ki ja sakti hai.

---

# 12. EF Core mein kaise call karte hain?

Yahan ek important distinction hai:

**EF Core ka raw SQL API aur PostgreSQL procedure ek hi cheez nahi hain.**

For example, non-query database command ke liye EF Core mein:

```csharp
await dbContext.Database.ExecuteSqlRawAsync(
    "CALL GetSomething()"
);
```

Parameters ke saath parameterized approach use karni chahiye.

For example:

```csharp
await dbContext.Database.ExecuteSqlInterpolatedAsync(
    $"CALL GetSomething({someValue})"
);
```

Lekin agar procedure **rows return** kar rahi hai, to PostgreSQL mein functions/query-returning patterns aur EF Core APIs ka choice thoda different ho sakta hai.

Isliye practical project mein pehle decide karte hain:

```text
Need database operation?
        ↓
Procedure may fit

Need query/result set?
        ↓
Function / normal SQL / EF LINQ may fit
```

---

# 13. Stored Procedure vs EF Core LINQ

Tumhare .NET work ke perspective se ye bahut important comparison hai.

### LINQ

```csharp
var orders = await dbContext.Orders
    .Where(x => x.Status == "Delivered")
    .ToListAsync();
```

EF Core internally SQL generate karega:

```sql
SELECT ...
FROM "Orders"
WHERE "Status" = 'Delivered';
```

### Stored Procedure

Database mein pehle:

```sql
CREATE PROCEDURE ...
```

Then application:

```text
.NET
 ↓
CALL procedure
 ↓
PostgreSQL
```

So:

```text
LINQ
.NET code mein query logic
        ↓
EF Core SQL generate karta hai


Stored Procedure
Database mein SQL logic
        ↓
.NET usko call karta hai
```

---

# 14. Kya har SQL query ko Stored Procedure banana chahiye?

**Nahi.**

Ye bahut important interview point hai.

Stored procedure koi rule nahi hai ki:

> "Complex query hai → Stored Procedure banao."

Decision project requirement par depend karta hai.

Aajkal applications mein commonly:

```text
Simple CRUD
   ↓
EF Core + LINQ
```

use hota hai.

For example:

```csharp
var orders = await dbContext.Orders
    .Where(x => x.Status == "Pending")
    .ToListAsync();
```

Stored procedure tab useful ho sakti hai jab:

* database-side logic complex ho
* same database operation repeatedly use ho
* legacy system already procedures use karta ho
* multiple applications same database logic share karti hon
* database-side transaction/operation encapsulation useful ho
* performance/DB-specific optimization ki specific need ho

---

# 15. Stored Procedure ke disadvantages bhi hain

Interview mein sirf advantages mat bolna.

### 1. Database dependency

Procedure PostgreSQL syntax mein likhi hai:

```sql
LANGUAGE plpgsql
```

Agar kal PostgreSQL → SQL Server migrate karna ho, procedure ko rewrite karna pad sakta hai.

---

### 2. Logic database mein chala jata hai

Application developer ko kabhi-kabhi logic trace/debug karna harder ho sakta hai because:

```text
C# code
    +
Database procedure
```

dono places mein logic distributed ho jata hai.

---

### 3. Version control / deployment complexity

Procedure database object hai.

Isliye proper projects mein procedure ko migrations/scripts ke through version-control mein maintain karna important hai.

---

# 16. Ek important misconception

### Stored Procedure = faster always?

**Nahi.**

Ye interview mein mat bolna:

> "Stored procedure is always faster than normal SQL/EF Core."

Performance query, indexes, execution plan, network round trips, data volume, DB design etc. par depend karti hai.

Stored procedure ka main benefit **reusability, encapsulation, database-side logic, controlled access** etc. ho sakta hai; automatic performance guarantee nahi.

---

# 17. Complete mental model

Isko yaad rakho:

```text
                 DATABASE
                     │
          ┌──────────┴──────────┐
          │                     │
       Tables              Stored Procedures
          │                     │
     ┌────┴────┐          ┌─────┴──────┐
     │         │          │            │
   Orders  OrderItems   GetOrders   CreateOrder
                          │            │
                          └─────┬──────┘
                                │
                          SQL Logic
```

Application:

```text
             .NET API
                │
        ┌───────┴────────┐
        │                │
     EF LINQ        CALL Procedure
        │                │
        ↓                ↓
     Database         Database
```

---

# 18. Interview mein short answer

Agar interviewer pooche:

**"What is a Stored Procedure?"**

Tum bol sakte ho:

> **A stored procedure is a pre-defined set of SQL statements stored inside the database and executed by calling its name. It is useful for reusable database logic, complex operations, transaction handling, and controlled database access.**

Example:

```sql
CREATE PROCEDURE GetOrdersByStatus(order_status VARCHAR)
LANGUAGE SQL
AS $$
    SELECT *
    FROM "Orders"
    WHERE "Status" = order_status;
$$;
```

Execute:

```sql
CALL GetOrdersByStatus('Delivered');
```

### One-line yaad rakhne wala rule:

**Normal SQL → query likho aur execute karo.**

**Stored Procedure → SQL logic ko database mein save karo, naam do, aur baad mein `CALL` karke execute karo.**

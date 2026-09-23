Bilkul. **SQL Transaction** ko zero se samjhte hain — **kya hota hai, kab use hota hai, kaha use hota hai, states kya hoti hain, COMMIT/ROLLBACK kaise kaam karte hain**, aur real project example ke saath.

# 1. Transaction kya hota hai?

**Transaction = database operations ka ek logical group jo ek unit ki tarah execute hota hai.**

Simple language mein:

> Agar 4–5 database operations ek saath complete hone chahiye, to unhe ek transaction ke andar rakhte hain.

Example: E-commerce mein order place karna:

```text
Customer Order Place karta hai
        ↓
1. Order create
        ↓
2. OrderItems create
        ↓
3. Stock reduce
        ↓
4. Payment record create
        ↓
5. COMMIT
```

Ye sab successfully ho gaya → **COMMIT**

Agar beech mein koi important operation fail ho gaya:

```text
1. Order create       ✅
2. OrderItems create  ✅
3. Stock reduce       ❌
```

To hum previous changes ko bhi undo kar sakte hain:

```text
ROLLBACK
   ↓
Order create       ❌ undo
OrderItems create  ❌ undo
Stock change       ❌ undo
```

Iska main purpose hai database ko **half-completed state** mein jaane se bachana.

---

# 2. Real-life example

Maan lo tumhare bank account mein ₹10,000 hain.

Tum ₹2,000 kisi aur account mein transfer kar rahe ho.

Actually database mein roughly:

```text
Account A:
10000 → 8000

Account B:
5000 → 7000
```

Do operations hain:

```sql
UPDATE Account
SET Balance = Balance - 2000
WHERE AccountId = 1;

UPDATE Account
SET Balance = Balance + 2000
WHERE AccountId = 2;
```

Ab imagine karo first query successfully execute ho gayi:

```text
A = 8000
```

Lekin second query fail ho gayi:

```text
B = 5000
```

To ₹2,000 disappear ho jayenge.

Transaction ensure karta hai:

```text
A se ₹2000 minus
        +
B mein ₹2000 plus
        ↓
dono successful → COMMIT
```

Agar koi fail:

```text
ROLLBACK
```

Aur database previous state mein.

---

# 3. Transaction ka basic structure

SQL mein commonly:

```sql
BEGIN;

-- Query 1
-- Query 2
-- Query 3

COMMIT;
```

Ya failure hone par:

```sql
BEGIN;

-- Query 1
-- Query 2
-- Query 3

ROLLBACK;
```

PostgreSQL example:

```sql
BEGIN;

UPDATE Accounts
SET Balance = Balance - 2000
WHERE AccountId = 1;

UPDATE Accounts
SET Balance = Balance + 2000
WHERE AccountId = 2;

COMMIT;
```

### Flow

```text
BEGIN
  ↓
Transaction Start
  ↓
Query 1
  ↓
Query 2
  ↓
Query 3
  ↓
Everything successful?
  ↓
 YES ─────→ COMMIT
             ↓
        Changes permanent
```

Failure:

```text
BEGIN
  ↓
Query 1 ✅
  ↓
Query 2 ❌
  ↓
ROLLBACK
  ↓
All uncommitted changes undo
```

---

# 4. Transaction ki zarurat kab hoti hai?

Ye bahut important interview question hai.

Transaction tab use karte hain jab:

### Case 1 — Multiple operations dependent hain

Example order creation:

```text
Orders
   ↓
OrderItems
   ↓
Stock
   ↓
Payment
```

Agar Order create ho gaya but OrderItems nahi bane, database inconsistent ho sakta hai.

Isliye:

```sql
BEGIN;

INSERT INTO Orders ...;

INSERT INTO OrderItems ...;

UPDATE Products ...;

INSERT INTO Payments ...;

COMMIT;
```

---

### Case 2 — Money transfer

```text
Account A → Account B
```

Dono updates ek saath hone chahiye.

---

### Case 3 — Inventory/stock

Suppose:

```text
Stock = 10
```

Customer order karta hai:

```text
Stock = Stock - 2
```

Saath mein order create hona hai.

Agar order create ho gaya but stock update fail ho gaya, problem ho sakti hai.

---

### Case 4 — Delete related data

Example:

```text
Customer
   ↓
Orders
   ↓
OrderItems
```

Agar multiple tables mein deletion karni hai:

```sql
BEGIN;

DELETE FROM OrderItems
WHERE OrderId = 101;

DELETE FROM Orders
WHERE OrderId = 101;

COMMIT;
```

Agar second DELETE fail:

```sql
ROLLBACK;
```

---

# 5. Transaction ke important commands

Generally ye commands ya concepts yaad rakho:

| Command                 | Meaning                          |
| ----------------------- | -------------------------------- |
| `BEGIN`                 | Transaction start                |
| `START TRANSACTION`     | Transaction start                |
| `COMMIT`                | Changes permanently save         |
| `ROLLBACK`              | Uncommitted changes undo         |
| `SAVEPOINT`             | Transaction ke andar checkpoint  |
| `ROLLBACK TO SAVEPOINT` | Specific checkpoint tak rollback |

---

# 6. COMMIT kya karta hai?

Example:

```sql
BEGIN;

UPDATE Products
SET Stock = Stock - 1
WHERE ProductId = 10;

COMMIT;
```

`COMMIT` ke baad:

```text
Transaction changes
       ↓
Committed
       ↓
Transaction finish
```

Matlab successfully transaction complete ho gayi.

Normally us committed transaction ko simple `ROLLBACK` se undo nahi kar sakte.

---

# 7. ROLLBACK kya karta hai?

Example:

```sql
BEGIN;

UPDATE Products
SET Stock = Stock - 1
WHERE ProductId = 10;

ROLLBACK;
```

Result:

```text
Stock change
    ↓
UNDO
```

Database transaction se pehle wali state par aa jayega.

---

# 8. SAVEPOINT kya hota hai?

Ye transaction ke andar **checkpoint** hota hai.

Example:

```sql
BEGIN;

INSERT INTO Orders (...);

SAVEPOINT order_created;

INSERT INTO OrderItems (...);

SAVEPOINT items_created;

UPDATE Products
SET Stock = Stock - 1
WHERE ProductId = 10;

COMMIT;
```

Suppose stock update mein problem aa gayi.

Hum poori transaction rollback karne ke bajaye:

```sql
ROLLBACK TO SAVEPOINT items_created;
```

kar sakte hain.

Concept:

```text
BEGIN
  ↓
Create Order
  ↓
SAVEPOINT order_created
  ↓
Create Items
  ↓
SAVEPOINT items_created
  ↓
Update Stock ❌
  ↓
ROLLBACK TO items_created
```

To transaction completely terminate nahi hoti.

---

# 9. Transaction ki states kitni hoti hain?

Database theory mein transaction ko commonly **5 main states** se explain kiya jata hai:

```text
1. Active
2. Partially Committed
3. Committed
4. Failed
5. Aborted
```

Ye interview ke liye important hai.

---

# 10. State 1 — Active

Jab transaction start ho gayi aur operations execute ho rahe hain.

Example:

```sql
BEGIN;

UPDATE Products
SET Stock = Stock - 1
WHERE ProductId = 10;
```

Abhi transaction:

```text
ACTIVE
```

Kyuki transaction complete nahi hui.

Flow:

```text
BEGIN
  ↓
ACTIVE
  ↓
Queries execute
```

---

# 11. State 2 — Partially Committed

Ye thoda confusing state hai.

Jab transaction ki **last statement execute ho chuki hai**, lekin transaction abhi fully durable/committed state mein nahi pahunchi.

Example:

```sql
BEGIN;

UPDATE Products
SET Stock = Stock - 1
WHERE ProductId = 10;

INSERT INTO Orders (...);

-- last operation completed
```

Conceptually:

```text
ACTIVE
   ↓
Last statement execute
   ↓
PARTIALLY COMMITTED
   ↓
COMMIT successfully
   ↓
COMMITTED
```

Important:

> **Partially committed ka matlab ye nahi ki user ne successfully permanent commit kar diya.**

Ye transition state hai.

---

# 12. State 3 — Committed

Jab transaction successfully `COMMIT` ho gayi.

```sql
BEGIN;

UPDATE Products
SET Stock = Stock - 1
WHERE ProductId = 10;

COMMIT;
```

Flow:

```text
ACTIVE
   ↓
PARTIALLY COMMITTED
   ↓
COMMIT
   ↓
COMMITTED
```

Ab changes successfully committed hain.

---

# 13. State 4 — Failed

Agar transaction ke andar koi error/problem aa jaye:

```text
ACTIVE
   ↓
Query
   ↓
ERROR ❌
   ↓
FAILED
```

Example:

```sql
BEGIN;

INSERT INTO Orders
VALUES (101, 1);

INSERT INTO Orders
VALUES (101, 2);
```

Agar `OrderId = 101` already exist karta hai aur Primary Key hai:

```text
ERROR
duplicate key
```

Transaction failure/error state mein ja sakti hai.

---

# 14. State 5 — Aborted

Failed transaction ko rollback karne ke baad transaction **aborted** state mein aa sakti hai.

Concept:

```text
ACTIVE
   ↓
ERROR
   ↓
FAILED
   ↓
ROLLBACK
   ↓
ABORTED
```

Ab transaction ke changes undo ho gaye.

Agar fresh work karna hai to generally **new transaction** start karte hain.

---

# 15. Complete Transaction State Diagram

Interview mein is diagram ko yaad rakh sakte ho:

```text
                    ┌──────────────┐
                    │    ACTIVE    │
                    └──────┬───────┘
                           │
                    Last operation
                       completed
                           │
                           ▼
                ┌─────────────────────┐
                │ PARTIALLY COMMITTED │
                └──────────┬──────────┘
                           │
                     COMMIT success
                           │
                           ▼
                    ┌─────────────┐
                    │  COMMITTED  │
                    └─────────────┘


ACTIVE
  │
  │ Error
  ▼
┌─────────┐
│ FAILED  │
└────┬────┘
     │
  ROLLBACK
     │
     ▼
┌─────────┐
│ ABORTED │
└─────────┘
```

### Simple memory trick

```text
ACTIVE
   ↓
PARTIALLY COMMITTED
   ↓
COMMITTED

OR

ACTIVE
   ↓
FAILED
   ↓
ABORTED
```

---

# 16. Transaction aur ACID ka relation

Ye dono confuse mat karna.

**Transaction** = operations ka group.

**ACID** = transaction ki properties.

```text
Transaction
     │
     ├── Atomicity
     ├── Consistency
     ├── Isolation
     └── Durability
```

### Atomicity

```text
All or Nothing
```

### Consistency

```text
Valid DB State → Valid DB State
```

### Isolation

```text
Concurrent transactions
ek dusre ko incorrectly interfere na karein
```

### Durability

```text
COMMIT ke baad data survive kare
```

---

# 17. Real project example — Order Placement

Maan lo tumhare project mein:

### Orders

```text
OrderId | CustomerId | Status
--------|------------|----------
101     | 5          | Pending
```

### OrderItems

```text
OrderItemId | OrderId | ProductId | Quantity
------------|---------|-----------|---------
1           | 101     | 10        | 2
```

### Products

```text
ProductId | Product | Stock
----------|---------|------
10        | Laptop  | 10
```

Customer 2 laptops order karta hai.

Transaction:

```sql
BEGIN;

INSERT INTO Orders
(
    OrderId,
    CustomerId,
    Status
)
VALUES
(
    102,
    5,
    'Confirmed'
);

INSERT INTO OrderItems
(
    OrderItemId,
    OrderId,
    ProductId,
    Quantity
)
VALUES
(
    2,
    102,
    10,
    2
);

UPDATE Products
SET Stock = Stock - 2
WHERE ProductId = 10
  AND Stock >= 2;

COMMIT;
```

Final:

```text
Orders
102 → Confirmed

OrderItems
102 → Laptop × 2

Products
Stock: 10 → 8
```

Sab successful → **COMMIT**

---

# 18. Agar stock update fail ho gaya?

Suppose:

```text
Stock = 1
```

but customer ne quantity:

```text
2
```

order ki.

Query:

```sql
UPDATE Products
SET Stock = Stock - 2
WHERE ProductId = 10
  AND Stock >= 2;
```

Affected rows:

```text
0
```

Application ko pata chalega:

```text
Stock insufficient
```

Then:

```sql
ROLLBACK;
```

Ab:

```text
Order creation → undo
OrderItems → undo
Stock → unchanged
```

Ye transaction ka real-world use hai.

---

# 19. .NET / EF Core mein transaction

Tumhare C# backend mein ye bahut important hai.

Example:

```csharp
await using var transaction =
    await dbContext.Database.BeginTransactionAsync();

try
{
    // 1. Create Order
    dbContext.Orders.Add(order);
    await dbContext.SaveChangesAsync();

    // 2. Create OrderItems
    dbContext.OrderItems.Add(orderItem);
    await dbContext.SaveChangesAsync();

    // 3. Update Stock
    product.Stock -= orderItem.Quantity;
    await dbContext.SaveChangesAsync();

    // Everything successful
    await transaction.CommitAsync();
}
catch
{
    await transaction.RollbackAsync();
    throw;
}
```

Flow:

```text
BeginTransactionAsync()
        ↓
Order INSERT
        ↓
OrderItem INSERT
        ↓
Stock UPDATE
        ↓
      Success?
      /     \
    YES      NO
     ↓        ↓
 COMMIT    ROLLBACK
```

---

# 20. Ek important point — `SaveChanges()` vs `Commit()`

Beginners often confuse these.

### `SaveChanges()`

EF Core ke changes ko database mein send karta hai.

### `Commit()`

Transaction ke changes ko permanently commit karta hai.

Example:

```csharp
await transaction.Begin;

await db.SaveChangesAsync();

await transaction.CommitAsync();
```

Agar `SaveChanges()` ho gaya but baad mein error aa gaya aur transaction rollback ho gayi, to transaction ke andar ke changes rollback ho sakte hain.

---

# 21. Har query ko transaction mein rakhna chahiye?

**Nahi.**

Agar simple query hai:

```sql
SELECT *
FROM Products;
```

Transaction ki generally need nahi.

Similarly simple independent:

```sql
INSERT INTO Categories ...
```

Agar ek hi operation hai aur usko multi-step atomic operation ki requirement nahi hai, explicit transaction unnecessary ho sakta hai.

Transaction tab useful hai jab:

```text
Multiple operations
        +
Operations logically connected
        +
All should succeed/fail together
```

---

# 22. Transaction vs COMMIT vs ROLLBACK

Very simple:

```text
TRANSACTION
    =
operations ka group

BEGIN
    =
transaction start

COMMIT
    =
changes accept/save

ROLLBACK
    =
changes undo
```

---

# 23. Interview mein ekdum simple answer

Agar interviewer pooche:

> **What is a transaction in SQL?**

Tum bol sakte ho:

> **A transaction is a logical unit of one or more database operations that should be treated as a single unit. Either all required operations succeed and are committed, or the changes are rolled back. Transactions are mainly used when multiple related operations must maintain data consistency, such as order creation, payment processing, money transfer, and inventory updates.**

Aur states pooche:

```text
1. Active
2. Partially Committed
3. Committed
4. Failed
5. Aborted
```

Flow:

```text
             ACTIVE
             /    \
            /      \
       Success     Error
          ↓          ↓
PARTIALLY COMMITTED FAILED
          ↓          ↓
       COMMITTED   ABORTED
```

**Sabse important distinction yaad rakho:**

> **ACID = transaction ki properties**
> **BEGIN / COMMIT / ROLLBACK = transaction control**
> **ACTIVE / FAILED / COMMITTED / ABORTED = transaction states**

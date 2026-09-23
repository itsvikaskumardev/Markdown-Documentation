Bilkul. **ACID properties** ko SQL ke actual queries aur `Orders` / `OrderItems` example se samjhte hain. Ye topic especially **transactions, banking, payments, order placement, inventory** mein bahut important hai.

# ACID Properties kya hoti hain?

**ACID =**

| Letter | Property    | Simple meaning                                                         |
| ------ | ----------- | ---------------------------------------------------------------------- |
| **A**  | Atomicity   | Ya to sab hoga, ya kuch bhi nahi                                       |
| **C**  | Consistency | Database valid state mein rahega                                       |
| **I**  | Isolation   | Concurrent transactions ek-dusre ko incorrectly interfere nahi karengi |
| **D**  | Durability  | Commit ke baad data persist rahega                                     |

Sabse pehle **Transaction** samajh lo, kyunki ACID transaction ki properties hain.

---

# 1. Transaction kya hoti hai?

Transaction = **multiple database operations ka ek logical unit**.

Example: Customer order place karta hai.

Suppose:

```text
Customer order place
       ↓
1. Orders mein order insert
       ↓
2. OrderItems mein items insert
       ↓
3. Stock reduce
       ↓
4. Payment record
```

Ye sab ideally ek transaction ka part ho sakte hain.

```text
BEGIN
   ↓
INSERT Order
   ↓
INSERT OrderItem
   ↓
UPDATE Stock
   ↓
INSERT Payment
   ↓
COMMIT
```

Agar beech mein problem aa gayi:

```text
BEGIN
   ↓
INSERT Order       ✓
   ↓
INSERT OrderItem   ✓
   ↓
UPDATE Stock       ✗ ERROR
   ↓
ROLLBACK
```

To pehle ke changes bhi undo ho sakte hain.

Yahin se **ACID** aati hai.

---

# A — Atomicity

## 2. Atomicity kya hai?

> **Atomicity means transaction is treated as one indivisible unit: either all required operations succeed, or the transaction is rolled back.**

Simple:

> **All or Nothing.**

Example:

Customer ne ₹5,000 ka order place kiya.

Suppose 3 operations hain:

```text
1. Create Order
2. Add OrderItem
3. Reduce Stock
```

Agar:

```text
Order INSERT       ✓
OrderItem INSERT   ✓
Stock UPDATE       ✗
```

To hum nahi chahte:

```text
Orders table:
Order exists ✓

OrderItems:
Item exists ✓

Stock:
Old stock ✓
```

Yaani half-completed order.

Atomicity ke through:

```text
Order INSERT       ✓
OrderItem INSERT   ✓
Stock UPDATE       ✗
       ↓
ROLLBACK
       ↓
Order INSERT       ✗
OrderItem INSERT   ✗
```

Database previous consistent state par aa sakta hai.

---

# 3. Atomicity ki SQL query

Let's create example tables:

```sql
CREATE TABLE Orders
(
    OrderId INT PRIMARY KEY,
    CustomerId INT,
    Status VARCHAR(30),
    Amount NUMERIC(10,2)
);

CREATE TABLE OrderItems
(
    OrderItemId INT PRIMARY KEY,
    OrderId INT,
    Product VARCHAR(100),
    Quantity INT,
    Price NUMERIC(10,2)
);
```

Now transaction:

```sql
BEGIN;

INSERT INTO Orders
(
    OrderId,
    CustomerId,
    Status,
    Amount
)
VALUES
(
    101,
    1,
    'Pending',
    5000
);

INSERT INTO OrderItems
(
    OrderItemId,
    OrderId,
    Product,
    Quantity,
    Price
)
VALUES
(
    1,
    101,
    'Laptop',
    1,
    5000
);

COMMIT;
```

Agar sab successful hai:

```text
BEGIN
 ↓
INSERT Orders       ✓
 ↓
INSERT OrderItems   ✓
 ↓
COMMIT
```

Changes permanent transaction result ka part ban jaate hain.

---

# 4. Atomicity with error

Suppose second query mein error:

```sql
BEGIN;

INSERT INTO Orders
(
    OrderId,
    CustomerId,
    Status,
    Amount
)
VALUES
(
    102,
    1,
    'Pending',
    5000
);

-- Suppose this fails
INSERT INTO OrderItems
(
    OrderItemId,
    OrderId,
    Product,
    Quantity,
    Price
)
VALUES
(
    2,
    102,
    'Laptop',
    -5,
    5000
);

ROLLBACK;
```

`ROLLBACK` ka matlab:

> Transaction ke andar ki uncommitted changes ko undo karo.

Concept:

```text
BEGIN
 │
 ├── Order INSERT ✓
 │
 ├── Item INSERT ✗
 │
 └── ROLLBACK
        ↓
   Order INSERT also undone
```

### Atomicity =

```text
ALL ✓
   → COMMIT

ANY important operation fails
   → ROLLBACK
```

---

# C — Consistency

## 5. Consistency kya hai?

> **Consistency means a transaction takes the database from one valid state to another valid state while respecting defined rules and constraints.**

Simple:

> Transaction ke baad database ke rules break nahi hone chahiye.

Ye rules ho sakte hain:

* Primary Key
* Foreign Key
* UNIQUE
* NOT NULL
* CHECK
* data types
* application/database constraints

---

# 6. Example: Primary Key consistency

Suppose:

```sql
CREATE TABLE Orders
(
    OrderId INT PRIMARY KEY,
    CustomerId INT,
    Amount NUMERIC(10,2)
);
```

Already:

| OrderId | CustomerId | Amount |
| ------: | ---------: | -----: |
|     101 |          1 |   5000 |

Ab:

```sql
INSERT INTO Orders
VALUES (101, 2, 3000);
```

Error aayega because:

```text
OrderId = 101
```

already exists.

Database ka rule:

```text
PRIMARY KEY → unique
```

violate ho raha hai.

So database invalid state mein jane nahi deta.

---

# 7. Foreign Key consistency

Suppose:

```sql
CREATE TABLE Customers
(
    CustomerId INT PRIMARY KEY,
    Name VARCHAR(100)
);
```

And:

```sql
CREATE TABLE Orders
(
    OrderId INT PRIMARY KEY,
    CustomerId INT REFERENCES Customers(CustomerId),
    Amount NUMERIC(10,2)
);
```

Customers:

| CustomerId | Name  |
| ---------: | ----- |
|          1 | Rahul |
|          2 | Amit  |

Ab:

```sql
INSERT INTO Orders
VALUES
(
    101,
    999,
    5000
);
```

Agar Customer `999` exist nahi karta, foreign key violation ho sakta hai.

Because:

```text
Orders.CustomerId
       ↓
must reference
       ↓
Customers.CustomerId
```

Database relationship rule maintain karta hai.

---

# 8. CHECK constraint consistency

Suppose quantity negative nahi honi chahiye:

```sql
CREATE TABLE OrderItems
(
    OrderItemId INT PRIMARY KEY,
    OrderId INT,
    Product VARCHAR(100),
    Quantity INT CHECK (Quantity > 0),
    Price NUMERIC(10,2)
);
```

Ab:

```sql
INSERT INTO OrderItems
VALUES
(
    1,
    101,
    'Laptop',
    -5,
    50000
);
```

Database reject karega:

```text
Quantity = -5
      ↓
CHECK Quantity > 0
      ↓
FALSE
      ↓
INSERT rejected
```

Ye consistency maintain karne mein help karta hai.

---

# 9. Atomicity + Consistency together

Ye dono confuse hote hain.

Suppose transaction:

```text
BEGIN
 ↓
Insert Order
 ↓
Insert OrderItem
 ↓
Commit
```

### Atomicity

Guarantee karta hai:

```text
All operations together
OR
rollback
```

### Consistency

Guarantee karta hai:

```text
Database rules/constraints
remain satisfied
```

Simple:

> **Atomicity = transaction complete hua ya nahi?**

> **Consistency = transaction ke baad database valid hai ya nahi?**

---

# I — Isolation

Ab sabse tricky property.

# 10. Isolation kya hai?

Real applications mein ek hi time par multiple users database use karte hain.

Example:

```text
User A ───────┐
              │
              ▼
           Database
              ▲
              │
User B ───────┘
```

Suppose stock:

```text
Product = Laptop
Stock = 1
```

Do users same time laptop order kar rahe hain.

```text
Transaction A
Transaction B
      ↓
same product
stock = 1
```

Agar database concurrency properly handle nahi kare to dono transaction stock `1` dekh sakti hain.

Result:

```text
User A → Laptop ✓
User B → Laptop ✓
```

But actual stock sirf `1` tha.

Isolation ka purpose concurrency ke time incorrect interference prevent karna hai.

---

# 11. Isolation example

Suppose:

### Products

| ProductId | Product | Stock |
| --------: | ------- | ----: |
|         1 | Laptop  |     1 |

Transaction A:

```sql
BEGIN;

SELECT Stock
FROM Products
WHERE ProductId = 1;
```

Result:

```text
Stock = 1
```

At nearly same time Transaction B bhi:

```sql
BEGIN;

SELECT Stock
FROM Products
WHERE ProductId = 1;
```

Agar dono independently `1` dekhkar stock reduce kar dein, problem ho sakti hai.

Proper transaction isolation/concurrency control database ko coordinate karne mein help karta hai.

---

# 12. Isolation levels

SQL databases commonly isolation levels provide karte hain.

Main four standard names:

```text
READ UNCOMMITTED
READ COMMITTED
REPEATABLE READ
SERIALIZABLE
```

PostgreSQL ka important point:

> PostgreSQL `READ UNCOMMITTED` ko effectively `READ COMMITTED` ki tarah treat karta hai.

Default PostgreSQL isolation level commonly:

```text
READ COMMITTED
```

---

# 13. READ COMMITTED

Meaning:

> Transaction normally doosri transactions ke **committed data** ko read karti hai, uncommitted changes ko nahi.

Example:

Transaction A:

```sql
BEGIN;

UPDATE Products
SET Stock = 0
WHERE ProductId = 1;
```

Abhi A ne `COMMIT` nahi kiya.

Transaction B:

```sql
SELECT Stock
FROM Products
WHERE ProductId = 1;
```

B ko A ka uncommitted change normally visible nahi hoga.

Agar A:

```sql
COMMIT;
```

kar de:

```text
Transaction B
   ↓
next statement
   ↓
new committed value visible
```

---

# 14. REPEATABLE READ

Concept:

> Transaction ke andar repeatedly same row read karne par consistent snapshot maintain kiya jata hai.

Example:

Transaction A:

```sql
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;

SELECT Stock
FROM Products
WHERE ProductId = 1;
```

Suppose:

```text
Stock = 10
```

Meanwhile another transaction update/commit kar deti hai.

Transaction A same transaction ke andar dobara read kare:

```sql
SELECT Stock
FROM Products
WHERE ProductId = 1;
```

PostgreSQL ke MVCC model mein transaction ka snapshot consistent rehta hai.

---

# 15. SERIALIZABLE

Highest standard isolation level.

Conceptually:

> Concurrent transactions ka result aisa hona chahiye jaise transactions one-by-one execute hui hon.

Example:

```sql
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;

-- operations

COMMIT;
```

Agar concurrent transactions logically conflict karengi, database ek transaction ko serialization failure ke saath abort kar sakta hai, jise application ko retry karna pad sakta hai.

---

# 16. Isolation ka practical example — inventory

Suppose:

```text
Stock = 1
```

Two orders:

```text
Transaction A → Order 101
Transaction B → Order 102
```

Goal:

```text
Only one should successfully consume the last unit.
```

Database transaction/concurrency controls help ensure that both transactions incorrectly consume the same stock na kar dein.

A common PostgreSQL pattern:

```sql
BEGIN;

UPDATE Products
SET Stock = Stock - 1
WHERE ProductId = 1
  AND Stock > 0;

-- Check affected rows

COMMIT;
```

Agar sirf ek stock tha:

```text
Transaction A
UPDATE → 1 row affected

Transaction B
UPDATE → 0 rows affected
```

Application affected-row count check karke B ko "out of stock" handle kar sakti hai.

Ye **database-level concurrency control + application logic** ka practical example hai.

---

# D — Durability

## 17. Durability kya hai?

> **Once a transaction is successfully committed, its changes should persist even if a failure occurs afterward, subject to the database's durability/recovery guarantees.**

Simple:

> **COMMIT ke baad data save hai.**

Example:

```sql
BEGIN;

INSERT INTO Orders
VALUES
(
    101,
    1,
    'Confirmed',
    5000
);

COMMIT;
```

`COMMIT` successful hone ke baad database crash/restart hone par database recovery mechanisms committed transaction ko preserve karne ke liye designed hote hain.

---

# 18. Durability ka real-life example

Bank transaction:

```text
Account A
₹10,000

Transfer ₹2,000
       ↓
Transaction
       ↓
COMMIT
       ↓
Account A = ₹8,000
Account B increased
```

Agar commit ke baad server restart ho jaye:

```text
Server restart
      ↓
Database recovery
      ↓
Committed transaction remains
```

Ye durability ka basic idea hai.

---

# 19. `COMMIT` vs `ROLLBACK`

ACID samajhne ke liye ye dono clear hone chahiye.

### COMMIT

```sql
COMMIT;
```

Means:

> Transaction ke changes successfully finalize karo.

### ROLLBACK

```sql
ROLLBACK;
```

Means:

> Transaction ke uncommitted changes undo karo.

Example:

```sql
BEGIN;

UPDATE Orders
SET Status = 'Delivered'
WHERE OrderId = 101;

ROLLBACK;
```

Status change rollback ho jayega.

---

# 20. Complete ACID example

Ab ek complete order transaction:

Suppose:

### Products

| ProductId | Product | Stock |
| --------: | ------- | ----: |
|         1 | Laptop  |     5 |

### Orders

| OrderId | CustomerId | Status  | Amount |
| ------: | ---------: | ------- | -----: |
|     100 |          1 | Pending |      0 |

Customer order place karta hai.

```sql
BEGIN;

-- 1. Create order
INSERT INTO Orders
(
    OrderId,
    CustomerId,
    Status,
    Amount
)
VALUES
(
    101,
    1,
    'Confirmed',
    50000
);

-- 2. Add order item
INSERT INTO OrderItems
(
    OrderItemId,
    OrderId,
    Product,
    Quantity,
    Price
)
VALUES
(
    1,
    101,
    'Laptop',
    1,
    50000
);

-- 3. Reduce stock
UPDATE Products
SET Stock = Stock - 1
WHERE ProductId = 1
  AND Stock > 0;

COMMIT;
```

Ab ACID identify karo.

### Atomicity

Agar step 2 ya 3 fail:

```text
ROLLBACK
```

to complete transaction undo ho sakti hai.

### Consistency

Constraints ensure:

```text
OrderId unique
OrderItem valid
ProductId valid
Stock rule valid
```

etc.

### Isolation

Agar multiple customers simultaneously laptop buy kar rahe hain, database concurrency controls transactions ko coordinate karte hain.

### Durability

`COMMIT` ke baad successfully committed data database recovery ke through persist rehne ke liye protected hota hai.

---

# 21. ACID ko ek real-world analogy se yaad karo

Imagine restaurant order:

```text
Customer orders
      ↓
Kitchen accepts
      ↓
Food prepared
      ↓
Payment processed
      ↓
Order confirmed
```

### Atomicity

Food/order process half-complete nahi:

```text
All required steps ✓
OR
Cancel/rollback
```

### Consistency

Restaurant ke rules break nahi:

```text
Quantity > 0
Valid item
Valid order
```

### Isolation

Do customers same last item order kar rahe hain:

```text
Concurrent orders
      ↓
Inventory incorrectly oversold nahi honi chahiye
```

### Durability

Order confirmed:

```text
COMMIT
 ↓
System restart
 ↓
Order still exists
```

---

# 22. ACID vs Transaction Commands

Ye difference bhi important hai.

**ACID properties**:

```text
A → Atomicity
C → Consistency
I → Isolation
D → Durability
```

**Transaction commands**:

```sql
BEGIN;
COMMIT;
ROLLBACK;
SAVEPOINT;
```

Commands ACID ko implement/use karne ke mechanisms ka part hain; ACID khud commands nahi hain.

---

# 23. SAVEPOINT bhi samajh lo

Kabhi transaction ke andar partial rollback chahiye.

```sql
BEGIN;

INSERT INTO Orders
VALUES (101, 1, 'Pending', 5000);

SAVEPOINT order_created;

INSERT INTO OrderItems
VALUES (1, 101, 'Laptop', 1, 5000);

ROLLBACK TO SAVEPOINT order_created;

COMMIT;
```

Yahan:

```text
BEGIN
 ↓
Order inserted
 ↓
SAVEPOINT
 ↓
OrderItem inserted
 ↓
ROLLBACK TO SAVEPOINT
 ↓
OrderItem change undone
 ↓
COMMIT
```

`SAVEPOINT` poori transaction ko rollback nahi karta; specified savepoint ke baad ke changes ko rollback kar sakta hai.

---

# 24. ACID ko SQL level par ek table mein

| Property        | SQL mein kya dekhoge?                        | Example                                 |
| --------------- | -------------------------------------------- | --------------------------------------- |
| **Atomicity**   | `BEGIN`, `COMMIT`, `ROLLBACK`                | Order + Items together                  |
| **Consistency** | PK/FK/UNIQUE/CHECK/NOT NULL + business rules | Invalid CustomerId reject               |
| **Isolation**   | Transaction isolation/concurrency controls   | Concurrent stock updates                |
| **Durability**  | Successful `COMMIT` + DB recovery            | Restart ke baad committed order remains |

---

# 25. Sabse important difference: Atomicity vs Consistency

Ye interview mein poocha ja sakta hai.

### Atomicity

Question:

> **Kya transaction ke saare operations together complete hue?**

Example:

```text
Order INSERT ✓
Item INSERT ✓
Stock UPDATE ✗
      ↓
ROLLBACK
```

### Consistency

Question:

> **Transaction ke baad database ke rules valid hain?**

Example:

```text
OrderId duplicate
      ↓
PRIMARY KEY violation
      ↓
Rejected
```

So:

> **Atomicity = all or nothing**

> **Consistency = valid state to valid state**

---

# 26. Isolation vs Atomicity

### Atomicity

```text
Transaction ke andar operations
      ↓
All or nothing
```

### Isolation

```text
Transaction A
       ↕
Transaction B
       ↓
Concurrent operations ko safely handle karna
```

Example:

```text
A: Buy last laptop
B: Buy last laptop
```

Ye **Isolation/concurrency** ka issue hai.

---

# 27. Final mental diagram

```text
                 TRANSACTION
                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼
     INSERT        UPDATE       DELETE
        │            │            │
        └────────────┼────────────┘
                     │
                    ACID
                     │
       ┌─────────────┼──────────────┐
       │             │              │
       ▼             ▼              ▼
   Atomicity     Consistency    Isolation
   All/None      Valid state    Concurrent
       │             │              │
       └─────────────┼──────────────┘
                     │
                     ▼
                 Durability
                 COMMIT
                     │
                     ▼
             Data persists
```

### Interview ke liye one-line definitions:

**A — Atomicity:**

> Transaction ke saare operations successfully execute honge, ya transaction rollback ho jayegi.

**C — Consistency:**

> Transaction database ko ek valid state se doosri valid state mein le jaati hai, constraints/rules ko maintain karte hue.

**I — Isolation:**

> Concurrent transactions ko aise control kiya jata hai ki unki intermediate/uncommitted changes incorrectly interfere na karein.

**D — Durability:**

> Successful commit ke baad committed changes database failure/restart ke baad bhi recover/persist rehne ke liye protected hote hain.

**Shortcut:**
**A = All or Nothing**
**C = Correct/Valid State**
**I = Independent/Isolated Transactions**
**D = Data stays after Commit**

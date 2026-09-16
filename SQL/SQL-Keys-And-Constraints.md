
---

# Q4) What is Primary Key?

## Primary Key kya hoti hai?

**Primary Key ek column (ya columns ka combination) hota hai jo table ke har row ko uniquely identify karta hai.**

Simple language:

> **Har record ki unique identity = Primary Key**

Maan lo `Orders` table hai:

| OrderId | CustomerName | Amount |
| ------: | ------------ | -----: |
|     101 | Rahul        |    500 |
|     102 | Amit         |    800 |
|     103 | Rahul        |   1200 |

Yahan `OrderId` Primary Key ho sakta hai.

```sql
CREATE TABLE Orders (
    OrderId INT PRIMARY KEY,
    CustomerName VARCHAR(100),
    Amount DECIMAL(10,2)
);
```

Ab:

```text
OrderId
   ↓
101 → Rahul
102 → Amit
103 → Rahul
```

`OrderId` se hum **exactly ek order identify** kar sakte hain.

---

## Primary Key ki properties

### 1. Duplicate nahi ho sakti

Ye allowed nahi:

```text
OrderId
--------
101
102
101 ❌
```

Kyuki `101` already exist karta hai.

---

### 2. NULL nahi ho sakti

Ye allowed nahi:

```text
OrderId
--------
101
102
NULL ❌
```

Har row ki identity honi chahiye.

---

### 3. Ek table mein only ONE Primary Key

Ek table mein **one primary key constraint** hota hai.

Lekin ek important point:

> Primary Key ke andar **multiple columns** ho sakte hain. Isko **Composite Primary Key** kehte hain.

Abhi beginner level par bas itna yaad rakho:

```text
One table
   ↓
One Primary Key
```

---

# Q5) What is Foreign Key?

Ab maan lo hamare paas 2 tables hain:

```

## Example

### Orders

| OrderId | CustomerName |
| ------: | ------------ |
|     101 | Rahul        |
|     102 | Amit         |
|     103 | Priya        |

### OrderItems

| OrderItemId | OrderId | Product  | Quantity |
| ----------: | ------: | -------- | -------: |
|           1 |     101 | Laptop   |        1 |
|           2 |     101 | Mouse    |        2 |
|           3 |     102 | Keyboard |        1 |
|           4 |     103 | Monitor  |        1 |

Yahan:

```text
Orders.OrderId
      ↑
      |
      | referenced by
      |
OrderItems.OrderId
```

`OrderItems.OrderId` **Foreign Key** hai.

---

## SQL

```sql
CREATE TABLE Orders (
    OrderId INT PRIMARY KEY,
    CustomerName VARCHAR(100)
);
```

Then:

```sql
CREATE TABLE OrderItems (
    OrderItemId INT PRIMARY KEY,
    OrderId INT,
    ProductName VARCHAR(100),
    Quantity INT,

    FOREIGN KEY (OrderId)
        REFERENCES Orders(OrderId)
);
```

Yahan:

```text
Orders
OrderId
   ↑
   |
   | FK relationship
   |
OrderItems
OrderId
```

---

## Foreign Key ki need kyun hai?

Suppose `Orders` mein ye orders hain:

```text
101
102
103
```

Ab koi `OrderItems` mein ye insert karne ki koshish kare:

```text
OrderId = 999
```

Lekin `Orders` table mein `999` order exist hi nahi karta.

Foreign Key database ko bolti hai:

> "OrderItems ka OrderId sirf wahi ho sakta hai jo Orders mein valid OrderId ho."

Isse **data consistency / referential integrity** maintain hoti hai.

---

## Foreign Key duplicate ho sakti hai?

### YES ✅

Dekho:

| OrderItemId | OrderId | Product  |
| ----------: | ------: | -------- |
|           1 |     101 | Laptop   |
|           2 |     101 | Mouse    |
|           3 |     101 | Keyboard |

`101` multiple times aa raha hai.

Why?

Because ek order mein **multiple items** ho sakte hain.

```text
Order 101
   |
   ├── Laptop
   ├── Mouse
   └── Keyboard
```

Isliye Foreign Key ko unique hona zaroori nahi hai.

---

## Foreign Key NULL ho sakti hai?

**Haan, agar column par NOT NULL nahi lagaya gaya ho.**

Example:

```sql
OrderId INT NULL
```

To:

```text
OrderId
--------
101
102
NULL
```

possible hai.

Lekin agar:

```sql
OrderId INT NOT NULL
```

hai, to NULL allowed nahi hogi.

So important:

> **Foreign Key by itself NULL ko automatically prevent nahi karti.**

---

# Primary Key vs Foreign Key

Bahut important interview comparison:

| Primary Key                        | Foreign Key                             |
| ---------------------------------- | --------------------------------------- |
| Row ko uniquely identify karti hai | Tables ke beech relationship banati hai |
| Duplicate ❌                        | Duplicate ✅                             |
| NULL ❌                             | NULL ✅, if column nullable              |
| Table mein one PK constraint       | Multiple FKs ho sakti hain              |
| Usually parent table mein          | Usually child table mein                |

---

# Q6) What is UNIQUE Key?

Ab maan lo `Users` table hai:

| UserId | Name  | Email                                     |
| -----: | ----- | ----------------------------------------- |
|      1 | Rahul | [rahul@gmail.com](mailto:rahul@gmail.com) |
|      2 | Amit  | [amit@gmail.com](mailto:amit@gmail.com)   |
|      3 | Priya | [priya@gmail.com](mailto:priya@gmail.com) |

Hum chahte hain:

> **Do users ka same email nahi hona chahiye.**

Iske liye `UNIQUE` constraint use kar sakte hain.

```sql
CREATE TABLE Users (
    UserId INT PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(200) UNIQUE
);
```

Ab:

```text
rahul@gmail.com ✅
amit@gmail.com  ✅
rahul@gmail.com ❌
```

Third row reject ho jayegi.

---

# Primary Key aur UNIQUE mein difference kya hai?

Sabse simple way:

### Primary Key

> **"Ye row ki identity hai."**

### UNIQUE

> **"Is column mein duplicate value nahi honi chahiye."**

Example:

```text
Users
--------------------------------
UserId        Email
  PK          UNIQUE
   ↓             ↓
  101      rahul@gmail.com
  102      amit@gmail.com
  103      priya@gmail.com
```

`UserId` tells us **which user**.

`Email` ensures **same email multiple users ko na mile**.

---

## Kya ek table mein multiple UNIQUE constraints ho sakte hain?

### YES ✅

Example:

```sql
CREATE TABLE Users (
    UserId INT PRIMARY KEY,
    Email VARCHAR(200) UNIQUE,
    Phone VARCHAR(20) UNIQUE,
    Username VARCHAR(50) UNIQUE
);
```

Yahan:

```text
1 Primary Key
3 UNIQUE constraints
```

possible hain.

---

## UNIQUE mein NULL?

Yahan thoda important database-specific point hai.

SQL databases mein NULL handling **database system par depend kar sakti hai**. PostgreSQL jaise systems mein a normal `UNIQUE` constraint multiple NULL values allow kar sakta hai, because NULL values equal nahi maani jaati.

Example PostgreSQL:

```text
Email
----------------
rahul@gmail.com
amit@gmail.com
NULL
NULL
```

Multiple `NULL` values possible hain.

Isliye interview mein sirf:

> "UNIQUE allows only one NULL"

bolna universally correct nahi hai.

Better answer:

> **UNIQUE duplicate non-NULL values prevent karta hai; NULL behavior database-specific hota hai.**

---

# Q7) Primary Key vs UNIQUE Key

| Feature          | Primary Key       | UNIQUE                                               |
| ---------------- | ----------------- | ---------------------------------------------------- |
| Duplicate values | ❌                 | ❌                                                    |
| NULL             | ❌                 | Depends on DB; PostgreSQL mein multiple NULL allowed |
| Main purpose     | Row ki identity   | Duplicate values prevent karna                       |
| Number per table | One PK constraint | Multiple UNIQUE constraints                          |
| Example          | `UserId`          | `Email`                                              |

### Easy trick:

```text
PRIMARY KEY
     ↓
Who are you?

UNIQUE
     ↓
Can someone else have the same value?
```

Example:

```text
UserId = 101
→ User ki identity

Email = abc@gmail.com
→ Kisi aur user ka same email nahi hona chahiye
```

---

# Q8) What is NOT NULL Constraint?

## NOT NULL kya karta hai?

**NOT NULL ensure karta hai ki column mein NULL value nahi aa sakti.**

Example:

```sql
CREATE TABLE Users (
    UserId INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(200)
);
```

Yahan:

```text
Name  → NOT NULL
Email → NULL allowed
```

So:

```sql
INSERT INTO Users (UserId, Name, Email)
VALUES (1, NULL, 'rahul@gmail.com');
```

❌ Error.

Because:

```text
Name = NULL
```

allowed nahi hai.

Lekin:

```sql
INSERT INTO Users (UserId, Name, Email)
VALUES (1, 'Rahul', NULL);
```

✅ allowed hai.

---

## Real-world example

Order mein:

```text
OrderId       → NOT NULL
CustomerId    → NOT NULL
OrderDate     → NOT NULL
CouponCode    → NULL allowed
```

Kyun?

Order ko customer aur date chahiye, but coupon optional hai.

---

# Q9) What is DEFAULT Constraint?

`DEFAULT` ka meaning:

> **Agar INSERT karte time value nahi di gayi, to database automatically default value use karega.**

Example:

```sql
CREATE TABLE Orders (
    OrderId INT PRIMARY KEY,
    Status VARCHAR(30) DEFAULT 'Pending'
);
```

Ab hum insert karte hain:

```sql
INSERT INTO Orders (OrderId)
VALUES (101);
```

Humne `Status` nahi diya.

Database automatically karega:

```text
OrderId   Status
--------------------
101       Pending
```

---

## DEFAULT aur NOT NULL same nahi hain

Ye bahut important hai.

### NOT NULL

Bolta hai:

> "NULL allowed nahi hai."

### DEFAULT

Bolta hai:

> "Value nahi di to ye value use kar lena."

Dono saath bhi use ho sakte hain:

```sql
Status VARCHAR(30) NOT NULL DEFAULT 'Pending'
```

Meaning:

* Status NULL nahi ho sakta
* Agar value nahi di → `Pending`

---

# Q10) DELETE vs TRUNCATE vs DROP

Ye bahut important interview question hai.

Maan lo:

```text
Orders
--------------------------------
OrderId | Customer | Amount
101     | Rahul    | 500
102     | Amit     | 800
103     | Priya    | 1200
```

---

# 1. DELETE

`DELETE` ka use **rows delete karne** ke liye hota hai.

### Specific row delete

```sql
DELETE FROM Orders
WHERE OrderId = 101;
```

Result:

```text
102 | Amit  | 800
103 | Priya | 1200
```

Sirf `101` delete hua.

---

## DELETE mein WHERE use kar sakte hain?

### YES ✅

```sql
DELETE FROM Orders
WHERE OrderId = 101;
```

Specific records delete kar sakte ho.

---

## Without WHERE?

```sql
DELETE FROM Orders;
```

Ye table ki **saari rows delete** kar dega.

Lekin table structure rahegi.

```text
Orders table
     ↓
Structure remains
Data removed
```

---

# 2. TRUNCATE

`TRUNCATE` ka use table ki **saari rows ek saath remove** karne ke liye hota hai.

```sql
TRUNCATE TABLE Orders;
```

Result:

```text
Orders
----------------
(empty)
```

Lekin:

```text
Orders table
     ↓
Structure remains
```

---

## TRUNCATE mein WHERE?

### ❌ No

Ye invalid hai:

```sql
TRUNCATE TABLE Orders
WHERE OrderId = 101;
```

TRUNCATE ka purpose hi hai:

> **poori table ka data empty karna.**

---

# 3. DROP

`DROP` sabse different hai.

```sql
DROP TABLE Orders;
```

Ye **table ko hi remove** kar deta hai.

Before:

```text
Database
   |
   └── Orders
          |
          ├── 101
          ├── 102
          └── 103
```

After:

```text
Database
   |
   └── Orders ❌
```

Table hi exist nahi karti.

---

# DELETE vs TRUNCATE vs DROP

| Feature                                 | DELETE                                  | TRUNCATE                                    | DROP                                        |
| --------------------------------------- | --------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| Specific rows delete                    | ✅                                       | ❌                                           | ❌                                           |
| `WHERE`                                 | ✅                                       | ❌                                           | ❌                                           |
| All rows remove                         | ✅ without WHERE                         | ✅                                           | Table itself removed                        |
| Table structure remains                 | ✅                                       | ✅                                           | ❌                                           |
| Table remains usable                    | ✅                                       | ✅                                           | ❌                                           |
| Usually faster for clearing whole table | ❌                                       | ✅                                           | N/A                                         |
| Rollback                                | Transaction ke andar generally possible | DB/transaction support par depend karta hai | DB/transaction support par depend karta hai |

### Ek correction tumhare notes mein important hai:

Tumhare notes mein likha hai:

> `TRUNCATE → Rollback ❌`
> `DROP → Rollback ❌`

Ye **universally correct nahi hai**.

For example, **PostgreSQL mein `TRUNCATE` aur `DROP` transactional hain**, so transaction ke andar rollback kiya ja sakta hai.

Example:

```sql
BEGIN;

TRUNCATE TABLE Orders;

ROLLBACK;
```

PostgreSQL mein rollback ke baad table ka data restore ho sakta hai.

So interview mein better statement:

> **DELETE, TRUNCATE aur DROP ka rollback behavior database system aur transaction context par depend karta hai. PostgreSQL mein ye transactional operations ho sakte hain.**

---
Bilkul bhai. **Normalization** ko ekdum zero se samjho. Ye SQL ka important interview topic hai, aur real backend/database design mein bhi kaafi useful hai.

Hum ek **bad/designed table** lenge aur step-by-step usko:

```text
Unnormalized
   ↓
1NF
   ↓
2NF
   ↓
3NF
   ↓
BCNF
```

mein convert karenge.

---

# Table of Contents

- [1. Normalization kya hoti hai?](#1-normalization-kya-hoti-hai)
- [2. Normalization kyu karte hain?](#2-normalization-kyu-karte-hain)
- [3. Normalization ka main idea](#3-normalization-ka-main-idea)
- [4. Ek important term — Functional Dependency](#4-ek-important-term-functional-dependency)
- [5. Start with a bad table](#5-start-with-a-bad-table)
- [6. 1NF — First Normal Form ⭐⭐⭐⭐⭐](#6-1nf-first-normal-form-)
- [7. 1NF ka correct version](#7-1nf-ka-correct-version)
- [8. 1NF ke baad problem kya hai?](#8-1nf-ke-baad-problem-kya-hai)
- [9. 2NF — Second Normal Form ⭐⭐⭐⭐⭐](#9-2nf-second-normal-form-)
- [10. Partial Dependency kya hai?](#10-partial-dependency-kya-hai)
- [11. 2NF kaise achieve karenge?](#11-2nf-kaise-achieve-karenge)
- [12. SQL mein 2NF design](#12-sql-mein-2nf-design)
- [13. 2NF ko simple language mein yaad rakho](#13-2nf-ko-simple-language-mein-yaad-rakho)
- [14. 3NF — Third Normal Form ⭐⭐⭐⭐⭐](#14-3nf-third-normal-form-)
- [15. Transitive Dependency kya hai?](#15-transitive-dependency-kya-hai)
- [16. 3NF ka rule](#16-3nf-ka-rule)
- [17. Bad 3NF example](#17-bad-3nf-example)
- [18. Correct 3NF design](#18-correct-3nf-design)
- [19. SQL](#19-sql)
- [20. Ab actual data ka flow dekho](#20-ab-actual-data-ka-flow-dekho)
- [21. Data retrieve karna hai to JOIN](#21-data-retrieve-karna-hai-to-join)
- [22. Order + Product complete data](#22-order-product-complete-data)
- [23. BCNF — Boyce-Codd Normal Form ⭐⭐⭐⭐](#23-bcnf-boyce-codd-normal-form-)
- [24. BCNF example](#24-bcnf-example)
- [25. Isko BCNF mein kaise split karenge?](#25-isko-bcnf-mein-kaise-split-karenge)
- [26. 1NF → 2NF → 3NF → BCNF ek hi example se](#26-1nf-2nf-3nf-bcnf-ek-hi-example-se)
- [27. Final normalized design](#27-final-normalized-design)
- [28. SQL complete example](#28-sql-complete-example)
- [29. Insert data](#29-insert-data)
- [30. Ab complete order information kaise nikale?](#30-ab-complete-order-information-kaise-nikale)
- [31. Normalization ka actual benefit](#31-normalization-ka-actual-benefit)
- [32. Product price change](#32-product-price-change)
- [33. But ek important real-world issue — historical price](#33-but-ek-important-real-world-issue-historical-price)
- [34. Normalization vs Denormalization](#34-normalization-vs-denormalization)
- [35. Kya har database ko BCNF tak normalize karna chahiye?](#35-kya-har-database-ko-bcnf-tak-normalize-karna-chahiye)
- [36. Normalization kab use karte hain?](#36-normalization-kab-use-karte-hain)
- [37. Normalization kab less important ho sakti hai?](#37-normalization-kab-less-important-ho-sakti-hai)
- [38. Sabse important difference: 1NF vs 2NF vs 3NF vs BCNF](#38-sabse-important-difference-1nf-vs-2nf-vs-3nf-vs-bcnf)
- [39. Ek aur easy way yaad rakho](#39-ek-aur-easy-way-yaad-rakho)
- [40. Interview answer](#40-interview-answer)
- [41. Final mental picture](#41-final-mental-picture)

---

# 1. Normalization kya hoti hai?

**Normalization = database tables ko logically organize karna taaki data duplication kam ho aur INSERT/UPDATE/DELETE anomalies avoid ho.**

Simple example:

Maan lo tumne ek hi table mein sab kuch daal diya:

```text
OrderId
CustomerName
CustomerPhone
Product1
Product2
Product3
ProductPrice1
ProductPrice2
ProductPrice3
```

Problem:

```text
Customer ka phone change hua
        ↓
multiple orders update karne padenge
```

Ya:

```text
Order mein 5 products aa gaye
        ↓
Product4, Product5 columns add?
```

Ye bad design hai.

Normalization ka goal:

```text
One huge table
      ↓
logical smaller tables
      ↓
relationships using PK/FK
```

---

# 2. Normalization kyu karte hain?

Main 3 problems solve karne ke liye:

### 1. Update Anomaly

Same information multiple places mein stored hai.

```text
CustomerId = 1
Name = Rahul
Phone = 9999999999
```

Agar Rahul ka phone change hua aur 100 orders hain:

```text
100 rows update?
```

Risk:

```text
Order 101 → new phone
Order 102 → new phone
Order 103 → old phone ❌
```

---

### 2. Insert Anomaly

Suppose product ko database mein add karna hai, but abhi koi order nahi hai.

Agar Product information `Orders` table mein hi stored hai:

```text
ProductId
ProductName
ProductPrice
OrderId
CustomerId
```

Order ke bina product insert karna awkward/impossible ho sakta hai.

---

### 3. Delete Anomaly

Suppose last order delete kar diya:

```text
Order 101
Product = Laptop
```

Agar product information sirf order table mein stored thi, order delete karne ke saath product ki information bhi lost ho sakti hai.

---

# 3. Normalization ka main idea

Simple:

```text
Duplicate data kam karo
        +
Data dependencies correct karo
        +
Tables ko logically separate karo
        +
PK/FK se relation banao
```

---

# 4. Ek important term — Functional Dependency

Normalization samajhne ke liye ye concept important hai.

Suppose:

```text
CustomerId → CustomerName
```

Meaning:

> Agar mujhe `CustomerId` pata hai, to corresponding `CustomerName` determine ho jata hai.

Example:

```text
CustomerId = 1
       ↓
CustomerName = Rahul
```

Similarly:

```text
ProductId → ProductName
ProductId → Price
```

Aur:

```text
OrderId → OrderDate
OrderId → CustomerId
```

Functional dependency ka basic notation:

```text
A → B
```

means:

> A determines B.

---

# 5. Start with a bad table

Maan lo requirement hai:

> Customer order karta hai aur order mein multiple products ho sakte hain.

Hum initially ye table banate hain:

### `OrderDetails`

| OrderId | CustomerId | CustomerName | CustomerPhone | ProductId | ProductName | ProductPrice | Quantity |
| ------: | ---------: | ------------ | ------------- | --------: | ----------- | -----------: | -------: |
|     101 |          1 | Rahul        | 9999          |        10 | Laptop      |        50000 |        1 |
|     101 |          1 | Rahul        | 9999          |        11 | Mouse       |         1000 |        2 |
|     102 |          2 | Amit         | 8888          |        10 | Laptop      |        50000 |        1 |

Isme already duplication hai.

For Order 101:

```text
Rahul
9999
```

2 times repeat.

Laptop:

```text
Laptop
50000
```

multiple orders mein repeat.

Ab normalization start karte hain.

---

# 6. 1NF — First Normal Form ⭐⭐⭐⭐⭐

## 1NF ka basic rule

Table mein:

1. Har cell mein **single/atomic value** honi chahiye.
2. Repeating groups nahi hone chahiye.
3. Har row uniquely identifiable honi chahiye.

### Bad example

```text
OrderId | Customer | Products
--------|----------|--------------------
101     | Rahul    | Laptop, Mouse, Keyboard
```

`Products` ke ek cell mein multiple values hain.

Ye 1NF violation hai.

---

# 7. 1NF ka correct version

Instead:

```text
OrderId | Customer | Product
--------|----------|--------
101     | Rahul    | Laptop
101     | Rahul    | Mouse
101     | Rahul    | Keyboard
```

Ab har cell mein one value hai.

Ya hamara pehle wala table:

| OrderId | CustomerId | CustomerName | CustomerPhone | ProductId | ProductName | ProductPrice | Quantity |
| ------: | ---------: | ------------ | ------------- | --------: | ----------- | -----------: | -------: |
|     101 |          1 | Rahul        | 9999          |        10 | Laptop      |        50000 |        1 |
|     101 |          1 | Rahul        | 9999          |        11 | Mouse       |         1000 |        2 |
|     102 |          2 | Amit         | 8888          |        10 | Laptop      |        50000 |        1 |

Ye **1NF structure** satisfy karta hai because each cell contains a single value.

But duplication abhi bhi hai.

---

# 8. 1NF ke baad problem kya hai?

Order 101:

```text
Rahul
9999
```

do rows mein repeated.

Laptop:

```text
Laptop
50000
```

multiple rows mein repeated.

So:

```text
1NF
=
Atomic values
```

Lekin:

```text
1NF ≠ No duplication
```

Ye important hai.

---

# 9. 2NF — Second Normal Form ⭐⭐⭐⭐⭐

2NF samajhne ke liye **composite key** samajhna zaroori hai.

Suppose `OrderDetails` ki key:

```text
(OrderId, ProductId)
```

kyunki:

```text
OrderId = 101
ProductId = 10
```

milkar ek specific order-product row identify karte hain.

Example:

```text
101 + 10 → Laptop in Order 101
101 + 11 → Mouse in Order 101
```

Ab functional dependencies dekho:

```text
(OrderId, ProductId) → Quantity
```

Correct.

But:

```text
OrderId → CustomerId
OrderId → CustomerName
OrderId → CustomerPhone
```

Aur:

```text
ProductId → ProductName
ProductId → ProductPrice
```

Yahan problem hai.

`CustomerName` ko poori composite key ki need nahi.

Sirf:

```text
OrderId
```

se CustomerName determine ho raha hai.

Aur:

```text
ProductId
```

se ProductName determine ho raha hai.

Isko **partial dependency** kehte hain.

---

# 10. Partial Dependency kya hai?

Suppose composite key:

```text
(OrderId, ProductId)
```

hai.

Agar koi non-key column sirf key ke ek part par depend karta hai:

```text
OrderId → CustomerName
```

to ye:

> **Partial Dependency**

hai.

2NF ka main goal:

> **Partial dependency remove karo.**

---

# 11. 2NF kaise achieve karenge?

Ek giant table ko 3 tables mein divide karte hain.

### Orders

| OrderId | CustomerId |
| ------: | ---------: |
|     101 |          1 |
|     102 |          2 |

### Customers

| CustomerId | CustomerName | CustomerPhone |
| ---------: | ------------ | ------------- |
|          1 | Rahul        | 9999          |
|          2 | Amit         | 8888          |

### Products

| ProductId | ProductName | ProductPrice |
| --------: | ----------- | -----------: |
|        10 | Laptop      |        50000 |
|        11 | Mouse       |         1000 |

### OrderItems

| OrderId | ProductId | Quantity |
| ------: | --------: | -------: |
|     101 |        10 |        1 |
|     101 |        11 |        2 |
|     102 |        10 |        1 |

Ab:

```text
Orders
  ↓
CustomerId → Customers

OrderItems
  ↓
ProductId → Products
```

Aur:

```text
(OrderId, ProductId) → Quantity
```

Quantity poori composite key par depend karti hai.

---

# 12. SQL mein 2NF design

### Customers

```sql
CREATE TABLE Customers
(
    CustomerId INT PRIMARY KEY,
    CustomerName VARCHAR(100),
    CustomerPhone VARCHAR(20)
);
```

### Products

```sql
CREATE TABLE Products
(
    ProductId INT PRIMARY KEY,
    ProductName VARCHAR(100),
    ProductPrice NUMERIC(10,2)
);
```

### Orders

```sql
CREATE TABLE Orders
(
    OrderId INT PRIMARY KEY,
    CustomerId INT NOT NULL,

    CONSTRAINT fk_orders_customer
        FOREIGN KEY (CustomerId)
        REFERENCES Customers(CustomerId)
);
```

### OrderItems

```sql
CREATE TABLE OrderItems
(
    OrderId INT,
    ProductId INT,
    Quantity INT NOT NULL,

    PRIMARY KEY (OrderId, ProductId),

    FOREIGN KEY (OrderId)
        REFERENCES Orders(OrderId),

    FOREIGN KEY (ProductId)
        REFERENCES Products(ProductId)
);
```

Ab data logically separate hai.

---

# 13. 2NF ko simple language mein yaad rakho

```text
1NF
↓
Atomic values

2NF
↓
1NF +
No Partial Dependency
```

Aur **partial dependency mainly tab relevant hoti hai jab composite key ho**.

Agar table ki key single column hai:

```text
OrderId
```

to partial dependency ka issue generally nahi hota.

---

# 14. 3NF — Third Normal Form ⭐⭐⭐⭐⭐

Ab maan lo humare `Orders` table mein:

```text
OrderId
CustomerId
CustomerName
CustomerPhone
OrderDate
```

hai.

Suppose:

```text
OrderId → CustomerId
CustomerId → CustomerName
CustomerId → CustomerPhone
```

So:

```text
OrderId
   ↓
CustomerId
   ↓
CustomerName
CustomerPhone
```

`CustomerName` directly `OrderId` se logically identify nahi ho raha.

It is dependent through `CustomerId`.

Isko **transitive dependency** kehte hain.

---

# 15. Transitive Dependency kya hai?

Simple:

```text
A → B
B → C

therefore

A → C
```

Example:

```text
OrderId → CustomerId
CustomerId → CustomerName
```

Therefore:

```text
OrderId → CustomerName
```

But CustomerName actually customer ki property hai.

So CustomerName ko Orders mein store karna unnecessary duplication hai.

---

# 16. 3NF ka rule

Simple interview definition:

> **Table should be in 2NF and no non-key attribute should depend on another non-key attribute.**

Ya easy language:

```text
Non-key column
      ↓
should depend on
      ↓
the key
```

Not:

```text
Key
 ↓
Non-key
 ↓
Another non-key
```

---

# 17. Bad 3NF example

```text
Orders

OrderId
CustomerId
CustomerName
CustomerPhone
OrderDate
```

Dependencies:

```text
OrderId → CustomerId
CustomerId → CustomerName
CustomerId → CustomerPhone
```

Problem:

```text
OrderId
   ↓
CustomerId
   ↓
CustomerName
```

---

# 18. Correct 3NF design

### Orders

```text
OrderId
CustomerId
OrderDate
```

### Customers

```text
CustomerId
CustomerName
CustomerPhone
```

Now:

```text
Orders
OrderId → CustomerId

Customers
CustomerId → CustomerName
CustomerId → CustomerPhone
```

Perfect logical separation.

---

# 19. SQL

```sql
CREATE TABLE Customers
(
    CustomerId INT PRIMARY KEY,
    CustomerName VARCHAR(100) NOT NULL,
    CustomerPhone VARCHAR(20)
);
```

```sql
CREATE TABLE Orders
(
    OrderId INT PRIMARY KEY,
    CustomerId INT NOT NULL,
    OrderDate TIMESTAMP NOT NULL,

    CONSTRAINT fk_orders_customer
        FOREIGN KEY (CustomerId)
        REFERENCES Customers(CustomerId)
);
```

Customer name order mein repeat nahi hoga.

---

# 20. Ab actual data ka flow dekho

### Customers

| CustomerId | CustomerName | Phone |
| ---------: | ------------ | ----- |
|          1 | Rahul        | 9999  |
|          2 | Amit         | 8888  |

### Orders

| OrderId | CustomerId | OrderDate  |
| ------: | ---------: | ---------- |
|     101 |          1 | 2026-09-20 |
|     102 |          1 | 2026-09-21 |
|     103 |          2 | 2026-09-22 |

Rahul ke 2 orders hain.

Lekin Rahul ka data sirf:

```text
Customers
CustomerId = 1
```

mein stored hai.

Orders mein:

```text
1
```

reference hai.

---

# 21. Data retrieve karna hai to JOIN

Normalization ke baad data multiple tables mein hai.

Agar complete order information chahiye:

```sql
SELECT
    o.OrderId,
    o.OrderDate,
    c.CustomerId,
    c.CustomerName,
    c.CustomerPhone
FROM Orders o
JOIN Customers c
    ON o.CustomerId = c.CustomerId;
```

Result:

| OrderId | OrderDate  | CustomerId | CustomerName | CustomerPhone |
| ------: | ---------- | ---------: | ------------ | ------------- |
|     101 | 2026-09-20 |          1 | Rahul        | 9999          |
|     102 | 2026-09-21 |          1 | Rahul        | 9999          |
|     103 | 2026-09-22 |          2 | Amit         | 8888          |

Database mein data normalized hai, but query result mein combined data mil gaya.

---

# 22. Order + Product complete data

```sql
SELECT
    o.OrderId,
    c.CustomerName,
    p.ProductName,
    p.ProductPrice,
    oi.Quantity
FROM Orders o
JOIN Customers c
    ON o.CustomerId = c.CustomerId
JOIN OrderItems oi
    ON o.OrderId = oi.OrderId
JOIN Products p
    ON oi.ProductId = p.ProductId;
```

Flow:

```text
Customers
    ↑
    |
Orders
    |
    ↓
OrderItems
    |
    ↓
Products
```

---

# 23. BCNF — Boyce-Codd Normal Form ⭐⭐⭐⭐

Ab advanced part.

BCNF ko samajhne ke liye functional dependency aur candidate key samajhna zaroori hai.

Simple rule:

> **For every non-trivial functional dependency `X → Y`, X should be a superkey.**

Easy Hinglish:

> Agar `A → B` hai, to `A` ko table ki row uniquely identify karne ki capability honi chahiye.

3NF se **BCNF stricter** hai.

```text
BCNF
   ↓
3NF se stricter
```

---

# 24. BCNF example

Suppose university table:

```text
StudentCourse

StudentId | CourseId | Instructor
----------|----------|-----------
1         | C101     | Ravi
2         | C101     | Ravi
3         | C102     | Amit
```

Assume business rule:

> Har course ka ek instructor hai.

So:

```text
CourseId → Instructor
```

But suppose primary key:

```text
(StudentId, CourseId)
```

hai.

Ab:

```text
CourseId → Instructor
```

Lekin `CourseId` alone complete row identify nahi karta.

Example:

```text
C101
```

ke multiple students hain.

So `CourseId` is **not a superkey**.

Therefore BCNF violation.

---

# 25. Isko BCNF mein kaise split karenge?

### Course

| CourseId | Instructor |
| -------- | ---------- |
| C101     | Ravi       |
| C102     | Amit       |

### StudentCourse

| StudentId | CourseId |
| --------- | -------- |
| 1         | C101     |
| 2         | C101     |
| 3         | C102     |

SQL:

```sql
CREATE TABLE Courses
(
    CourseId VARCHAR(20) PRIMARY KEY,
    Instructor VARCHAR(100) NOT NULL
);
```

```sql
CREATE TABLE StudentCourses
(
    StudentId INT,
    CourseId VARCHAR(20),

    PRIMARY KEY (StudentId, CourseId),

    FOREIGN KEY (CourseId)
        REFERENCES Courses(CourseId)
);
```

Now:

```text
CourseId → Instructor
```

and:

```text
CourseId
```

is primary key of `Courses`.

So dependency valid hai.

---

# 26. 1NF → 2NF → 3NF → BCNF ek hi example se

Ye sabse important revision hai.

Initial table:

```text
OrderDetails

OrderId
CustomerId
CustomerName
CustomerPhone
ProductId
ProductName
ProductPrice
Quantity
```

---

## Step 1 — 1NF

Ensure:

```text
One cell = one value
```

Example:

❌

```text
Products = Laptop, Mouse, Keyboard
```

Correct:

```text
Laptop
Mouse
Keyboard
```

---

## Step 2 — 2NF

Remove:

```text
Partial Dependency
```

Composite key:

```text
(OrderId, ProductId)
```

But:

```text
OrderId → CustomerId
ProductId → ProductName
```

So separate:

```text
Orders
Customers
Products
OrderItems
```

---

## Step 3 — 3NF

Remove:

```text
Transitive Dependency
```

Bad:

```text
Orders

OrderId
CustomerId
CustomerName
```

because:

```text
OrderId
 ↓
CustomerId
 ↓
CustomerName
```

Move CustomerName to Customers.

---

## Step 4 — BCNF

Check every functional dependency:

```text
X → Y
```

`X` should be a superkey.

If not:

```text
decompose table
```

---

# 27. Final normalized design

```text
                 Customers
                 ----------
                 CustomerId PK
                 CustomerName
                 CustomerPhone
                     ↑
                     |
                     |
Orders               |
------               |
OrderId PK ----------+
CustomerId FK
OrderDate
   |
   |
   ↓
OrderItems
----------
OrderId PK/FK
ProductId PK/FK
Quantity
   |
   |
   ↓
Products
--------
ProductId PK
ProductName
ProductPrice
```

Actually relationship:

```text
Customers
    |
    | 1
    |
    | many
Orders
    |
    | 1
    |
    | many
OrderItems
    |
    | many
    |
    | 1
Products
```

---

# 28. SQL complete example

### Customers

```sql
CREATE TABLE Customers
(
    CustomerId INT PRIMARY KEY,
    CustomerName VARCHAR(100) NOT NULL,
    CustomerPhone VARCHAR(20)
);
```

### Products

```sql
CREATE TABLE Products
(
    ProductId INT PRIMARY KEY,
    ProductName VARCHAR(100) NOT NULL,
    ProductPrice NUMERIC(10,2) NOT NULL
);
```

### Orders

```sql
CREATE TABLE Orders
(
    OrderId INT PRIMARY KEY,
    CustomerId INT NOT NULL,
    OrderDate TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_orders_customer
        FOREIGN KEY (CustomerId)
        REFERENCES Customers(CustomerId)
);
```

### OrderItems

```sql
CREATE TABLE OrderItems
(
    OrderId INT NOT NULL,
    ProductId INT NOT NULL,
    Quantity INT NOT NULL CHECK (Quantity > 0),

    PRIMARY KEY (OrderId, ProductId),

    CONSTRAINT fk_orderitems_order
        FOREIGN KEY (OrderId)
        REFERENCES Orders(OrderId),

    CONSTRAINT fk_orderitems_product
        FOREIGN KEY (ProductId)
        REFERENCES Products(ProductId)
);
```

---

# 29. Insert data

### Customers

```sql
INSERT INTO Customers
(
    CustomerId,
    CustomerName,
    CustomerPhone
)
VALUES
(1, 'Rahul', '9999999999'),
(2, 'Amit', '8888888888');
```

### Products

```sql
INSERT INTO Products
(
    ProductId,
    ProductName,
    ProductPrice
)
VALUES
(10, 'Laptop', 50000),
(11, 'Mouse', 1000),
(12, 'Keyboard', 2000);
```

### Orders

```sql
INSERT INTO Orders
(
    OrderId,
    CustomerId
)
VALUES
(101, 1),
(102, 2);
```

### OrderItems

```sql
INSERT INTO OrderItems
(
    OrderId,
    ProductId,
    Quantity
)
VALUES
(101, 10, 1),
(101, 11, 2),
(102, 12, 1);
```

---

# 30. Ab complete order information kaise nikale?

```sql
SELECT
    o.OrderId,
    o.OrderDate,
    c.CustomerName,
    c.CustomerPhone,
    p.ProductName,
    p.ProductPrice,
    oi.Quantity
FROM Orders o
JOIN Customers c
    ON o.CustomerId = c.CustomerId
JOIN OrderItems oi
    ON o.OrderId = oi.OrderId
JOIN Products p
    ON oi.ProductId = p.ProductId;
```

Result:

| OrderId | CustomerName | Product  | Price | Qty |
| ------: | ------------ | -------- | ----: | --: |
|     101 | Rahul        | Laptop   | 50000 |   1 |
|     101 | Rahul        | Mouse    |  1000 |   2 |
|     102 | Amit         | Keyboard |  2000 |   1 |

---

# 31. Normalization ka actual benefit

Suppose Rahul ka phone change hua:

```sql
UPDATE Customers
SET CustomerPhone = '7777777777'
WHERE CustomerId = 1;
```

Bas **one row update**.

Agar unnormalized table hota:

```text
Order 101 → Rahul → old phone
Order 105 → Rahul → old phone
Order 110 → Rahul → old phone
Order 150 → Rahul → old phone
...
```

multiple rows update karni padti.

---

# 32. Product price change

Suppose Mouse price:

```text
1000 → 1200
```

Normalized:

```sql
UPDATE Products
SET ProductPrice = 1200
WHERE ProductId = 11;
```

Bas one product record.

---

# 33. But ek important real-world issue — historical price

Yahan real systems mein ek interesting design question aata hai.

Suppose:

```text
Products
Mouse = ₹1000
```

Customer ne January mein Mouse ₹1000 mein kharida.

March mein:

```text
Mouse = ₹1200
```

Agar `OrderItems` sirf:

```text
ProductId
Quantity
```

rakhta hai aur current price `Products` se leta hai, old order incorrectly ₹1200 show kar sakta hai.

Isliye real e-commerce systems often `OrderItems` mein **price-at-purchase** store karte hain:

```text
OrderItems

OrderId
ProductId
Quantity
UnitPrice
```

Example:

| OrderId | ProductId | Quantity | UnitPrice |
| ------: | --------: | -------: | --------: |
|     101 |        11 |        2 |      1000 |

Even if:

```text
Products.ProductPrice = 1200
```

old order ka `UnitPrice = 1000` preserve rahega.

### Ye denormalization nahi necessarily bad design hai.

Ye **business requirement / historical snapshot** ho sakta hai.

Normalization ka matlab blindly har repeated value remove karna nahi hai.

---

# 34. Normalization vs Denormalization

Real production databases mein **100% normalization always goal nahi hota**.

Sometimes read performance ke liye intentionally data duplicate/store kiya jata hai.

Example:

Normalized:

```text
Orders
   ↓
Customers
   ↓
JOIN
```

Agar extremely high-volume reporting query hai, system intentionally some derived/duplicated data maintain kar sakta hai.

Isko:

> **Denormalization**

kehte hain.

Example:

```text
Orders

OrderId
CustomerId
CustomerName
TotalAmount
```

`CustomerName` theoretically Customers table se aa sakta tha, but intentionally duplicate stored ho sakta hai for a specific use case.

Trade-off:

```text
Normalization
→ less redundancy
→ easier consistency
→ more JOINs sometimes

Denormalization
→ potentially faster/simple reads
→ more storage
→ duplicate data
→ consistency maintenance harder
```

---

# 35. Kya har database ko BCNF tak normalize karna chahiye?

**Nahi.**

Practical database design mein usually:

```text
1NF
 ↓
2NF
 ↓
3NF
```

common target hai for OLTP-style relational design.

BCNF tab relevant hota hai jab functional dependencies ki wajah se 3NF ke baad bhi redundancy/anomaly remain karti ho.

Aur performance/business requirements ke according controlled denormalization bhi ho sakti hai.

---

# 36. Normalization kab use karte hain?

Especially:

### OLTP systems

Jaise:

```text
E-commerce
Banking
ERP
WMS
Hospital Management
Order Management
Inventory
```

Jahan:

```text
INSERT
UPDATE
DELETE
```

frequently hote hain aur data consistency important hoti hai.

Tumhare WMS/e-commerce type systems mein normalization ka concept particularly relevant hai.

---

# 37. Normalization kab less important ho sakti hai?

Analytics/reporting/data warehouse systems mein sometimes denormalized models deliberately use hote hain.

Example:

```text
FactSales
    +
DimensionCustomer
    +
DimensionProduct
```

ya reporting-friendly wide tables.

Reason:

```text
Fast analytical reads
```

But ye domain aur architecture par depend karta hai.

---

# 38. Sabse important difference: 1NF vs 2NF vs 3NF vs BCNF

| Normal Form | Main Problem Solved                    |
| ----------- | -------------------------------------- |
| **1NF**     | Repeating/multi-valued groups          |
| **2NF**     | Partial dependency                     |
| **3NF**     | Transitive dependency                  |
| **BCNF**    | Every determinant should be a superkey |

Memory trick:

```text
1NF
↓
Atomic

2NF
↓
No Partial Dependency

3NF
↓
No Transitive Dependency

BCNF
↓
Every determinant = Superkey
```

---

# 39. Ek aur easy way yaad rakho

Imagine:

```text
StudentId
CourseId
CourseName
TeacherId
TeacherName
```

### 1NF

Har cell single:

```text
CourseName = "SQL"
```

not:

```text
SQL, C#, Java
```

### 2NF

Composite key:

```text
(StudentId, CourseId)
```

But:

```text
CourseId → CourseName
```

So CourseName only CourseId par dependent hai.

Separate:

```text
Courses
```

### 3NF

Suppose:

```text
CourseId → TeacherId
TeacherId → TeacherName
```

Then:

```text
CourseId → TeacherName
```

transitive dependency.

Separate:

```text
Teachers
```

### BCNF

Agar koi dependency:

```text
X → Y
```

hai aur X superkey nahi hai, BCNF issue ho sakta hai.

---

# 40. Interview answer

### What is normalization?

> **Normalization is the process of organizing relational database tables to reduce data redundancy and avoid insertion, update, and deletion anomalies while maintaining data integrity.**

### 1NF?

> **Each column contains atomic values and there are no repeating groups.**

### 2NF?

> **A table must be in 1NF and every non-key attribute must depend on the whole candidate key, not just a part of a composite key.**

### 3NF?

> **A table must be in 2NF and non-key attributes should not depend on other non-key attributes; transitive dependencies are removed.**

### BCNF?

> **For every non-trivial functional dependency X → Y, X must be a superkey. BCNF is stricter than 3NF.**

---

# 41. Final mental picture

```text
                 NORMALIZATION
                       │
                       ↓
              Reduce Redundancy
                       │
          ┌────────────┼────────────┐
          ↓            ↓            ↓
       INSERT        UPDATE       DELETE
       anomaly       anomaly      anomaly
          │            │            │
          └────────────┼────────────┘
                       ↓
                    1NF
                       │
               Atomic values
                       ↓
                    2NF
                       │
             Remove partial dependency
                       ↓
                    3NF
                       │
            Remove transitive dependency
                       ↓
                   BCNF
                       │
       Every determinant should be superkey
```

Aur tumhare **Orders example** ko ek line mein:

```text
Bad:
Order + Customer + Product sab ek table

        ↓ 1NF

Atomic rows

        ↓ 2NF

Orders + Products + OrderItems

        ↓ 3NF

Customers ko separate karo

        ↓ BCNF

Check functional dependencies:
har determinant superkey hai ya nahi
```

**Most important point:** Normalization ka purpose **"tables ko maximum todna"** nahi hai. Purpose hai **data ko logically store karna, unnecessary duplication aur anomalies avoid karna**. Real production design mein uske baad query performance, indexes, historical data, reporting needs, aur business requirements dekhkar denormalization bhi ki ja sakti hai.

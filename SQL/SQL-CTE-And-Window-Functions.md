Bilkul. **Window Functions** aur **CTE (Common Table Expression)** dono SQL interviews mein important hain, aur dono ka purpose alag hai.

Main `Orders` + `OrderItems` example se zero se samjhaunga.

---

# Part 1 — Window Functions

## 1. Window Function kya hoti hai?

Simple definition:

> **Window Function kisi row ke saath related rows ka calculation karti hai, lekin original rows ko collapse nahi karti.**

Ye point sabse important hai.

### `GROUP BY` vs Window Function

Suppose `Orders`:

| OrderId | CustomerId | Amount |
| ------: | ---------: | -----: |
|     101 |          1 |   5000 |
|     102 |          1 |   3000 |
|     103 |          1 |   7000 |
|     104 |          2 |   2000 |
|     105 |          2 |   4000 |

Agar:

```sql
SELECT CustomerId, SUM(Amount) AS TotalAmount
FROM Orders
GROUP BY CustomerId;
```

Result:

| CustomerId | TotalAmount |
| ---------: | ----------: |
|          1 |       15000 |
|          2 |        6000 |

Notice:

**5 rows → 2 rows**

`GROUP BY` rows ko groups mein collapse kar deta hai.

---

## Window Function

Ab same calculation window function se:

```sql
SELECT
    OrderId,
    CustomerId,
    Amount,
    SUM(Amount) OVER (
        PARTITION BY CustomerId
    ) AS CustomerTotal
FROM Orders;
```

Result:

| OrderId | CustomerId | Amount | CustomerTotal |
| ------: | ---------: | -----: | ------------: |
|     101 |          1 |   5000 |         15000 |
|     102 |          1 |   3000 |         15000 |
|     103 |          1 |   7000 |         15000 |
|     104 |          2 |   2000 |          6000 |
|     105 |          2 |   4000 |          6000 |

Yahan:

**5 rows → 5 rows**

Bas har row ke saath us customer ka total add ho gaya.

### Isliye:

```text
GROUP BY
Rows
 ↓
Group
 ↓
One result per group
```

while:

```text
Window Function
Rows
 ↓
Calculate over related rows
 ↓
Original rows remain
```

---

# 2. Window Function ka basic syntax

General syntax:

```sql
function_name(...) OVER (
    PARTITION BY ...
    ORDER BY ...
)
```

Example:

```sql
SUM(Amount) OVER (
    PARTITION BY CustomerId
)
```

Teen important parts:

```text
SUM(Amount)
    ↓
Kya calculation karni hai?

OVER(...)
    ↓
Window define karta hai

PARTITION BY CustomerId
    ↓
Kin rows ke group ke andar calculation karni hai?
```

---

# 3. `PARTITION BY` kya hota hai?

`PARTITION BY` window ke andar groups banata hai.

Example:

```sql
SUM(Amount) OVER (
    PARTITION BY CustomerId
)
```

Data internally:

```text
Customer 1
──────────
5000
3000
7000
 ↓
15000


Customer 2
──────────
2000
4000
 ↓
6000
```

Lekin rows delete/collapse nahi hoti.

---

# 4. `PARTITION BY` vs `GROUP BY`

Bahut important interview question.

### GROUP BY

```sql
SELECT
    CustomerId,
    SUM(Amount)
FROM Orders
GROUP BY CustomerId;
```

Result:

```text
Customer 1 → 15000
Customer 2 → 6000
```

### Window

```sql
SELECT
    OrderId,
    CustomerId,
    Amount,
    SUM(Amount) OVER (
        PARTITION BY CustomerId
    ) AS CustomerTotal
FROM Orders;
```

Result:

```text
101 → 5000 → 15000
102 → 3000 → 15000
103 → 7000 → 15000
104 → 2000 → 6000
105 → 4000 → 6000
```

### Simple rule

> **GROUP BY = rows ko combine karta hai.**

> **Window Function = rows ko combine nahi karta, calculation ka result har relevant row ke saath deta hai.**

---

# 5. Window Functions ke main types

Window functions ko practical usage ke according samjho:

### Ranking Functions

* `ROW_NUMBER()`
* `RANK()`
* `DENSE_RANK()`
* `NTILE()`

### Aggregate Window Functions

* `SUM()`
* `COUNT()`
* `AVG()`
* `MIN()`
* `MAX()`

### Value / Navigation Functions

* `LAG()`
* `LEAD()`
* `FIRST_VALUE()`
* `LAST_VALUE()`

Ab ek-ek karke.

---

# 6. `ROW_NUMBER()`

`ROW_NUMBER()` har row ko unique sequential number deta hai.

Example:

```sql
SELECT
    OrderId,
    CustomerId,
    Amount,
    ROW_NUMBER() OVER (
        ORDER BY Amount DESC
    ) AS RowNumber
FROM Orders;
```

Result:

| OrderId | CustomerId | Amount | RowNumber |
| ------: | ---------: | -----: | --------: |
|     103 |          1 |   7000 |         1 |
|     101 |          1 |   5000 |         2 |
|     105 |          2 |   4000 |         3 |
|     102 |          1 |   3000 |         4 |
|     104 |          2 |   2000 |         5 |

### Kab use?

Jab tumhe:

* numbering chahiye
* latest record per customer nikalna ho
* duplicate records identify/remove karne ho
* top record per group chahiye

---

# 7. `ROW_NUMBER()` + `PARTITION BY`

Ye bahut important practical use case hai.

Requirement:

> Har customer ka highest-value order nikalna hai.

Query:

```sql
SELECT
    OrderId,
    CustomerId,
    Amount,
    ROW_NUMBER() OVER (
        PARTITION BY CustomerId
        ORDER BY Amount DESC
    ) AS rn
FROM Orders;
```

Result:

| OrderId | CustomerId | Amount | rn |
| ------: | ---------: | -----: | -: |
|     103 |          1 |   7000 |  1 |
|     101 |          1 |   5000 |  2 |
|     102 |          1 |   3000 |  3 |
|     105 |          2 |   4000 |  1 |
|     104 |          2 |   2000 |  2 |

Notice:

Customer 1 ki numbering:

```text
7000 → 1
5000 → 2
3000 → 3
```

Customer 2 ki numbering separately:

```text
4000 → 1
2000 → 2
```

---

# 8. `RANK()`

`RANK()` ranking deta hai, lekin **same value hone par same rank** deta hai.

Example:

| OrderId | Amount |
| ------: | -----: |
|     101 |   5000 |
|     102 |   5000 |
|     103 |   3000 |
|     104 |   2000 |

```sql
SELECT
    OrderId,
    Amount,
    RANK() OVER (
        ORDER BY Amount DESC
    ) AS RankNo
FROM Orders;
```

Result:

| OrderId | Amount | RankNo |
| ------: | -----: | -----: |
|     101 |   5000 |      1 |
|     102 |   5000 |      1 |
|     103 |   3000 |      3 |
|     104 |   2000 |      4 |

Notice:

```text
5000 → Rank 1
5000 → Rank 1
3000 → Rank 3
```

**Rank 2 skip ho gaya.**

---

# 9. `DENSE_RANK()`

Same values ko same rank deta hai, lekin gap nahi chhodta.

```sql
SELECT
    OrderId,
    Amount,
    DENSE_RANK() OVER (
        ORDER BY Amount DESC
    ) AS RankNo
FROM Orders;
```

Result:

| OrderId | Amount | RankNo |
| ------: | -----: | -----: |
|     101 |   5000 |      1 |
|     102 |   5000 |      1 |
|     103 |   3000 |      2 |
|     104 |   2000 |      3 |

Difference:

```text
ROW_NUMBER
5000 → 1
5000 → 2


RANK
5000 → 1
5000 → 1
3000 → 3


DENSE_RANK
5000 → 1
5000 → 1
3000 → 2
```

### Interview shortcut

```text
ROW_NUMBER → always unique number

RANK → same rank + gaps

DENSE_RANK → same rank + no gaps
```

---

# 10. `SUM()` Window Function

Ab aggregate window functions.

```sql
SELECT
    OrderId,
    CustomerId,
    Amount,
    SUM(Amount) OVER (
        PARTITION BY CustomerId
    ) AS CustomerTotal
FROM Orders;
```

Iska use:

> Har order ke saath customer ka total spending dikhana.

---

# 11. Running Total

Ye window functions ka bahut common use case hai.

Suppose:

| OrderId | CustomerId | Amount |
| ------: | ---------: | -----: |
|     101 |          1 |   5000 |
|     102 |          1 |   3000 |
|     103 |          1 |   7000 |

Query:

```sql
SELECT
    OrderId,
    CustomerId,
    Amount,
    SUM(Amount) OVER (
        PARTITION BY CustomerId
        ORDER BY OrderId
    ) AS RunningTotal
FROM Orders;
```

Result:

| OrderId | Amount | RunningTotal |
| ------: | -----: | -----------: |
|     101 |   5000 |         5000 |
|     102 |   3000 |         8000 |
|     103 |   7000 |        15000 |

Calculation:

```text
5000
  ↓
5000 + 3000 = 8000
  ↓
8000 + 7000 = 15000
```

### Kab use?

* Running sales
* Running balance
* Cumulative quantity
* Cumulative revenue
* Daily/monthly cumulative totals

---

# 12. `COUNT()` Window Function

Suppose customer ke kitne orders hain, har order ke saath dikhana hai:

```sql
SELECT
    OrderId,
    CustomerId,
    COUNT(*) OVER (
        PARTITION BY CustomerId
    ) AS CustomerOrderCount
FROM Orders;
```

Result:

| OrderId | CustomerId | CustomerOrderCount |
| ------: | ---------: | -----------------: |
|     101 |          1 |                  3 |
|     102 |          1 |                  3 |
|     103 |          1 |                  3 |
|     104 |          2 |                  2 |
|     105 |          2 |                  2 |

Again:

**Rows remain.**

---

# 13. `AVG()`, `MIN()`, `MAX()`

Same concept.

```sql
SELECT
    OrderId,
    CustomerId,
    Amount,

    AVG(Amount) OVER (
        PARTITION BY CustomerId
    ) AS AverageAmount,

    MIN(Amount) OVER (
        PARTITION BY CustomerId
    ) AS MinimumAmount,

    MAX(Amount) OVER (
        PARTITION BY CustomerId
    ) AS MaximumAmount

FROM Orders;
```

Har customer ke order ke saath:

* average
* minimum
* maximum

dikhega.

---

# 14. `LAG()`

Ab ek bahut useful function:

`LAG()` → **previous row ki value**

Suppose:

| OrderId | CustomerId | Amount |
| ------: | ---------: | -----: |
|     101 |          1 |   5000 |
|     102 |          1 |   3000 |
|     103 |          1 |   7000 |

Query:

```sql
SELECT
    OrderId,
    Amount,
    LAG(Amount) OVER (
        ORDER BY OrderId
    ) AS PreviousAmount
FROM Orders;
```

Result:

| OrderId | Amount | PreviousAmount |
| ------: | -----: | -------------: |
|     101 |   5000 |           NULL |
|     102 |   3000 |           5000 |
|     103 |   7000 |           3000 |

`LAG()` previous row se value laata hai.

### First row NULL kyun?

Because usse pehle koi row hai hi nahi.

---

# 15. `LEAD()`

`LEAD()` opposite hai.

> Next row ki value deta hai.

```sql
SELECT
    OrderId,
    Amount,
    LEAD(Amount) OVER (
        ORDER BY OrderId
    ) AS NextAmount
FROM Orders;
```

Result:

| OrderId | Amount | NextAmount |
| ------: | -----: | ---------: |
|     101 |   5000 |       3000 |
|     102 |   3000 |       7000 |
|     103 |   7000 |       NULL |

Yaad rakho:

```text
LAG  → Previous
LEAD → Next
```

---

# 16. Window Functions ka real-world use

Backend/WMS type systems mein examples:

### 1. Latest order per customer

```sql
ROW_NUMBER()
```

### 2. Highest order per customer

```sql
ROW_NUMBER()
ORDER BY Amount DESC
```

### 3. Customer total

```sql
SUM() OVER (PARTITION BY CustomerId)
```

### 4. Running sales

```sql
SUM() OVER (ORDER BY OrderId)
```

### 5. Previous order amount

```sql
LAG()
```

### 6. Next order amount

```sql
LEAD()
```

### 7. Ranking

```sql
RANK()
DENSE_RANK()
```

---

# Part 2 — CTE

Ab CTE samjho.

## 17. CTE kya hota hai?

CTE = **Common Table Expression**

Simple definition:

> CTE ek temporary named result set hota hai jo ek query ke duration ke liye exist karta hai.

Syntax:

```sql
WITH cte_name AS
(
    SELECT ...
)
SELECT *
FROM cte_name;
```

Important:

**CTE permanent table nahi hai.**

Database mein permanently save nahi hota.

---

# 18. CTE kyun use karte hain?

Suppose query bahut complex hai:

```sql
SELECT ...
FROM ...
JOIN ...
WHERE ...
GROUP BY ...
HAVING ...
```

Aur tumhe us query ke result ko next query mein use karna hai.

Without CTE query difficult/readability poor ho sakti hai.

CTE:

```text
Step 1
  ↓
CTE
  ↓
Step 2
  ↓
Final Result
```

Yaani complex query ko logical steps mein divide kar sakte ho.

---

# 19. Simple CTE example

Suppose hume delivered orders chahiye.

```sql
WITH DeliveredOrders AS
(
    SELECT *
    FROM Orders
    WHERE Status = 'Delivered'
)
SELECT *
FROM DeliveredOrders;
```

Yahan:

```text
WITH
 ↓
DeliveredOrders
 ↓
SELECT delivered orders
```

`DeliveredOrders` actual table nahi hai.

Ye sirf **is query ke execution ke liye named result** hai.

---

# 20. CTE + GROUP BY

Suppose pehle customer-wise order total calculate karna hai:

```sql
WITH CustomerTotals AS
(
    SELECT
        CustomerId,
        SUM(Amount) AS TotalAmount
    FROM Orders
    GROUP BY CustomerId
)
SELECT *
FROM CustomerTotals;
```

CTE result:

| CustomerId | TotalAmount |
| ---------: | ----------: |
|          1 |       15000 |
|          2 |        6000 |

Then outer query:

```sql
SELECT *
FROM CustomerTotals
WHERE TotalAmount > 10000;
```

Complete:

```sql
WITH CustomerTotals AS
(
    SELECT
        CustomerId,
        SUM(Amount) AS TotalAmount
    FROM Orders
    GROUP BY CustomerId
)
SELECT *
FROM CustomerTotals
WHERE TotalAmount > 10000;
```

Result:

| CustomerId | TotalAmount |
| ---------: | ----------: |
|          1 |       15000 |

---

# 21. CTE ka main benefit

Without CTE:

```sql
SELECT *
FROM
(
    SELECT
        CustomerId,
        SUM(Amount) AS TotalAmount
    FROM Orders
    GROUP BY CustomerId
) x
WHERE TotalAmount > 10000;
```

Ye bhi valid hai.

Lekin CTE:

```sql
WITH CustomerTotals AS
(
    SELECT
        CustomerId,
        SUM(Amount) AS TotalAmount
    FROM Orders
    GROUP BY CustomerId
)
SELECT *
FROM CustomerTotals
WHERE TotalAmount > 10000;
```

**Readability better.**

Tum logically soch sakte ho:

```text
Step 1:
CustomerTotals calculate karo

Step 2:
CustomerTotals mein se > 10000 nikalo
```

---

# 22. CTE + JOIN

Suppose pehle delivered orders nikalne hain, then items join karne hain:

```sql
WITH DeliveredOrders AS
(
    SELECT *
    FROM Orders
    WHERE Status = 'Delivered'
)
SELECT
    o.OrderId,
    o.CustomerId,
    oi.Product,
    oi.Quantity
FROM DeliveredOrders o
JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

Flow:

```text
Orders
  ↓
WHERE Status = Delivered
  ↓
DeliveredOrders CTE
  ↓
JOIN OrderItems
  ↓
Final result
```

---

# 23. CTE + Window Function

Ye bahut important combination hai.

Requirement:

> Har customer ka highest-value order nikalna hai.

Directly:

```sql
SELECT
    OrderId,
    CustomerId,
    Amount,
    ROW_NUMBER() OVER (
        PARTITION BY CustomerId
        ORDER BY Amount DESC
    ) AS rn
FROM Orders;
```

Ab hume sirf `rn = 1` chahiye.

Problem:

Window function ko same query ke `WHERE` mein directly aise nahi use kar sakte:

```sql
SELECT ...
FROM Orders
WHERE ROW_NUMBER() OVER (...) = 1;
```

Ye valid approach nahi hai.

Yahan CTE useful hai:

```sql
WITH RankedOrders AS
(
    SELECT
        OrderId,
        CustomerId,
        Amount,
        ROW_NUMBER() OVER (
            PARTITION BY CustomerId
            ORDER BY Amount DESC
        ) AS rn
    FROM Orders
)
SELECT
    OrderId,
    CustomerId,
    Amount
FROM RankedOrders
WHERE rn = 1;
```

Result:

| OrderId | CustomerId | Amount |
| ------: | ---------: | -----: |
|     103 |          1 |   7000 |
|     105 |          2 |   4000 |

Ye **real-world SQL mein bahut common pattern** hai:

```text
CTE
 ↓
Window Function
 ↓
Create ranking
 ↓
Outer query
 ↓
Filter rank
```

---

# 24. CTE ke types

Mainly do concepts yaad rakho:

### 1. Non-recursive CTE

Normal CTE:

```sql
WITH CustomerTotals AS
(
    SELECT ...
)
SELECT *
FROM CustomerTotals;
```

Ye sabse commonly used hai.

### 2. Recursive CTE

Jab data hierarchical/recursive nature ka ho.

Example:

```text
Employee
   ↓
Manager
   ↓
Manager's Manager
   ↓
CEO
```

Ya:

```text
Category
  ↓
Subcategory
  ↓
Sub-subcategory
```

Recursive CTE mein query khud ko recursively reference kar sakti hai.

Basic structure:

```sql
WITH RECURSIVE cte_name AS
(
    -- Anchor query

    UNION ALL

    -- Recursive query
)
SELECT *
FROM cte_name;
```

Isko abhi bas conceptually yaad rakho; recursive CTE thoda advanced topic hai.

---

# 25. CTE vs Temporary Table

Ye bhi interview mein aa sakta hai.

| CTE                                           | Temporary Table                                      |
| --------------------------------------------- | ---------------------------------------------------- |
| Query ke duration ke liye                     | Session/transaction scope depending on DB definition |
| `WITH` se define                              | `CREATE TEMP TABLE`                                  |
| Usually readability/query structuring ke liye | Intermediate data ko explicitly store karne ke liye  |
| Physical table jaisa persistent object nahi   | Actual temporary table object hota hai               |
| Simple complex-query decomposition            | Multiple operations / reuse cases mein useful        |

Simple:

```text
CTE
→ Query ke andar temporary named result

TEMP TABLE
→ Database mein temporary table object
```

---

# 26. CTE vs Stored Procedure

Ye bhi confuse mat karna.

### CTE

```sql
WITH CustomerTotals AS (...)
SELECT ...
```

* Query ke andar use hota hai
* Temporary result set
* Permanently save nahi hota
* Query readability improve karta hai

### Stored Procedure

```sql
CREATE PROCEDURE ...
```

* Database mein stored object
* Permanently defined until altered/dropped
* `CALL` se execute
* Multiple SQL statements/business/database operations ho sakte hain

Simple:

```text
CTE
↓
Query ko organize karna


Stored Procedure
↓
Reusable database operation/logic
```

---

# 27. Window Function vs CTE

Ye dono same category ke nahi hain.

### Window Function

**Calculation technique**

```sql
ROW_NUMBER()
RANK()
SUM() OVER()
LAG()
LEAD()
```

### CTE

**Query structuring technique**

```sql
WITH Something AS (...)
```

Aur dono ko saath bhi use kar sakte ho:

```sql
WITH RankedOrders AS
(
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY CustomerId
            ORDER BY Amount DESC
        ) AS rn
    FROM Orders
)
SELECT *
FROM RankedOrders
WHERE rn = 1;
```

Yahan:

```text
CTE
 ↓
Query ko temporary naam diya

Window Function
 ↓
Rows ko rank kiya
```

---

# 28. Ek complete example

Suppose Orders:

| OrderId | CustomerId | Amount | Status    |
| ------: | ---------: | -----: | --------- |
|     101 |          1 |   5000 | Delivered |
|     102 |          1 |   3000 | Pending   |
|     103 |          1 |   7000 | Delivered |
|     104 |          2 |   2000 | Delivered |
|     105 |          2 |   4000 | Delivered |

Requirement:

> Har customer ka **highest-value Delivered order** nikalna hai.

### Step 1 — Delivered orders

```sql
WHERE Status = 'Delivered'
```

### Step 2 — Customer ke andar ranking

```sql
ROW_NUMBER() OVER (
    PARTITION BY CustomerId
    ORDER BY Amount DESC
)
```

### Step 3 — Rank 1 filter

CTE use:

```sql
WITH RankedOrders AS
(
    SELECT
        OrderId,
        CustomerId,
        Amount,
        ROW_NUMBER() OVER (
            PARTITION BY CustomerId
            ORDER BY Amount DESC
        ) AS rn
    FROM Orders
    WHERE Status = 'Delivered'
)
SELECT
    OrderId,
    CustomerId,
    Amount
FROM RankedOrders
WHERE rn = 1;
```

Result:

| OrderId | CustomerId | Amount |
| ------: | ---------: | -----: |
|     103 |          1 |   7000 |
|     105 |          2 |   4000 |

Pure flow:

```text
Orders
  │
  ▼
WHERE Status = Delivered
  │
  ▼
Delivered Orders
  │
  ▼
PARTITION BY CustomerId
  │
  ▼
ROW_NUMBER()
  │
  ▼
Rank 1
  │
  ▼
Highest Delivered Order
```

---

# 29. Quick revision

## Window Functions

Window function:

> **Related rows par calculation/ranking karti hai without collapsing the original rows.**

Basic:

```sql
FUNCTION() OVER (
    PARTITION BY ...
    ORDER BY ...
)
```

Important functions:

```text
ROW_NUMBER()
RANK()
DENSE_RANK()

SUM()
COUNT()
AVG()
MIN()
MAX()

LAG()
LEAD()
```

### Yaad rakho:

```text
ROW_NUMBER → unique numbering
RANK       → same rank, gap
DENSE_RANK → same rank, no gap

LAG  → previous
LEAD → next

PARTITION BY → groups/window define karta hai
ORDER BY     → window ke andar order define karta hai
```

---

# 30. CTE Quick Revision

**CTE = Common Table Expression**

Syntax:

```sql
WITH CTEName AS
(
    SELECT ...
)
SELECT *
FROM CTEName;
```

Use when:

* complex query ko readable banana ho
* query ko logical steps mein divide karna ho
* intermediate result ko next query mein use karna ho
* window-function result ko filter karna ho
* recursive/hierarchical data handle karna ho → recursive CTE

### One-line difference:

```text
GROUP BY
→ rows combine

WINDOW FUNCTION
→ rows remain + calculation/ranking

CTE
→ query ko temporary named result ke through structure karta hai
```

**Sabse important practical pattern jo yaad rakhna hai:**

```sql
WITH RankedOrders AS
(
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY CustomerId
            ORDER BY Amount DESC
        ) AS rn
    FROM Orders
)
SELECT *
FROM RankedOrders
WHERE rn = 1;
```

Is ek query mein **CTE + Window Function + PARTITION BY + ORDER BY + WHERE** sab combine ho rahe hain.

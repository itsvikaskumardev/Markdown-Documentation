Haan. SQL mein `IN`, `EXISTS` ke alawa bhi kuch **bahut important operators/keywords** hain jo interviews aur real projects dono mein frequently use hote hain.

Main tumhe **importance order** mein samjha raha hoon, aur examples ke liye `Orders`, `Customers`, `OrderItems`, `Products` use karunga.

---

# 1. `IN` ⭐⭐⭐⭐⭐

`IN` ka use tab hota hai jab hume check karna ho ki koi value **given values ki list mein hai ya nahi**.

### Example

```sql
SELECT *
FROM Orders
WHERE Status IN ('Pending', 'Delivered');
```

Iska meaning:

```text
Status = Pending
       OR
Status = Delivered
```

Instead of:

```sql
SELECT *
FROM Orders
WHERE Status = 'Pending'
   OR Status = 'Delivered';
```

### `IN` with numbers

```sql
SELECT *
FROM Orders
WHERE CustomerId IN (1, 3, 5);
```

Meaning:

```text
CustomerId = 1
OR CustomerId = 3
OR CustomerId = 5
```

### `NOT IN`

```sql
SELECT *
FROM Orders
WHERE Status NOT IN ('Cancelled', 'Delivered');
```

Meaning:

```text
Cancelled ❌
Delivered ❌
baaki statuses ✅
```

### Important: NULL ke saath `NOT IN`

`NOT IN` + `NULL` confusing ho sakta hai because SQL uses three-valued logic (`TRUE`, `FALSE`, `UNKNOWN`).

Example:

```sql
WHERE CustomerId NOT IN (1, 2, NULL)
```

Ye expected result nahi de sakta.

Agar NULL possible ho, carefully handle karo.

---

# 2. `EXISTS` ⭐⭐⭐⭐⭐

`EXISTS` check karta hai:

> **Kya subquery se kam se kam ek row milti hai?**

Example:

> Aise customers ke orders nikalo jinke kam se kam ek order hai.

```sql
SELECT *
FROM Customers c
WHERE EXISTS
(
    SELECT 1
    FROM Orders o
    WHERE o.CustomerId = c.CustomerId
);
```

### Andar kya ho raha hai?

Suppose:

```text
Customers

CustomerId | Name
-----------|------
1          | Rahul
2          | Amit
3          | Priya
```

Orders:

```text
OrderId | CustomerId
--------|-----------
101     | 1
102     | 1
103     | 3
```

Rahul:

```text
EXISTS order for CustomerId = 1
        ↓
       YES
```

Amit:

```text
EXISTS order for CustomerId = 2
        ↓
       NO
```

Priya:

```text
EXISTS order for CustomerId = 3
        ↓
       YES
```

Result:

```text
Rahul
Priya
```

### `SELECT 1` kyun?

```sql
SELECT 1
FROM Orders ...
```

`EXISTS` ko actual columns ki value nahi chahiye.

Usko bas ye jaana hai:

```text
Row exists?
YES / NO
```

Isliye commonly:

```sql
EXISTS (SELECT 1 ...)
```

likhte hain.

---

# 3. `NOT EXISTS` ⭐⭐⭐⭐⭐

Iska meaning:

> Related row exist **nahi** karti.

Example:

> Aise customers jinhone kabhi order nahi kiya.

```sql
SELECT *
FROM Customers c
WHERE NOT EXISTS
(
    SELECT 1
    FROM Orders o
    WHERE o.CustomerId = c.CustomerId
);
```

Flow:

```text
Customer
   ↓
Orders mein check
   ↓
Order mila?
 ┌───────┴───────┐
 YES             NO
  ↓               ↓
exclude          include
```

Ye real projects mein bahut useful hai.

---

# 4. `BETWEEN` ⭐⭐⭐⭐⭐

Range check karne ke liye.

```sql
SELECT *
FROM OrderItems
WHERE Price BETWEEN 1000 AND 5000;
```

Meaning:

```text
Price >= 1000
AND
Price <= 5000
```

Important:

> `BETWEEN` generally **inclusive** hota hai.

So:

```text
1000 ✅
2500 ✅
5000 ✅
```

---

# 5. `LIKE` ⭐⭐⭐⭐⭐

Pattern matching ke liye.

Suppose:

```text
Products

Product
--------
Laptop
Laptop Bag
Mouse
Keyboard
Monitor
```

### Starts with L

```sql
SELECT *
FROM Products
WHERE ProductName LIKE 'L%';
```

Result:

```text
Laptop
Laptop Bag
```

`%` = zero ya more characters.

---

### Ends with `top`

```sql
SELECT *
FROM Products
WHERE ProductName LIKE '%top';
```

Result:

```text
Laptop
Desktop
```

---

### Contains `lap`

```sql
SELECT *
FROM Products
WHERE ProductName LIKE '%lap%';
```

---

### `_`

`_` means exactly **one character**.

```sql
WHERE ProductName LIKE 'L____p';
```

Pattern ka meaning:

```text
L
+
4 characters
+
p
```

---

# 6. `IS NULL` / `IS NOT NULL` ⭐⭐⭐⭐⭐

NULL ko compare karne ke liye `=` use **nahi** karna.

❌ Wrong:

```sql
WHERE Email = NULL;
```

Correct:

```sql
WHERE Email IS NULL;
```

Example:

```sql
SELECT *
FROM Customers
WHERE Phone IS NULL;
```

Phone missing customers.

### NOT NULL

```sql
SELECT *
FROM Customers
WHERE Phone IS NOT NULL;
```

Phone available customers.

### Why `= NULL` doesn't work?

SQL mein `NULL` ka meaning:

```text
unknown / missing value
```

So:

```sql
NULL = NULL
```

normal `TRUE` nahi deta.

Isliye:

```sql
IS NULL
IS NOT NULL
```

use karte hain.

---

# 7. `CASE` ⭐⭐⭐⭐⭐

SQL ka **if/else** samajh lo.

Example:

```sql
SELECT
    OrderId,
    Amount,
    CASE
        WHEN Amount >= 10000 THEN 'High'
        WHEN Amount >= 5000 THEN 'Medium'
        ELSE 'Low'
    END AS OrderCategory
FROM Orders;
```

Suppose:

```text
Amount
------
15000 → High
7000  → Medium
2000  → Low
```

### Multiple conditions

```sql
SELECT
    OrderId,
    Status,
    CASE
        WHEN Status = 'Delivered' THEN 'Completed'
        WHEN Status = 'Cancelled' THEN 'Failed'
        ELSE 'In Progress'
    END AS OrderStatus
FROM Orders;
```

Real projects mein `CASE` bahut common hai.

---

# 8. `COALESCE` ⭐⭐⭐⭐⭐

`COALESCE` ka meaning:

> **First non-NULL value return karo.**

Example:

```sql
SELECT
    CustomerId,
    COALESCE(Phone, 'No Phone') AS Phone
FROM Customers;
```

Suppose:

```text
Phone
-----------
9876543210
NULL
9123456789
```

Result:

```text
9876543210
No Phone
9123456789
```

### Multiple values

```sql
SELECT COALESCE(NULL, NULL, 'Hello', 'World');
```

Result:

```text
Hello
```

Because first non-NULL value `Hello` hai.

---

# 9. `DISTINCT` ⭐⭐⭐⭐

Duplicate values remove karne ke liye.

Suppose Orders:

```text
CustomerId
----------
1
1
2
3
3
```

Query:

```sql
SELECT CustomerId
FROM Orders;
```

Result:

```text
1
1
2
3
3
```

With DISTINCT:

```sql
SELECT DISTINCT CustomerId
FROM Orders;
```

Result:

```text
1
2
3
```

### Multiple columns

```sql
SELECT DISTINCT CustomerId, Status
FROM Orders;
```

Yahan uniqueness **combination** par check hogi:

```text
CustomerId + Status
```

---

# 10. `UNION` ⭐⭐⭐⭐

Do queries ke results combine karne ke liye.

```sql
SELECT CustomerId
FROM Orders
WHERE Status = 'Delivered'

UNION

SELECT CustomerId
FROM Orders
WHERE Status = 'Pending';
```

`UNION` duplicate rows remove karta hai.

---

# 11. `UNION ALL` ⭐⭐⭐⭐

Same concept, but duplicates remove **nahi** karta.

```sql
SELECT CustomerId
FROM Orders
WHERE Status = 'Delivered'

UNION ALL

SELECT CustomerId
FROM Orders
WHERE Status = 'Pending';
```

### Difference

```text
UNION
→ duplicates remove

UNION ALL
→ duplicates keep
```

Generally agar duplicate removal ki need nahi hai, `UNION ALL` often preferable hota hai because unnecessary deduplication avoid hoti hai.

---

# 12. `ANY` / `SOME` ⭐⭐⭐

Ye thoda advanced hai but interview mein useful hai.

Example:

> Aise orders jinka amount kisi customer ke orders ke amount se bada hai.

Conceptually:

```sql
SELECT *
FROM Orders
WHERE Amount > ANY
(
    SELECT Amount
    FROM Orders
    WHERE CustomerId = 1
);
```

`ANY` ka meaning roughly:

```text
condition at least one value ke saath TRUE ho
```

Agar subquery returns:

```text
1000
5000
10000
```

Then:

```text
Amount > ANY(...)
```

ka matlab:

```text
Amount > 1000
OR
Amount > 5000
OR
Amount > 10000
```

---

# 13. `ALL` ⭐⭐⭐

`ALL` ka meaning:

> Condition **har value** ke saath true honi chahiye.

Example:

```sql
SELECT *
FROM Orders
WHERE Amount > ALL
(
    SELECT Amount
    FROM Orders
    WHERE CustomerId = 1
);
```

Suppose customer 1 ke amounts:

```text
1000
5000
10000
```

Then:

```text
Amount > ALL(...)
```

means:

```text
Amount > 1000
AND
Amount > 5000
AND
Amount > 10000
```

So practically:

```text
Amount > 10000
```

---

# 14. `ANY` vs `ALL`

Easy trick:

```text
ANY
 ↓
At least ONE
```

```text
ALL
 ↓
EVERY value
```

Example values:

```text
10
20
30
```

### `> ANY`

```text
> 10
OR
> 20
OR
> 30
```

### `> ALL`

```text
> 10
AND
> 20
AND
> 30
```

---

# 15. `FETCH FIRST` / `LIMIT` ⭐⭐⭐⭐

Top records lene ke liye.

PostgreSQL:

```sql
SELECT *
FROM Orders
LIMIT 5;
```

First 5 rows.

Better with ordering:

```sql
SELECT *
FROM Orders
ORDER BY Amount DESC
LIMIT 5;
```

Meaning:

> Highest amount wale top 5 orders.

Standard SQL style:

```sql
SELECT *
FROM Orders
ORDER BY Amount DESC
FETCH FIRST 5 ROWS ONLY;
```

---

# 16. `OFFSET` ⭐⭐⭐⭐

Pagination mein use hota hai.

```sql
SELECT *
FROM Orders
ORDER BY OrderId
LIMIT 10
OFFSET 20;
```

Meaning:

```text
First 20 rows skip
Next 10 rows return
```

Pagination:

```text
Page 1
LIMIT 10 OFFSET 0

Page 2
LIMIT 10 OFFSET 10

Page 3
LIMIT 10 OFFSET 20
```

---

# 17. `WITH` — CTE ⭐⭐⭐⭐⭐

Isko tum already padh chuke ho, but keywords ke perspective se important hai.

`WITH` temporary named result banata hai.

Example:

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

Complex query ko readable banane ke liye bahut useful.

---

# 18. `RETURNING` — PostgreSQL ⭐⭐⭐⭐⭐

Tum PostgreSQL use karte ho, isliye ye **particularly important** hai.

Insert/update/delete ke baad affected row ka data return kar sakte ho.

### INSERT

```sql
INSERT INTO Orders
(
    CustomerId,
    Status,
    Amount
)
VALUES
(
    10,
    'Pending',
    5000
)
RETURNING OrderId;
```

Agar DB generated:

```text
OrderId = 105
```

to query directly `105` return kar sakti hai.

---

### UPDATE

```sql
UPDATE Orders
SET Status = 'Delivered'
WHERE OrderId = 105
RETURNING OrderId, Status;
```

Result:

```text
OrderId | Status
--------|----------
105     | Delivered
```

---

### DELETE

```sql
DELETE FROM Orders
WHERE OrderId = 105
RETURNING *;
```

Delete hone wali row return ho jayegi.

Backend development mein ye kaafi useful hai.

---

# 19. `ON CONFLICT` — PostgreSQL ⭐⭐⭐⭐⭐

Duplicate insert handle karne ke liye.

Suppose:

```text
Email UNIQUE hai
```

Normal:

```sql
INSERT INTO Customers (Email, Name)
VALUES ('abc@gmail.com', 'Rahul');
```

Agar email already exists:

```text
ERROR ❌
```

PostgreSQL:

```sql
INSERT INTO Customers (Email, Name)
VALUES ('abc@gmail.com', 'Rahul')
ON CONFLICT (Email)
DO NOTHING;
```

Meaning:

```text
Duplicate hai?
   ↓
Yes → kuch mat karo
```

### Update on conflict

```sql
INSERT INTO Customers (Email, Name)
VALUES ('abc@gmail.com', 'Rahul')
ON CONFLICT (Email)
DO UPDATE
SET Name = EXCLUDED.Name;
```

Ye **UPSERT** pattern hai:

```text
Insert if doesn't exist
       +
Update if already exists
```

---

# 20. `HAVING` — already important ⭐⭐⭐⭐⭐

Tumne ye padha hai, but keywords list mein definitely important hai.

`WHERE`:

```sql
SELECT *
FROM Orders
WHERE Status = 'Delivered';
```

Individual rows filter.

`HAVING`:

```sql
SELECT CustomerId, COUNT(*) AS TotalOrders
FROM Orders
GROUP BY CustomerId
HAVING COUNT(*) >= 2;
```

Groups filter.

Easy:

```text
WHERE
 ↓
Rows filter

GROUP BY
 ↓
Groups create

HAVING
 ↓
Groups filter
```

---

# 21. `ORDER BY` ⭐⭐⭐⭐⭐

Sorting:

```sql
SELECT *
FROM Orders
ORDER BY Amount ASC;
```

Ascending.

```sql
SELECT *
FROM Orders
ORDER BY Amount DESC;
```

Descending.

Multiple columns:

```sql
SELECT *
FROM Orders
ORDER BY CustomerId ASC, Amount DESC;
```

Meaning:

```text
First CustomerId
        ↓
same CustomerId ke andar
        ↓
Amount DESC
```

---

# 22. `GROUP BY` ⭐⭐⭐⭐⭐

Aggregation ke liye.

```sql
SELECT
    CustomerId,
    COUNT(*) AS TotalOrders
FROM Orders
GROUP BY CustomerId;
```

Example:

```text
CustomerId | TotalOrders
-----------|------------
1          | 5
2          | 2
3          | 7
```

---

# 23. `JOIN` ⭐⭐⭐⭐⭐

Related tables combine karne ke liye.

```sql
SELECT
    o.OrderId,
    o.CustomerId,
    oi.Product,
    oi.Quantity
FROM Orders o
JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

Important JOIN types:

```text
INNER JOIN
LEFT JOIN
RIGHT JOIN
FULL OUTER JOIN
CROSS JOIN
SELF JOIN
```

Tum JOIN detail mein already padh chuke ho.

---

# 24. `ANY` vs `EXISTS` vs `IN`

Ye interview mein interesting comparison hai.

Suppose:

> Customer 1 ke orders mein se kisi matching amount ko find karna hai.

### IN

```sql
SELECT *
FROM Orders
WHERE CustomerId IN
(
    SELECT CustomerId
    FROM Orders
    WHERE Status = 'Delivered'
);
```

### EXISTS

```sql
SELECT *
FROM Orders o
WHERE EXISTS
(
    SELECT 1
    FROM Orders d
    WHERE d.CustomerId = o.CustomerId
      AND d.Status = 'Delivered'
);
```

Conceptually:

```text
IN
→ value list mein hai?

EXISTS
→ matching row exist karti hai?

ANY
→ condition at least one value par true?

ALL
→ condition every value par true?
```

Performance ke liye blindly ye mat bolo ki `EXISTS` hamesha `IN` se faster hai. Actual performance DB optimizer, indexes, query shape, data distribution etc. par depend karti hai.

---

# 25. Important keywords — priority order

Tum SQL interview preparation kar rahe ho, to main is order mein padhne ko suggest karunga:

### 🔥 Must Know

```text
1. SELECT
2. WHERE
3. AND / OR / NOT
4. IN / NOT IN
5. BETWEEN
6. LIKE
7. IS NULL / IS NOT NULL
8. ORDER BY
9. GROUP BY
10. HAVING
11. JOIN
12. EXISTS / NOT EXISTS
13. DISTINCT
14. CASE
15. COALESCE
16. UNION / UNION ALL
17. LIMIT / OFFSET
18. WITH (CTE)
19. Subquery
```

### 🔥 Backend Developer ke liye especially important

```text
20. INSERT
21. UPDATE
22. DELETE
23. RETURNING          PostgreSQL
24. ON CONFLICT        PostgreSQL
25. Transactions
26. COMMIT / ROLLBACK
27. Window Functions
28. CTE
29. Stored Procedures
30. Functions
31. Triggers
32. Indexes
```

### Advanced

```text
33. ANY
34. ALL
35. LATERAL
36. Recursive CTE
37. MERGE
38. UPSERT
39. EXISTS + correlated subquery
40. Query execution / EXPLAIN
```

---

# 26. Ek query mein multiple keywords kaise combine hote hain?

Real project query usually sirf ek keyword ki nahi hoti.

Example:

> Delivered orders ke andar un customers ko find karo jinke 2 se zyada delivered orders hain, total amount ₹10,000 se zyada hai, aur result highest total amount ke order mein dikhao.

```sql
SELECT
    CustomerId,
    COUNT(*) AS TotalOrders,
    SUM(Amount) AS TotalAmount
FROM Orders
WHERE Status = 'Delivered'
GROUP BY CustomerId
HAVING COUNT(*) > 2
   AND SUM(Amount) > 10000
ORDER BY TotalAmount DESC;
```

Yahan:

```text
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
```

Conceptually SQL query ka processing flow yaad rakho.

---

## Final Cheat Sheet

| Keyword       | Simple Meaning                         |
| ------------- | -------------------------------------- |
| `WHERE`       | Rows filter                            |
| `IN`          | Value list mein hai?                   |
| `NOT IN`      | Value list mein nahi hai?              |
| `EXISTS`      | Matching row exist karti hai?          |
| `NOT EXISTS`  | Matching row exist nahi karti?         |
| `BETWEEN`     | Range check                            |
| `LIKE`        | Pattern search                         |
| `IS NULL`     | NULL check                             |
| `CASE`        | SQL if/else                            |
| `COALESCE`    | First non-NULL                         |
| `DISTINCT`    | Duplicate remove                       |
| `GROUP BY`    | Groups banana                          |
| `HAVING`      | Groups filter                          |
| `ORDER BY`    | Sorting                                |
| `JOIN`        | Tables combine                         |
| `UNION`       | Results combine + duplicates remove    |
| `UNION ALL`   | Results combine + duplicates keep      |
| `LIMIT`       | Kitni rows chahiye                     |
| `OFFSET`      | Kitni rows skip                        |
| `WITH`        | CTE                                    |
| `ANY`         | At least one value                     |
| `ALL`         | Every value                            |
| `RETURNING`   | Changed row return — PostgreSQL        |
| `ON CONFLICT` | Duplicate conflict handle — PostgreSQL |

**Tumhare level par next SQL topics ka natural order:** `Subquery → IN vs EXISTS → CASE/COALESCE → Indexes → EXPLAIN/EXPLAIN ANALYZE → Views → Functions vs Procedures → Transactions/Isolation → Locks → Query Optimization`.

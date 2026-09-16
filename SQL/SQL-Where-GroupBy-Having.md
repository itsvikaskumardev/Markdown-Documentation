

---

# 1. Pehle hamara data samjho

Maan lo hamare paas e-commerce database hai.


### `Orders`

| OrderId | CustomerId | Status    | OrderDate  |
| ------: | ---------: | --------- | ---------- |
|     101 |          1 | Delivered | 2026-09-01 |
|     102 |          2 | Pending   | 2026-09-02 |
|     103 |          1 | Delivered | 2026-09-03 |
|     104 |          3 | Cancelled | 2026-09-04 |
|     105 |          2 | Delivered | 2026-09-05 |

### `OrderItems`

| OrderItemId | OrderId | Product  | Quantity | Price |
| ----------: | ------: | -------- | -------: | ----: |
|           1 |     101 | Laptop   |        1 | 50000 |
|           2 |     101 | Mouse    |        2 |  1000 |
|           3 |     102 | Keyboard |        1 |  2000 |
|           4 |     102 | Mouse    |        1 |  1000 |
|           5 |     103 | Laptop   |        1 | 50000 |
|           6 |     103 | Mouse    |        1 |  1000 |
|           7 |     104 | Monitor  |        1 | 15000 |
|           8 |     105 | Keyboard |        2 |  2000 |
|           9 |     105 | Mouse    |        1 |  1000 |

---


# 3. WHERE kya karta hai?

`WHERE` ka use **rows ko filter karne ke liye** hota hai.

Simple definition:

> **WHERE decide karta hai ki kaunsi rows result mein aayengi.**

Example:

```sql
SELECT *
FROM Orders
WHERE Status = 'Delivered';
```

Result:

| OrderId | CustomerId | Status    |
| ------: | ---------: | --------- |
|     101 |          1 | Delivered |
|     103 |          1 | Delivered |
|     105 |          2 | Delivered |


---

# 4. WHERE mein conditions

Tum multiple operators use kar sakte ho.

### Equal

```sql
WHERE Status = 'Delivered'
```

### Not equal

```sql
WHERE Status <> 'Cancelled'
```


---

# 5. AND

Agar **dono conditions true** honi chahiye:

```sql
SELECT *
FROM Orders
WHERE Status = 'Delivered'
AND CustomerId = 1;
```

Result:

| OrderId | CustomerId | Status    |
| ------: | ---------: | --------- |
|     101 |          1 | Delivered |
|     103 |          1 | Delivered |

Meaning:

```text
Status = Delivered
       AND
CustomerId = 1
```

Dono condition satisfy honi chahiye.

---

# 6. OR

Agar **koi ek condition true** ho:

```sql
SELECT *
FROM Orders
WHERE Status = 'Pending'
OR Status = 'Cancelled';
```

Result:

| OrderId | Status    |
| ------: | --------- |
|     102 | Pending   |
|     104 | Cancelled |

---

# 7. IN

Agar multiple possible values hain:

```sql
SELECT *
FROM Orders
WHERE Status IN ('Pending', 'Cancelled');
```

Ye basically:

```sql
WHERE Status = 'Pending'
   OR Status = 'Cancelled'
```

jaisa hai.

---

# 8. BETWEEN

Range ke liye:

```sql
SELECT *
FROM OrderItems
WHERE Price BETWEEN 1000 AND 10000;
```

Meaning:

> Price 1000 se 10000 ke beech honi chahiye.

---

# 9. LIKE

Text pattern search ke liye:

```sql
SELECT *
FROM OrderItems
WHERE Product LIKE 'M%';
```

`M%` ka meaning:

> Product ka naam `M` se start ho.

Result:

```text
Mouse
Mouse
Monitor
Mouse
```

---

# 10. Ab GROUP BY samjho

Yahan se important concept start hota hai.

Suppose mujhe pata karna hai:

> **Har customer ne kitne orders place kiye?**

Hamare data mein:

| CustomerId |   Orders |
| ---------: | -------: |
|          1 | 101, 103 |
|          2 | 102, 105 |
|          3 |      104 |

Hume customer ke according grouping karni hai.

```sql
SELECT CustomerId, COUNT(*) AS TotalOrders
FROM Orders
GROUP BY CustomerId;
```

Result:

| CustomerId | TotalOrders |
| ---------: | ----------: |
|          1 |           2 |
|          2 |           2 |
|          3 |           1 |

---

# GROUP BY ka actual meaning

`GROUP BY` ka matlab:

> **Same value wali rows ko ek group mein combine karo.**

Before:

```text
Customer 1 → Order 101
Customer 2 → Order 102
Customer 1 → Order 103
Customer 3 → Order 104
Customer 2 → Order 105
```

`GROUP BY CustomerId` ke baad conceptually:

```text
Customer 1
 ├── Order 101
 └── Order 103

Customer 2
 ├── Order 102
 └── Order 105

Customer 3
 └── Order 104
```

Ab `COUNT()` laga sakte hain.

---

# 11. GROUP BY + SUM

Suppose hume har order ka total amount nikalna hai.

Formula:

```text
Quantity × Price
```

Query:

```sql
SELECT
    OrderId,
    SUM(Quantity * Price) AS TotalAmount
FROM OrderItems
GROUP BY OrderId;
```

Result:

| OrderId | TotalAmount |
| ------: | ----------: |
|     101 |       52000 |
|     102 |        3000 |
|     103 |       51000 |
|     104 |       15000 |
|     105 |        5000 |

For Order `101`:

```text
Laptop
1 × 50000 = 50000

Mouse
2 × 1000 = 2000

Total = 52000
```

---

# 12. GROUP BY + COUNT

Har product kitni baar order hua?

```sql
SELECT
    Product,
    COUNT(*) AS NumberOfOrderItems
FROM OrderItems
GROUP BY Product;
```

Result:

| Product  | NumberOfOrderItems |
| -------- | -----------------: |
| Keyboard |                  2 |
| Laptop   |                  2 |
| Monitor  |                  1 |
| Mouse    |                  3 |

---

# 13. GROUP BY + SUM

Har product ki total quantity kitni sold hui?

```sql
SELECT
    Product,
    SUM(Quantity) AS TotalQuantity
FROM OrderItems
GROUP BY Product;
```

Result:

| Product  | TotalQuantity |
| -------- | ------------: |
| Keyboard |             3 |
| Laptop   |             2 |
| Monitor  |             1 |
| Mouse    |             4 |

Mouse:

```text
Order 101 → 2
Order 102 → 1
Order 103 → 1

Total = 4
```

---

# 14. Important Aggregate Functions

`GROUP BY` ke saath commonly ye functions use hote hain:

```text
COUNT()
SUM()
AVG()
MIN()
MAX()
```

### COUNT

Kitne records?

```sql
COUNT(*)
```

### SUM

Total:

```sql
SUM(Quantity)
```

### AVG

Average:

```sql
AVG(Price)
```

### MIN

Minimum:

```sql
MIN(Price)
```

### MAX

Maximum:

```sql
MAX(Price)
```

---

# 15. Ab HAVING samjho

Ye sabse important difference hai:

> **WHERE rows ko filter karta hai.**

> **HAVING groups ko filter karta hai.**

Example:

Mujhe sirf woh customers chahiye jinhone **2 ya usse zyada orders** kiye hain.

```sql
SELECT
    CustomerId,
    COUNT(*) AS TotalOrders
FROM Orders
GROUP BY CustomerId
HAVING COUNT(*) >= 2;
```

Result:

| CustomerId | TotalOrders |
| ---------: | ----------: |
|          1 |           2 |
|          2 |           2 |

Customer 3 nahi aaya because:

```text
Customer 3 → 1 order
```

Aur condition:

```text
COUNT(*) >= 2
```

hai.

---

# 16. WHERE vs HAVING — Sabse important

Suppose question hai:

> Delivered orders mein se un customers ko find karo jinhone 1 se zyada delivered orders kiye hain.

Pehle **rows filter** hongi:

```sql
WHERE Status = 'Delivered'
```

Phir grouping:

```sql
GROUP BY CustomerId
```

Phir groups filter:

```sql
HAVING COUNT(*) > 1
```

Complete query:

```sql
SELECT
    CustomerId,
    COUNT(*) AS TotalOrders
FROM Orders
WHERE Status = 'Delivered'
GROUP BY CustomerId
HAVING COUNT(*) > 1;
```

---

# 17. Is query ko step-by-step execute karo

Original Orders:

| OrderId | CustomerId | Status    |
| ------: | ---------: | --------- |
|     101 |          1 | Delivered |
|     102 |          2 | Pending   |
|     103 |          1 | Delivered |
|     104 |          3 | Cancelled |
|     105 |          2 | Delivered |

### Step 1 — FROM

```sql
FROM Orders
```

Saara Orders data liya.

---

### Step 2 — WHERE

```sql
WHERE Status = 'Delivered'
```

Bach gaya:

| OrderId | CustomerId | Status    |
| ------: | ---------: | --------- |
|     101 |          1 | Delivered |
|     103 |          1 | Delivered |
|     105 |          2 | Delivered |

---

### Step 3 — GROUP BY

```sql
GROUP BY CustomerId
```

Groups:

```text
Customer 1
 ├── 101
 └── 103

Customer 2
 └── 105
```

---

### Step 4 — COUNT

```sql
COUNT(*)
```

Result:

| CustomerId | TotalOrders |
| ---------: | ----------: |
|          1 |           2 |
|          2 |           1 |

---

### Step 5 — HAVING

```sql
HAVING COUNT(*) > 1
```

Customer 2 remove.

Final:

| CustomerId | TotalOrders |
| ---------: | ----------: |
|          1 |           2 |

---

# 18. WHERE mein aggregate function kyun nahi?

Ye generally wrong approach hai:

```sql
SELECT CustomerId, COUNT(*)
FROM Orders
WHERE COUNT(*) > 1
GROUP BY CustomerId;
```

❌ `WHERE COUNT(*) > 1` nahi karte.

Kyun?

Because `WHERE` **individual rows ko filter karta hai**, jabki `COUNT(*)` **group banne ke baad calculate hota hai**.

Isliye:

```text
WHERE
 ↓
Rows filter

GROUP BY
 ↓
Groups create

COUNT/SUM/AVG
 ↓
Group calculation

HAVING
 ↓
Groups filter
```

---

# 19. WHERE + GROUP BY + HAVING together

Ye pattern interview mein bahut important hai:

```sql
SELECT
    CustomerId,
    COUNT(*) AS TotalOrders,
    SUM(...) AS TotalAmount
FROM Orders
WHERE ...
GROUP BY CustomerId
HAVING ...
ORDER BY ...;
```

Example:

> Delivered orders mein se un customers ko find karo jinhone kam se kam 2 orders kiye aur total order count ko descending mein dikhao.

```sql
SELECT
    CustomerId,
    COUNT(*) AS TotalOrders
FROM Orders
WHERE Status = 'Delivered'
GROUP BY CustomerId
HAVING COUNT(*) >= 2
ORDER BY TotalOrders DESC;
```

---

# 20. ORDER BY kya karta hai?

`ORDER BY` result ko sort karta hai.

### Ascending

```sql
ORDER BY TotalOrders ASC;
```

Small → Large

### Descending

```sql
ORDER BY TotalOrders DESC;
```

Large → Small

Example:

```sql
SELECT
    Product,
    SUM(Quantity) AS TotalQuantity
FROM OrderItems
GROUP BY Product
ORDER BY TotalQuantity DESC;
```

Result:

| Product  | TotalQuantity |
| -------- | ------------: |
| Mouse    |             4 |
| Keyboard |             3 |
| Laptop   |             2 |
| Monitor  |             1 |

---

# 21. SQL Query ka logical order

Query hum usually aise **likhte** hain:

```sql
SELECT ...
FROM ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...;
```

Lekin SQL ko conceptually samajhne ke liye logical processing roughly:

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

Ye distinction important hai.

### Simple meaning:

```text
FROM
↓
Data kahan se lena hai?

WHERE
↓
Kaunsi rows chahiye?

GROUP BY
↓
Kin rows ko group karna hai?

HAVING
↓
Kaunse groups chahiye?

SELECT
↓
Final mein kya dikhana hai?

ORDER BY
↓
Result kis order mein dikhana hai?
```

---

# 22. Ek real-world question solve karte hain

### Question:

> Find products whose total sold quantity is greater than 2.

Data:

| Product  | Quantity |
| -------- | -------: |
| Laptop   |        1 |
| Mouse    |        2 |
| Keyboard |        1 |
| Mouse    |        1 |
| Laptop   |        1 |
| Mouse    |        1 |
| Monitor  |        1 |
| Keyboard |        2 |
| Mouse    |        1 |

Query:

```sql
SELECT
    Product,
    SUM(Quantity) AS TotalQuantity
FROM OrderItems
GROUP BY Product
HAVING SUM(Quantity) > 2;
```

Result:

| Product  | TotalQuantity |
| -------- | ------------: |
| Keyboard |             3 |
| Mouse    |             5 |

Yahan **HAVING** use hua because:

```text
SUM(Quantity) > 2
```

ek **group-level condition** hai.

---

# 23. WHERE kab use karna hai?

Jab condition **individual row** par hai.

Example:

> Price 5000 se zyada wale order items.

```sql
SELECT *
FROM OrderItems
WHERE Price > 5000;
```

Because hum individual records filter kar rahe hain.

---

# 24. HAVING kab use karna hai?

Jab condition **group ke result** par hai.

Example:

> Aise products jinki total quantity 2 se zyada hai.

```sql
SELECT
    Product,
    SUM(Quantity) AS TotalQuantity
FROM OrderItems
GROUP BY Product
HAVING SUM(Quantity) > 2;
```

Because `SUM(Quantity)` group-level calculation hai.

---

# 25. WHERE + HAVING ka difference ek line mein

### WHERE

```text
Individual Rows
      ↓
    WHERE
      ↓
Filtered Rows
```

### HAVING

```text
Rows
 ↓
GROUP BY
 ↓
Groups
 ↓
 HAVING
 ↓
Filtered Groups
```

---

# 26. Ek common mistake

Suppose tumhe chahiye:

> Product ka price 5000 se zyada hai aur total quantity 2 se zyada hai.

Tum aise likhoge:

```sql
SELECT
    Product,
    SUM(Quantity) AS TotalQuantity
FROM OrderItems
WHERE Price > 5000
GROUP BY Product
HAVING SUM(Quantity) > 2;
```

Yahan:

```text
WHERE Price > 5000
```

individual rows filter karega.

Then:

```text
GROUP BY Product
```

group banayega.

Then:

```text
HAVING SUM(Quantity) > 2
```

groups filter karega.

So **WHERE aur HAVING dono ek query mein ho sakte hain**.

---

# 27. WHERE, GROUP BY, HAVING ko yaad rakhne ka easiest example

Imagine restaurant mein orders hain.

Question:

> **₹500 se zyada ke orders mein, customer-wise total orders count karo, aur sirf un customers ko dikhao jinke 3 se zyada orders hain.**

Concept:

```text
Orders
  ↓
WHERE Amount > 500
  ↓
Sirf > ₹500 orders
  ↓
GROUP BY CustomerId
  ↓
Customer-wise groups
  ↓
COUNT(*)
  ↓
Total orders per customer
  ↓
HAVING COUNT(*) > 3
  ↓
Sirf 3+ orders wale customers
```

Exactly isi logic ko SQL mein likhte hain:

```sql
SELECT
    CustomerId,
    COUNT(*) AS TotalOrders
FROM Orders
WHERE Amount > 500
GROUP BY CustomerId
HAVING COUNT(*) > 3;
```

---

# 28. Ek aur important cheez — JOIN

Real applications mein data ek table mein nahi hota.

Hamare paas:

```text
Orders
   |
   | OrderId
   ↓
OrderItems
```

Agar hume customer + order + product information ek saath chahiye, `JOIN` use karenge.

Example:

```sql
SELECT
    o.OrderId,
    o.CustomerId,
    oi.Product,
    oi.Quantity,
    oi.Price
FROM Orders o
JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

Result conceptually:

| OrderId | CustomerId | Product  | Quantity | Price |
| ------: | ---------: | -------- | -------: | ----: |
|     101 |          1 | Laptop   |        1 | 50000 |
|     101 |          1 | Mouse    |        2 |  1000 |
|     102 |          2 | Keyboard |        1 |  2000 |
|     102 |          2 | Mouse    |        1 |  1000 |
|     103 |          1 | Laptop   |        1 | 50000 |

---

# 29. JOIN + WHERE

Question:

> Sirf Delivered orders ke items chahiye.

```sql
SELECT
    o.OrderId,
    o.CustomerId,
    oi.Product,
    oi.Quantity
FROM Orders o
JOIN OrderItems oi
    ON o.OrderId = oi.OrderId
WHERE o.Status = 'Delivered';
```

Yahan `WHERE` **Orders ki rows** filter kar raha hai.

---

# 30. JOIN + GROUP BY

Question:

> Har customer ne kitne products purchase kiye?

```sql
SELECT
    o.CustomerId,
    SUM(oi.Quantity) AS TotalProducts
FROM Orders o
JOIN OrderItems oi
    ON o.OrderId = oi.OrderId
GROUP BY o.CustomerId;
```

Result:

| CustomerId | TotalProducts |
| ---------: | ------------: |
|          1 |             5 |
|          2 |             4 |
|          3 |             1 |

---

# 31. JOIN + WHERE + GROUP BY + HAVING

Ab ek proper interview-level query:

> **Delivered orders mein har customer ki total purchased quantity nikalo, aur sirf un customers ko dikhao jinhone 3 se zyada items purchase kiye hain.**

```sql
SELECT
    o.CustomerId,
    SUM(oi.Quantity) AS TotalQuantity
FROM Orders o
JOIN OrderItems oi
    ON o.OrderId = oi.OrderId
WHERE o.Status = 'Delivered'
GROUP BY o.CustomerId
HAVING SUM(oi.Quantity) > 3
ORDER BY TotalQuantity DESC;
```

Isko line-by-line read karo:

```sql
SELECT o.CustomerId, SUM(oi.Quantity)
```

→ Mujhe customer aur total quantity chahiye.

```sql
FROM Orders o
```

→ Orders table se start karo.

```sql
JOIN OrderItems oi
ON o.OrderId = oi.OrderId
```

→ Orders ko unke items ke saath connect karo.

```sql
WHERE o.Status = 'Delivered'
```

→ Sirf delivered orders lo.

```sql
GROUP BY o.CustomerId
```

→ Customer-wise group banao.

```sql
HAVING SUM(oi.Quantity) > 3
```

→ Sirf woh groups rakho jinka total quantity 3 se zyada hai.

```sql
ORDER BY TotalQuantity DESC
```

→ Highest quantity pehle dikhao.

---

# Final Cheat Sheet

| Clause     | Question it answers                    | Example                    |
| ---------- | -------------------------------------- | -------------------------- |
| `SELECT`   | **Kya chahiye?**                       | `SELECT Product`           |
| `FROM`     | **Kahan se chahiye?**                  | `FROM Orders`              |
| `JOIN`     | **Do tables ko kaise connect karein?** | `JOIN OrderItems ON...`    |
| `WHERE`    | **Kaunsi individual rows chahiye?**    | `WHERE Status='Delivered'` |
| `GROUP BY` | **Kin rows ko group karein?**          | `GROUP BY CustomerId`      |
| `HAVING`   | **Kaunse groups chahiye?**             | `HAVING COUNT(*) > 2`      |
| `ORDER BY` | **Result ko kaise sort karein?**       | `ORDER BY Total DESC`      |

### Bas ye flow dimaag mein rakho:

```text
              DATABASE
                  ↓
               FROM
                  ↓
             JOIN (if needed)
                  ↓
               WHERE
          "Kaunsi rows?"
                  ↓
             GROUP BY
          "Kaise group?"
                  ↓
          Aggregate Function
       COUNT / SUM / AVG / MAX
                  ↓
              HAVING
          "Kaunse groups?"
                  ↓
               SELECT
           "Kya dikhana hai?"
                  ↓
             ORDER BY
            "Kis order mein?"
```

**Sabse important difference:**

```text
WHERE  → rows filter karta hai
GROUP BY → rows ko groups mein convert karta hai
HAVING → groups filter karta hai
```

Agar tum `WHERE + GROUP BY + HAVING + JOIN` ko ye **Order/OrderItem flow** se samajh gaye, to SQL ki bahut saari interview queries isi pattern se solve ho jayengi.

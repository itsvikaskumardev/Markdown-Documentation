
---

# 1. JOIN ki need kyun padti hai?

Real database mein hum usually **saara data ek table mein nahi rakhte**.

Maan lo e-commerce application hai.

### `Orders`

| OrderId | CustomerId | Status    |
| ------: | ---------: | --------- |
|     101 |          1 | Delivered |
|     102 |          2 | Pending   |
|     103 |          1 | Delivered |
|     104 |          3 | Cancelled |

### `OrderItems`

| OrderItemId | OrderId | Product  | Quantity | Price |
| ----------: | ------: | -------- | -------: | ----: |
|           1 |     101 | Laptop   |        1 | 50000 |
|           2 |     101 | Mouse    |        2 |  1000 |
|           3 |     102 | Keyboard |        1 |  2000 |
|           4 |     103 | Laptop   |        1 | 50000 |
|           5 |     104 | Monitor  |        1 | 15000 |

Ab question:

> Order `101` mein kaunse products hain?

`Orders` mein sirf:

```text
OrderId = 101
CustomerId = 1
Status = Delivered
```

Product information `OrderItems` mein hai:

```text
OrderId = 101
Laptop
Mouse
```

Toh dono tables ka data combine karna padega.

**Yahin JOIN ka use hota hai.**

---

# 2. JOIN kya hota hai?

Simple definition:

> **JOIN ka use multiple tables ke related data ko ek result mein combine karne ke liye hota hai.**


---

# 3. JOIN ka basic syntax

Sabse basic syntax:

```sql
SELECT columns
FROM Table1
JOIN Table2
    ON Table1.CommonColumn = Table2.CommonColumn;
```

Example:

```sql
SELECT *
FROM Orders o
JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

Yahan:

```text
Orders       → o
OrderItems   → oi
```

`o` aur `oi` **aliases** hain.

---

# 4. Alias kya hota hai?

Ye:

```sql
Orders o
```

ka matlab:

> `Orders` table ko is query ke andar `o` naam se refer karo.

Similarly:

```sql
OrderItems oi
```

means:

> `OrderItems` ko `oi` naam se refer karo.

Ab:

```sql
o.OrderId
```

means:

> Orders table ka OrderId.

And:

```sql
oi.OrderId
```

means:

> OrderItems table ka OrderId.

---

# 5. `ON` kya karta hai?

Ye JOIN ka **sabse important part** hai.

```sql
ON o.OrderId = oi.OrderId
```

`ON` batata hai:

> **Dono tables ki kaunsi columns ke basis par records ko connect karna hai?**

Hamare case mein:

```text
Orders.OrderId
       =
OrderItems.OrderId
```

---

# 6. INNER JOIN

Sabse commonly used JOIN:

> **INNER JOIN sirf matching records return karta hai.**

Syntax:

```sql
SELECT columns
FROM Table1
INNER JOIN Table2
    ON Table1.Column = Table2.Column;
```

Example:

```sql
SELECT
    o.OrderId,
    o.CustomerId,
    o.Status,
    oi.Product,
    oi.Quantity
FROM Orders o
INNER JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

Result:

| OrderId | CustomerId | Status    | Product  | Quantity |
| ------: | ---------: | --------- | -------- | -------: |
|     101 |          1 | Delivered | Laptop   |        1 |
|     101 |          1 | Delivered | Mouse    |        2 |
|     102 |          2 | Pending   | Keyboard |        1 |
|     103 |          1 | Delivered | Laptop   |        1 |
|     104 |          3 | Cancelled | Monitor  |        1 |

---

# 7. INNER JOIN actually kya kar raha hai?

Orders:

```text
101
102
103
104
```

OrderItems:

```text
101
101
102
103
104
```

JOIN:

```sql
ON o.OrderId = oi.OrderId
```

Database har relevant matching `OrderId` ko combine karta hai:

```text
101 → 101 ✅
101 → 101 ✅
102 → 102 ✅
103 → 103 ✅
104 → 104 ✅
```

Isliye result mein:

```text
101 Laptop
101 Mouse
102 Keyboard
103 Laptop
104 Monitor
```

---

# 8. Important: One-to-Many relationship

Ye concept bahut important hai.

Ek `Order` ke multiple `OrderItems` ho sakte hain.

```text
Order 101
   |
   ├── Laptop
   └── Mouse
```

Isliye:

```text
Orders
101
```

JOIN ke baad do rows ban sakti hain:

```text
101 | Laptop
101 | Mouse
```

**Ye duplicate nahi hai.**

Ye actually **one order + multiple related items** ko represent kar raha hai.

---

# 9. LEFT JOIN

Ab maan lo Orders mein ek order hai jiske koi items nahi hain.

### Orders

| OrderId | CustomerId | Status    |
| ------: | ---------: | --------- |
|     101 |          1 | Delivered |
|     102 |          2 | Pending   |
|     103 |          1 | Delivered |
|     104 |          3 | Cancelled |
|     105 |          4 | Pending   |

### OrderItems

| OrderItemId | OrderId | Product  |
| ----------: | ------: | -------- |
|           1 |     101 | Laptop   |
|           2 |     101 | Mouse    |
|           3 |     102 | Keyboard |
|           4 |     103 | Laptop   |
|           5 |     104 | Monitor  |

Order `105` ka koi item nahi hai.

---

## LEFT JOIN

```sql
SELECT
    o.OrderId,
    o.CustomerId,
    oi.Product
FROM Orders o
LEFT JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

Result:

| OrderId | CustomerId | Product  |
| ------: | ---------: | -------- |
|     101 |          1 | Laptop   |
|     101 |          1 | Mouse    |
|     102 |          2 | Keyboard |
|     103 |          1 | Laptop   |
|     104 |          3 | Monitor  |
|     105 |          4 | NULL     |

Notice:

```text
Order 105
    ↓
No matching OrderItem
    ↓
Still appears
    ↓
Product = NULL
```

---

# 10. INNER vs LEFT JOIN

Ye sabse important difference hai.

### INNER JOIN

> Sirf matching records.

```text
Orders        OrderItems
   │              │
   └──── MATCH ───┘
          ↓
       Result
```

### LEFT JOIN

> Left table ki **saari rows**, matching right table data ke saath.

```text
LEFT TABLE
Orders
   ↓
SAARI ROWS
   ↓
Matching OrderItems
   ↓
No match → NULL
```

---

# 11. LEFT JOIN kab use karna hai?

Question:

> **Mujhe saare orders chahiye, chahe unke items hain ya nahi.**

Use:

```sql
LEFT JOIN
```

Example:

```sql
SELECT
    o.OrderId,
    oi.Product
FROM Orders o
LEFT JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

---

# 12. INNER JOIN kab use karna hai?

Question:

> **Mujhe sirf woh orders chahiye jinke items available hain.**

Use:

```sql
INNER JOIN
```

```sql
SELECT
    o.OrderId,
    oi.Product
FROM Orders o
INNER JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

---

# 13. RIGHT JOIN

RIGHT JOIN basically LEFT JOIN ka opposite perspective hai.

Syntax:

```sql
SELECT *
FROM Orders o
RIGHT JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

Meaning:

> **Right table (`OrderItems`) ki saari rows rakho.**

Agar kisi OrderItem ka matching Order nahi mila, tab bhi OrderItem result mein aa sakta hai, aur `Orders` ke columns NULL honge.

Lekin practical development mein:

> **LEFT JOIN generally easier to read hota hai**, kyunki hum usually important/main table ko left side rakhte hain.

RIGHT JOIN ko samajhna important hai, but tum frequently LEFT JOIN use karoge.

---

# 14. FULL OUTER JOIN

FULL JOIN ka meaning:

> **Left table ki saari rows + right table ki saari rows.**

Matching hui:

```text
Orders + OrderItems
```

Matching nahi hui:

```text
Orders → NULL
```

ya:

```text
OrderItems → NULL
```

Syntax:

```sql
SELECT *
FROM Orders o
FULL OUTER JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

Conceptually:

```text
LEFT ONLY       → included
MATCHING        → included
RIGHT ONLY      → included
```

PostgreSQL mein `FULL OUTER JOIN` supported hai.

---

# 15. JOIN ka visual

Suppose:

```text
Orders                 OrderItems

101                     101
102                     101
103                     102
104                     103
105                     999
```

`999` ka Order Orders table mein nahi hai.

### INNER JOIN

```text
101
101
102
103
```

Sirf matching.

### LEFT JOIN

```text
101
101
102
103
104 → NULL
105 → NULL
```

Left table ki sab rows.

### RIGHT JOIN

```text
101
101
102
103
999 → NULL
```

Right table ki sab rows.

### FULL JOIN

```text
101
101
102
103
104 → NULL
105 → NULL
999 → NULL
```

Dono sides ki unmatched rows bhi.

---

# 16. Multiple JOIN kaise lagta hai?

Real application mein sirf 2 tables nahi hote.

Suppose:

### Customers

| CustomerId | Name  |
| ---------: | ----- |
|          1 | Rahul |
|          2 | Amit  |
|          3 | Priya |

### Orders

| OrderId | CustomerId | Status    |
| ------: | ---------: | --------- |
|     101 |          1 | Delivered |
|     102 |          2 | Pending   |
|     103 |          1 | Delivered |

### OrderItems

| OrderItemId | OrderId | Product  |
| ----------: | ------: | -------- |
|           1 |     101 | Laptop   |
|           2 |     101 | Mouse    |
|           3 |     102 | Keyboard |

Ab hume chahiye:

> Customer Name + Order ID + Product

Hume 3 tables join karni hongi.

```sql
SELECT
    c.Name,
    o.OrderId,
    oi.Product
FROM Customers c
INNER JOIN Orders o
    ON c.CustomerId = o.CustomerId
INNER JOIN OrderItems oi
    ON o.OrderId = oi.OrderId;
```

Result:

| Name  | OrderId | Product  |
| ----- | ------: | -------- |
| Rahul |     101 | Laptop   |
| Rahul |     101 | Mouse    |
| Amit  |     102 | Keyboard |

---

# 17. JOIN + WHERE

Suppose:

> Sirf Delivered orders ke products chahiye.

```sql
SELECT
    o.OrderId,
    oi.Product
FROM Orders o
INNER JOIN OrderItems oi
    ON o.OrderId = oi.OrderId
WHERE o.Status = 'Delivered';
```

Result:

| OrderId | Product |
| ------: | ------- |
|     101 | Laptop  |
|     101 | Mouse   |
|     103 | Laptop  |

Yahan:

```text
JOIN
↓
Tables connect

WHERE
↓
Delivered orders filter
```

---

# 18. LEFT JOIN + WHERE mein important trap

Ye query:

```sql
SELECT
    o.OrderId,
    oi.Product
FROM Orders o
LEFT JOIN OrderItems oi
    ON o.OrderId = oi.OrderId
WHERE oi.Product = 'Laptop';
```

Ab tum soch sakte ho:

> LEFT JOIN hai, toh saare Orders aayenge.

**Nahi.**

`WHERE oi.Product = 'Laptop'` ki wajah se jahan `oi.Product` NULL hai, wo rows remove ho jayengi.

So practically ye matching rows tak restrict ho sakta hai.

---

# 19. JOIN condition mein condition lagana

Kabhi-kabhi condition ko `ON` mein lagana important hota hai.

Example:

> Saare orders dikhao, but agar item Laptop hai toh usko join karo.

```sql
SELECT
    o.OrderId,
    oi.Product
FROM Orders o
LEFT JOIN OrderItems oi
    ON o.OrderId = oi.OrderId
    AND oi.Product = 'Laptop';
```

Ab `Orders` ki saari rows retain hongi.

Ye distinction **LEFT JOIN + ON vs WHERE** samajhne ke liye bahut important hai.

---

# 20. JOIN + GROUP BY

Question:

> Har order mein kitne items hain?

```sql
SELECT
    o.OrderId,
    COUNT(oi.OrderItemId) AS TotalItems
FROM Orders o
LEFT JOIN OrderItems oi
    ON o.OrderId = oi.OrderId
GROUP BY o.OrderId;
```

Why `LEFT JOIN`?

Because hume **zero-item orders bhi chahiye**.

Result:

| OrderId | TotalItems |
| ------: | ---------: |
|     101 |          2 |
|     102 |          1 |
|     103 |          1 |
|     104 |          1 |
|     105 |          0 |

Notice:

```text
Order 105
No OrderItems
↓
LEFT JOIN
↓
Still exists
↓
COUNT(OrderItemId) = 0
```

---

# 21. `COUNT(*)` vs `COUNT(column)` yahan important hai

Agar tum likho:

```sql
COUNT(*)
```

to LEFT JOIN mein unmatched row bhi count ho sakti hai.

Isliye:

```sql
COUNT(oi.OrderItemId)
```

better hai jab tum **actual child records count** karna chahte ho.

Because:

```text
Order 105
oi.OrderItemId = NULL
```

and:

```text
COUNT(oi.OrderItemId)
= 0
```

---

# 22. JOIN + GROUP BY + HAVING

Question:

> Un customers ko find karo jinhone 2 se zyada items purchase kiye hain.

```sql
SELECT
    o.CustomerId,
    SUM(oi.Quantity) AS TotalQuantity
FROM Orders o
INNER JOIN OrderItems oi
    ON o.OrderId = oi.OrderId
GROUP BY o.CustomerId
HAVING SUM(oi.Quantity) > 2;
```

Flow:

```text
Orders
  +
OrderItems
  ↓
JOIN
  ↓
Customer-wise grouping
  ↓
SUM Quantity
  ↓
HAVING > 2
```

---

# 23. JOIN mein `ON` aur `WHERE` ka difference

Ye interview mein pucha ja sakta hai.

### ON

> Tables ke records ko **kaise match/connect karna hai?**

```sql
ON o.OrderId = oi.OrderId
```

### WHERE

> Joined result mein se **kaunsi rows rakhni hain?**

```sql
WHERE o.Status = 'Delivered'
```

Simple:

```text
ON
↓
Relationship / matching condition

WHERE
↓
Final row filtering
```

---

# 24. JOIN aur Foreign Key ka relation

Humne pehle Foreign Key padha tha.

```text
Orders
----------------
OrderId PK
      ↑
      |
      |
OrderItems
----------------
OrderId FK
```

Foreign Key database mein relationship define kar sakti hai.

But JOIN:

> **Us relationship ka data query mein use karke tables ko combine karta hai.**

Important:

**Foreign Key hona mandatory nahi hai JOIN lagane ke liye.**

Technically tum kisi bhi logically related columns ko join kar sakte ho:

```sql
ON table1.SomeColumn = table2.SomeColumn
```

Lekin proper relational design mein related tables ke beech FK commonly use hoti hai.

---

# 25. JOIN ke types ek baar

```text
JOIN
 │
 ├── INNER JOIN
 │      → only matching
 │
 ├── LEFT JOIN
 │      → all left + matching right
 │
 ├── RIGHT JOIN
 │      → all right + matching left
 │
 ├── FULL OUTER JOIN
 │      → everything from both
 │
 └── SELF JOIN
        → table ko khud se join karna
```

---

# 26. SELF JOIN kya hota hai?

Iske liye Orders/OrderItems se better `Employees` example hai.

### Employees

| EmployeeId | Name  | ManagerId |
| ---------: | ----- | --------: |
|          1 | Rahul |      NULL |
|          2 | Amit  |         1 |
|          3 | Priya |         1 |
|          4 | Neha  |         2 |

Yahan same `Employees` table mein:

```text
Rahul
 ├── Amit
 │    └── Neha
 └── Priya
```

Agar hume employee + manager name chahiye:

```sql
SELECT
    e.Name AS Employee,
    m.Name AS Manager
FROM Employees e
LEFT JOIN Employees m
    ON e.ManagerId = m.EmployeeId;
```

Yahan:

```text
Employees e
```

employee ke liye.

```text
Employees m
```

manager ke liye.

Same table ko **do different aliases** ke through join kiya.

Ye **SELF JOIN** hai.

---

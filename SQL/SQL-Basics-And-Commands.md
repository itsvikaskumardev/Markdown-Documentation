
# Q1) What is SQL?


**SQL ek language hai jiska use hum Relational Database ke saath kaam karne ke liye karte hain.**


---

## SQL se hum kya-kya kar sakte hain?

### 1. Data read/retrieve karna

```sql
SELECT * FROM Employees;
```

### 2. Data insert karna

```sql
INSERT INTO Employees (Name, Salary)
VALUES ('Vikas', 70000);
```

### 3. Data update karna

```sql
UPDATE Employees
SET Salary = 75000
WHERE Name = 'Vikas';
```

### 4. Data delete karna

```sql
DELETE FROM Employees
WHERE Name = 'Vikas';
```

### 5. Database structure modify karna

For example, table mein new column add karna:

```sql
ALTER TABLE Employees
ADD COLUMN Email VARCHAR(100);
```

---

# Q2) What is a Database?

**Database ek organized collection of data hota hai jahan data ko electronically store aur manage kiya jata hai.**

Simple example:

Suppose tum ek company mein kaam karte ho aur company ke paas employees ka data hai:

```text
Employee Database
       |
       ├── Employees
       ├── Departments
       ├── Salaries
       └── Attendance
```

`Employees` table:

| Id | Name  | Department |
| -: | ----- | ---------- |
|  1 | Rahul | IT         |
|  2 | Amit  | HR         |
|  3 | Priya | IT         |

Ye poora organized data **database** ka part hai.

---

## Database ki need kyun hai?

Suppose tum 1 million employees ka data simple Excel/text files mein manage karne lago.

Problems aa sakti hain:

* Data search karna difficult
* Duplicate data
* Multiple users simultaneously kaam karna difficult
* Security manage karna difficult
* Data update/delete difficult
* Relationships maintain karna difficult

Database systems in problems ko efficiently handle karte hain.

---

# Database vs SQL

Ye dono same nahi hain.

### Database

**Database = jahan data store hota hai.**

Example:

```text
CompanyDB
```

### SQL

**SQL = database ke saath communicate karne ki language.**

Example:

```sql
SELECT * FROM Employees;
```

Ek simple analogy:

```text
Database = Library 📚
SQL      = Librarian se baat karne ki language
```

Tum librarian ko bolte ho:

> "Mujhe SQL wali books do."

Similarly SQL se database ko query dete ho:

```sql
SELECT * FROM Books;
```

---

# Q3) What are the types of SQL Commands?


```text
                         SQL
                          |
          ┌───────────────┼────────────────┐
          │               │                │
         DDL             DML              DQL
          │               │                │
    Structure          Data change       Read data
          │               │                │
    CREATE             INSERT            SELECT
    ALTER              UPDATE
    DROP               DELETE
    TRUNCATE

          ┌───────────────────────────────┐
          │                               │
         DCL                             TCL
          │                               │
      Permissions                     Transactions
          │                               │
       GRANT                          COMMIT
       REVOKE                         ROLLBACK
                                      SAVEPOINT
```

---

# 1. DDL — Data Definition Language

### Meaning

**DDL ka use database ki structure/schema define ya modify karne ke liye hota hai.**

Yaani:

> **Table/database ka structure banana ya change karna.**

Common commands:

```text
CREATE -New table create karna:
ALTER-Existing table ki structure change karna:
DROP-Table ko completely remove karna:Table + uska data remove ho jayega.
TRUNCATE-Table ke **saare records remove** karna, lekin table ki structure generally retain hoti hai:

```


---

# 2. DML — Data Manipulation Language

### Meaning

**DML ka use table ke andar stored data ko manipulate/change karne ke liye hota hai.**

Common commands:

### INSERT -New data add:
UPDATE -Existing data change:
DELETE -Delete Data:
```

---

# 3. DCL — Data Control Language

### Meaning

**DCL ka use database mein permissions/access control karne ke liye hota hai.**

Simple words:

> **Kaun kya kar sakta hai?**

For example company database mein:

```text
Admin
  ↓
Full access

Developer
  ↓
Read + Write

Intern
  ↓
Read only
```

Common commands:

```text
GRANT
REVOKE
```

### GRANT

Kisi user ko permission dena.

Example:

```sql
GRANT SELECT ON Employees TO developer;
```

Meaning:

> `developer` ko Employees table se data read karne ki permission do.

### REVOKE

Di hui permission hata dena.

```sql
REVOKE SELECT ON Employees FROM developer;
```

---

# 4. TCL — Transaction Control Language

### Meaning

**TCL ka use database transactions ko manage karne ke liye hota hai.**

Pehle samjho transaction kya hai.

Suppose bank transfer:

```text
Rahul ke account se ₹1000 minus
              ↓
Amit ke account mein ₹1000 add
```

Ye ideally ek transaction ka part hona chahiye.

Agar first operation successful ho gaya:

```text
Rahul → -₹1000 ✅
```

but second fail ho gaya:

```text
Amit → +₹1000 ❌
```

to problem hogi.

Isliye transaction system help karta hai ki operations ko safely manage kiya ja sake.

Common TCL commands:

```text
COMMIT
ROLLBACK
SAVEPOINT
```

### COMMIT

Changes permanently save karna.

```sql
COMMIT;
```

### ROLLBACK

Transaction ke changes undo karna.

```sql
ROLLBACK;
```

### SAVEPOINT

Transaction ke andar ek checkpoint create karna.

```sql
SAVEPOINT point1;
```

Baad mein us point tak rollback kar sakte ho.

---

# 5. DQL — Data Query Language

### Meaning

**DQL ka use database se data retrieve/read karne ke liye kiya jata hai.**

Main command:

```text
SELECT
```


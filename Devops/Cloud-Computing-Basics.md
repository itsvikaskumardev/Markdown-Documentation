# Basics of Cloud Computing

## 1. What is Cloud?

Simple words mein, **Cloud ka matlab hai internet ke through kisi remote computer/server ke resources ko use karna.**

Cloud ke peeche actually **real physical servers, storage devices, networking devices aur data centers** hote hain. Ye resources cloud providers ke data centers mein physically available hote hain.

Hum apne computer par hardware physically install ya maintain karne ke bajay, internet ke through in remote resources ko use kar sakte hain.

For example, agar hum kisi cloud server par apni application deploy karte hain, toh hamari application **cloud provider ke data center mein kisi physical server par run ho rahi hoti hai**.

### Simple Definition:

> **Cloud = Internet ke through remote computing resources ko access aur use karna.**

---

# 2. What is Cloud Computing?

**Cloud Computing** ek technology/model hai jisme computing resources ko internet ke through **on-demand** provide kiya jata hai.

Computing resources mein:

* **Servers / Compute**
* **Storage**
* **Databases**
* **Networking**
* **Operating Systems**
* **Applications**
* **Security services**
* etc.

include ho sakte hain.

Instead of company khud:

* physical servers kharide
* data center banaye
* hardware maintain kare
* storage setup kare
* networking manage kare

company cloud provider se ye resources **internet ke through use** kar sakti hai.

### Example:

Suppose hume ek ASP.NET Core application deploy karni hai.

Traditional approach mein:

```text
Our Company
     ↓
Buy Physical Server
     ↓
Install OS
     ↓
Configure Network
     ↓
Install Database
     ↓
Deploy Application
     ↓
Maintain Hardware
```

Cloud mein:

```text
Developer
    ↓
Internet
    ↓
Cloud Provider
    ↓
Cloud Server
    ↓
ASP.NET Core Application
```

Yaani hum cloud provider ke infrastructure ko use kar rahe hain instead of everything physically apne paas rakhne ke.

---

# 3. Cloud Provider kya hota hai?

**Cloud Provider** ek company hoti hai jo computing resources provide karti hai.

Examples:

* AWS — Amazon Web Services
* Microsoft Azure
* Google Cloud

Ye companies huge **data centers** operate karti hain jahan thousands/millions of physical machines aur other infrastructure hota hai.

```text
                CLOUD PROVIDER
                     |
              ┌──────┴──────┐
              ↓             ↓
          Data Center    Data Center
              |             |
        ┌─────┴─────┐   ┌───┴─────┐
        ↓     ↓     ↓   ↓    ↓    ↓
      Server Server Storage Network
```

Hum internet ke through in resources ko use karte hain.

---

# 4. Public Cloud

**Public Cloud** mein cloud resources ek cloud provider own aur manage karta hai aur multiple customers un resources ko use kar sakte hain.

```text
             AWS / Azure / GCP
                    |
        ┌───────────┼───────────┐
        ↓           ↓           ↓
    Customer A  Customer B  Customer C
```

Har customer ko apne resources milte hain, lekin underlying cloud infrastructure provider manage karta hai.

### Who Uses It?

Individuals, startups, companies aur large organizations — almost anyone.

### Example:

A company AWS par apni backend application deploy karti hai.

Company ko AWS ke physical servers kharidne ki zarurat nahi hai.

AWS infrastructure provide aur manage karta hai.

### Examples:

* AWS
* Microsoft Azure
* Google Cloud

---

# 5. Private Cloud

**Private Cloud** ek organization ke liye dedicated cloud environment hota hai.

Yaani infrastructure/resources primarily **ek single organization** ke liye hote hain.

```text
          Company
             |
      ┌──────┴──────┐
      ↓             ↓
   Servers       Storage
      |
   Network
```

Ismein organization khud infrastructure manage kar sakti hai, ya kisi third party se manage karwa sakti hai.

### Example:

Ek large company apne organization ke liye dedicated cloud infrastructure maintain karti hai jahan sirf us organization ke authorized users/applications access karte hain.

### Simple Meaning:

> **Private Cloud = Ek organization ke liye dedicated cloud environment.**

---

# 6. Hybrid Cloud

**Hybrid Cloud = Public Cloud + Private Cloud**

Yaani organization dono environments ko combine karke use karti hai.

```text
             Company
                |
        ┌───────┴────────┐
        ↓                ↓
   Private Cloud    Public Cloud
        |                |
   Sensitive Data    Web Application
```

### Example:

Suppose ek company ke paas sensitive customer data hai.

Company:

* sensitive data → **Private Cloud**
* normal application/web server → **Public Cloud**

use kar sakti hai.

Is tarah company dono environments ka benefit le sakti hai.

### Simple Meaning:

> **Hybrid Cloud = Private Cloud aur Public Cloud ka combination.**

---

# 7. Public vs Private vs Hybrid

| Type              | Meaning                                                            |
| ----------------- | ------------------------------------------------------------------ |
| **Public Cloud**  | Cloud provider ka infrastructure multiple customers use karte hain |
| **Private Cloud** | Cloud environment ek organization ke liye dedicated hota hai       |
| **Hybrid Cloud**  | Public + Private Cloud ko combine karke use karna                  |

### Yaad rakhne ka simple way:

```text
Public  → Shared Cloud Environment

Private → Dedicated Cloud Environment

Hybrid  → Public + Private
```

---

# 8. Cloud aur Cloud Computing mein Difference

Ye interview mein important hai.

### Cloud

**Cloud** basically remote computing resources/infrastructure ko refer karta hai jo network/internet ke through available hai.

### Cloud Computing

**Cloud Computing** un remote computing resources ko **on-demand access aur use karne ka model** hai.

Simple:

```text
Cloud
  ↓
Remote Computing Resources

Cloud Computing
  ↓
Internet ke through un resources ko
On-Demand use karna
```

---

# 9. Real Meaning of Cloud

Jab hum bolte hain:

> "My application is running on the cloud."

iska matlab ye nahi hai ki application literally hawa mein ya kisi invisible virtual place mein run ho rahi hai.

Actually:

```text
Your Application
       ↓
Virtual Machine / Container
       ↓
Physical Server
       ↓
Data Center
       ↓
Cloud Provider
```

Cloud provider ke **real physical data centers** hote hain.

Hum un physical machines ko directly manage nahi karte. Cloud provider unke upar services/resources provide karta hai.

---

# 10. In a Nutshell

### Cloud

> Internet/network ke through available remote computing resources.

### Cloud Computing

> Internet ke through computing resources ko on-demand use karne ka model.

### Public Cloud

> Cloud provider ka infrastructure multiple customers ke liye available hota hai.

### Private Cloud

> Ek organization ke liye dedicated cloud environment.

### Hybrid Cloud

> Public Cloud + Private Cloud ka combination.

### Cloud Provider

> Jo company cloud infrastructure aur services provide karti hai.

Examples:

> **AWS, Azure, Google Cloud**


---


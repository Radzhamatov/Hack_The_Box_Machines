# Hack The Box — Appointment Walkthrough

## 📌 Machine Info
- **Name:** Appointment
- **Difficulty:** Very Easy
- **Category:** Web / SQL Injection

---

## 1️⃣ Reconnaissance
First, scan the target to identify open ports and services:

```bash
nmap -sC -sV -Pn <TARGET_IP>
```

### Result:
```text
80/tcp open  http
```

This indicates a web application running on port 80.

---

## 2️⃣ Web Enumeration
Open the website in the browser:

```
http://<TARGET_IP>
```

The page contains a **login form** with username and password fields.

---

## 3️⃣ Vulnerability Identification
The login form does not properly sanitize user input, making it vulnerable to **SQL Injection**.

This vulnerability is classified as:
- **OWASP Top 10 (2021): A03 – Injection**

---

## 4️⃣ Exploitation (SQL Injection Login Bypass)
To bypass authentication, the following payload was used in the **username** field:

```text
admin' OR '1'='1
```

The password field can be left empty or filled with random data.

### Explanation:
- `'1'='1'` is always TRUE
- The SQL query logic is bypassed
- Authentication succeeds without knowing the password

---

## 5️⃣ Flag Retrieval
After successful login, the application redirects to the dashboard where the **flag is displayed**.

---

## 🧠 Key Takeaways
- User input was directly embedded into SQL queries
- Lack of input validation and prepared statements caused SQL Injection
- SQL Injection can be used to bypass authentication

---

## ✅ Machine Status
🏁 **Appointment — Completed Successfully**


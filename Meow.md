# 🐱 Hack The Box — Meow Write-up

## 📌 Machine Information
- **Name:** Meow  
- **Difficulty:** Very Easy  
- **Platform:** Hack The Box (Starting Point)  
- **IP:** <TARGET_IP>  
- **OS:** Linux  

---

## 🎯 Objective
The objective of this machine is to gain initial access by identifying an exposed service and logging in using weak or default credentials.

---

## 🔍 Enumeration

### Connectivity Test
```bash
ping <TARGET_IP>
```
The target responded successfully.

---

### Port Scan
```bash
nmap -sC -sV <TARGET_IP>
```

**Result:**
```text
23/tcp open  telnet
```

- Port **23** is open
- Service running: **Telnet**
- Telnet does not use encryption and is insecure

---

## 🚪 Exploitation

### Telnet Connection
```bash
telnet <TARGET_IP>
```

### Credentials Used
- **Username:** root
- **Password:** (empty)

Login was successful.

---

## 🏁 Flag
After logging in, I listed the files and found the flag.

```bash
ls
cat flag.txt
```

🎉 Flag successfully obtained.

---

## 🧠 Lessons Learned
- Telnet is insecure and should not be exposed
- Weak or empty credentials are dangerous
- Enumeration is a critical step in penetration testing

---

## ✅ Conclusion
The Meow machine demonstrates the risks of exposing insecure services with poor authentication and is ideal for beginners.

---

### 🧑‍💻 Author
Aziz Radzhamatov


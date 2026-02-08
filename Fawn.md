# 🦌 Hack The Box — Fawn Write-up

## 📌 Machine Information
- **Name:** Fawn  
- **Difficulty:** Very Easy  
- **Platform:** Hack The Box (Starting Point)  
- **IP:** <TARGET_IP>  
- **OS:** Linux  

---

## 🎯 Objective
The goal of this machine is to enumerate an FTP service and retrieve files using anonymous access.

---

## 🔍 Enumeration

### Connectivity Test
```bash
ping <TARGET_IP>
```

---

### Port Scan
```bash
nmap -sC -sV <TARGET_IP>
```

**Result:**
```text
21/tcp open  ftp
```

- FTP service detected on port **21**

---

## 🔑 FTP Enumeration

### Connecting to FTP
```bash
ftp <TARGET_IP>
```

### Anonymous Login
```text
Username: anonymous
Password: anonymous
```

Login was successful.

---

## 📂 File Listing
```text
ftp> ls
```

A file containing sensitive information was found.

---

## ⬇️ File Download
```text
ftp> get <filename>
```

The file was downloaded successfully.

---

## 🏁 Flag
The downloaded file contained the flag.

🎉 Flag successfully obtained.

---

## 🧠 Lessons Learned
- FTP often allows anonymous access
- Anonymous FTP is a serious security issue
- Basic FTP commands (`ls`, `get`) are essential

---

## ✅ Conclusion
The Fawn machine highlights the risks of misconfigured FTP services and helps beginners understand service enumeration.

---

### 🧑‍💻 Author
Aziz Radzhamatov


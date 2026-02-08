# TryHackMe — Dancing

## 🧠 Overview
**Dancing** is a beginner-level Windows machine on the TryHackMe platform.  
This machine focuses on basic **network enumeration** and identifying **misconfigured SMB services**.

---

## 🎯 Objective
- Perform network scanning
- Identify exposed services
- Enumerate SMB shares
- Access shared files anonymously
- Retrieve the flag

---

## 🔍 Network Enumeration (Nmap)

The assessment started with an Nmap scan:

```bash
nmap -sC -sV <IP>
```

### Open Ports
```
135/tcp   msrpc
139/tcp   netbios-ssn
445/tcp   smb
5985/tcp  winrm
```

The results indicate a **Windows-based system** with SMB enabled.

---

## ⚠️ Weak Service Identification

The weakest and most important service identified was:

**SMB (port 445)**

SMB is commonly targeted because:
- It provides file-sharing functionality
- Misconfigurations can allow anonymous access
- Shared files may be exposed without authentication

---

## 📂 SMB Enumeration and Access

SMB enumeration confirmed that **file shares were available** on the target system.

The share was accessed using **anonymous authentication** with the following command:

```bash
smbclient //<IP>/<SHARE> -N
```

This allowed browsing the shared directories without valid credentials.

---

## 🏁 Flag Retrieval

A flag file was found inside the SMB share.  
The file was **downloaded using the `get` command** and opened locally.

Example:
```bash
get flag.txt
```

---

## ✅ Result
- Network services successfully enumerated
- SMB identified as the primary attack vector
- Anonymous SMB access achieved
- Flag successfully retrieved
- Machine completed 🎉

---

## 📌 Key Takeaways
- Always begin with Nmap enumeration
- SMB (port 445) is a high-value target on Windows systems
- Anonymous SMB access is a serious security misconfiguration
- Enumeration is more important than brute-force on beginner machines

---

## 🧑‍💻 Platform Information
- Platform: TryHackMe
- Room: Dancing
- Difficulty: Easy / Beginner


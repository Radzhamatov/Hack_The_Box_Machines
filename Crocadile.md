# HTB Crocodile -- Write-up

## Machine Information

-   **Name:** Crocodile
-   **Difficulty:** Very Easy
-   **Platform:** Hack The Box

------------------------------------------------------------------------

## 1. Reconnaissance

Initial reconnaissance was performed using Nmap to identify open ports
and running services.

``` bash
nmap -sC -sV -oN nmap.txt TARGET_IP
```

### Results

-   **21/tcp** -- FTP (vsftpd 3.0.3)
-   **80/tcp** -- HTTP

------------------------------------------------------------------------

## 2. FTP Enumeration

Anonymous FTP login was enabled on the target system.

``` bash
ftp TARGET_IP
```

Credentials:

    Username: anonymous
    Password: anonymous

The server returned **FTP code 230**, confirming successful anonymous
authentication.

------------------------------------------------------------------------

## 3. Credential Disclosure

Listing files on the FTP server revealed sensitive files containing
credentials.

``` ftp
ls
get allowed.userlist
get allowed.userlist.passwd
```

These files contained valid usernames and passwords used by the web
application.

------------------------------------------------------------------------

## 4. Web Enumeration

Gobuster was used to enumerate hidden web files.

``` bash
gobuster dir -u http://TARGET_IP -w /usr/share/wordlists/dirb/common.txt -x php
```

Discovered file: - `login.php`

------------------------------------------------------------------------

## 5. Web Authentication

The credentials obtained from the FTP server were used to authenticate
at:

    http://TARGET_IP/login.php

Login was successful.

------------------------------------------------------------------------

## 6. Flag

After successful authentication, the user dashboard was accessed and the
flag was retrieved.

    HTB{REDACTED}

------------------------------------------------------------------------

## 7. Conclusion

### Vulnerabilities Identified

-   Anonymous FTP access (Security Misconfiguration)
-   Sensitive information disclosure
-   Credential reuse

### Notes

-   No exploits were required
-   The machine was compromised by abusing misconfigurations

------------------------------------------------------------------------

## Tools Used

-   Nmap
-   FTP
-   Gobuster
-   Web Browser

# Enumeration Check List
## Author: JamesB9
## Description: A list of steps I carry out during the enumeration stage of a CTF.


### 1) Full Port Scan
#### Commands:
* "nmap [IP] -p- -sV -A"
#### Information:
What to look out for:
Port | Service | Example
------------ | ------------- | -------------
21 | FTP | vsftpd
22 | SSH | OpenSSH
23 | Telnet | Unencrypted command-line
25 | SMTP | Mail Server
69 | TFTP | FTP server using UDP
80 | HTTP | Apache
110 | POP3 | Mail Server
139 | SMB | Sharing files, printers, etc.
443 | HTTPS | Encrypted HTTP
445 | SMB | SMB over IP

### 2) Vulnerability Scans
#### Commands:
* nmap -sV --script=vulscan/vulscan.nse [IP]
* nikto -host [IP] -p [minPort],[maxPort] -maxtime [seconds]

### 2) Web Server Directory Scan
#### Commands:
* gobuster dir -w [wordlist] -u [URL]
#### Information:
Good word lists:
* /usr/share/wordlist

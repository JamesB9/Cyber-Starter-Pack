# Scoping Check List
## Author: JamesB9
## Description: 
A list of steps I carry out before I have a shell on the target machine.


### Full Port Scan

    nmap [IP] -p- -sV  [-A]
	
What ports to look for:
	21	 - FTP
	22	 - SSH
	23	 - Telnet 
	25	 - SMTP 	(Mail Server)
	69   - TFTP		(FTP Server using UDP)
	80	 - HTTP 	
	110  - POP3		(Mail Server)
	139  - SMB 		(Shring files, printes etc.)
	443  - HTTPS
	445  - SMB		(SMB over IP)
	
### Nikto Vulnerability Scan

	nikto -host [IP]


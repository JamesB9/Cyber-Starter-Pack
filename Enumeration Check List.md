# --== Enumeration Check List ==----------------------------------------------------------------------------------
## Author: JamesB9
## Description: A list of steps I carry out during the enumeration stage of a CTF.
# ------------------------------------------------------------------------------------------------------

### 1) Full port scan with service scan 
	Command: "nmap [IP] -p- -sV [-A]"
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
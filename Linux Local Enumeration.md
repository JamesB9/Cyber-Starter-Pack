# Linux Local Enumeration Check List
## Author: JamesB9
## Description: A list of steps I carry out once I have a basic shell on the target machine

### Find id_rsa: 
id_rsa is a file containing the SSH private key for a user to use to sign into the machine via ssh without a password
It is usually found in the user's home directory within a .ssh/ folder.

### Create a more stable shell:
There are many ways for this. A commands are listed below
    python3 -c 'import pty; pty.spawn("/bin/bash")'
	/bin/bash

- "uname -a" print system info then look for exploits/vulns for that os distro/version

- Auto generated bash files: check user history via ~/.bash_history or "history" command

- Check .bash_profile and .bashrc in user's home directories. Contain commands that are run when bash starts.

- Check sudo version by "sudo -V" if out of date may have vulnerability for example sudo versions < 1.8.28 are vulnerable to CVE-2019-14287, which is a vulnerability that allows to gain root access with 1 simple command. 

- Check /etc/passwd (for read and write access)

goldfish:x:1003:1003:,,,:/home/goldfish:/bin/bash

1. (goldfish) - Username
2. (x) - Password. (x character indicates that an encrypted account password is stored in /etc/shadow file and cannot be displayed in the plain text here)
3. (1003) - User ID (UID): Each non-root user has his own UID (1-99). UID 0 is reserved for root.
4. (1003) - Group ID (GID): Linux group ID
5. (,,,) - User ID Info: A field that contains additional info, such as phone number, name, and last name. (,,, in this case means that I did not input any additional info while creating the user)
6. (/home/goldfish) - Home directory: A path to user's home directory that contains all the files related to them.
7. (/bin/bash) - Shell or a command: Path of a command or shell that is used by the user. Simple users usually have /bin/bash as their shell, while services run on /usr/sbin/nologin. 

- Check /etc/shadow

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

- Check /etc/hosts: gives names to IP addresses. this file may reveal a local address of devices in the same network. It can help us to enumerate the network further.

- Find logs, configuration files and backup files. (.log, .conf, .bak)

- Find files with SUID bit set. Use GTFOBins to find ways of exploiting these binaries.

- use commands "netstat -at | less" & "netstat -tulpn" to view port forwarding and therefore other machines on the network.




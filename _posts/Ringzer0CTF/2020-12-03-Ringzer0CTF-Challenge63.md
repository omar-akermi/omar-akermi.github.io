---
layout: post
title: Ringzer0CTF Martian Message part 2
categories: [Write-Ups, Ringzer0CTF, Cryptography]
tags: [Ringzer0CTF, Cryptography, Vigenere Cipher]
featured-image:  ringzer0CTF/icon.png
featured-image-alt: ringzer0CTF
---

It's a write-up about the challenge : [Ringzer0CTF - Martian Message part 2](https://ringzer0ctf.com/challenges/63)

# Challenge 63 - Martian Message part 2

## 1. Challenge

```
I think that's the key "fselkladfklklakl"

KDERE2UNX1W1H96GYQNUSQT1KPGB 
```

## 2. Solution

It is a vigenere cipher. Simply shift the ciphertext by the key.
We used this website to decode the Martian message.
https://www.dcode.fr/vigenere-cipher

The flag is : **FLAGU2JNU1R1X96VOFNKHLB1GEWQ**
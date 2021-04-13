---
layout: post
title: Ringzer0CTF Can you understand this sentence ?
categories: [Write-Ups, Ringzer0CTF, Cryptography]
tags: [Ringzer0CTF, Cryptography, Bubblebabble]
featured-image:  ringzer0CTF/icon.png
featured-image-alt: ringzer0CTF
---

It's a write-up about the challenge : [Ringzer0CTF - Can you understand this sentence](https://ringzer0ctf.com/challenges/23)

# Challenge 23 - Can you understand this sentence ?

## 1. Challenge

```xipak-comok-repuk-vanik-dytuk-dimyk-sinyx```

## 2. Solution

Bubble Babble is a binary data encoding designed by Antti Huima. This encoding uses alternation of consonants and vowels to encode binary data to pseudowords that can be pronounced more easily than arbitrary lists of hexadecimal digits. 

We used this git : https://github.com/bohwaz/bubblebabble

We created a php file flag which contains the code to decrypt :

```
<?php

// Check Bubble Babble encoding / decoding

require 'bubble_babble.php';

$decoded = BubbleBabble::Decode('xipak-comok-repuk-vanik-dytuk-dimyk-sinyx');

print $decoded;
```

Then in a terminal we launched the program : `php flag.php`

The flag was : **hackingbubble**
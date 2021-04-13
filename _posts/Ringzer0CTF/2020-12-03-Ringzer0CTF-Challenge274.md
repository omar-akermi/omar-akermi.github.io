---
layout: post
title: Ringzer0CTF You're Drunk !
categories: [Write-Ups, Ringzer0CTF, Cryptography]
tags: [Ringzer0CTF, Cryptography]
featured-image:  ringzer0CTF/icon.png
featured-image-alt: ringzer0CTF
---

It's a write-up about the challenge : [Ringzer0CTF - You're Drunk ! : Bacon](https://ringzer0ctf.com/challenges/274)

# Challenge 274 - You're Drunk !

## 1. Challenge 

The crypto challenge is : 

```Ayowe awxewr nwaalfw die tiy rgw fklf ua xgixiklrw! Tiy lew qwkxinw.```

## 2. Solution 

We used this website to decode the message :
https://planetcalc.com/8047/
The decrypeted text was :
`SFPER SECRET LESSANE WOR DOF THE NMAN IS CHOCOMATE! DOF ARE VEMCOLE.`

So, we tried to decode the message with the word secret, by replacing all the letters.
First try : 
S--er secret -ess--e --r --- t-e ---- is c--c---te ! --- are -e-c--e

At the end with some deduction we had :

```Super secret message for you the glag is chocolate ! you are welcome.```

We saw that the letter in the code is the letter on the right in a qwerty keyboard.

The flag is : **chocolate**
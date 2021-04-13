---
layout: post
title: Ringzer0CTF Bash Jail 1
categories: [Write-Ups, Ringzer0CTF, Jail-Escaping]
tags: [Ringzer0CTF, Jail Escaping]
featured-image:  ringzer0CTF/icon.png
featured-image-alt: ringzer0CTF
---

It's a write-up about the challenge : [Ringzer0CTF - Bash Jail 1](https://ringzer0ctf.com/challenges/218)

# Challenge 218 - Bash Jail 1

## 1. Challenge

After login into the level1 with this command : 
`ssh -l level1 -p 10218 challenges.ringzer0team.com`
and this password : level1

We saw : 

```
RingZer0 Team Online CTF

BASH Jail Level 1:
Current user is uid=1000(level1) gid=1000(level1) groups=1000(level1)

Flag is located at /home/level1/flag.txt

Challenge bash code:
-----------------------------

while : do
done
  echo "Your input:" 
  read input 
  output=`$input`

-----------------------------

Your input:
```

## 2. Solution

The problem is that we couldn't receive the stdout. 
But after, some tries we saw that we received stderr.

We launched the bash with `bash`. 
And we pipe stdout to stderr to read the flag file. 
1 stands for stdout and 2 stands for stderr. 
So we found the solution with this command : `cat flag.txt 1>&2`

The flag is : **FLAG-U96l4k6m72a051GgE5EN0rA85499172K**

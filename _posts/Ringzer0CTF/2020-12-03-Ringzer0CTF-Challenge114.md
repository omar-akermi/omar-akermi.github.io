---
layout: post
title: Ringzer0CTF Hangovers and more Bacon
categories: [Write-Ups, Ringzer0CTF, Cryptography]
tags: [Ringzer0CTF, Cryptography, Bacon cipher]
featured-image:  ringzer0CTF/icon.png
featured-image-alt: ringzer0CTF
---

It's a write-up about the challenge : [Ringzer0CTF - Hangovers and more : Bacon](https://ringzer0ctf.com/challenges/114)

# Challenge 114 - Hangovers and more : Bacon

``` 
VoiCI unE SUpeRbe reCeTtE cONcontee pAR un GrouPe d'ArtistEs culinaiRe, dONT le BOn Gout et lE SeNs de LA cLasSe n'est limIteE qUe par LE nombre DE cAlOries qU'ils PeUVEnt Ingurgiter. Ces virtuoses de la friteuse vous presente ce petit clip plein de gout savoureux !!

We hijack the Bacon Bacon Truck in San Francisco!


The cure for your worst
Hangovers and more: Bacon
True: Science says so
```

## 2. Solution 

We knew the Bacon cipher. 
We created a script to encode this text : 
a `B` for a supper character and a `A` lower character.

We obtained : 
```
BAABBAABBBAABAAAABABABABBAAAAAAABBAABAAABAABAAAAABAAAAAAAABAABBBAABBABAAAAAABBABAAABBABAABAAAAAAAABAABABAAAABBAAAAAABBABABAAAAABAAABABBBAABAAAAAAAAA
```

We put this on this website : http://rumkin.com/tools/cipher/baconian.php

We obtained : `THEFLAGISBACONANDJACKDANIELSA`

The solution of the challenge is : **BACONANDJACKDANIELS**.
---
layout: post
title: Ringzer0CTF I lost my password can you find it ?
categories: [Write-Ups, Ringzer0CTF, Cryptography]
tags: [Ringzer0CTF, Cryptography, AES]
featured-image:  ringzer0CTF/icon.png
featured-image-alt: ringzer0CTF
---

It's a write-up about the challenge : [Ringzer0CTF - I lost my password can you find it ?](https://ringzer0ctf.com/challenges/51)


# Challenge 51 - I lost my password can you find it ?

## 1. Challenge

It's an archive.


## 2. Solution

Group policy preferences allows domain admins to create and deploy across the domain local users and local administrators accounts.
We searched into all directories. In one of them, we found a file called Groups.xml.


```
<?xml version="1.0" encoding="utf-8"?>
<Groups clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}"><User clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}" name="Administrator (built-in)" image="1" changed="2014-02-06 19:33:28" uid="{C73C0939-38FB-4287-AC48-478F614F5EF7}" userContext="0" removePolicy="0"><Properties action="R" fullName="Administrator" description="Administrator" cpassword="PCXrmCkYWyRRx3bf+zqEydW9/trbFToMDx6fAvmeCDw" changeLogon="0" noChange="0" neverExpires="1" acctDisabled="0" subAuthority="" userName="Administrator (built-in)"/></User>
</Groups>
```

We saw the encrypted password in cpassword.
These password are all encrypted with AES, so we get the ruby script on this website : https://pentestlab.blog/tag/cpassword/

We put the cpassword on this file flag.rb.
We launched this one : `ruby flag.rb`

We found the solution : **LocalRoot!**
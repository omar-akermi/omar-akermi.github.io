---
layout: post
title: Tryhackme - Rick And Morty
categories: [Write-Ups, THM]
tags: [Write-ups, THM]
featured-image:  rickandmorty/logo.jpg
featured-image-alt: Rick and Morty
---

It's a write-up about the Tryhackme room : [Rick and Morty]
	
## 1. Challenge

This Rick and Morty themed challenge requires you to exploit a webserver to find 3 ingredients that will help Rick make his potion to transform himself back into a human from a pickle.

## 2. Solution


First thing let's run a scan on the IP we got

for this i've used nmapAutomator 

![nmapAutomator scan](/assets/img/rickandmorty/1.jpg)

let's go ahead to the 80 port and check the website

![site](/assets/img/rickandmorty/2.jpg)

there's nothing interesting here so let's take a look on the webpage source code

![Source code](/assets/img/rickandmorty/3.jpg)

that's great we have found the username

```
Username: R1ckRul3s
```

but we can't see any login form 

let's continue our enumeration

for this im going to use dirsearch

![dirsearch](/assets/img/rickandmorty/4.jpg)

okay let's check the robots.txt

![robots.txt](/assets/img/rickandmorty/5.jpg)

hmmm i'll keep that in mind

let's go and check /assets/

![assets](/assets/img/rickandmorty/6.jpg)

i guess nothing is important here

let's go to the login.php page

![login.php](/assets/img/rickandmorty/7.jpg)

let's use the username we found earlier and for the password i'll go with that text we found in robots.txt

![logged](/assets/img/rickandmorty/8.jpg)

okay we are in

let's try to create a reverse shell through it

![shell](/assets/img/rickandmorty/9.jpg)

okay we are able to get our shell

i've ran ```ls``` and we got a suspicious file name

Sup3rS3cretPickl3Ingred.txt

when we cat that file we find our first flag

then let's cat the ```clue.txt```
```
$cat clue.txt

Look around the file system for the other ingredient.
```
if we play around in the system we'll find the second flag in ```/home/rick```
```
$cat 'second ingredients'

**!REDACTED!**
```
okay we still have to find the third flag now

let's check our current user

```
$whoami

www-var
```

can we preform a preform a simple PE ?

let's check our sudo permissions

![sudo -l](/assets/img/rickandmorty/10.jpg)

let's go root then check the /root directory

```
$sudo su
$ls /root
3rd.txt
snap
```

finally let's cat our 3rd flag
```
$cat /root/3rd.txt
!REDACTED!
```

i guess that's it for this room.
Peace.
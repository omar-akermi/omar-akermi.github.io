---
layout: post
title: Ringzer0CTF Bash Jail 3
categories: [Write-Ups, Ringzer0CTF, Jail-Escaping]
tags: [Ringzer0CTF, Jail Escaping]
featured-image:  ringzer0CTF/icon.png
featured-image-alt: ringzer0CTF
---

It's a write-up about the challenge : [Ringzer0CTF - Bash Jail 3](https://ringzer0ctf.com/challenges/31)

# Challenge 31 - Bash Jail 3

## 1. Challenge

After login into the level3 with this command : 
`ssh -l level3 -p 10220 challenges.ringzer0team.com`
and this password : FLAG-a78i8TFD60z3825292rJ9JK12gIyVI5P

```
RingZer0 Team Online CTF

BASH Jail Level 3:
Current user is uid=1002(level3) gid=1002(level3) groups=1002(level3)

Flag is located at /home/level3/flag.txt

Challenge bash code:
-----------------------------

WARNING: this prompt is launched using ./prompt.sh 2>/dev/null

# CHALLENGE

function check_space {
	if [[ $1 == *[bdksc]* ]]
	then 	
    		return 0
	fi

	return 1
}

while :
do
	echo "Your input:"
	read input
	if check_space "$input" 
	then
		echo -e '\033[0;31mRestricted characters has been used\033[0m'
	else
		output=`$input` &>/dev/null
		echo "Command executed"
	fi
done 

-----------------------------
Your input:
```

## 2. Solution

The script threw stdout and stder to /dev/null. 
Even if the function check_space didn't accept some charcacters, we could use the command : `eval uniq flag.txt`

It exists three file descriptors : stdout (1), stderr (2) and stdin (0)
We could pass the filter with a pipe to stdin. 

So we wrote : `eval uniq flag.txt >&0`.
The uniq command in Linux is a command line utility that reports or filters out the repeated lines in a file.

The flag is : **FLAG-s9wXyc9WKx1X6N9G68fCR0M78sx09D3j**
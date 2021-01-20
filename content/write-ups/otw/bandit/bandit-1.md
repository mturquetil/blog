+++
title = "Bandit1"
authorTwitter = "" #do not include @
cover = ""
date = "2020-01-15"
tags = ["linux", "cli"]
keywords = ["", ""]
description = "Read a file called '-' without being interpreted by the shell"
showFullContent = true
isTitle = true
+++

# Access level
```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
# Password: boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

# Level goal
Read the password stored in a file called "-" located in the home directory.

# Explanation

Approaching this challenge, the common reflex would be to simply:

```bash
bandit1@bandit:~$ cat -
text
text
```

Wow! Why do my **cat** command doesn't terminate and when I'm typing its just repeating it ?
</br>
</br>
As we will often see in Bandit wargames, answer will be found in the man page.

```bash
CAT(1)                                                                    User Commands

NAME
       cat - concatenate files and print on the standard output

SYNOPSIS
       cat [OPTION]... [FILE]...

DESCRIPTION
       Concatenate FILE(s) to standard output.

       With no FILE, or when FILE is -, read standard input.
```

This means **cat** will display what you're typing on your keyboard and submitting. (more info on <a href="https://tldp.org/LDP/abs/html/io-redirection.html">IO Redirection</a>)

Knowing this, we have to find a way to provide our file "-" without it being interpreted by the shell.
</br>
</br>
The trick is to prefix it with "./". When the shell will encounter "./-" the block will be considered as a PATH and directly passed to **cat** without being modified.

```bash
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```

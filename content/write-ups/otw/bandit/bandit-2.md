+++
title = "Bandit2"
authorTwitter = "" #do not include @
cover = ""
date = "2020-01-15"
tags = ["linux", "cli"]
keywords = ["", ""]
description = "Read a file containing spaces"
showFullContent = true
isTitle = true
+++

# Access level
```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
# Password: CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```

# Level goal
Read the password stored in a file containing spaces located in the home directory.
The filename is "spaces in this filename"

# Explanation

Similarly to the previous challenge, we will use **cat** to get the password.

The problem with the filename having spaces is that the shell interpret each word as a different argument for the command.

```bash
bandit2@bandit:~$ cat spaces in this filename
cat: spaces: No such file or directory
cat: in: No such file or directory
cat: this: No such file or directory
cat: filename: No such file or directory
```

There's various ways to solve this, unfortunately for the "challenge", the autocompletion now detect spaces in a filename we're trying to open in the **cwd** (current working directory) and prefix it with "\\" as follow.

```bash
bandit2@bandit:~$ cat spaces\ in\ this\ filename
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK

# Another way
bandit2@bandit:~$ cat "spaces in this filename"
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

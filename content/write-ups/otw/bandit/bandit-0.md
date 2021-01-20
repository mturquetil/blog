+++
title = "Bandit0"
authorTwitter = "" #do not include @
cover = ""
date = "2020-01-15"
tags = ["linux", "cli", "ssh"]
keywords = ["", ""]
description = "A challenge to learn using SSH to connect to a remote host"
showFullContent = true
isTitle = true
+++

# Level goal
Connect to **bandit.labs.overthewire.org**, on port **2220** with username and password: **bandit0**.<br>
Then get the password stored in the file called **readme**.
</br>
</br>

# RTFM (Read The F*cking Manual)
Bandit wargame has for goal to get you familiar with Linux basic commands. Therefore, to be autonomous and flourish with Linux commands you have to learn using the manual describing how these works.

After a quick look at the man page:
```bash
man ssh
```

We come up with this command layout:

```bash
ssh [username]@[host] -p [port]
```

Replacing with the data provided in the challenge:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

We then just have to read the password using the **cat** command.
```bash
bandit0@bandit:~$ cat readme
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

Here it is for the first step into Linux commands usage! See you soon for bandit1!

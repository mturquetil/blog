+++
title = "Leviathan0"
authorTwitter = "" #do not include @
date = "2020-01-15"
cover = ""
tags = ["linux"]
description = "Finding informations on a remote machine"
showFullContent = true
isTitle = true
+++

# Access level
```bash
ssh leviathan0@leviathan.labs.overthewire.org -p 2223
# Password: leviathan0
```

# Level goal
Find the password to access next level on the leviathan0 instance.

# Explanation

We start inspecting what file is usable on leviathan0 user, using **ls** we see below a *.backup* folder.</br>

```bash
leviathan0@leviathan:~$ ls -a
.  ..  .backup  .bash_logout  .bashrc  .profile

leviathan0@leviathan:~/.backup$ ls
.  ..  bookmarks.html
```

Inside of it there's a bookmarks.html file.</br></br>
We can pretend the password may be located in this file and use **grep** to find something interesting.</br>

```bash
leviathan0@leviathan:~$ grep "password" bookmarks.html
<DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is rioGegei8m" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
```

Password is **rioGegei8m**.

I promise, next challenges will be more gripping 😃.

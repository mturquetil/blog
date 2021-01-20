+++
title = "Natas1"
authorTwitter = "" #do not include @
cover = ""
date = "2020-01-15"
tags = ["web", "devtools"]
description = "A challenge to learn inspect a website source code"
showFullContent = true
isTitle = true
+++

# Credentials
URL: **http://natas1.natas.labs.overthewire.org**</br>
Username: **natas1**</br>
Password: **gtVrDuiDfck831PqWsLEZy5gyDz1clto**</br>

# Level goal
Find the password on the webpage

# Explanation 
This level is similar to the previous one, the only difference is the blocking of the right-click

Looking at the source code we got:

```html
<body>
	<h1>natas1</h1>
	<div id="content">
		You can find the password for the next level on this page.
		<!--The password for natas2 is ZluruAthQk7Q2MqmDeTiUij2ZvWy2mBi -->
	</div>
</body>
```

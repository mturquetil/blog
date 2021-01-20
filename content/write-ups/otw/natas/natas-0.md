+++
title = "Natas0"
authorTwitter = "" #do not include @
cover = ""
date = "2020-01-15"
tags = ["web", "devtools"]
description = "A challenge to learn inspect a website source code"
showFullContent = true
isTitle = true
+++

# Credentials
URL: **http://natas0.natas.labs.overthewire.org**</br>
Username: **natas0**</br>
Password: **natas0**</br>

# Level goal
Find the password on the webpage

# Explanation 

On first sight, there's nothing visible on the page itself even when you try to highlight text.

Maybe we can find something in the source code 🤐.

Looking inside the body tag:
```html
<body>
	<h1>natas0</h1>
	<div id="content">
		You can find the password for the next level on this page.
		<!--The password for natas1 is gtVrDuiDfck831PqWsLEZy5gyDz1clto -->
	</div>
</body>
```

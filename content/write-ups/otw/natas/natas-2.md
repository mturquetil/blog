+++
title = "Natas2 - Still easy shit"
date = "2016-08-20T14:15:59-06:00"
author = "Maxime"
authorTwitter = "" #do not include @
cover = ""
tags = ["perl", "lfi"]
keywords = ["", ""]
description = "Description of my post"
showFullContent = false
+++


# Natas29 - Exploit
```javascript
var languageItems = [];
	var languages = components.languages;
	var count = 0;
	for (var id in languages) {
		if (id == 'meta') {
			continue;
		}
		count++;

		var lang = languages[id];
		var name = lang.title || lang;

		var contents = [
			name,
			' - ',
			{
				tag: 'code',
				contents: id
			}
		];

		var alias = lang.alias;
		if (typeof alias === 'string')
			alias = [alias];

		if (alias) {
			for (var i = 0, l = alias.length; i < l; i++) {
				contents.push(
					', ',
					{
						tag: 'code',
						contents: alias[i]
					});
			}
		}

		languageItems.push({
			tag: 'li',
			attributes: {
				'data-id': id
			},
			contents: contents
		});
	}
	$u.element.create('ul', {
		contents: languageItems,
		inside: '#languages-list'
	});
	$u.element.contents($('#languages-list-count'), count);
```


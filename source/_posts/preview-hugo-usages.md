---
title: hugo usages
draft: false
tags:
  - hugo usages
categories:
  - Even theme preview
hiddenFromHomePage: true
abbrlink: 959d1b61
date: 2017-08-30 13:56:44
lastmod: 2018-03-23 13:56:44
---

### Add Some Content 
```bash
add an article [my-first-post.md] to folder [post]
$ hugo new post/my-first-post.md

add a new menu [about]
$ hugo new about.md
```


Edit the newly created content file if you want. Now, start the Hugo server with drafts enabled:
```bash
$ hugo server -D

Started building sites ...
Built site for language en:
1 of 1 draft rendered
0 future content
0 expired content
1 regular pages created
8 other pages created
0 non-page files copied
1 paginator pages created
0 categories created
0 tags created
total in 18 ms
Watching for changes in /Users/bep/sites/quickstart/{data,content,layouts,static,themes}
Serving pages from memory
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```
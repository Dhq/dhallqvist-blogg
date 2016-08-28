---
layout: post
title:  "Elm package install \"Access is denied\" error on Windows"
date:   2016-05-28
---

So this weekend I decided to get try out the Elm programming language. After having watched a bunch of presentations from enthusiastic people and getting psyched up, I began setting it up on my windows machine. 
I followed the [installation guide](http://elm-lang.org/install) and so far so good. 
But when I started to go through the tutorials on [elm-tutorial](http://www.elm-tutorial.org/en/01-foundations/01-hello.html) 
I ran into a brick wall immidietely when trying to install the first package: `elm package install elm-lang/html`. The error I got was: 

```
elm-package.exe: elm-lang-virtual-dom-7bdfbb4: MoveFileEx 
"elm-lang-virtual-dom-7bdfbb4" "elm-lang\\virtual-dom\\1.1.1": 
permission denied (Access is denied.)
```

After a bunch of googling, ripping my hair out and the Elm-hype declining with drastic speed, it turned out that it was my **McAfee anti-virus live-scanning** that somehow blocked this operation.
You can turn it off by opening McAfee > Virus and Spyware protection > Real-Time scanning off.
So, it turned out this wasn't Elm's fault at all, phew.
All is forgiven and the hype is at a max again! I have actually had this problem before, some cli-tool didn't work because of McAfee's live scanning.
But seeing as McAfee often comes pre-installed on PC's and usually goes pretty unnoticed, it's not the first thing you suspect for errors like these.

So, putting this here to potentially save someone else from this frustration. Now: Back to Elming... 

+++
title = "Hugo (static site generator) gui"
draft = false
date = "2017-08-04T21:22:02+01:00"
tags = ["hugo", "static site generator", "vuejs", "electron", "frontend", "GUI", "desktop"]
+++

I'm very upset that my writing frequency is so low. I'd love to write more, because I found out that writing about something is the best way to learn it. There is so much tremendous technologies, concepts to learn. So I asked myself a question:

### What stops me from writing more often ?

Let's put apart laziness and the fact that English is not my primary language. After careful consideration I realized that one of very important factor is **flow** which is related to **writing** process in my current technology stack (Hugo static site generator, no CMS). Since I keep my files local it looks like this:
* let's run Hugo serve
* now, what's the command for adding post? I know it's freaking trivial, but still can't remember it! Screw it, let's copy previous one and change date etc.
* open sublime/webstorm - write as long as I need to use *hyperlink* or insert image in Markdown. Google for "link in markdown"
* preview changes, correct mistakes, typo, write
* build with hugo
* open FileZilla, or other WinSCP and deploy

Wrapping up - I needed 3 programs (4 including the browser:). It doesn't sound very appealing.. And for some time I even miss my old Wordpress CMS, where I can login, click "create new post" button and 3 seconds later it's visible on the web! It's so much easier and enjoyable. I should focus on a content, not on my editor which doesn't support Markdown very well. I know that there are folks who loves terminal, and Vim or emacs is their first-choice for everything. But I'm not one of them.

### Exploration
I started to *Googling* for some kind of GUI editor for writing in Hugo. Some simple desktop application with options like serve with one click, build with one click... This definitely should make my life easier with Hugo and take away all excuses :) No luck! If I don't want to integrate with Github or keep everything in some cloud, or maybe switch completely to use some CMS - there is no interesting desktop app. 
There is official site in documentation related to [hugo frontends](https://gohugo.io/tools/frontends/)
So taking a quick glimpse: 

* [enwrite](https://github.com/zzamboni/enwrite) is tightly coupled with Evernote - drops out
* [filemanager](https://github.com/hacdias/filemanager) is a... weell ugh, file manager
* [lipi](https://github.com/SohanChy/Lipi) is closest to my needs, but It's so ugly ;)

### So I decided to take matters into my own hands!
Why ? Since I'm beloved fan of Javascript, and I haven't had chance to use [electron](https://electron.atom.io) with vue.js yet... My requirements are following:

* Desktop app (no internet connection required to use it)
* As a main feature: it should allow user to edit his local Hugo content *\*.md* files
* It should have easy, modern look&feel
* It should provide good markdown writing experience (with buttons for text formating: bold, list, link etc). I'm looking at Ghost CMS right now, and I'm blown away by it's beauty and simplicity
* ~~It should display basic statistics: number of posts, number of words in post~~  - let's keep it as simple as possible for v1.0! Todo after successful 1.0 release
* ~~should allow to deploy files to my hosting with one click~~ - not in 1.0 release. Todo after successful 1.0 release
* should mark somehow draft's and allow to make them publish with one click
* there should be UI for editing date, tags, author and other meta data
* UI buttons for building/serving page with Hugo


Ahhh, yes. And I gave it a bit fancy name: **[Huvue](http://https://github.com/czytelny/huvue)** (as a **Hu**go + **vue**js). I thought it's funny :) 
It's just on the beginning of the road. I hope that next blog post I'll write down in my own editor :)

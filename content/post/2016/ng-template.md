+++
date = "2016-03-25T21:26:44+01:00"
tags = ["angular", "javascript", "angular-templates"]
title = "angular-templates - another useful tool"

+++

I'm constantly amazed how many useful tools front-end community produce. And constantly ashamed how many of them I'm not aware of. Recently I joined to a project where guys use **grunt-angular-templates**. Simple to setup, simple to use. What it does?

The idea is quite easy: by adding additional step in your build process (Grunt in this case but there is also a Gulp equivalent) it allows you to combine (optionally minify) all `*.html` templates which you usually keep in files and put them to a `$templateCache`. As a result every `ng-include` directive and other references like router `templateUrl` are loaded not via additional AJAX request, but rather from the angular cache. The profit of that solution is obvious - it's faster and decrease network traffic. For application with huge amount of separate templates it's a big profit.

Once you set it up it's easy to forget about it. That's why I'm writing it down ;)
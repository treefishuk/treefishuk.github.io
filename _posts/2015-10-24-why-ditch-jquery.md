---
layout: post
title: Why I choose to ditch JQuery
--- 

For projects I work on from this point on I am going to (where possible) ditch JQuery. There are three big factors which made me come to this decision. They are...

*Size
*Speed
*Simplicity

##Size

JQuery is about 90kb. Which isn't particularly huge, but at the same time its bigger then 0kb. 90kb wouldn't be an issue to me if I used every function contained within that 90k, but.. I don't. I probably use 10% of it at most! It's like buying a whole chocolate cake when you can only eat a single slice (or possibly two...). Why spend your precious resources on something unnecessarily large and expensive when you would be happy with just a fraction of it. 

Most of the time the functions I use are:

*Find element
*Add/Remove Class
*Get or set attribute (quite often a HTML5 data-attribute)
*Do something funky with Ajax

..and thats about it. Other then that I tend to use the built in bootstrap Javascript functions like show a modal pop-up, show a tool-tip or popover, or hide collapse a section. 

Does this require an entire library? I don't think so.

###But if you use all the bootstrap Javascript functions then you need JQuery..

That's what I thought as well...

However I cam across: [http://thednp.github.io/bootstrap.native/](http://thednp.github.io/bootstrap.native/){:target="_blank"}

...which is basically a native Javascript version of the bootstrap functions. Win!

##Speed

Vanilla/Native Javascript is quicker then JQuery at finding elements. Not by a little bit... by a mile!

Check out some speed comparisons like this one: [https://jsperf.com/jquery-vs-native-element-performance](https://jsperf.com/jquery-vs-native-element-performance){:target="_blank"}

Not having to load the 90kb library will also of course mean a speed boost for the site.

##Simplicity

This is the one I was torn about for a while, because:

*JQuery is really easy to use. (Finding elements is easy and you chain function after function together and it just works)
*JQuery gets around browser inconsistencies
*It's a faff writing 10 lines of Native Javascript when you can just write a single line of JQuery.

So what made me get over this and decide ditch it...?

Well, after watching an interesting video from a conference where Todd Motto spoke ([Watch it here](https://www.youtube.com/watch?v=keyCg253S-o){:target="_blank"}), I came across [Apollo JS](https://github.com/toddmotto/apollo){:target="_blank"}. A bunch of wrapper functions for vanilla Javascript that make adding and removing classes really easy. As easy as with JQuery. 

So then I thought, why don't I expand upon Apollo.js and make some simple helper functions to do all the things I used to do in JQuery. So I did and created [SQuery](/SQuery). It uses a JQuery like syntax, with native Javascript speed and it's small, at only 3kb.

So SQuery is:

*Small (only 3kb)
*Quick (uses native Javascript) 
*Simple (JQuery like syntax)


##Final Thoughts

Feel free to use SQuery in your projects. You can fork it and amend it as you see fit. Or alternatively give a go at making your own. I found it a very helpful learning experience. 
---
layout: post
title:  "Evercraft Clojure"
date:   2016-05-11 20:00:00 -0400
categories: posts 
disqus_id: c6f095ac-bf57-4bae-bd40-4c1622c2ceef
tags: [ kata tdd clojure functional-programming ]
---

intro

maybe a bit about environment setup. like, editing a lisp in vim is a drag. and
i don't know how the plugins work. and i'm also starting to want a git plugin.
and gunnar glasses. and the use of both screens. so that's a lot of shit to do.
also I want to build a new computer. tmux is great for blogging and working on
code- just use sessions ya dingus

1st commit - ok, so I'm going in feeling pretty dumb. I got midje setup - a
choice made primarily because I had used it in the Sax Marathon thing. Which I
still need to finish. So, I'm at that magical point where TDD can begin - I have
an editor with tests and src and I've got a terminal window that can run them.
With midje you get autotest for free which is rad. 

Ok, but now I'm runnign into a problem (which has kinda been a mental block on
me starting this) - I don't know how to model data in clojure at all. I've used
Clojure quite a bit, but only for one specific thing, and only in a small subset
of the language - for using overtone. I really haven't built large programs. 

One thing I've heard of is that you can basically use maps like a class. You can
store anything in the maps with string or symbol keys, so you can get the basic
functionality of a class just using a basic clojure data structure. In fact, you
get a lot of functionality for free - I don't have to implement a getter for
field x in a map, just use the builtin syntax to retrieve the value for my key.
the problem though is that you can't define any custom behavior - the mfirst
example that comes to mind is to balidate arguments. in the ruby kata i just
avalidated the arguments in the setter and then threw an excvpetion if they were
invalid. i think that's probably the same behavior I would wamt in CLijrue (or
is there some more idiomatic way to hand that). But you can't use a setter to do
the valuidation. I think that atoms have something to do with how you soolve
this problem so learning about that is prpbablay gonna be necessary for this
kata. I have some code and a bunch of blog so I'm gonna switch off this for now.


Ok, but one thing I don't fully understand the ramifications of is immutability
here. I guess I'm just going to have to see how that one plays out based on the
kata. 

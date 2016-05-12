---
layout: post
title:  "Fabric is Awesome"
date:   2016-05-11 20:00:00 -0400
categories: posts 
tags: [ devops fabric python ]
---

Earlier today, I tweeted this:

<!-- {% tweet https://twitter.com/EpicColeman/status/730448832031502336 %} -->


I made this tweet because I had just used a new tool to solve a problem that's
been a pain in my ass for a couple of months now, and I was pretty excited about
it. The tool in question is called [Fabric](http://www.fabfile.org/), and I was
excited mostly because of how easy Fabric made it to get done what I needed to
do.

For the last two or three months, I've been the main guy doing DevOps type work
at my current client. Our team is kind of unique at the client in that we have
our own network. As a result, we're responsible for our own infrastructure
and the configuration of the dev machines and build farm boxes for the room. Some people might think this is a pain in the ass. I think it's *awesome*. 

Anyway, the problem that I ran into today was a simple one, with an easy fix. We
have a suite of Appium tests that run against our app, and there's some code in
the build that stamps the screenshots Appium takes with the name of the feature
being run. One of the devs was trying to run the test suite locally, and got an
error, which looked like this: 

```
This installation of RMagick was configured with ImageMagick a.b.c but
ImageMagick a.b.d is in use. (RuntimeError)
```

Well, that was a pretty clear error message, and it was pretty obvious what had
gone wrong: Our automated configuration scripts, which run nightly, had updated
ImageMagick to a later version that the RMagick ruby gem knew about. The
solution was trivial: 

{% highlight bash %}
gem uninstall rmagick
gem install rmagick
{% endhighlight %}

The dev got his test suite spinning away and thanked me. Simple problem, easy
fix. DevOps guy to the rescue! 
 
Of course, just as I was turning back to my computer, a realization hit me: "If
this machine was misconfigured by the nightly update, then so are all the
others. Great." Then, a more _familiar_ thought: "It would sure be nice if there
was some way to run those commands on every machine in the room that didn't
involve ssh'ing into every box."

Today was the right day to have that thought, because I had some downtime
waiting on various processes to complete. I knew there were solutions for this
problem out there, but I was pretty sure the ones I had already heard of were
going to be more work to set up than I wanted. So I Googled around, and found a
Stack Overflow post where someone recommended Fabric.

Here's the description, right from their website: 
> Fabric is a Python (2.5-2.7) library and command-line tool for streamlining
> the use of SSH for application deployment or systems administration tasks.

I haven't used Python much, but this seemed awesome enough that I was willing to
try it out. Fabric is dirt simple: Install it (I used pip), and define functions
in `fabfile.py`. Run `fab ${function_name}` in the same directory, and Fabric
executes that function. This is cool on its own, but Fabric's api also has two
methods called `local()` and `remote()`. `local()` takes a string with a shell
command in it and executes it on the machine Fabric is running on. `remote()` is
the same, but it executes the command you pass in on every server in
`env.hosts`. 

Wait, let me repeat that - `remote()` takes a shell command in a string and
executes it on a list of servers. That's awesome. And while I'm sure a lot of
people reading this already have that ability through one piece of software or
another, _I_ didn't until today. The best part is that the tool is ridiculously
simple to use and it does its work over ssh - which means I didn't have to
install it on any machines, didn't have to add anything to the config scripts,
nothing. This is the kind of tool that gets me excited, and gets added to my
mental toolbelt immediately. It's incredibly easy to use - I was up an running
nearly immediately. In fact, even accounting for the time I spent Googling,
reading about it, and learning to use it, using Fabric _still_ took me less time
than it would have to do it manually. 

This is the entirety of the code (well, not exactly _the_ code, but a contrived
version for this post): 

{% highlight python %}
# fabfile.py

__future__ import with_statement
from fabric.api import local, remote

with open("hosts") as f:
    env.hosts = f.readlines()

def fix_gem():
    remote("gem uninstall rmagick") 
    remote("gem install rmagick")
    
{% endhighlight %}


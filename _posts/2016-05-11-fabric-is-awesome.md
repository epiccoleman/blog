---
layout: post
title:  "Fabric is Awesome"
date:   2016-05-11 20:00:00 -0400
categories: posts 
tags: [ devops fabric python ]
---

Earlier today, I made [this tweet](https://twitter.com/EpicColeman/status/730448832031502336)  because I had just used a new tool to solve a problem that's
been a pain in my ass for a couple of months now, and I was pretty excited about
it. The tool in question is called [Fabric](http://www.fabfile.org/), and I was
excited mostly because of how easy Fabric made it to fix my issue.

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
_his_ machine was misconfigured by the nightly update... then so are all the
others. Great." Then, a more _familiar_ thought: "It would sure be nice if there
was some way to run those commands on every machine in the room that didn't
involve ssh'ing into every box."

Today was the right day to have that thought, because I had some downtime
waiting on some builds. I knew there were solutions for this
problem out there, but I was pretty sure the ones I had already heard of were
going to be more work to set up than I wanted. I _could_ have run those commands as part of the nightly configuration update, but then I would have had to wait until the next day for it to be fixed everywhere, and since I only would have had to do it once, I would have had to remember to take that code out of the config repo the next morning. Suboptimal.

So I Googled around, and found a Stack Overflow post where someone recommended Fabric. Here's the description, right from their website:
 
> Fabric is a Python (2.5-2.7) library and command-line tool for streamlining
> the use of SSH for application deployment or systems administration tasks.

I haven't used Python much, but this seemed simple enough that I was willing to
try it out. Fabric is dirt simple: 

* Install it (I used `pip install fabric`),
* define functions in `fabfile.py`. 
* Run `fab ${function_name}` in the same directory, and Fabric executes that function.

This is pretty cool on its own, but Fabric's real magic comes from two
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
nothing. 

This is the kind of tool that gets me excited, and gets added to my
toolbelt immediately. It's incredibly easy to use - I was up and running
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

That's it. You run `fab fix_gem` and it just happily spins off, connecting to
each of the servers in env.hosts and running the commands on them. I read in the
server list from a file in the directory, which just has hostnames on each line.
I wouldn't be surprised if there's something built in to Fabric to make that
easier, but even so - it's 5 lines of code. 

For something so useful, that's pretty awesome. Plus, Fabric is loaded with
features, probably far beyond what I'll ever use. Python is a great language for
this kind of thing - I'll definitely be thinking about it in the future when I'm
writing scripts that are too complex for bash. If you're interested in it, go
check out their [tutorial](http://docs.fabfile.org/en/1.11/tutorial.html) -
reading this gave me all the information that I needed to get up and running.
All in all, this feels like something that I'll continue to find useful for a
long time, and I'm pretty excited about Fabric. 

As cool as Fabric is, it's worth mentioning that even in my minimal example
there are some caveats. First, my `hosts` file is a mix of hostnames and IPs. 
The reason for this is that the only machines whose IPs are mapped to hostnames 
in `/etc/hosts` are our CI boxes, which are the only machines with static IPs. 
This is fine, but since the IPs on the dev workstations can change (they don't
much, but they _can_), my list isn't set in stone. In fact, I ran into this that same day - after copying a list of the IPs that I had and running my command, I got a timeout from an ip that was no longer pointing to anything. That pointed out another
problem: by default, Fabric just errors out whenever it encounters a non-zero exit code, which means that if you hit an error halfway down the list, you'll have to go fix your hosts file and try again.

These two problems are pretty easy to solve - respectively, set up static ips, and handle the
failure exception from Fabric. I won't get into that here, but it's worth noting. 
Either way, Fabric is pretty damn cool.

I'm planning on getting Disqus comments up and running shortly, and will probably 
do a blog post about setting up this blog in the near future. I've also got 
another kata post in the works, and am planning a post about my dev system's
configuration. Stay tuned for all that and more, and thanks for reading.

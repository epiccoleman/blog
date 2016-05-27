---
layout: post
title:  "My Environment"
date:   2016-05-18 20:00:00 -0400
categories: posts 
disqus_id: 4382f2e8-e297-4e0c-8358-ab410e2e6248
tags: []
---

I have to admit, I've been looking forward to this post since before I even had
a blog. I'm a configuration nerd, and  screwing around with my environment and my toolsis one of my favorite parts of writing code. There's just something about getting everything adjusted _just_ right that gives me a little high REST OF INTRO

As much fun as the tweaking itself is, there's a specific goal that I'm going
for when I'm setting up my environment. Basically, my goal is to remove as much
friction from the process of writing code as I possibly can. The reason is
simple - I _suck_ at focus. 

How you get code
from your brain onto the screen is a huge part of this, but writing code entails
more than just getting text into the editor. Think of all the other things
you're managing while you write code: version control, building the code,
testing the code, running a server, or maybe an emulator, running a web browser
- you get the idea. That's a lot to manage, and when you haven't invested time
  in making things easy for yourself, switching contexts between all these
things can put a heavy load on your brain. 



I have to admit, I've been looking forward to writing this post since before
I even had a blog. I'm always screwing around with my environment and my
tools - honestly, it might be my favorite thing about being a developer. Lately,
it seems like whenever I start a new project, I wind up spending a bunch of time
setting up my environment, and half the time I wind up never writing any code.
I can't help it though - there's this little high I get when all the pieces just
click into place, and I'm always tweaking here and there to get things _juuust_
right. 

I enjoy configuring machines - it's why I like DevOps work - but I'm usually not
just configuring for fun. I have a specific goal in mind whenever I'm tweaking
my various dotfiles and plugins. My goal is twofold. The first part is to make
it as easy as possible for myself to stay focused on whatever I'm working on.
Focus is something I really struggle with - I'm easily distracted, and I tend to
go off on tangents, or Googling some elusive shortcut. The second goal is
similar, but approaches the problem from the other side - I want it to be as
easy as possible to drop what I'm doing and pick it back up. Minimizing context
switching is a discipline of mindfulness that I haven't quite mastered, but if
I can make it as easy as possible to reengage in a particular task, I can
minimize the impact while I continue to work on the mindfulness bit. My goal with this post is to get a high level overview of some of the tools
I use and how they're helping me to achieve those two goals. I'll likely go
deeper into each of these tools and more in subsequent posts. 

For the last several months, I've been doing most of my at-home development work on a Xubuntu 15.04 VM running on my desktop PC. I've become a big fan of Xubuntu as a
no frills development OS - it's got all the good stuff about Ubuntu with a much
lighter weight GUI and window manager. Having a minimalistic GUI is totally fine
with me, as I do pretty much all of my coding (and writing) in the terminal.
I run my shell in Terminator, which I've heard described as "iTerm 2 for Linux."
I don't really use its features much, as I get a lot of the functionality from
the tools I use. Running a dev environment inside a VM has a lot of upsides
- it's easy to backup and move around, and you don't have to mess around with
  dual booting. There's limitations introduced by having to share resources with
the host machine, but since I'm just running text based terminal apps for 99% of
my activity in the VM, I haven't felt much pain from this. Long story short, when I'm working on programming related things, pretty much
all the magic is happening inside the terminal. Let's get to the good stuff and talk a little bit about
some of the tools I use.

## zsh
I'm a very new convert to zsh, but I'm already a big fan. Zsh is a very full
featured shell that I use instead of bash. As a new user of zsh, I haven't
delved that deeply into the features that differentiate it from bash, but the
defaults are already an improvement. Just out of the box, it has better tab
completion and recursive path completion, which were the features that got me to
switch over. 

## tmux
Tmux is another tool that I'm a relative newcomer too, but it's already made big
impacts on the way I work. One of the things that first drew me to working
mostly in the terminal was the ability to split windows in iTerm 2. I first
started learning about tmux as a way to duplicate this functionality on a Linux
box. I tend to run my editor in a split on the left half of the screen, and
split the right half between two terminals. Generally, one of those terminals
will be running a REPL or auto test, and one is left reserved for running
version control commands and other miscellaneous tasks. Splitting windows isn't
all tmux is good for though - recently I've been starting to play with its
session management features as well. Tmux has this concept of 'attaching' to and
'detaching' from sessions - which basically amount to a new terminal window.
This is what I've been using to make it easy to drop in and out of different
tasks.  

## vim


---
layout: post
title:  "My Development Environment"
date:   2016-05-27 17:00:00 -0400
categories: posts 
disqus_id: 4382f2e8-e297-4e0c-8358-ab410e2e6248
tags: [ vim bash zsh homesick tmux config ]
---

I have to admit, I've been looking forward to writing this post since before
I even had a blog. I'm always screwing around with my environment and my
tools - honestly, it might be my favorite thing about being a developer. Lately,
it seems like whenever I start a new project, I wind up spending a bunch of time
setting up my environment, and sometimes I don't even end up writing any code.
I can't help it though - there's this little high I get when all the pieces just
click into place, and I'm always tweaking here and there to get things _juuust_
right. 

I enjoy configuring machines - it's one of the reasons I like DevOps work - but I'm usually not
just configuring for fun. I have a specific goal in mind whenever I'm tweaking
my various dotfiles and plugins. The goal is twofold. The first part is to make
it as easy as possible for myself to stay focused on whatever I'm working on.
Focus is something I really struggle with - I'm easily distracted, and I tend to
go off on tangents, or Googling some elusive shortcut, so getting things set up to minimize the amount of distraction is pretty helpful. The second goal is
similar, but approaches the problem from the other side - I want it to be as
easy as possible to pick up a task after I've dropped it while chasing some tangent. Minimizing context
switching is a discipline of mindfulness that I haven't quite mastered, but if
I can make it as easy as possible to reengage in a particular task, I can
minimize the impact while I continue to work on the mindfulness bit. My goal with this post is to get a high level overview of some of the tools
I use and how they're helping me to achieve those two goals. 

For the last several months, I've been doing most of my at-home development work on a Xubuntu 15.04 VM running on my desktop PC. I've become a big fan of Xubuntu as a
no frills development OS - it's got all the good stuff about Ubuntu with a much
lighter weight GUI and window manager. Having a minimalistic GUI is totally fine
with me, as I do pretty much all of my coding (and writing) in the terminal.
I run my shell in Terminator, which I've heard described as "iTerm 2 for Linux."
But I don't really use its features much, as I already get a lot of the functionality it provides from
the tools I use. 

Running a dev environment inside a VM has a lot of upsides
- it's easy to backup and move around, and you don't have to mess around with
  dual booting. There's limitations introduced by having to share resources with
the host machine, but since I'm just running text based terminal apps for 99% of
my activity in the VM, I haven't felt much pain from this. Long story short, when I'm working on programming related things, pretty much
all the magic is happening inside the terminal. Let's get to the good stuff and talk a little bit about
some of the tools I use.

## [zsh](http://www.zsh.org/)
I'm a very new convert to zsh, but I'm already a big fan. Zsh is a very full
featured shell that I use instead of bash. As a new user of zsh, I haven't
delved that deeply into the features that differentiate it from bash, but the
defaults are already an improvement. Just out of the box, it has better tab
completion and recursive path completion, which were the features that got me to
switch over. I'm not really planning on using its features in shell scripts or anything, since pure bash is more portable, and if I need something more complex than what I can get from bash, I'm more likely to reach for Ruby or Python than zsh features. zsh also has great customizability - I'm using [Pure zsh prompt](https://github.com/sindresorhus/pure) which is very simple and looks great. If you're interested in zsh, you could also look into [oh my zsh](https://github.com/robbyrussell/oh-my-zsh), which you can use to manage your zsh configuration easily.

## [tmux](https://tmux.github.io/)
Tmux is another tool that I'm a relative newcomer too, but it's already made big
impacts on the way I work. One of the things that first drew me to working
mostly in the terminal was the ability to split windows in iTerm 2. I first
started learning about tmux as a way to duplicate this functionality on a Linux
box. I tend to run my editor in a split on the left half of the screen, and
split the right half between two terminals. Generally, one of those terminals
will be running a REPL or auto test, and one is left reserved for running
version control commands and other miscellaneous tasks. I can use shortcuts to hop between different splits without my hands having to leave the keyboard (though I'm not enough of a purist to go without mouse mode, which can be enabled easily for tmux. See my dotfiles to see an example - there's a link below in the section on Homesick.)

Splitting windows isn't
all tmux is good for though - recently I've been starting to play with its
session management features as well. Tmux has this concept of 'attaching' to and
'detaching' from sessions - which basically amount to a new terminal window.
This is really effective for making it easy to drop in and out of different
tasks. I start a session for each task I'm working on, set up my splits and various processes how I like, and then, if I get bored, have an ADD moment, or need to test something out in an environment beyond what the REPL can provide, I can just detach from my current session and spin up a new one. Currently, I have sessions for blogging, a code kata I'm working on, one that's running the Jekyll server in the background, and one where I'm screwing around with Clojure's atoms. This makes it easy for me to drop out and tune back in to something, which helps minimize the impact of context switching.

It's also worth mentioning that being able to background these sessions has been pretty useful at work. We have one process that takes a few hours, and I'll ssh into the machine, start a session, run the update, and then I can detach from it and go back to it later. Prior to discovering this, I'd wind up with a few windows on my machine that were just ssh'ed into a machine watching the output scroll by. Now, I just detach, and check in on the output every few hours. 

## [todo.txt](http://todotxt.com/)
Have you ever written a shell function like this? 

{% highlight bash %}
note() {
    echo $1 >> "~/notes.md"
}
{% endhighlight %}

Well, I have. It's pretty handy at times to have a little notes file, and, since I'm ADD and my brain runs a million miles a minute, it's pretty useful to me to be able to quickly capture an idea or note. I just type `note "is there a way to get git intergrated into vim??"` and then then go back to what I'm doing. Later, I can go through that file and process those thoughts more thoroughly. Anyway, Todo.txt is basically that, but for you todo list. I like it because it's very simple, and it's just one more tool that I can use to rein in my brain. It's got the ability to assign tasks to specific projects, and you can then filter your list on a specific project. It's the only to do list tool that's ever really worked for me, because it's simple enough that adding to it doesn't really disrupt my workflow. Anything that helps me dump out those little random thoughts is a major help in keeping me focused. I've got my `~/.todo` directory under VC, with cron jobs on my Macbook and my VM to keep things in sync. 

## [vim](http://www.vim.org/)
Alright, here's the big one. There's 1000 posts out there about all the things that are cool about vim, so I'll be brief in my rehashing. I've spent about the last 6-8 months taking a deep dive in vim, and I'm completely addicted. I got into it because when you're ssh'ed into a remote machine, it's one of your only options. The fact that it runs inside the terminal means you can bet on having access to it pretty much wherever you go. I'm a big fan of the modal text editing too - when I need to insert text, the keys type letters, and when I don't, I have a giant gamepad with buttons that perform various functions. It really lends itself to never taking your hands off the keyboard, and it enables the whole flow state thing in that way. When I started out with vim, it was probably more of a distraction than anything else - I would constantly find myself googling something or other. Now that I've gotten the basics down, I only do that when I want to add a feature or figure out the best way to do something. There's a lot of good resources out there about learning vim, so I won't talk about that here except to say that your first stop is probably already installed if you have vim - just type `vimtutor` at the terminal, and you'll be editing text like a wizard in no time. 

My customization of vim is fairly limited, but you can check out my dotfile [here](INSERT LINK). The one thing I have that I can't live without is a mapping which switches you out of insert mode when you type 'jk'. You will almost never type this when you're trying to insert text, and having the switch out of insert mode be right on the home row is worth the minor inconvenience of waiting a second after you type 'j' if you ever need to. Reaching up to escape is a non starter for keeping your hands on the home row, so this is an important mapping. The rest is really just down to what you need out of your text editor. There are tons of good plugins out there for using vim for basically anything you need to do. One place that I feel it kind of falls apart is when you're trying to manage a project with a ton of files - if I get into that situation, I'll likely switch over to the appropriate JetBrains IDE, but I don't find that happening often. I also use Vim to write these blog posts - but I'm still experimenting with the right settings for writing prose vs code. That's all I'll say about vim for now, but expect to read more in subsequent posts.

Lastly, here's a couple of the plugins I use:

* [Vim Surround](https://github.com/tpope/vim-surround) - support for surrounding text - with quotes, brackets, parens, etc.
* [Vim Fireplace](https://github.com/tpope/vim-fireplace) - builds a Clojure REPL into Vim. This is ridiculously awesome, and I really fell in love with it from screwing around with [Overtone](https://github.com/epiccoleman/pillarcon-overtone-lightning-talk). 
* [Pathogen](https://github.com/tpope/vim-pathogen) - takes all the effort out of managing vim plugins.  
* [Vim Airline](https://github.com/vim-airline/vim-airline) - enables awesome custom status lines. I haven't gotten around to setting this up in the Linux VM, but it's a staple of my Mac vim setup.

## [Homesick](https://github.com/technicalpickles/homesick)
Homesick is the last piece of my config that I want to mention. This tool is brand new for me - I just started playing with it this week, but I'm already a fan. You create a repo (which homesick calls a 'castle) and stick a directory called `home` in it. You add your dotfiles here, use homesick to pull the repo down (which is basically a git clone), and then run `homesick symlink`. Homesick then symlinks your dotfiles into your home directory. It's pretty slick, and I've been wanting to get my dotfiles under VC for a while, so I'm excited about it. It also has mechanisms for symlinking entire directories into your home directory, so I'm planning on eventually getting all my vim and todo.sh configuration into the castle as well. You can take a look at my dotfiles repo / castle [here](https://github.com/epiccoleman/dotfiles). 

Alright, well, this post has ballooned in size, so I'm going to cut it off here. This is really just a high level overview of the various tools I use on a daily basis, and I plan on diving deeper into most or all of the tools in later posts. Hope you enjoyed the little peek into my setup - feel free to leave any questions, comments, or tips in the comments, and thanks for reading!


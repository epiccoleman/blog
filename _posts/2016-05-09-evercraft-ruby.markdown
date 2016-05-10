---
layout: post
title:  "Evercraft Kata - Ruby"
date:   2016-05-09 20:00:00 -0400
categories: blog ruby kata
---

Alright, here we go. This is the first post of my blog, but I promised myself
that I wouldn't bore anyone with a whole big self-congratulatory introduction
post until I had some substance. I do have a lot of cool ideas for things to
talk about on this blog, and I'll probably get around to an introductory/coming
soon post before too long.

What I want to talk about today, however, is the code kata that I worked on last
week. My goal is to do a coding exercise along the same line roughly every week,
and then do a little writeup/retro about what I learned. I worked on the
[Evercraft Kata](https://github.com/PuttingTheDnDInTDD/EverCraft-Kata) in Ruby.
My repo is [here](https://github.com/epiccoleman/evercraft-ruby/commits/master),
if you're interested in reading - or, even better, reviewing - my code. 

I've done a little bit of Ruby here and there, but hadn't really dived
in until recently. The tooling took a little bit of getting used to. I've been
doing the majority of my at-home hacking around on a xubuntu 15.10 vm, and this
didn't have any ruby installed on it yet. From my Googling around, `rvm` seemd
like the way to go, so I got myself set up with that. I knew that Pillar (the
company I work for) had some template projects on their site, so I cloned the
[Ruby skeleton](https://github.com/PillarTechnology/ruby-skeleton) project. I
stripped out the `spec-helper` stuff, just because I didn't really know what it
did, and I wanted the most basic setup possible. 

One new thing I tried with this kata was taking some more detailed notes in my
commit log. Wouldn't do this on a real project, but it was kind of a good way to
track various questions and thoughts that I had as I was doing the kata. My
[first
commit](https://github.com/epiccoleman/evercraft-ruby/commit/436a3efb4a3bb71bf45c3f1a02bb13634aa2e1b3)
has a note where I'm wondering about the Gemfile vs the Rakefile, and what their
purposes are. I knew that a Gemfile was used by Bundler to manage dependencies,
and I knew that I could pull down whatever was specified in that file with
`bundle install`. I didn't really know anything about `rake` except that it's
Ruby's version of `make`, but a glance at the Rakefile made me think that
running `rake` would run the tests, and that was true (although the output is
horrific, learning to tweak that is definitely on my short list).

OK, so great. At this point I've been screwing around for an hour or so, but now
I have something that I know how to work with - `src/` and `test/` directories.
Well, not exactly, but `lib/` and `spec/` directories. And I've got a command to
run them. That's enough to start writing some code.

The Evercraft Kata basically fleshes out a D&D style RPG game. The first few
features are pretty much no brainers, but by about halfway through the first
iteration, I had to think a bit about how best to implement the various
features.

The first feature is dirt simple - "Character can have a name." My first test
looked like this: 

{% highlight ruby %}
  it "can set its name" do
    expected_name = "Nathan Explosion"
    subject.name = expected_name 
    expect(subject.name).to eq(expected_name)
  end
{% endhighlight %}

Which is so "basic TDD" that I wouldn't bother to mention it, but I had learned
a little Ruby trick earlier that day on CodeWars that made the implentation 
super easy:

{% highlight ruby %}
class Character
  attr_accessor :name
end
{% endhighlight %}
([commit](https://github.com/epiccoleman/evercraft-ruby/commit/e338088d03c7b88bd613aa505ede51aac5836cb0))

My college CS coursework was 90% in C++, so I'm used to having to write
accessors and mutators manually. But shit, it's 2016, and who wants to do that
anyway? 2011 me would be impressed. It was cool to have encountered this earlier
the same day and be able to put it to use so soon.

The next feature was alignment, and the first test and first pass code I wrote
for that were basically identical to the code above. The next thing I wanted to
drive in was validation for the alignment value. Valid values are "Good",
"Evil", and "Neutral". This was my first attempt at a test, but it didn't feel
quite right: 
 
{% highlight ruby %}
  it "raises ArgumentError when given an invalid alignment" do
    expect{subject.alignment = "Decent"}.to raise_error(ArgumentError)
  end
{% endhighlight %}

And here's the code I wrote to make it pass: 

{% highlight ruby %}
class Character
  attr_accessor :name
  attr_reader :alignment

  def alignment=(alignment) 
    valid_alignments = ['Good', 'Evil', 'Neutral']
    if not valid_alignments.include? alignment.capitalize
      raise ArgumentError
    end
    @alignment = alignment.capitalize
  end
end
{% endhighlight %}
([commit](https://github.com/epiccoleman/evercraft-ruby/commit/cc123322f55cf2f254d556c2647fc8d9aef87205))

You can see that I was taking a few liberties with "Only write the necessary
code to make the test pass." I fixed the lack of a test for the capitalization
of the input to the setter in the next commit, but the test didn't really feel
like it necessitated the 3 valid alignments. Here's the test I ended up using: 

{% highlight ruby %}
  it "raises ArgumentError for invalid alignment" do
    expect{subject.alignment = "Good"}.not_to raise_error
    expect{subject.alignment = "Evil"}.not_to raise_error
    expect{subject.alignment = "Neutral"}.not_to raise_error
    expect{subject.alignment = "Nice"}.to raise_error(ArgumentError)
    expect{subject.alignment = "Literally Hitler"}.to raise_error(ArgumentError)
  end
{% endhighlight %}
([commit](https://github.com/epiccoleman/evercraft-ruby/commit/89d0d8f2ed61995490fcc4c3a8a1a230c7be68fe))

I talked to a coworker (DJ), who agreed that this
style was better - the phrase he used was "executable documentation," which is
one of the things I like best about TDD. I do have a slight sticking point with
this kind of test though - I don't feel it _fully_ characterizes the _possible_
behavior of this validation. 

The test as shown only tells me 3 values that are valid, and two that aren't, so all that I know for sure from reading it are that
"Good", "Evil", and "Neutral" are valid values, and that "Nice" and "Literally
Hitler" are not. Here's where a reader needs to be able to understand the intent
of the person writing the test, and I think it's a reasonable expectation for me
to have - that the reader will get my point. But if we think about it from a pure
logic standpoint, it's incomplete. 

Is there a way to write a test condition such
that it only can pass "if these three values are the only valid values?", short
of enumerating every possbile invalid value? I'm thinking
back to proofs by math induction, which lets you  
demonstrate that a proposition holds for all values, without 
having to write out every number to do it. I'd be interested to hear anyone's
thoughts about how to make something like this logically rigorous, or why that's
silly or impossible or a bad idea. 

The last features I'd like to talk about are damage and attacking. These
required a bit more thought than the others. In the kata, attacking is listed as
a feature before damage, and works like this: if the roll passed into the attack
function beats the enemy's armor class, the attack is successful. That's three
concepts tied together - damage, attacking, and success/failure of attacks. I couldn't figure out how to drive in attacks
without having a method to damage a character. So I wrote a test for the ability
of a character to take damage:

{% highlight ruby %}
  it "can be damaged" do 
    subject.damage 1
    expect(subject.hp).to eq 4 
  end
{% endhighlight %}
([commit](https://github.com/epiccoleman/evercraft-ruby/blob/6fc3c83c5c4d25adfbf3657eb2bb64dcdcfeb7d3/spec/character_spec.rb))

But that didn't feel quite right. The problem here is that the test assumes the
reader knows that a character has a default HP of 5. Even _worse_, it assumes
that the reader knows that the `describe` whose scope this test is in doesn't
have a `before(:each)`, and even _**worser**_, it assumes the reader knows that
when that's the case rspec just instantiates a test object named `subject` with
the default initializer. 

Here's the test I settled on, which DJ agreed was a better test. It assumes
less - but this introduces another question: "what happens if hp goes below 0?"

{% highlight ruby %}
  it "loses 1 hit point when damaged by 1" do 
    expected_hp = subject.hp - 1 
    subject.damage 1
    expect(subject.hp).to eq expected_hp 
  end 
{% endhighlight %}
([commit](https://github.com/epiccoleman/evercraft-ruby/blob/89d0d8f2ed61995490fcc4c3a8a1a230c7be68fe/spec/character_spec.rb))

I can't think of a better way to write this, however. We could validate that hp
hasn't dropped below 0, but I think that would clutter the intent of the test.
_I_ know that when the test runs, the subject has 5 hp, and maybe that's enough. 

Now that I had the ability to damage a character, a test for attack becomes more
simple:

{% highlight ruby %}
it "does 1 damage to enemy when roll beats enemy armor class" do
      enemy = Character.new
      success_roll = enemy.armor_class + 1
      damaged_hp = enemy.hp - 1 

      subject.attack(enemy, success_roll)
      
      expect(enemy.hp).to eq damaged_hp 
    end
{% endhighlight %}

I was worried that this test conflates the concepts of attacking and a success
roll a bit, but following the rule of writing the minimum code to make the
test pass, my implementation looked like this: 

{% highlight ruby %}
 def attack(receiver, roll)
    receiver.damage 1
  end
{% endhighlight %}
([commit](https://github.com/epiccoleman/evercraft-ruby/commit/fece034c2f4b088e3becc77f37d8e924f7278ca1))

Which obviously doesn't meet the feature's criteria that the roll beats the
enemy. This approach forces you to write another test to be able to implement
the "attack success" feature. I think of this as the "other half" of the test:

{% highlight ruby %}
    it "does no damage when roll doesn't beat enemy armor class" do 
      enemy = Character.new 
      bad_roll = enemy.armor_class - 1 
      hp_before_attack = enemy.hp

      subject.attack(enemy, bad_roll)

      expect(enemy.hp).to eq hp_before_attack
    end
{% endhighlight %}

This allowed me to change `attack` to look like this: 
 
{% highlight ruby %}
  def attack(receiver, roll)
    if roll > receiver.armor_class
      receiver.damage 1
    end
  end
{% endhighlight %}
([commit](https://github.com/epiccoleman/evercraft-ruby/blob/c3cf74eb09e0ac20771bb29c1e31a92a0701b12e/lib/character.rb))

This is about as far as I got working a little bit each day. This is not a
lot of code, but it felt good to flex my muscle. I'm trying to get in the habit
of doing at least a commit to some repo or another daily, and this was a good
start to that. 

This post has gotten lengthier than I meant it to be, so I want to cut it off
here. I've officially earned myself the right to write an introduction post, so
keep an eye out for that. I have a lot of ideas for future articles, and will be working
with a couple of other people so that all of us can have kickass blogs in time.
Just as a teaser, here are some of the things I'm planning on talking about in
the near future:
* This same kata in Clojure, and maybe Swift
* Details about my personal code writin' setup (all the hipster tools)
* Overtone (which I've done a few
  [talks](https://github.com/epiccoleman/pillarcon-overtone-lightning-talk)
[about](https://github.com/epiccoleman/overtone-columbus-clojure) 
* bash, boxen, jenkins, docker, and other DevOps-y type stuff 
* stuff I'm trying to do to get better at being a programmer
* stuff I'm trying to do to get better at being an adult.o

If you like, follow me on
[Twitter](https://twitter.com/EpicColeman). I'm not a particularly exciting
social media user, but I plan to at least tweet whenever I write a blog post.
Stay tuned, and thanks for reading.

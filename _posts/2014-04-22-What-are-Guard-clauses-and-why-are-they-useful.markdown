---
layout: post
title:  "What Are Guard Clauses and Why Are They Useful?"
date:   2014-04-22 19:08:00
published: true
categories: Code Design Nested Conditionals
tags:
- Guard Clause
- Nested Conditionals
- Clean Code
- Maintainance
---

So, Guard clauses. If you're unaware of these beautiful statements then you're going to be pleasantly surprised. They
are going to help you write clear, concise and therefore easier to maintain code. They are also really useful in
situations where there isn't enough time to refactor code out to other classes. 

So first things first, I'm sure we're all familiar with code that has multiple leveles of nested conditionals and that
looks similar to this.

{% highlight php5 funcnamehighlighter linenos %}
<?php
public function outcome()
{
    if ($this->is28DaysLaterZombie()) {
        if ($this->attackCritical) {
            return "One shot one kill, one less in the horde!";
        } else if ($this->attackHit) {
            return "It's down, finish this!";
        } else {
            return "You got chomped, braaaaainnnns!";
        }
    } else {
        if ($this->attackCritical) {
            return "One shot one kill, one less in the horde!";
        } elseif ($this->attackHit) {
            return "It's down, finish this!";
        } else {
            return 'You escaped!';
        }
    }
}
{% endhighlight %}

Yeah the example is completely made up, I couldn't find an appropriate example from any of the code bases I have access
to so I used my love for zombie fiction to come up with something contrived. As you can see this function has a lot
going on it has much duplication and has many conditional checks that can be reduced. So lets get started.

The only differences that exist between the two legs of the function are the `else` statements. This indicates that the
zombie type check isn't the predicate it was first thought to be. Straight away we can simplify by moving the common
attack checks out side of the main `if/else` block

{% highlight php5 funcnamehighlighter linenos %}
<?php
public function outcome()
{
    if ($this->attackCritical) {
        return "One shot one kill, one less in the horde!";
    } 
    
    if ($this->attackHit) {
        return "It's down, finish this!";
    } 

    if ($this->is28DaysLaterZombie()) {
        return "You got chomped, braaaaainnnns!";
    } else {
        return 'You escaped!';
    }
}
{% endhighlight %}

So straight away we can see that we've removed a lot of duplication by moving those conditionals and combining them.
Returning early is the key to this technique. There is more that we can do though. That last `if/else` block can also
be shrunk. Almost anywhere you have an `if/else` you can remove the `else`, with the caveat that you return early in
the `if` condition.

{% highlight php5 funcnamehighlighter linenos %}
<?php
public function outcome()
{
    if ($this->attackCritical) {
        return "One shot one kill, one less in the horde!";
    } 
    
    if ($this->attackHit) {
        return "It's down, finish this!";
    } 

    if ($this->is28DaysLaterZombie()) {
        return "You got chomped, braaaaainnnns!";
    }

    return 'You escaped!';
}
{% endhighlight %}

And that's all there is too it, combine duplicate conditionals, return early, remove all the complexity of nested
conditionals. It's not always possible to do, but when it is, refactor. This process would ideally be done with tests
to ensure the refactor hasn't broken any functionality, I excluded these for the sake of brevity.

As I mentioned previously we've also realised that the type of zombie wasnt any more important than any of the other
attack types. We'd just assumed throughout the previous lifetime of that function that it mattered. One last bonus of 
this technique is it makes the function so much easier to understand, you don't have to hold two/three levels of
conditions in your memory to understand whats going on. Also we can easily add other clauses if we need to.

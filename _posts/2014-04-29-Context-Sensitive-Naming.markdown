---
layout: post
title: Context Sensitive Naming
date: 2014-04-29 14:35:00
published: true
categories:
- code style
- naming convention
tags:
- Naming Convention
- Contextual Naming
- Clean Code
---
A short post about an idea that's been knocking around the head space for a few weeks. Naming things is hard to do. It's
very difficult to get the right balance between verbosity and brevity. Too verbose and you end up with names that are
hard to comprehend, too brief and the names are meaningless.

I think that in some situations the problem of overly verbose variables and class property names can be solved by taking
into account the scope or context that the name is defined. Let me try to explain with an example.

{% highlight php5 funcnamehighlighter linenos %}
<?php

class ZombieBattle 
{
    public function zombieBattleOutcome()
    {
        //return something
    }
}
{% endhighlight %}

The method name in this example is overly verbose. We've added context to the method name where it isn't necessary. The
scope that the method sits in `ZombieBattle`, is enough. We can remove the redundancy from the method name and not worry
about losing meaning.

{% highlight php5 funcnamehighlighter linenos %}
<?php

class ZombieBattle 
{
    public function outcome()
    {
        //return something
    }
}
{% endhighlight %}

Redundancy removed. With no loss of meaning. I find it useful to think about how the calling code will look, this will
give you an idea of how much verbosity/brevity you can get away with, without sacrificing meaning. In the first example
the calling code might look like this.

{% highlight php5 funcnamehighlighter linenos %}
<?php

$zombieBattle = new ZombieBattle();
$zombieBattle->zombieBattleOutcome();

{% endhighlight %}

As you can see the duplicity of `ZombieBattle` is really highlighted when the class is instantiated in an appropriately
named variable and then the method called.

If we now see the same example with our shorter method name.

{% highlight php5 funcnamehighlighter linenos %}
<?php

$zombieBattle = new ZombieBattle();
$zombieBattle->outcome();

{% endhighlight %}

It's succinct and it's clear that the `outcome` method will give us the outcome of the `ZombieBattle`. If you have 
classes where you see this and you can't change the name, it might be caused by giving a class too much responsibility.
This would indicate that you can refactor to two classes.

{% highlight php5 funcnamehighlighter linenos %}
<?php

class ZombieBattle 
{
    public function zombieBattleOutcome()
    {
        //return something
    }

    public function vampireBattleOutcome()
    {
        //return something
    }
}
{% endhighlight %}

This is bad. This is a clear indication that you have other classes, hidden away in `ZombieBattle`. Refactoring this
to two classes would yield a result similar to this.

{% highlight php5 funcnamehighlighter linenos %}
<?php

interface Battle
{
    public function outcome();
}


class VampireBattle implements Battle
{
    public function outcome()
    {
        //return something
    }
}

class ZombieBattle implements Battle 
{
    public function outcome()
    {
        //return something
    }
}
{% endhighlight %}

You'll notice that I introduced an interface. This forces all classes that implement it to provide the methods it
specifies. You'll notice that the method name is `outcome`. Now anywhere we might want to have a `Battle` we can be
sure that we're able to get it's `outcome`.

{% highlight php5 funcnamehighlighter linenos %}
<?php

interface Battle
{
    public function outcome();
}


class VampireBattle implements Battle
{
    public function outcome()
    {
        return "Vampire Battle";
    }
}

class ZombieBattle implements Battle 
{
    public function outcome()
    {
        return "Zombie Battle";
    }
}

class War
{
    private $battles = [];

    public function addBattle(Battle $battle)
    {
        $this->battles[] = $battle;    
    }

    public function history()
    {
        foreach ($this->battles as $key => $battle) {
            echo $key++ . ": ". $battle->outcome();
        }
    }
}

$war = new War();
$war->addBattle(new ZombieBattle);
$war->addBattle(new VampireBattle);
$war->history();

{% endhighlight %}

That got out of hand quickly, from a little analysis of a method name we discovered how surrounding context can help
remove redundancy and verbosity in our names. We also looked at a possible cause of overly verbose names and 
demonstrated a possible solution to that problem.

I hope it helped.




---
layout: post
title:  "Tracking Down Bugs in Recent Commits"
date:   2015-03-12 11:17:00
published: true
categories: Code Design Nested Conditionals
tags:
- Git
- Tip
- Bug
- Maintainance
---

A really quick post just so I remember this because it helped me today.

I managed to introduce a system breaking bug by removing a logic check that looked obsolete. A check in a place that looked like a check shouldn't ever need to be made which broke a system in a place away from where I was working.

When the break was detected by another team member we couldn't figure out how it had broken everything looked like it should work and the tests passed. We spent some time looking through files trying to jog out memory on why certain things were done the way they were and came up with nothing.

In the end we decided to go back through all the recent commits to find the bug. To do this we checked out each commit in the history to see which one introduced the break. It was super easy to do and I can't belive I only just found out about it.

So if you need to checkout how the system worked in a particular commit you can try this:

{% highlight bash funcnamehighlighter linenos %}
    git checkout <commit-hash>
{% endhighlight %}

You'll get put into detached head state where you can play around with the system to see if it behaves as expected.

Once you've found your culprit and fixed it you can get back to your latest commit by using

{% highlight bash funcnamehighlighter linenos %}
    git checkout <branch>
{% endhighlight %}




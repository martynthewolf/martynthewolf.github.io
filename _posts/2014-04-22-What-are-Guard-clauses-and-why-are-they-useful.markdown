---
layout: post
title:  "What Are Guard Clauses and Why Are They Useful?"
date:   2014-04-22 19:08:00
published: false
categories: Code Design Nested Conditionals
tags:
- Guard Clause
- Nested Conditionals
- Clean Code
- Maintainance
---

So, Guard clauses. If you're unaware of these beautiful statements then you're going to be pleasantly surprised. They
are going to help you write clear, concise and therefore easier to maintain code.

So first things first, I'm sure we're all familiar with code that has multiple leveles of nested conditionals and that
look similar to this.

{% highlight php5 funcnamehighlighter linenos %}
<?php
public function threadManager($threadVO)
{
    $thread = $this->search($threadVO);

    if ($thread->getThreadPK()) {
        $threadVO->setThreadPK($thread->getThreadPK());
        return $threadVO;
    } else {
        return $this->save($threadVO);
    }
}

{% endhighlight %}

It's a simple function that should really be more than one function and should be named differently and probably many 
more problems but it's the only one I could find in my code base that would enable me to demonstrate how useful Guard
clauses are. So what we'll do is refactor this code block to get rid of the else leg, thereby removing the need for
some of the indentation. It'll also allow us to more easily determine the main use case of this function.

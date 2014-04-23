---
layout: post
title:  "What are Guard clauses and why are they useful?"
date:   2014-04-22 19:08:00
published: false
categories: Code Design Nested Conditionals
---

So, a Guard clause, if you're unaware of these beautiful statements then you're going to be pleasantly surprised. They
are going to help you write nicer looking, easier to read and therefore easier to maintain code.

So first things first, I'm sure we're all familiar with code that looks similar to this.

{% highlight php5 funcnamehighlighter linenos %}
    <?php
        class Article
        {
            public function getStatus()
            {
                if ($this->scheduled) {
                    if ($this->
                }
            }
        }
{% endhighlight %}

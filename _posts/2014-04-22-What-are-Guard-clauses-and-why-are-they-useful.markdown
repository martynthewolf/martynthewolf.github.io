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

function convertUKDateToMySQLDate($ukdate)
{
    $ukdate = trim($ukdate);
    if ($ukdate=="") {
        return NULL;
    } else {
        $day=substr($ukdate, 0, 2);
        $month=substr($ukdate, 3, 2);
        $year=substr($ukdate, 6, 4);
        
        $retVal = "$year-$month-$day";
        
        //check for time at the end of the string
        if (strlen($ukdate) > 10) {
            $time = substr($ukdate, 11);
            $retVal.= " " . $time;
        }
        return $retVal;
    }
}
{% endhighlight %}

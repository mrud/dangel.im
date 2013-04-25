---
title: Analyzing rc bug messages
categories: [debian]
layout: default
prefix: /
---


Michael Stapelberg recently posted a
[blog post](http://people.debian.org/~stapelberg//2013/03/31/analyzing-rc-bug-messages.html)
about looking into the number of Debian Developers actively working on RC
bugs for the upcoming wheezy release.

In this blog post I analyze the data shared by Michael and provide the `R`
commands used to generate the plots & findings. If you are interested into
looking into the data yourself, but don't like `R`, I suggest using ipython
notebook + numpy instead.

## Analysis

After parsing the data file we typically want to get an understanding of the
data, by using `summary(bugs)` we get the `minimum(1)`, `median(5)`,
`mean(15.4)`, `max(716)` and quantiles of the data. This shows that the
number of messages is wide-spread and a few people contribute a lot. To
visualize the dispersion of the data we can create a box plot showing  the range of messages:

![boxplot](/blog/assets/img/debian-boxplot.png)

As the first and third quantile are close together we can assume that the
majority of the work is done by a few, especially since the second quantile
is 5. This is supported by the histogram below, where the x axis is the number of recorded messages and y is the
number of developers.

![histogram](/blog/assets/img/debian-histogram.png)

## Top 10 contributors

The TOP 10 contributors, according to the dataset, are:
<ol>
<li>Lucas Nussbaum - 716 messages</li>
<li>Gregor Herrmann - 270 messages</li>
<li value="2" style='list-style-type: none'>Jakub Wilk - 270 messages</li>
<li value="4">Andreas Beckmann - 225 messages</li>
<li>Julien Cristau - 205 messages </li>
<li>Cyril Brulebois - 169 messages </li>
<li>Moritz Muehlenhoff - 162 messages</li>
<li>Michael Biebl - 159 messages</li>
<li>Salvatore Bonaccorso - 158 messages</li>
<li>Christoph Egger - 142 messages</li>
</ol>

## r commands

These are the commands used to generate the plots and information in this plot:
{% highlight r %}
bugs <- read.csv("by-msg.csv")
summary(bugs)
boxplot(bugs$rcbugmsg, log='y', range=0, ylab="# bugs")
quantile(bugs$rcbugmsg)
0%  25%  50%  75% 100%
1    2    5   12  716

# create histogram
llibrary('ggplot2')
ggplot(bugs, aes(x=rcbugmsg)) + geom_histogram(binwidth=.5, colour="black", fill="black") + scale_x_sqrt()

top10 <- tail(bugs[order(bugs$rcbugmsg),], 10)
top10
{% endhighlight %}

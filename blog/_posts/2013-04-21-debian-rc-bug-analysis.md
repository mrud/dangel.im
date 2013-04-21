---
title: Analyzing rc bug messages
categories: [debian]
layout: default

---


Michael Stapelberg recently posted a
[blog post](http://people.debian.org/~stapelberg//2013/03/31/analyzing-rc-bug-messages.html)
about looking into the number of Debian Developers actively working on RC
bugs for the upcoming wheezy release.

In this blog post I analyze the data shared by Michael and provide the R
commands used to generate the data. Instead of using R you can use any
other statistical environment such as numpy (have a look at ipython
notebook + numpy).

In order to work with the data we have to read the provided csv file. To
get an initial understanding of the distribution of the data we can create
a box plot to show the range of contributions:

![boxplot](/blog/assets/img/debian-boxplot.png)

As the first and third quantile are close together we can assume that the
majority of the work is done by a few, especially since the second quantile
is 5. This is supported by the histogram below, where the x axis is the number of recorded messages and y is the
number of developers.

![histogram](/blog/assets/img/debian-histogram.png)

## Top 10 contributors

As there is a broad range of contribution we should name the DDs who
contributed the most. Here is the TOP 10 of DDs:

1. Lucas Nussbaum - 716 messages
2. Gregor Herrmann & Jakub Wilk - 270 messages
3. Andreas Beckmann - 225 messages
4. Julien Cristau - 205 messages
5. Cyril Brulebois - 169 messages
6. Moritz Muehlenhoff - 162 messages
7. Michael Biebl - 159 messages
8. Salvatore Bonaccorso - 158 messages
9. Christoph Egger - 142 messages


## r commands

These are the commands used to generate the plots and information in this plot:
{% highlight r %}
bugs <- read.csv("by-msg.csv")
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

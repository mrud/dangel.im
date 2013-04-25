---
title: Visualizing DPL votes
categories: [debian]
layout: default
---

[Axel 'XTaran' Beckert](http://noone.org/blog) recently asked in `#debian-devel` for a visualization
of the
[Debian Project Leader Election results](https://lists.debian.org/debian-devel-announce/2013/04/msg00004.html).
Unfortunately there seems to be none, except the table in the mail. As  I
am currently trying to use `ggplot2` for more things, I thought I would
give it a try and convert the data into a [csv file](/blog/assets/data/dpl-2013.csv) and process the data via `R`.

For converting the
[original data](https://lists.debian.org/debian-devel-announce/2013/04/msg00004.html)
i used `vim` to do a little text processing and created a
[csv file](/blog/assets/data/dpl-2013.csv). By running the following code
we can create a nice looking simple graphic:

{% highlight r %}

library("ggplot2")
votes <- read.csv("dpl-2013.csv")
votes2 <- melt(subset(votes, select=c("Year", "DDs", "unique")), id="Year")

p <- ggplot(votes2, aes(Year, value, group=variable, fill=variable)) + ylab('DD #')
p <- p + geom_point() + geom_line() + geom_area(position='identity')
p <- p + scale_fill_manual("Values", values=c(alpha('#d70a53', 0.5),
alpha("blue", 0.5)), breaks=c("DDs", "unique"), labels=c("Developer count", "Voters"))

print(p)

{% endhighlight %}

![DPL Votes 2013](/blog/assets/img/dpl-2013.png)

I also created a [little script](https://gist.github.com/mrud/5455016) for
automatically processing the csv file and creating a similar plot. Feel
free to fork/clone extend this script.

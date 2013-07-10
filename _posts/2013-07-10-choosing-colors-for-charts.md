---
title: Choosing chart colors with R
layout: default
---

Every time I create charts for visualisation I have a problem choosing
appropriate colors for the data. Ever since I started to use
[`ggplot2`](http://ggplot2.org/) the problem went away as `ggplot2` tries best
to choose appropriate color, and has a wide range of
[available color themes](http://blog.ggplot2.org/post/24607351280/choosing-colour-palettes-part-ii-educated-choices).

Unfortunately you can't use `ggplot2` all the time, e.g. when you want to plot
a dendrogram or use another platform than R. While looking for a way
to color my dendrogram with the color choices from `ggplot2`, I discovered
the [`scales`](http://cran.r-project.org/web/packages/scales/index.html)
package. `scales` can be easily used to get the colors from `ggplot2`,
determine breaks for axes or labels.

The following codes plots the colors, with `brewer_pal()` returning a vector
of the requested colors and `show_col()` plots them.

Diverging colors to express similarity in the middle but extreme cases at
the beginning and the end:
{% highlight r %}
library('scales')
show_col(brewer_pal('div')(6))
{% endhighlight %}
![color-divider](/blog/assets/img/colors-div.png)


Qualitative pattern to distinguish between different classes:

{% highlight r %}
show_col(brewer_pal('qual')(6))
{% endhighlight %}
![color-divider](/blog/assets/img/colors-qual.png)


and last but not least, a sequential color palette to express progressing data:
{% highlight r %}
show_col(brewer_pal('seq')(6))
{% endhighlight %}

![color-divider](/blog/assets/img/colors-seq.png)

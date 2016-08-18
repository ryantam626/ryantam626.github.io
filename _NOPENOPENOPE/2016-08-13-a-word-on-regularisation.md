---
layout: post
title:  "Dafuq Is Overfitting?"
date:   2016-08-13
category:
  - Machine Learning
tags:
  - Dafuq
  - Terminology
---



Overfitting, a term to discribe the situation where a model describes random error/noise in the data on top of modelling the underlying relationship within the data. 

Model that has been overfitted usually has a poor predictive performance on new unseen data, because if the error/noise is truly random, chances are that you are never going to get similar instances as the training data.

We now dive into a bit of R code to have see the effects of overfitting in a very simple example.

We first load the library for plotting,


{% highlight r %}
library(ggplot2)
{% endhighlight %}

The problem is to find a function to model $$\sin{x}$$ with some additive white noise (Gaussian) in the range of $$(-3.2, 3.2)$$, we generate some data of the underlying function ($$\sin{x}$$) for plotting first,


{% highlight r %}
sin.x <- seq(-3.2,3.2,0.05)
sin.y <- sin(sin.x)
sin.df <- data.frame(x=sin.x, y=sin.y)
{% endhighlight %}




## Including Plots

You can also embed plots, for example:

![plot of chunk pressure](/assets/Rfig/pressure-1.svg)

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.

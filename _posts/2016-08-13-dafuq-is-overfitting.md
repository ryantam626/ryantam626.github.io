---
layout: post
title:  "Dafuq Is Overfitting? [DRAFT]"
date:   2016-08-13
category:
  - Machine Learning
tags:
  - Dafuq
  - Terminology

---



Overfitting, a term to discribe the situation where a model describes random error/noise in the data on top of modelling the underlying relationship within the data. 

Model that has been overfitted usually has a poor predictive performance on new unseen data, because if the error/noise is truly random, chances are that you are never going to get similar instances as the training data.

## Example

We now dive into a bit of R code to have see the effects of overfitting in a very simple example.

We first load the library for plotting:-


{% highlight r %}
library(ggplot2)
{% endhighlight %}

The problem is to find a function to model $$\sin{x}$$ with some additive white noise (Gaussian) in the range of $$(-3.2, 3.2)$$, we generate some data of the underlying function $$(\sin{x})$$ for plotting first:-


{% highlight r %}
sin.x <- seq(-3.2,3.2,0.05)
sin.y <- sin(sin.x)
sin.df <- data.frame(x=sin.x, y=sin.y)
{% endhighlight %}

Then we produce some data for trianing:-

{% highlight r %}
generate_train_data <- function(size){
  x <- seq(-3.2, 3.2, 6.4/size) # get a range of x
  y <- sin(x) + rnorm(length(x), 0, sqrt(0.04)) # simulate the corresponding y with error
  return(data.frame(x=x,y=y))
}

set.seed(888)
train.df <- generate_train_data(9)
{% endhighlight %}

We first fit a linear equation to fit the function:-

{% highlight r %}
linear.model <- lm(y ~ x, data=train.df)
linear.model.pred <- predict(linear.model, sin.df)
linear.model.pred.df <- data.frame(x=sin.df$x, y=linear.model.pred, order=1)
{% endhighlight %}

The residual sum of squares (RSS) is,

{% highlight r %}
sum(linear.model$residuals^2)
{% endhighlight %}



{% highlight text %}
## [1] 2.066533
{% endhighlight %}

Then fit a third, sixth and nineth order equation similarly:-

{% highlight r %}
third.order.model <- lm(y ~ poly(x, 3, raw=TRUE), data=train.df)
third.order.model.pred <- predict(third.order.model, sin.df)
third.order.model.pred.df <- data.frame(x=sin.df$x, y=third.order.model.pred, order=3)

sixth.order.model <- lm(y ~ poly(x, 6, raw=TRUE), data=train.df)
sixth.order.model.pred <- predict(sixth.order.model, sin.df)
sixth.order.model.pred.df <- data.frame(x=sin.df$x, y=sixth.order.model.pred, order=6)

nineth.order.model <- lm(y ~ poly(x, 9, raw=TRUE), data=train.df)
nineth.order.model.pred <- predict(nineth.order.model, sin.df)
nineth.order.model.pred.df <- data.frame(x=sin.df$x, y=nineth.order.model.pred, order=9)
{% endhighlight %}

The RSS for them respectively are:-

{% highlight r %}
sum(third.order.model$residuals^2)
{% endhighlight %}



{% highlight text %}
## [1] 0.2565181
{% endhighlight %}



{% highlight r %}
sum(sixth.order.model$residuals^2)
{% endhighlight %}



{% highlight text %}
## [1] 0.1702409
{% endhighlight %}



{% highlight r %}
sum(nineth.order.model$residuals^2)
{% endhighlight %}



{% highlight text %}
## [1] 0
{% endhighlight %}


The overlayed plot of all these models together is:-

{% highlight r %}
everything <- rbind(linear.model.pred.df, third.order.model.pred.df, sixth.order.model.pred.df, nineth.order.model.pred.df)
everything$order <- as.factor(everything$order)
ggplot() + geom_line(data=sin.df, aes(x=x, y=y)) + geom_line(data=everything, aes(x=x, y=y, colour=order)) + 
  geom_point(data=train.df, aes(x=x, y=y), colour='red') 
{% endhighlight %}

![plot of chunk plot.all](/assets/Rfig/plot.all-1.svg)

The effects of overfitting is apparent here, the nineth ordered equation passes through all the training data and has achieved a RSS of $$0$$, in doing so it also captured the random errors and has failed spectacularly in generalising to unseen data.

On the other end of the spectrum, we have the linear model, it has also failed to model the underlying function due to the lack of complexity in the model, also known as underfitting. 

## Detecting Overfit


## Dealing with Overfit

One "easy" way to deal overfitting is to obtain more data, in the polynomial regression above, if we obtained more training data, models with enough complexity looks roughly like the underlying function:-

![plot of chunk more.data](/assets/Rfig/more.data-1.svg)




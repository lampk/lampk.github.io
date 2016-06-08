---
layout: post
title: Tobit regression for left censoring data
status: publish
published: true
---
 
## Data with under detection limit
 
Approaches to analyse data with under detection limit are summarised in [http://www.ats.ucla.edu/stat/r/dae/tobit.htm][1].
 
One of the approaches is to treat them as left censoring data. To analyse left censoring data, one can use Tobit model.
 
## Fit tobit regression in R
 
In R, one can fit a tobit regression model using:
 
    * vglm (package VGAM): [https://cran.r-project.org/web/packages/VGAM/index.html][2]
    * zelig (package Zelig): [http://docs.zeligproject.org/en/latest/zelig-tobit.html][3] 
    * survreg (package survival): [https://cran.r-project.org/web/packages/survival/index.html][4]
 
### An example

{% highlight r %}
library(survival)
data(tobin)
dat <- transform(tobin, group = rep(1:2, each = nrow(tobin)/2))
{% endhighlight %}
 
### Fit tobit model with vglm
 
* Source: [http://www.ats.ucla.edu/stat/r/dae/tobit.htm][1]
 
* Model without clustering
 

{% highlight r %}
library(VGAM)
{% endhighlight %}



{% highlight text %}
## Error in library(VGAM): there is no package called 'VGAM'
{% endhighlight %}



{% highlight r %}
fit1 <- vglm(durable ~ age + quant, tobit(Lower = 0), data = dat)
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): could not find function "vglm"
{% endhighlight %}



{% highlight r %}
summary(fit1)
{% endhighlight %}



{% highlight text %}
## Error in summary(fit1): object 'fit1' not found
{% endhighlight %}
 
### Fit tobit model with zelig
 
* Source: [http://stackoverflow.com/questions/21608561/how-to-use-robust-se-and-cluster-se-with-vglm-tobit-model][5]
 
* Model without clustering
 

{% highlight r %}
library(Zelig)
{% endhighlight %}



{% highlight text %}
## Error in library(Zelig): there is no package called 'Zelig'
{% endhighlight %}



{% highlight r %}
fit2 <- zelig(durable ~ age + quant, below = 0, above = Inf, model = "tobit", data = dat)
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): could not find function "zelig"
{% endhighlight %}



{% highlight r %}
summary(fit2)
{% endhighlight %}



{% highlight text %}
## Error in summary(fit2): object 'fit2' not found
{% endhighlight %}
 
* Model with clustering
 

{% highlight r %}
fit22 <- zelig(durable ~ age + quant, below = 0, above = Inf, model = "tobit", data = dat, robust = TRUE, cluster = "group")
{% endhighlight %}



{% highlight text %}
## Error in eval(expr, envir, enclos): could not find function "zelig"
{% endhighlight %}



{% highlight r %}
summary(fit22)
{% endhighlight %}



{% highlight text %}
## Error in summary(fit22): object 'fit22' not found
{% endhighlight %}
 
### Fit tobit model with survreg
 
* Model without clustering
 

{% highlight r %}
fit3 <- survreg(Surv(durable, I(durable > 0), type = 'left') ~ age + quant, data=dat, dist='gaussian')
summary(fit3)
{% endhighlight %}



{% highlight text %}
## 
## Call:
## survreg(formula = Surv(durable, I(durable > 0), type = "left") ~ 
##     age + quant, data = dat, dist = "gaussian")
##               Value Std. Error      z        p
## (Intercept) 15.1449    16.0795  0.942 3.46e-01
## age         -0.1291     0.2186 -0.590 5.55e-01
## quant       -0.0455     0.0583 -0.782 4.34e-01
## Log(scale)   1.7179     0.3103  5.536 3.10e-08
## 
## Scale= 5.57 
## 
## Gaussian distribution
## Loglik(model)= -28.9   Loglik(intercept only)= -29.5
## 	Chisq= 1.1 on 2 degrees of freedom, p= 0.58 
## Number of Newton-Raphson Iterations: 3 
## n= 20
{% endhighlight %}
 
* Model with clustering
 

{% highlight r %}
fit32 <- survreg(Surv(durable, I(durable > 0), type = 'left') ~ age + quant + cluster(group), data=dat, dist='gaussian', robust = TRUE)
summary(fit32)
{% endhighlight %}



{% highlight text %}
## 
## Call:
## survreg(formula = Surv(durable, I(durable > 0), type = "left") ~ 
##     age + quant + cluster(group), data = dat, dist = "gaussian", 
##     robust = TRUE)
##               Value Std. Err (Naive SE)      z        p
## (Intercept) 15.1449  13.8402    16.0795  1.094 2.74e-01
## age         -0.1291   0.0833     0.2186 -1.549 1.21e-01
## quant       -0.0455   0.0725     0.0583 -0.628 5.30e-01
## Log(scale)   1.7179   0.0910     0.3103 18.882 1.60e-79
## 
## Scale= 5.57 
## 
## Gaussian distribution
## Loglik(model)= -28.9   Loglik(intercept only)= -29.5
## 	Chisq= 1.1 on 2 degrees of freedom, p= 0.58 
## (Loglikelihood assumes independent observations)
## Number of Newton-Raphson Iterations: 3 
## n= 20
{% endhighlight %}
 
### Comparing results
 

{% highlight r %}
all <- cbind(summary(fit1)@coef3[3:4, 1:2],
             cbind(coef(fit2)[[1]][2:3], sqrt(diag(fit2$getvcov()[[1]]))[2:3]),
             cbind(coef(fit3)[2:3], sqrt(diag(fit3$var))[2:3]),
             cbind(coef(fit32)[2:3], sqrt(diag(fit32$var))[2:3]))
{% endhighlight %}



{% highlight text %}
## Error in summary(fit1): object 'fit1' not found
{% endhighlight %}



{% highlight r %}
library(knitr)
kable(round(all, 3))
{% endhighlight %}



{% highlight text %}
## Error in round(all, 3): non-numeric argument to mathematical function
{% endhighlight %}
 
 
[1]: http://www.ats.ucla.edu/stat/r/dae/tobit.htm
[2]: https://cran.r-project.org/web/packages/VGAM/index.html
[3]: http://docs.zeligproject.org/en/latest/zelig-tobit.html
[4]: https://cran.r-project.org/web/packages/survival/index.html
[5]: http://stackoverflow.com/questions/21608561/how-to-use-robust-se-and-cluster-se-with-vglm-tobit-model

---
layout: default
title: Gaussian Process
parent: Math
nav_order: 1
has_children: false
---
<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>

# Gaussian Process
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}

---


## What is Gaussian Process:
Recap gaussian distribution:
* Singlevariate
The distribution is normal distribution a long the variable.

$$p(x) = N(\mu, \sigma^2)$$ 

 $$p(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{1}{2}(\frac{x-\mu }{\sigma})^2  } $$

* Multivariate
The distribution is normal distribution a long each segment.

$$p(X_n) = N(\mu_n, \Sigma_{n \times n} )$$

$${\displaystyle p(x_{1},\ldots ,x_{k})={\frac {\exp \left(-{\frac {1}{2}}({\mathbf {x} }-{\boldsymbol {\mu }})^{\mathrm {T} }{\boldsymbol {\Sigma }}^{-1}({\mathbf {x} }-{\boldsymbol {\mu }})\right)}{\sqrt {(2\pi )^{k}|{\boldsymbol {\Sigma }}|}}}}$$

* Gaussian Process
The distribution at each time point is normal distribution. Means the process will have stochastic behaviour, but at each time point, the all possible follows a gauss distribution. But the distribution at each time point is not the same. 

**GP describes the scenario where the uncertainty of each time point is correlated to all other time. The uncertainty dependence between time point is actually the covariance between these 2 time points (which is also the value of kernal function $$\sigma = kernal(t_a,t_b)$$). All correlated time states (infinite amount) form a "multi-variant gauss distribution". (All other time state is not related, thus not a part of the distribution)**

Kernal function actually describes the dependence between time points. So given 2 time points, it will tell you how related they are (covariance, kind of replacement of covariance matrix). Most common kernal function is `RBF`: $$k(t_a,t_b) = \sigma^2 exp(-\frac{\left \| t_a - t_b\right \| ^{2} }{2l^{2}})$$, where &&\sigma&& and $$l$$ is hyper-parameters for controlling the "influence range between time points" (you could choose like both equal to 1 or something).

Based on the basic principal, we know if in some related time points, the distribution are tight, then the the distribution at given point should also be relative tight. So, once we observed values for some time points (which the related time points). The state of the rest related time points should be easier to predicted using attribution of multivarient distribution. (see reference below)

The mean value could be simply represented by $$\mu(t)$$.

But covariance cannot be done like multi-variant, which use covariance matrix between each pair-of-variables (It's countable size!). The gaussian process has infinit amount of time points, whose  covariance matrix will be endless large. So intead, kernal function are used to calculate the covariance between each  

$$p(\xi _t) = N(\mu_t, \sigma_t)$$


## Use Gaussian Process for prediction and correction:
Because the conditional distribution of multi-variant gaussian distribution is still gaussian distribution. The only thing we need to know is the $$\mu^*$$ and $$\sigma^*$$ for time point to be predicted based on the known timje point (which is actually a conditional distribution). To calculate the . Use known time point value $$(x_{1-n},y_{1-n})$$ 

```python

```
[reference](https://www.zhihu.com/question/46631426/answer/1735470753)
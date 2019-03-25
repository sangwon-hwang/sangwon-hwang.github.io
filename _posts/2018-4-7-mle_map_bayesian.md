---
layout: post
title:  "MLE, MAP, and Bayesian Parameter Estimation"
---

I originally posted this on Medium 
[here](https://medium.com/@amatsukawa/maximum-likelihood-maximum-a-priori-and-bayesian-parameter-estimation-d99a23a0519f), 
reproduced here with minor edits to consolidate my posts.

You have a coin that isn't necesarily fair. You flip it 3 times, it comes up heads all 3 times. 
What’s the probability of the coin coming up heads for the next flip? 
This is a foundational machine learning problem of parameter estimation from data. 
In this case, we want to estimate the probability of heads $$h$$, from data $$D$$.
In this post, I'll discuss fundamental frameworks for performing this task. 

## Maximum Likelihood Estimation

One way to do this is to find the value of the parameter $$h$$ that maximizes the likelihood of the observed data, 
$$P(D;h)$$ . Here, we use $$;$$ to denote that $$h$$ is a parameter of the distribution $$P$$, 
meaning $$h$$ is part of the definition of $$P$$ but $$P$$ is only a function over $$D$$, 
with the usual requirements about a probability distribution.

$$
h^*_{\text{MLE}} = \mathrm{argmax}_{h} \: P(D;h)
$$

This is a frequentist approach to parameter estimation known as maximum likelihood estimation. 
Under this method, we would estimate that $$h = 1.0$$.

But our intuition tells us this is not probably not true. We know that for most coins, 
there is some probability that we can get tails as well, and usually we expect something like $$h = 0.5$$.


## Priors and Posteriors

How do we encode this intuition mathematically? We can specify a joint distribution over data and 
our parameters: $$p(D, h) = P(D|h)p(h)$$ . We specify a *prior* distribution $$p(h)$$ that encodes our intuition about what 
value $$h$$ should have, and a conditional distribution given that value of $$h$$, $$p(D|h)$$. 

How do we do parameter estimation in this framework? We need the *posterior* distribution,
which is our new belief after seeing the data, $$p(h|D)$$. But we only have $$P(D|h)$$ and 
$$p(h)$$!  Bayes rule to the rescue!

$$
p(h|D) = \frac{P(D|h)p(h)}{P(D)}
$$

However, the denominator here is a problem. It's equal to this integral, which is not possible to compute in general. For this coin 
example, if you use very specific distributions known as conjugates you can side-step this issue, 
but we’ll cover that in some other post.

$$
P(D) = \int{p(D, h) dh}
$$


## Maximum a Posteriori

But wait, we can say something intelligent about $$p(h|D)$$ without the normalization constant $$P(D)$$! 
Namely, the normalizing *constant* doesn’t change the relative magnitudes of the distribution, 
we can find the mode without doing the integral:

$$
\begin{align}
h^*_{\text{MAP}} &= \mathrm{argmax}_{h} \: p(h|D) \\
&= \mathrm{argmax}_{h} \: \frac{P(D|h)p(h)}{P(D)} \\
&= \mathrm{argmax}_{h} \: P(D|h)p(h)
\end{align}
$$

This is known as maximum a posteriori (MAP). There are many ways you can find this particular value of $$h$$, 
for example using [conjugate gradient descent](https://en.wikipedia.org/wiki/Conjugate_gradient_method).


## Bayesian Parameter Estimation

With MAP, we incorporated our intuition with priors, and ignored the normalizing integral to obtained a *point estimate*
of $$h$$ at the mode of the posterior distribution.

But what if we instead tried to use approximation methods for the integral? 
If we make the usual iid assumption, we can use the fact that future data $$x$$ is conditionally independent of 
observed data $$D$$ given $$h$$:

$$
\begin{align}
P(x|D) &= \int{p(x, h|D) dh} \\
&= \int{P(x|h) p(h|D) dh}
\end{align}
$$

Rather than using the single value of $$h$$ that corresponds to the mode of the posterior $$p(h|D)$$ to compute $$P(x|h)$$, 
we take into account all possible values of $$h$$ and weight it by how likely that particular value is under the 
posterior distribution. This is known as Bayesian parameter estimation. Sometimes it's referred to being "full Bayesian".

Note that this is really quite beautiful. There are two things about a probability distribution you may care about:

* **Inference**: Given the joint distribution with known parameters, estimate the distribution over a subset of variables by 
marginalizing and conditioning other variables.

* **Parameter Estimation**: Estimate the unknown parameters of a probability distribution from data.

* (Representation: There is actually a third, how you represent the conditional independences to reflect the world and make
computation more efficient, but that's not as relevant here.)

Bayesian parameter estimation frames these two tasks as two sides of the same coin:

>**Estimating parameters of a distribution over a set of variables is inference of a larger meta-distribution over the original 
variables and the parameters.**

Of course, actually doing this requires us to compute these difficult integrals, which is computationally expensive.
We’ll likely have to use with approximate methods such as MCMC or variational inference, which is a topic for a future post.





---
title: 'Inequalities to find bounds in Econometrics and Machine Learning'
date: 2020-03-30
permalink: /posts/2020/inequalities/
tags:
  - Inequalities
  - Hoeffding's Inequality
  - Markov's Inequality
  - Chebyshev's Inequality
  - Chernoff Bounds

---

In this blog post I will give definitions, derivations and interpretations of some of the most important inequalities  that we use in Econometrics and Machine Learning to find bounds. 

I am going to go over :

1. Markov's Inequality
2. Chebyshev's Inequality
3. Chernoff Bounds
4. Hoeffding's Inequality

This may seem a lot at the first galance but we use the same logic in all of the derivations. So it is more like applying tha same way of thinking and iteratively improving the result. I am covering all of these in a sequential manner firstly because it is way easier to think about Hoeffding's Inequality after covering Markov, Chebyshev and Chernoff; and secondly because I believe it is a really good example of taking a simple idea and iteratively improving it.



## Markov's Inequality

First things first. This is going to be the base for the derivations of other inequalities.

Lets start by the definiton:

---
> **[Markov's Inequality]**  If **X is a nonnegative random variable**(This is importnant) and a > 0, then the probability that X is at least a is at most the expectation of X divided by a: 
>
> $$Pr(X \geq a ) \leq \frac {E[X]}{a}$$

---
Before going over the relatively simple proof let me try to give some intuition of this inequalty with an example.

Suppose you are going to roll a die and if the die comes out 6 you lose whole of your wealth. To make things a little more interesting assume we do not know the distribution of the outcome of the roll(i.e. we do not know if the die is fair or not) but we know the expected value of the roll.


A natural thing that you may want to know is the probability that you lose your entire wealth. (or if you can not know the exact probability some bound on the probability: i.e. " the probability that I lose my wealth is less than some number".) The Markov Inequality gives you an upper bound on this. In this example assume $E[X] = 2$

Then the thing that you are interested in is:

$$Pr(X\geq6) \leq \frac{E[X]}{6} = 2/6= 1/3$$

So the probability that you lose everything is not more than 1/3, not so good right? As you can see this bound is rather loose and does not provide much information but using this inequality we can derive stricter bounds.

Now, lets go over the proof.

**Proof.** (Note that there are other ways to prove this but I do the simplest one here.)
$$E[X] = \int_{-\infty}^{\infty} x f(x) dx $$

For non-negative random variables:

$$ E[X] = \int_{0}^{\infty} x f(x) dx$$

For any non-negative a:

$$E[X] =  \int_{0}^{\infty} x f(x) dx \geq \int_{a}^{\infty} x f(x) dx$$

Since x>a (because of the bounds of the integral):

$$ \int_{a}^{\infty} x f(x) dx \geq \int_{a}^{\infty} a f(x) = a \int_{a}^{\infty}  f(x) dx  = a.Pr(X\geq a) $$

Thus:

$$E[X] \geq a. Pr(X\geq a)$$

or 

$$Pr(X \geq a ) \leq \frac {E[X]}{a}$$



## Chebyshev's Inequality 

Chebyshev's Inequality is one of the so called _concentration inequalities_. Given a random variable X with expectation $E[X]$, how likely is X to be close to its expectation? Again, we will start by the definition of the inequality and we will see that it is just a direct application of Markov's Inequality.


Here one important difference between Markov's and Chebyshev's Inequality is that we no longer need a RV that is non-negative.(The reason will be much clear if you look at the derivation of the Chebyshev's Inequality.)

---
> **[Chebyshev's Inequality]** Let X (integrable) be a random variable with finite expected value and finite non-zero variance. Then for any real number k > 0,
>
> $$ Pr( \text{  } \big| X - E[X] \big| \geq a) \leq \frac{Var(X)}{a^2}$$

---

Here, in contrast to the Markov's Inequality we are interested in the deviation from the mean and the bound is related to the variance(second moment) not the expectation(first moment). In other words, the probability of the deviation from X of its mean, being grater than some value is bounded by $Var(X)$(the greater the variance the greater the probability, this is intuitively expected as variance shows on average how far we are from the mean.) and  $a^2$ (the greater the deviation the smaller the probability.) 

**Note :** Chebyshev's Inequality is important as we use it to prove the Weak Law of Large Numbers and/or show consistency of estimators. (More on this later, possibly in another post.)

**Proof :** The proof is a direct application of Markov's Inequality.

X is any random variable(integrable). Since it is not non-negative we can not directly use the Markov Inequality.We may also want to use the additional information that the second moment provides.

Since we can only apply Markov's Inequality to non-negative random values we define a new random variable $Y = (X - E[X])^2 $. Then we apply Markov's Ineq. directly to Y:

$$ Pr(Y \geq a^2) \leq \frac{E[Y]}{a^2}$$


Note that $E[Y] = E[(X - E[X])^2] = Var(X)$, plug in $Y$ and $E[Y]$:


$$ Pr((X - E[X])^2 \geq a^2) \leq \frac{Var(X)}{a^2}$$


Then:

$$ Pr( \text{  } \big| X - E[X] \big| \geq a) \leq \frac{Var(X)}{a^2}$$



## Chernoff Bounds

This is another direct application of Markov's Inequality. However this is going to be pretty useful because it uses exponentials and thus play nicely with sums of random variables.There are many different forms of Chernoff bounds, I will go over 2 of them in this blog post.

---
> **[Chernoff Bounds v.1]** Let X be any random variable. Then for any a ≥ 0,
>
> $$ Pr(X - E[X] \geq a) \leq \underset{\lambda \geq 0}{min } \frac{E \Big[exp\big(\lambda(X-E[X]) \big)\Big]}{exp(\lambda a)}$$

---

**Proof.** This is again another direct application of the Markov's Inequality. 

Consider the event $(X - E[X] \geq a)$. This is equilvalent to:

$$ \lambda( X - E[X]) \geq \lambda a \qquad \text{ for any  $\lambda \geq 0$}$$

exponenting both sides:

$$exp\big(\lambda( X - E[X])\big)\geq exp\big(\lambda a\big)$$

So:

$$Pr(X - E[X] \geq a) = Pr(exp\big(\lambda( X - E[X])\big)\geq exp\big(\lambda a\big)) \leq \frac{E\Big[exp(X - E[X])\lambda \Big]}{exp(\lambda t)}$$

where the last inequality follows from the application of the Markov's Inequality to the random variable $exp\big(\lambda( X - E[X])\big)$ with the constant $exp\big(\lambda a\big)$.

Note that this statement is valid for all $\lambda \geq 0. $. So it is also valid for the $\lambda^*$ that is argmin of the right hand side.

So we can write:


$$ Pr(X - E[X] \geq a) \leq \underset{\lambda \geq 0}{min } \frac{E \Big[exp\big(\lambda(X-E[X]) \big)\Big]}{exp(\lambda a)} $$


Note that the reason that we introduced $\lambda$ is that we want to express the right hand side using Moment generating functions. 

**Recall:**

>**[Moment Generating Function]** Moment Generating Function of a random variable X is :
>
>$$M_X(\lambda) = E[exp(\lambda X)]$$

Then, this in mind, we can write the Chernoff Bound like this as well:

---
> **[Chernoff Bounds v.1]** Let X be any random variable. Then for any a ≥ 0,
>
> $$ Pr(X - E[X] \geq a) \leq \underset{\lambda \geq 0}{min }\quad M_{X - E[X]}(\lambda). exp(- \lambda a) $$

---


**Another Recall About Moment Generating Functions:**

> If . If X = $\sum_{i=1}^{n} X_i$ where $X_1, X_2, . . . , X_n$ are independent random variables, then
>
> $$M_X(\lambda) = \prod_{i=1}^{n} M_{X_i}(\lambda)$$

---

**Proof**
$$M_X(\lambda) = E\big[ exp(\lambda \sum_{i} X_i) \big]$$

$$  = E \big[ \prod_{i} exp(\lambda X_i)     \big]$$

Using independence assumption:

$$ = \prod_{i} E\big[ exp(\lambda X_i)\big] = \prod_{i=1}^{n} M_{X_i}(\lambda) . $$  


Using the statement above we can now define the Chernoff bounds for a random variable that is summation of iid random variables.This means that when we calculate a
Chernoff bound of a sum of i.i.d. variables, we need only calculate the moment
generating function for one of them. 

---
> **[Chernoff Bounds v.2]** Let X = $\sum_{i=1}^{n} X_i$ where $X_1, X_2, . . . , X_n$ are independent random variables, then
>
>
> $$Pr(X \geq a) \leq \prod_{i} E[exp(\lambda X_i)] . exp(-\lambda a) = \big( E[exp(\lambda X_1)]\big)^n exp(-\lambda a ) $$
>
> $$Pr(X \geq a) \leq (M_{X_i}(\lambda))^n exp(-\lambda a)$$

---



## Hoeffding's Inequality

Hoeffding’s inequality is a powerful inequality that you will hear a lot in learning theory  for bounding the probability of sums of __*bounded*__ random variables is too large or not. Again I am going to state the inequality and go about the proof to understand better where this inequality comes from. We will need a lemma, namely Hoeffdings Lemma to prove the inequality but I will omit the proof of the lemma here.

---
> **[Hoeffding's Inequality]** . Let $X_1, . . . , X_n$ be independent bounded
> random variables with $X_i \in [a, b]$ for all i, where $|a|< \infty$, $|b|< \infty$, then 
>
> $$Pr(\frac{1}{n} \sum_{i}^{n} X_i - E[X] \geq t ) \leq exp(-\frac{2nt^2}{(b-a)^2}) \qquad \text{for all $t \geq 0$}$$

---

---
> **[Hoeffding's Lemma]** . Let X be a random variable that is bounded with $X \in [a, b]$ for all i, where $|a|< \infty$, $|b|< \infty$
>
> $$E\Big[exp(\lambda(X - E(X)))\Big] \leq exp\Big(\frac{\lambda^2(b-a)^2}{8}\Big) \qquad \text{for all $\lambda \in R$}$$

---
**Proof.**

We are going to use the same trick that we used in the proof of Chernoff Bounds. 

Consider the event $(\frac{1}{n} \sum_{i}^{n} X_i - E[X] \geq t )$, then by just manipulating the statement we can get different expressions for the same event. What we will do is we will introduce the $\lambda$ to the equation and then minimize over all $\lambda s$(This is what I call Chernoff Bound Trick).

$$\Big(\frac{1}{n} \sum_{i}^{n} X_i - E[X] \geq t \Big) = \Big( \sum_{i}^{n} X_i - E[X] \geq nt \Big) = \Big(exp\big(\lambda\sum_{i}^{n} X_i - E[X]\big) \geq exp(nt\lambda) \Big)$$


Then applying the Markov's Inequality (like we did in the Chernoff Bounds):
$$Pr \Big(\frac{1}{n} \sum_{i}^{n} X_i - E[X] \geq t \Big) \leq 
E\Big[ exp(\lambda\sum_{i}^{n} X_i - E[X])\Big]exp(-\lambda n t )$$

Because of the independence:

$$E\Big[ exp(\lambda\sum_{i}^{n} X_i - E[X])\Big]exp(-\lambda n t ) = 
\Big( \prod_{i}^{n} E\big[ exp(\lambda(X_i - E[X_i]))\big] \Big).exp(-\lambda n t )$$

Now putting together and using Hoeffding's Lemma:

$$Pr \Big(\frac{1}{n} \sum_{i}^{n} X_i - E[X] \geq t \Big) \leq \Big( \prod_{i}^{n} E\big[ exp(\lambda(X_i - E[X_i]))\big] \Big).exp(-\lambda n t ) \leq \Big( \prod_i^{n} exp(\frac{\lambda^2(b-a)^2}{8})\Big)exp(-\lambda n t) $$

Finally, since this is true for all $\lambda$, we can minimize over lambda.

$$Pr \Big(\frac{1}{n} \sum_{i}^{n} X_i - E[X] \geq t \Big) \leq \underset{\lambda \geq 0}{min} \quad
exp\Big( \frac{n \lambda^2 (b-a)^2}{8}- \lambda nt \Big) = exp \Big(\frac{-2nt^2}{(b-a)^2}\Big)$$

where the last equality is coming from taking the first order condition and plugging the $\lambda^{*}$to the objective function. 

So we can get the Hoeffding's Inequality :

$$Pr(\frac{1}{n} \sum_{i}^{n} X_i - E[X] \geq t ) \leq exp(-\frac{2nt^2}{(b-a)^2}) \qquad \text{for all $t \geq 0$}$$


I hope this post helps you to understand these foundational inequalities better, easier and perhaps more importantly faster. 

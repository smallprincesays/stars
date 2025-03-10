+++
title = "Conditional Expectation"
date = 2025-01-03
+++

One general definition of conditional expectation is as follows:

Consider $X$, $Y$ random variables on $(\Omega, \mathcal{F}, P)$
and $\mathbb{E}|X| < \infty$. If there exists a measurable function
$h : \RR \to \RR$ such that:
$$
\mathbb{E}[g(Y)h(Y)] = \mathbb{E}[g(Y)X]
$$
for all bounded, measurable functions $g : \RR \to \RR$,
$h(Y)$ is a conditional expectation of $X$ given $Y$.

This is based on the tower property being the defining property of
conditional expectation. Now let's look at an alternative definition.

Assume $X$, $Y$ are integrable random variables on $(\Omega, \mathcal{F}, P)$.

**Prove that integrable r.v. $Z$ is (a.s.) the conditional expectation of $X$ given $Y$ iff**
$$
\mathbb{E}[Z 1_{\\{Y \in B\\}}] = \mathbb{E}[X 1_{\\{Y \in B\\}}]
$$
**for any event $B$**.

<br/>

We want to show that we can use definition (1) and reach this result, and vice versa.

Let's start with $(1) \implies (2)$. We want to show that if $Z$ is
a conditional expectation, then $\EE[Z 1_{\\{Y \in B\\}}] = \EE[X 1_{\\{Y \in B\\}}]$.
If $Z$ is a conditional expectation, then
$$
\EE[g(Y)Z] = \EE[g(Y)X]
$$
for all bounded, measurable functions $g$.

$1_{\\{Y \in B\\}}$ is a bounded, measurable function because the indicator
of a measurable set is a measurable function (remember from yesterday's problem
that a measurable function's preimage should be Borel! trivially satisfied
by this indicator). So we just substitute in this indicator:
$$
\EE[Z 1_{\\{Y \in B\\}}] = \EE[X 1_{\\{Y \in B\\}}]
$$
which is our desired result! We've completed one side of the proof.

Now we want $(2) \implies (1)$. What do we know? Well, we know that we have
some $Z$ such that
$$
\EE[Z 1_{\\{Y \in B\\}}] = \EE[X 1_{\\{Y \in B\\}}]
$$
and we want to show that $Z$ is a conditional expectation:
$$
\EE[g(Y)Z] = \EE[g(Y)X]
$$
Let's try to motivate our proof by noticing that we have this
cool property using indicators on all Borel sets. If we can
approximate any bounded measurable function $g$ with
these indicators, then perhaps we can use some convergence
theorem to complete the proof.

Consider some bounded measurable function $g$. We can approximate
this using nonnegative simple $g_n$:
$$
g_n = \frac{\lfloor2^n g\rfloor}{2^n}
$$
(see [Integration from GATech](https://heil.math.gatech.edu/6337/spring11/section4.1.pdf) for
details), and we have increasing pointwise convergence to $g$. The intuition behind
this is that for every point $x$, we first take the binary representation of $g(x)$.
Then, for each $n$, we'll round down to the nearest integer multiple of $2^{-k}$. Usually,
if $g$ is unbounded, then we also cap the value at e.g. $n$. But here we know that $g$ is
bounded, so we don't need this restriction.

We know that each $g_n$ is simple, so it will take on a finite set of values. Since $g_n$
is measurable, this means that for each finite value, there exists a Borel preimage mapping to it.
Using this fact, we can represent $g(Y)$ with a linear combination of indicators. Remember that
$Y$ is function from the measurable space $(\Omega, \mathcal{F})$ to the real numbers.
We simply check which Borel preimage set $Y$ belongs to and use $g_n$'s finite value there:
$$
g(Y) = \sum_{i = 1}^{k_n} c_{i, n} 1_{\\{Y \in B_{i, n}\\}}
$$
where $c_{i, n}$ are the finite values (coefficients) and $B_{i, n}$ are the Borel
preimage sets that we check if $Y$ is in. From here, we can try going from
$\EE[X g(Y)]$ to reach $\EE[Z g(Y)]$ using our $(1)$ assumption and
the dominated convergence theorem with $X g(Y)$ and $Z g(Y)$:
$$
\begin{align}
\EE[X g(Y)] \&= \EE[\lim_{n \to \infty} X g_n (Y)] = \lim_{n \to \infty} \EE[X g_n (Y)]\\\\
\&= \lim_{n \to \infty} \sum_{i = 1}^{k_n} c_{i, n} \EE[X 1_{\\{Y \in B_{i, n}\\}}]
= \lim_{n \to \infty} \sum_{i = 1}^{k_n} c_{i, n} \EE[Z 1_{\\{Y \in B_{i, n}\\}}]\\\\
&= \lim_{n \to \infty} \EE[Z g_n (Y)] = \EE[Z g(Y)]
\end{align}
$$
which gives us our desired result!

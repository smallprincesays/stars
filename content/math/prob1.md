+++
title = "Events and Sets!"
date = 2025-01-01
+++

Consider $(A_n)_{n \geq 1}$ a sequence of events. For each of the following, find
an expression.

**1. "exactly one of these events occurs"**

Let's break this down! We can interpret this as EITHER
$A_1$ occurs and everything else doesn't, OR $A_2$ occurs
and everything else doesn't and so on. How do we represent only $A_1$
occurring?
$$
A_1 \cap \bigcap_{i \neq 1} A_i^c
$$

Now we just extend this to be an OR of all the different $A_k$s:
$$
\bigcup_{k \geq 1} \left(A_k \cap \bigcap_{i \neq k} A_i^c\right)
$$

And this is still an event! It's a countable union of countable intersections,
so will fall inside our $\sigma$-algebra.


**2. "For all $n \geq n_0$, the event $A_n$ occurs"**

This is easier&mdash;we can just intersect across all the $n$
after $n_0$:
$$
\bigcap_{n \geq n_0} A_n
$$

Again, this is a countable intersection of events and so is an event itself.

**3. Infinitely many of the events $(A_n)_{n \geq 1}$ occur**

We'll have to break this down again. First, let's choose some $n$.
We can represent an event (we need at LEAST one) happening at some $i$ after $n$ as:
$$
\bigcup_{i \geq n} A_i
$$
(this is sorta the opposite of (2)!)
Then, we just need this new event to happen for every single $n$:
$$
\bigcap_{n \geq 1} \bigcup_{i \geq n} A_i
$$

And this is exactly the famous event $A_i \text{ infinitely often}$,
which is seen in the Borel Cantelli lemma. I think this approach helped me understand
where this set notation comes from&mdash;we impose some constraints on ourselves and build
it up from scratch.

We'll tackle some harder problems starting tomorrow!

*Note*: problems inspired from EE 226A.

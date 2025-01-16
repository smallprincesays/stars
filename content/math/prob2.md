+++
title = "Good Set Principle"
date = 2025-01-02
+++

Today, let's take a closer look into $\sigma$-algebras! One particular
$\sigma$-algebra that we know of is $\mathcal{B}_\RR$, the
Borel $\sigma$-algebra. $\mathcal{B}\_\RR$ can be generated using the closed
sets (we'll use the convention of extended reals):
$$
\mathcal{B}\_\RR = \sigma(\\{[-\infty, \alpha], \forall \alpha \in \RR \\})
$$

Now, suppose that we know that any function $g : \RR \to \RR$
is a Borel function if $g^{-1} ([-\infty, \alpha]) \in \mathcal{B}\_\RR$
for all $\alpha \in \RR$. **Prove that for Borel $g$,
$g^{-1}(B) \in \mathcal{B}\_\RR$ for any Borel $B$.**

<br/>

We know that we have a family of sets $[-\infty, \alpha]$ that are "good", i.e. that satisfy
our condition of having a Borel preimage. We want to expand this family to the full
$\sigma$-algebra. To do this, we'll use the **good set principle**, commonly used for this
sort of property satisfaction with $\sigma$-algebras.

Here is our setup:
1. Want to show: each member of $\mathcal{B}\_\RR$ has Borel preimage.
2. Define $\mathcal{A} \coloneqq \\{A \in \mathcal{B}\_\RR : A \text{ has Borel preimage} \\}$.
3. We need to first show that $\mathcal{A}$ is a $\sigma$-algebra.
4. Once we show this, we show that $\mathcal{A}$ contains some inner class $\mathcal{C}$ such that $\sigma(\mathcal{C}) = \mathcal{B}\_\RR$.

Just these four steps are sufficient! How?

Well, our goal is to show $\mathcal{A} = \mathcal{B}\_\RR$, which just means that ALL Borel sets have a Borel preimage.
$\mathcal{A}$ is defined as:
$$
\mathcal{A} \coloneqq \\{A \in \mathcal{B}\_\RR : g^{-1}(A) \in \mathcal{B}\_\RR \\}
$$

Is this a $\sigma$-algebra? Let's check:

If $A \in \mathcal{A}$, then $g^{-1}(A) \in \mathcal{B}\_\RR$. We then check if the preimage of $A^c$ is Borel:
$$g^{-1}(A^c) = \left(g^{-1}(A)\right)^c \in \mathcal{B}\_\RR \implies A^c \in \mathcal{A}$$
so $\mathcal{A}$ is closed under complements (preimages commute under set operations!).

If $A_i \in \mathcal{A}$ for countable $i$, then each $g^{-1}(A_i) \in \mathcal{B}\_\RR$. We then check if the preimage
of $\bigcup_{i} A_i$ is Borel:
$$
g^{-1}\left(\bigcup_{i} A_i\right) = \bigcup_i g^{-1} (A_i) \in \mathcal{B}\_\RR \implies \bigcup_i A_i \in \mathcal{A}
$$
so $\mathcal{A}$ is closed under countable unions (and since complements, also intersections).

So $\mathcal{A}$ is a $\sigma$-algebra! Now, we should use the other fact we know: sets of the
form $[-\infty, \alpha]$ have a Borel preimage. Let's define $\mathcal{C}$ as the family of these sets:
$$
\mathcal{C} = \\{[-\infty, \alpha], \forall \alpha \in \RR \\}
$$

Since all such sets are also Borel, $\mathcal{C} \subset \mathcal{A}$. We ALSO know that we generated
the Borel $\sigma$-algebra using these sets, so we have $\sigma(\mathcal{C}) = \mathcal{B}\_\RR$.
Now here's the cool part:

1. Since $\mathcal{C} \subset \mathcal{A}$ AND $\mathcal{A}$ is a $\sigma$-algebra, $\sigma(\mathcal{C}) \subset \mathcal{A}$.
2. $\sigma(\mathcal{C}) = \mathcal{B}\_\RR$, so $\mathcal{B}\_\RR \subset \mathcal{A}$.
3. But $\mathcal{A}$ is *made* from Borel sets, so by definition $\mathcal{A} \subset \mathcal{B}\_\RR$.
4. Since $\mathcal{B}\_\RR \subset \mathcal{A}$ and $\mathcal{A} \subset \mathcal{B}\_\RR$, $\mathcal{A} = \mathcal{B}\_\RR$!

So somehow, we've jumped to knowing that all Borel sets are good sets (having a Borel preimage).

<br/>

I think this is a really cool proof :) We are playing catch-up a little since I was out of town, but will have more to come.

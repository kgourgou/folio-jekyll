---
layout: distill
title: 'Quick notes on the "Enhanced Cauchy Schwarz inequality and some of its statistical applications'
date: 2024-04-11 12-11-19 +0100
category:
tags:
---

I briefly read this nice paper by [Sergio Scarlatti](#Scarlatti;2024).

In this, the author proves an intermediate term between the classical bounds of the CS inequality.

## Classical CS inequality

First things first, for all $$x,y$$ in some Hilbert space $$H$$ with inner product $$(.,.)$$ we have:

$$
|(x,y)|\leq \|x\|\|y\|.
$$

The author shares a nice proof of CS that I’m not sure if I’ve seen before (but looks neat). We define the matrix $$C=C(x,y)$$ with $$c_{ij}=\frac{1}{\sqrt{2}}(x_iy_j-x_jy_i),\ i,j=1,\ldots,n$$. Then, its second norm is

$$
\|C\|_2=\sqrt{\sum_{ij}c_{ij}^2}.
$$

If we substitute the definition of $$c_{ij}$$ to the above and carry out the algebra, we have
$$\|C\|^2_2=\|x\|^2\|y\|^2-(x,y)^2,$$
which proves CS as the norm is non-negative.

## Enhanced CS

Now, suppose $$V\subseteq H$$ is some closed subspace of $$H$$. Then if $$P$$ is the orthogonal projection onto $$V$$ (i.e., $$PH=V$$), the author defines:

$$
D(x,y|P):=\|Px\|\cdot \|Py\|+\|P^{\perp}x\|\cdot \|P^{\perp}y\|,
$$

for all $$x,y\in H$$ and where $$P^{\perp}$$ is the projection on the orthogonal complement of $$V$$. Then,
$$|(x,y)|\leq D(x,y|P)\leq \|x\|\|y\|.$$

I like inequalities with free terms! We can pick $$P$$ depending on the problem and the bounds would adapt correspondingly. If $$P$$ is a trivial projection to $$H$$ or its complement, we recover usual CS.

**Proof**: It’s a short argument.

$$|(x,y)|=|(Px,Py)+(P^{\perp}x, P^{\perp}y)|\leq |(Px,Py)|+|(P^{\perp}x, P^{\perp}y)|\leq \|Px\|\|Py\|+\|P^{\perp}x\|\|P^{\perp}y\|,$$

where we used bilinearity of inner product, triangle inequality, and CS inequality for each term. Now, if $$a=\|Px\|, b=\|Py\|, c=\|P^{\perp}x\|, d=\|P^{\perp}y\|$$, the author shows that

$$
ab+cd=\sqrt{(ab+cd)^2}\leq \sqrt{(ab+cd)^2+(ac-bd)^2}=\sqrt{(a^2+c^2)(d^2+b^2)},
$$

which is important because with the definitions of $$a,b,c,d$$ above, $$a^2+c^2=\|x\|^2$$ and similarly for $$y$$.

The author shows this with an appeal to algebra (as shown above), but you will notice a simpler way; this bound is just CS but applied to the vectors $$u=(a,c)$$ and $$v=(b,d)$$. $$\square$$

## Going beyond $$P$$

What if we have more than one subspace? Suppose $$V, U\subseteq H$$ with corresponding projections $$P,Q$$, then $$x=Px+P^{\perp}x$$ and similarly for $$y$$, which gives

$$
\begin{align}
|(x,y)|&\leq |(Px,Qy)|+|(Px, Q^{\perp}y)|+|(P^{\perp}x,Qy)|+|(P^{\perp}x, Q^{\perp}y)|\\
&\leq (\|Px\|+\|P^{\perp}x\|)(\|Qy\|+\|Q^{\perp}y\|),
\end{align}
$$

Again, by bilinearity, triangle ineq., and CS.

The last bound is not as good; setting $$P=Q$$ there does not recover the previous results (note $$\|x\|\leq \|Px\|+\|P^{\perp}x\|$$). That’s because some terms would be cancelled from the first bound but are not cancelled from the second bound. For example, $$(Px, P^{\perp}y)=0$$ regardless of $$x,y$$, and so on.

### References

1. <a name='Scarlatti;2024'>Scarlatti, S., 2024. Enhanced Cauchy Schwarz inequality and some of its statistical applications. arXiv preprint arXiv:2403.13964.
   </a>

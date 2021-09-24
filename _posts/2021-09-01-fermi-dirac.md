---
layout: post
title:  "Demystifying a Surpising Relationship between Statistical Physics and Machine Learning."
---

### Introduction
On August 24th, 2021 a very interesting paper was published in the
[Proceedings of the National
Academy of Sciences](https://www.pnas.org/) with the title
[The Fermi–Dirac distribution provides a calibrated probabilistic output for binary classifiers](https://www.pnas.org/content/118/34/e2100761118) [[1]](#1). The
authors report on a "suprising relationship between the probability
of a sample belonging to one of the two classes and the Fermi-Dirac distribution
determining the probability that a fermion occupies a given single-particle
quantum state in a physical system of noninteracting fermions." They
describe a method for calibrating the probabilities predicted by a
binary classifier and a method for ensembling disparate classifiers, each based
upon this suprising relationship. The connection between binary
classification and statistical physics is indeed remarkable but it is not novel.
The authors are in fact discussing applications of the venerable method known
as [logistic regression](https://en.wikipedia.org/wiki/Logistic_regression).
The authors claim that their method is
distinct from logistic regression but they are mistaken. Let's trace through
the paper with a view to demystifying this surprising connection. The authors
of the paper may be surprised to find that the end result is not that their
research topic is less rich than they believe, but that logistic regression is a
richer and more interesting method than they've imagined.

### Review of Fermi-Dirac Statistics
First let's give a brief introduction to the basics of [Fermi-Dirac statistics](https://en.wikipedia.org/wiki/Fermi%E2%80%93Dirac_statistics). I'll
try to make the presentation accessible to those from a general mathematically
literate audience who may not have experience with modern physics. We're only
going through the minimum background necessary, describing the mathematical
properties of this model without going into any of the underlying physics.


The problem concerns systems of many indistinguisable and non-interacting 
elementary particles called
fermions. Systems of non-interacting particles are also known as
gases and the systems of interest are known as
[Fermi gases](https://en.wikipedia.org/wiki/Fermi_gas).


Each fermion can be thought of as having an energy associated to it which comes
from a discrete set of nonzero values

$$\mathcal{E} = \left\{\epsilon_1, \epsilon_2, \ldots, \epsilon_m\right\}$$

without loss of generality we suppose

$$0 < \epsilon_1 < \epsilon_2 < \ldots < \epsilon_m$$

each fermion has a state associated to it which also comes from a discrete
set of values

$$\mathcal{S} = \left\{s_1, s_2, \ldots, s_M\right\}$$

and there is mapping from states onto energy levels such that any particle
in state $$s_i$$ will have energy $$\epsilon_j$$ for some fixed $$j$$, but
multiple states can have the same associated energy.

Suppose there are $$g_i$$ states associated to energy level $$\epsilon_i$$.
$$g_i$$ is referred to as the degeneracy of the system at energy level
$$\epsilon_i$$. The unique properties of a Fermi gas are due to fermions
obeying something called the  [Pauli exclusion principle](https://en.wikipedia.org/wiki/Pauli_exclusion_principle). This principle asserts that within a system,
at most one particle can occupy any given state $$s_i$$. There is elegant math
and physics lurking behind this principle but it is outside the scope of this
post. We will take it as given within the mathematical model.

Fermi-Dirac statistics concerns the distribution of energy values for particles
within a Fermi gas with known possible energy levels and degeneracies and
where the total number of particles $$N$$ and the total energy
$$E$$ within the system are known. $$N$$ is assumed to be sufficiently large to
allow for some simplifications that we will see shortly. The problem is to
determine the distribution $$\pi(\epsilon\left|N, E\right.)$$ describing the
probability that a state $$s$$ is occupied given its associated energy level
$$\epsilon$$ and with fixed $$N$$ and $$E$$.

There are multiple ways to derive the correct distribution, the most relevant
for this post relies on something called the **Max Entropy Principle**.

Suppose that any arrangement
of $$N$$ total fermions among the 
$$M = \sum_{i=1}^{m}g_i$$ states in $$\mathcal{S}$$ is equally likely.
Given a random distribution of $$N$$ fermions among the $$M$$ states, what is the
probability $$\pi(\mathbf{n})$$ that $$n_i$$ of the $$g_i$$ states at
energy level $$\epsilon_i$$ are occupied, given a sequence of natural numbers

$$\mathbf{n} = \left(n_1, n_2, \ldots, n_m\right)$$

such that

$$\sum_{i=1}^{m}n_i = N$$

and

$$0 <= n_i <= g_i$$

for each $$i$$.

The problem is in essence a simple combinatorial puzzle. The number of ways to
distribute $$N$$ identical fermions among $$M$$ states is given by the [binomial
coefficient](https://en.wikipedia.org/wiki/Binomial_coefficient)

$$\binom{M}{N} = \binom{g_1 + g_2 + \cdots + g_m}{N}$$

(recall that by the Pauli exclusion principle, at most
one fermion can occupy any given state). Similarly, the number of ways to distribute
$$n_i$$ fermions among the $$g_i$$ states at energy level $$\epsilon_i$$ is given by
the binomial coefficient $$\binom{g_i}{n_i}$$. Through the [rule of product](https://en.wikipedia.org/wiki/Rule_of_product)
we see that the total number of ways to distribute the $$N$$ fermions among the $$M$$ states with
$$n_i$$ fermions occupying the $$g_i$$ states at energy level $$\epsilon_i$$ for $$\mathbf{n}$$ satisfying the
properties above is

$$\operatorname{W}\left(\mathbf{n}\right) = \prod_{i=1}^{m}\binom{g_i}{n_i}$$

and thus

$$\pi(\mathbf{n}) = \frac{\operatorname{W}\left(\mathbf{n}\right)}{\binom{M}{N}}$$

The standard way to solve the problem from here is to attempt to find the value of
$$\mathbf{n}$$ that maximizes the probability $$\pi(\mathbf{n})$$ given the additional
constraint that the total energy is equal to $$E$$. You may have noticed that this is a challenging
discrete problem and that it is ill posed in the sense that for many values of $$E$$ and
$$N$$ there is no choice of $$\mathbf{n}$$ that even satisfies the constraints. This is
handled by passing to the continuous [relaxation](https://en.wikipedia.org/wiki/Relaxation_(approximation))
of the discrete problem. That is, we no longer restrict the $$n_i$$'s to be natural numbers, but allow
them to take on real values as well. It suffices to maximize $$\log W\left(\mathbf{n}\right)$$ subject
to the constraints since the map $$x \rightarrow \log\left(x \binom{M}{N}\right)$$ is monotonic and thus $$\pi\left(\mathbf{n}\right)$$ and
$$\log W\left(\mathbf{n}\right)$$ will be maximized for the same values of $$\mathbf{n}$$.

The problem becomes to maximize

$$\log W\left(\mathbf{n}\right) = \sum_{i=1}^{m}\log \binom{g_i}{n_i} = 
\sum_{i=1}^{m}\log \frac{\Gamma(g_i + 1)}{\Gamma(n_i + 1)\Gamma(g_i - n_i + 1)} =$$

$$\sum_{i=1}^{m}\log \Gamma(g_i + 1) - \log \Gamma(n_i + 1) - \log \Gamma(g_i - n_i + 1)$$

subject to the constraints

$$\sum_{i=1}^{m}n_i = N,\:\:\:\:\:\:\sum_{i=1}^{m}n_i\epsilon_i = E,\:\:\:\:0 \leq n_i \leq g_i$$

where $$\Gamma$$ is the [Gamma function](https://en.wikipedia.org/wiki/Gamma_function), a generalization
of the factorial to real and complex values which is being used because $$n_i$$ can take on any real
value, and thus the usual formula $$\binom{g_i}{n_i} = \frac{g_i!}{n_i!(g_i - n_i)!}$$ is not valid.
For natural numbers $$n$$, $$n! = \Gamma(n+1)$$.

Since $$\sum_{i=1}^{m}\log \Gamma(g_i + 1)$$ does not depend on $$\mathbf{n}$$, it suffices to maximize

$$-\sum_{i=1}^{m}\log\Gamma(n_i + 1) + \log\Gamma(g_i - n_i + 1)$$

subject to the constraints.

The expression appears unwieldy but can be simplified
with [Stirling's approximation](https://en.wikipedia.org/wiki/Stirling%27s_approximation)
in the form

$$\log\Gamma(x + 1) = x\log x - x + O\left(\log\left(1 + \frac{1}{x}\right)\right)$$

by replacing $$\log \Gamma$$ terms with Stirling's approximation, ignoring error terms,
simplifying, and then ignoring terms that do not depend on $$\mathbf{n}$$ we end up
with the problem of maximizing

$$-\sum_{i=1}^mn_i\log(n_i) + (g_i - n_i)\log(g_i - n_i)$$

again subject to the constraints

$$\sum_{i=1}^{m}n_i = N,\:\:\:\:\:\:\sum_{i=1}^{m}n_i\epsilon_i = E,\:\:\:\:0 \leq n_i \leq g_i$$

As an aside, note that we haven't justified ignoring the error terms. It's common in physics
to treat error terms as negligible when deriving a mathematical model. There
are often implicit assumptions baked into the treatment of certain terms as
negligible. Agreement with experiment remains the ultimate test of physical
theory. It will serve a mathematician well to become comfortable working
non-rigorously like a phycisist; one can cover more ground that way. A vital
skill for a mathematician is then to be able to convert a loose argument into
a rigorous one or otherwise to identify when obstructions make this
difficult or impossible. See [here](https://terrytao.wordpress.com/career-advice/theres-more-to-mathematics-than-rigour-and-proofs/)
for an excellent post on this matter by an immeasurably more skilled and knowledgeable mathematician.

This constrained optimization problem can be solved by the method of [Lagrange multipliers](https://en.wikipedia.org/wiki/Lagrange_multiplier).
(Technically, since some of the constraints are inequalities, one actually applies the
[Karush Kuhn Tucker conditions](https://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions).)

one can then find the optimal value for $$\mathbf{n}$$ to be

$$\hat{n}_i = \frac{g_i}{1 + e^{\alpha + \beta\epsilon_i}}\:\:\:i=1\ldots m$$

for real constants $$\alpha$$ and $$\beta$$ for which there is no closed form solution except in special cases.
These constants have a physical interpretation that we will not discuss here. The probability $$\pi(\epsilon |N, E)$$
that a state $$s$$ is occupied given its energy level and for fixed $$N$$ and $$E$$ is then taken to be

$$\pi(\epsilon |N, E) = \frac{\hat{n_i}}{g_i} = \frac{1}{1 + e^{\alpha + \beta\epsilon_i}}$$

This expression may look familiar if you've seen logistic regression before. The resulting probability distribution is known as the
[Fermi-Dirac distribution](https://en.wikipedia.org/wiki/Fermi%E2%80%93Dirac_statistics#Fermi%E2%80%93Dirac_distribution).
It was first derived in 1926 independently by both the Italian physicist [Enrico Fermi](https://en.wikipedia.org/wiki/Enrico_Fermi) and
the British physicist [Paul Dirac](https://en.wikipedia.org/wiki/Paul_Dirac).

You may not be entirely satisfied with the above derivation.

Through a series of simplifications we see that the quantity we are maximizing satisifes the equivalence


$$-\sum_{i=1}^mn_i\log n_i + (g_i - n_i)\log(g_i - n_i) =$$

$$-g_i\sum_{i=1}^m\frac{n_i}{g_i}\log\frac{n_i}{g_i} + (1 - \frac{n_i}{g_i})\log\left(1 - \frac{n_i}{g_i}\right) + \log g_i$$

maximizing it under the constraints is then equivalent to maximizing

$$-\sum_{i=1}^m\frac{n_i}{g_i}\log\frac{n_i}{g_i} + (1 - \frac{n_i}{g_i})\log\left(1 - \frac{n_i}{g_i}\right)$$

letting $$p_i = \frac{n_i}{g_i}$$ and interpreting $$p_i$$ to be the probability of that a state $$s$$ of energy level $$\epsilon_i$$ is occupied
for fixed $$N$$ and $$E$$, the above expression becomes

$$-\sum_{i=1}^mp_i\log p_i + (1 - p_i)\log\left(1 - p_i\right)$$

you may recognize this expression as the [Shannon entropy](https://en.wikipedia.org/wiki/Entropy_(information_theory)) of a discrete probability
distribution [[2]](#2).

### References
<a id="1">[1]</a>
Kim SC, Arun AS, Ahsen ME, Vogel R, Stolovitzky G. The Fermi-Dirac distribution provides a calibrated probabilistic output for binary classifiers. Proc Natl Acad Sci U S A. 2021 Aug 24;118(34):e2100761118. doi: 10.1073/pnas.2100761118. PMID: 34413191; PMCID: PMC8403970.

<a id="2">[2]</a>
Shannon, C.E. (1948), A Mathematical Theory of Communication. Bell System Technical Journal, 27: 379-423. https://doi.org/10.1002/j.1538-7305.1948.tb01338.x

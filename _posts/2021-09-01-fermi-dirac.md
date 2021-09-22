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
as logistic regression. The authors claim that their method is
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


The problem concerns systems of many non-interacting elementary particles called
fermions. Systems of non-interacting particles are also known as
gases and the systems of interest are known as
[Fermi gases](https://en.wikipedia.org/wiki/Fermi_gas).


Each fermion can be thought of as having an energy associated to it which comes
from a discrete set of nonzero values

$$\mathcal{E} = \left\{\epsilon_1, \epsilon_2, \ldots\right\}$$

without loss of generality we suppose

$$0 < \epsilon_1 < \epsilon_2 < \ldots$$

each fermion has a state associated to it which also comes from a discrete
set of values

$$\mathcal{S} = \left\{s_1, s_2, \ldots\right\}$$

and there is mapping from states onto energy levels such that any particle
in state $$s_i$$ will have energy $$\epsilon_j$$ for some fixed $$j$$, but
multiple states can have the same associated energy.

Suppose there are $$g_i$$ states associated to energy level $$\epsilon_i$$.
$$g_i$$ is referred to as the degeneracy of the system at energy level
$$\epsilon_i$$. The unique properties of a Fermi gas are due to fermions
obeying something called the  [Pauli exclusion principle](https://en.wikipedia.org/wiki/Pauli_exclusion_principle). This principle asserts that within a system,
at most one particle can occupy any given state $$s_i$$. There is elegant math
lurking behind this principle but it is outside the scope of this
post. We will take it as given within the mathematical model.

Fermi-Dirac statistics concerns the distribution of energy values for particles
within a Fermi gas with known energy levels and degeneracies and
where the total number of particles $$N$$ and the total energy
$$E$$ within the system are known. $$N$$ is assumed to be sufficiently large to
allow for some simplifications that we will see shortly. The solution of the
problem is a probability distribution $$\pi(\epsilon\left|N, E\right.)$$ giving the probability
that a state $$s$$ is occupied given its associated energy level $$\epsilon$$.


### References
<a id="1">[1]</a>
Kim SC, Arun AS, Ahsen ME, Vogel R, Stolovitzky G. The Fermi-Dirac distribution provides a calibrated probabilistic output for binary classifiers. Proc Natl Acad Sci U S A. 2021 Aug 24;118(34):e2100761118. doi: 10.1073/pnas.2100761118. PMID: 34413191; PMCID: PMC8403970.

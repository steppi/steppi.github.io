---
layout: post
title:  "Demystifying a Surpising Relationship between Statistical Physics and Machine Learning."
---

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
research is less rich than they believe, but that logistic regression is a
richer and more interesting method than they've imagined.

First let's give a brief introduction to the basics of [Fermi-Dirac statistics](https://en.wikipedia.org/wiki/Fermi%E2%80%93Dirac_statistics). I'll
try to make the presentation accessible to those from a general mathematically
literate audience who may not have experience with modern physics. 









## References
<a id="1">[1]</a>
Kim SC, Arun AS, Ahsen ME, Vogel R, Stolovitzky G. The Fermi-Dirac distribution provides a calibrated probabilistic output for binary classifiers. Proc Natl Acad Sci U S A. 2021 Aug 24;118(34):e2100761118. doi: 10.1073/pnas.2100761118. PMID: 34413191; PMCID: PMC8403970.

---
layout: post
title: "How are SciPy's Fortran libraries used in biomedicine?"
published: true
---

*Hi CZI EOSS reviewers, if by chance you're taking an early look, this article is under
construction and should be completed by 11:00pm PT on December 5th 2023.*


### Introduction

SciPy is an open source library for mathematics, science, and engineering
[[1]](#1).  It provides user-friendly APIs to a wide range of tools for
statistics, optimization, numerical integration, linear algebra, Fourier
transforms, signal and image processing, solutions of ordinary differential
equations, and more. SciPy owes much of its success to wrappers around venerable
scientific computing libraries that were posted on NETLIB [[2]](#2): a
repository of freely available software, documents, and databases of interest to
the numerical, scientific computing, and other communities [[3]](#3) created by
Jack Dongarra of Argonne National Laboratory and Eric Gross of Bell Labs in the
1980s [[4]](#4). Much of this software is written in Fortran 77, with a
monolithic and unstructured __GOTO__ laden style which can be difficult for even
modern subject experts to untangle.  It is consequently now mostly unmaintained.
To serve as an aid in understanding how modernization and structural
improvement of these libraries could benefit the biomedical sciences, we
catalog the Fortran libraries wrapped by SciPy and list ways these tools are or
could be applied to biomedical research and development.



#### __ODEPACK and dop__: <small>Systems of ordinary differential equations</small>
Ordinary differential equations (ODEs) are a fundemental tool for modeling
dynamical processes.  ODEPACK [[5]](#5) contains a collection of ODE solvers
developed in the Lawrence Livermore National Library (LLNL). Although there are
9 solvers in this collection, SciPy only uses LSODA [[6]](#6)[[7]](#7), an
adaptive solver which automatically switches between methods suitable for stiff
or nonstiff problems [[8]](#8), giving it the flexibility to tackle a diverse
range of problems without needing advanced knowledge of the problems
characteristics. Though not strictly part of ODEPACK, SciPy vendors LLNL's
variable-coefficient VODE and ZVODE solvers [[9]](#9) together with ODEPACK,
which can be preferable when the user has a good grip on a problem's stiffness
characteristics and needs closer control over the integration process. dop
[[10]](#10) implements the Dormand-Prince method, an explicit Runge-Kutta
method. This simpler algorithm offers superior performance and accuracy, with
great error estimates, for smooth and non-stiff problems. SciPy exposes all of
these methods through the functions
[scipy.integrate.solve_ivp](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.solve_ivp.html#scipy.integrate.solve_ivp),
[scipy.integrate.ode](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.ode.html),
and offers a direct interface to LSODA through
[scipy.integrate.odeint](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.odeint.html#scipy.integrate.odeint).


Applications of systems of ODEs to biomedicine include:
- __Systems biology__: for modeling networks of biomolecular interactions [[11]](#11)
- __Cardiology__: for modeling electrical activities of the heart [[12]](#12), dynamics of
blood flow [[13]](#13), and other cardiovascular phenomena
- __Epidemiology__: to model the spread of infectious disease [[14]](#14)
- __Pharmacokinetics and Pharmacodynamics (PK/PD)__: for modeling how
drugs are processed by and impact the body [[15]](#15)[[16]](#16)
- __Neuoroscience__: for modeling electrical activity of neurons and neural networks [[17]](#17)
- __Cancer Biology__: for modeling tumor growth and interactions between cancer cells and the immune system [[18]](#18)
- __Biomechanics__: for modeling things like muscle dynamics and joint movements [[19]](#19)[[20]](#20)



#### __quadpack__: <small>Numerical integration [[4]](#4)</small>
Essential component of an enormous range of methods and algorithms.


#### __fitpack__: <small>smoothing splines [[5]](#5)</small>
Smoothing splines make smooth approximations to a function based on noisy observations.
Some applications to biomedicine include:
- __Biomedical signal processing__: for smoothing and noise reduction of electrocardiogram (ECG), electroencephalogram (EEG) data
- __Medical Imaging__: used in the image reconstruction process to reduce noise and improve
image clarity
- __Statistical Curve Fitting__: fitting curves to biological data to capture trends, e.g.
dose response modeling in pharmacology, modeling growth curves in developmental biology,
survival curves in epidemiology, relationships between biomarkers and clinical outcomes


#### __id_dist__: <small>Low rank matrix approximation [[6]](#6)</small>
Low rank matrix approximation is valuable for dimension reduction and noise filtering
in high dimensional datasets. Some biomedical applications include:
- __Omics analysis__: finding clusters of co-expressed genes in gene expression microarray,
or RNASeq data. Uncovering relationships among proteins/metabolites in
proteomics/metabolomics data.
- __Medical imaging__: separating signal from noise in image reconstruction.
- __Machine learning__: feature extraction/dimensionality reduction as a preprocessing
step for machine learning models applied to biomedical problems.
- __Systems biology__ Finding important interactions within biological networks,
e.g. protein-protein interactions, gene regulatory networks.



#### __odrpack__: <small>orthogonal distance regression [[7]](#7)</small>
Orthogonal distance regression is able to account for errors in both the predictor
and response variables. Some biomedical applications include:
- __Calibration of biomedical instruments__:
- __Dose-response analys__:
- __Pharmokinetic/Pharmodynamic Modeling__:
- __Epidemiology__:



#### __cobyla, L-BFGS-B, slsqp__: <small>constrained optimization [[8]](#8)[[9]](#9)[[10]](#10)</small>


#### __minpack__: <small>nonlinear equations and least squares minimization [[11]](#11)</small>


#### __ARPACK and PROPACK__: <small>sparse linear algebra [[12]](#12)[[13]](#13)</small>
Sparse eigenvalue and singular value decomposition problems are essential for dealing with
large, sparse matrices (where most elements are zero) which are common in biomedical data.


#### __AMOS, cdflib, and specfun__: <small>special functions [[14]](#14)[[15]](#15)[[16]](#16)</small>


#### __mvndist__: <small>multivariate normal distribution function [[17]](#17)</small>




### References
<a id="1">[1]</a>
Pauli Virtanen, Ralf Gommers, Travis E. Oliphant, Matt Haberland, Tyler Reddy, David Cournapeau, Evgeni Burovski, Pearu Peterson, Warren Weckesser, Jonathan Bright, Stéfan J. van der Walt, Matthew Brett, Joshua Wilson, K. Jarrod Millman, Nikolay Mayorov, Andrew R. J. Nelson, Eric Jones, Robert Kern, Eric Larson, CJ Carey, İlhan Polat, Yu Feng, Eric W. Moore, Jake VanderPlas, Denis Laxalde, Josef Perktold, Robert Cimrman, Ian Henriksen, E.A. Quintero, Charles R Harris, Anne M. Archibald, Antônio H. Ribeiro, Fabian Pedregosa, Paul van Mulbregt, and SciPy 1.0 Contributors. (2020) SciPy 1.0: Fundamental Algorithms for Scientific Computing in Python. Nature Methods, 17(3), 261-272.


<a id="2">[2]</a>
[https://www.netlib.org](https://www.netlib.org)

<a id="3">[3]</a>
[https://www.netlib.org/misc/faq.html#2.1](https://www.netlib.org/misc/faq.html#2.1)

<a id="4">[4]</a>
[https://steppi.github.io/siam_history/Dongarra_returned_SIAM_copy.pdf](https://steppi.github.io/siam_history/Dongarra_returned_SIAM_copy.pdf)


<a id="5">[5]</a>
Hindmarsh AC. ODEPACK, A Systematized Collection of ODE Solvers. In: Stepleman RS, et al, eds. Scientific Computing. Vol 1 of IMACS Transactions on Scientific Computation. Amsterdam, Netherlands: North-Holland; 1983:55-64.

<a id="6">[6]</a>
Petzold LR. Automatic selection of methods for solving stiff and nonstiff systems of ordinary differential equations. SIAM J Sci Stat Comput. 1983;4:136-148.


<a id="7">[7]</a>
[https://github.com/scipy/scipy/blob/deeb67ffc68d66a9b3069c1bb9cc3bb195c91b66/scipy/integrate/odepack/lsoda.f#L1C69-L1C69](https://github.com/scipy/scipy/blob/deeb67ffc68d66a9b3069c1bb9cc3bb195c91b66/scipy/integrate/odepack/lsoda.f#L1C69-L1C69)

<a id="8">[8]</a>
Wikipedia contributors. (n.d.). Stiff equation. Wikipedia. Retrieved December 5, 2023, from [https://en.wikipedia.org/wiki/Stiff_equation](https://en.wikipedia.org/wiki/Stiff_equation).

<a id="9">[9]</a>
Brown PN, Byrne GD, Hindmarsh AC. VODE: A Variable-Coefficient ODE Solver. SIAM J Sci Stat Comput. 1989;10(5):1038-1051. doi:10.1137/0910062.

<a id="10">[10]</a>
Hairer E, Norsett S, Wanner G. Solving Ordinary Differential Equations I: Nonstiff Problems. 1993. doi:10.1007/978-3-540-78862-1.

<a id="11">[11]</a>
Städter P, Schälte Y, Schmiester L, et al. Benchmarking of numerical integration methods for ODE models of biological systems. Sci Rep. 2021;11:2696. doi:10.1038/s41598-021-82196-2.

<a id="12">[12]</a>
Sundnes J, Lines GT, Tveito A. Efficient solution of ordinary differential equations modeling electrical activity in cardiac cells. Math Biosci. 2001;172(2):55-72. doi:10.1016/S0025-5564(01)00069-4.

<a id="13">[13]</a>
Myers TG, Ribas Ripoll V, Sáez de Tejada Cuenca A, Mitchell SL, McGuinness MJ. Modelling the cardiovascular system for assessing the blood pressure curve. Math Ind Case Stud. 2017;8(1):2. doi:10.1186/s40929-017-0011-1

<a id="14">[14]</a>
Beira MJ, Sebastião PJ. A differential equations model-fitting analysis of COVID-19 epidemiological data to explain multi-wave dynamics. Sci Rep. 2021;11:16312. doi:10.1038/s41598-021-95494-6.

<a id="15">[15]</a>
Krzyzanski W. Interpretation of transit compartments pharmacodynamic models as lifespan based indirect response models. J Pharmacokinet Pharmacodyn. 2011;38:179-204. doi:10.1007/s10928-010-9183-z.

<a id="16">[16]</a>
Rodríguez-Díaz JM, Sánchez-León G. Design optimality for models defined by a system of ordinary differential equations. Biom J. 2014;56:886-900. doi:10.1002/bimj.201300145.

<a id="17">[17]</a>
Ashby FG, Helie S. The Neurodynamics of Cognition: A Tutorial on Computational Cognitive Neuroscience. J Math Psychol. 2011;55(4):273-289. doi:10.1016/j.jmp.2011.04.003

<a id="18">[18]</a>
Koziol JA, Falls TJ, Schnitzer JE. Different ODE models of tumor growth can deliver similar results. BMC Cancer. 2020;20(1):226. Published 2020 Mar 17. doi:10.1186/s12885-020-6703-0

<a id="19">[19]</a>
Walcott S. Muscle activation described with a differential equation model for large ensembles of locally coupled molecular motors. Phys Rev E Stat Nonlin Soft Matter Phys. 2014;90(4):042717. doi:10.1103/PhysRevE.90.042717

<a id="20">[20]</a>
van den Bogert AJ, Blana D, Heinrich D. Implicit methods for efficient musculoskeletal simulation and optimal control. Procedia IUTAM. 2011;2(2011):297-316. doi:10.1016/j.piutam.2011.04.027

<!---

<a id="6">[6]</a>
Piessens R, deDoncker-Kapenga E, Uberhuber C, Kahaner D. Quadpack: A Subroutine Package for Automatic Integration. Series in Computational Mathematics v.1. Berlin, Germany: Springer-Verlag; 1983. 515.43/Q1S 100394Z.

<a id="7">[7]</a>
Dierckx P. Curve and Surface Fitting with Splines. Oxford, UK: Oxford University Press; 1993.

<a id="8">[8]</a>
Martinsson P-G, Rokhlin V, Shkolnisky Y, Tygert M. ID: A Software Package for Low-Rank Approximation of Matrices via Interpolative Decompositions, Version 0.2. 2008.



<a id="9">[9]</a>
Boggs PT, Donaldson JR, Byrd RH, Schnabel RB. Algorithm 676: ODRPACK: Software for Weighted Orthogonal Distance Regression. ACM Trans Math Softw. 1989;15(4):348-364. doi:10.1145/76909.76913.

<a id="10">[10]</a>
M. J. D. Powell. A direct search optimization method that models the objective and constraint functions by linear interpolation. In S. Gomez and J. P. Hennart, editors, Advances in Optimization and Numerical Analysis, pages 51–67, Dordrecht, NL, 1994. Springer.

<a id="11">[11]</a>
Morales JL, Nocedal J. Remark on Algorithm 778: L-BFGS-B, FORTRAN Routines for Large Scale Bound Constrained Optimization. ACM Trans Math Softw. 2011;38(1).

<a id="12">[12]</a>
Kraft D. A Software Package for Sequential Quadratic Programming. DFVLR-FB 88-28. 1988.

<a id="13">[13]</a>
Moré JJ, Garbow BS, Hillstrom KE. User Guide for MINPACK-1. Argonne, IL: Argonne National Laboratory; 1980. Report ANL-80-74.

<a id="14">[14]</a>
Lehoucq R, Maschhoff K, Sorensen D, et al. ARPACK-NG: Large Scale Eigenvalue Problem Solver. Astrophysics Source Code Library. 2023. Ascl:2306.049.

<a id="15">[15]</a>
Larsen RM. Lanczos Bidiagonalization with Partial Reorthogonalization for Large Sparse SVD and Linear Least Squares Calculations. Tech Rep. Computer Science Department, Aarhus University; August 1998.

<a id="14">[14]</a>
Amos DE. Algorithm 644: A Portable Package for Bessel Functions of a Complex Argument and Nonnegative Order. ACM Trans Math Softw. 1986;12(3):265-273. doi:10.1145/7921.214331.

<a id="15">[15]</a>
Brown B, Lovato J, Russell K. CDFLIB: Library of Fortran Routines for Cumulative Distribution Functions, Inverses, and Other Parameters. February 1994.

<a id="16">[16]</a>
Zhang S, Jin J. Computation of Special Functions. Wiley; 1996. ISBN: 0-471-11963-6. LC: QA351.C45.

<a id="17">[17]</a>
Genz A. MVNDST: Software for the Numerical Computation of Multivariate Normal Probabilities. 1998. Accessed November 28, 2023. Available from: https://www.sci.wsu.edu/math/faculty/genz/homepage.

-->





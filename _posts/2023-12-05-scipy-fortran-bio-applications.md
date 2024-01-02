---
layout: post
title: "How are SciPy's Fortran libraries used in biomedicine?"
published: true
---


### Introduction
This article is intended as supplementary information for a CZI EOSS Cycle 6
grant proposal asking for funding to support efforts to modernize SciPy's
aging Fortran dependencies. There are myriad reasons why this effort would
vastly improve the health of SciPy as a whole, but the grant proposal did
not go into just how widely applicable the tools provided by this Fortran
code are in the biomedical sciences.

First, a short summary. SciPy is an open source library for mathematics,
science, and engineering [[1]](#1).  It provides user-friendly APIs to a wide
range of tools for statistics, optimization, numerical integration, linear
algebra, Fourier transforms, signal and image processing, solutions of ordinary
differential equations, and more. SciPy owes much of its success to wrappers
around venerable scientific computing libraries that were posted on NETLIB
[[2]](#2): a repository of freely available software, documents, and databases
of interest to the numerical, scientific computing, and other communities
[[3]](#3) created by Jack Dongarra of Argonne National Laboratory and Eric Gross
of Bell Labs in the 1980s [[4]](#4). Much of this software is written in Fortran
77, with a monolithic and unstructured __GOTO__ laden style which can be
difficult for even modern subject experts to untangle.  It is consequently now
mostly unmaintained.  To serve as an aid in understanding how modernization and
structural improvement of these libraries could benefit the biomedical sciences,
we catalog the Fortran libraries wrapped by SciPy and list ways these tools are
or could be applied to biomedical research and development. This is just a
cursory survey of applications, and is not intended to be exhaustive.


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
good error estimates, for smooth and non-stiff problems. SciPy exposes all of
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



#### __quadpack__: <small> Adaptive numerical integration</small>
quadpack provides a suite of mostly adaptive methods for numerical integration
of one-dimensional functions [[21]](#21). Adaptive routines can adjust automatically
to a problem, offering the ability to integrate a wide range of functions without
needing to study their particular properties. Integration is a fundamental operation
with applications too numerous to list. SciPy exposes quadpack through the function
[scipy.integrate.quad](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.quad.html).


#### __fitpack__: <small>Smoothing splines</small>
Smoothing splines are used to create smooth approximations to a function
based on noisy observations. Not to be confused with the FITPACK library of Alan Cline,
this is a package of Fortran subroutines for calculating smoothing splines for various
kinds of data and geometries, and with automatic knot selection, and is also known as
DIERCKX [[22]](#22). SciPy exposes fitpack through the classes [scipy.interpolate.UnivariateSpline](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.UnivariateSpline.html), [scipy.interpolate.InterpolatedUnivariateSpline](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.InterpolatedUnivariateSpline.html), and
[scipy.interpolate.LSQUnivariateSpline](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.LSQUnivariateSpline.html), directly through wrappers for fitpack's
individual subroutines, and through the legacy function [interp1d](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.interp1d.html).


Some applications of smoothing splines to biomedicine include:
- __Biomedical signal processing__: e.g. for smoothing and noise reduction of electrocardiogram (ECG) data [[23]](#23)
- __Medical Imaging__: used in the image reconstruction process to reduce noise and improve
image clarity [[24]](#24)
- __Statistical Curve Fitting__: fitting curves to biological data to capture trends, e.g.
dose response modeling in pharmacology [[25]](#25), modeling growth curves in developmental
biology [[26]](#26), survival curves in epidemiology [[27]](#27),
relationships between biomarkers and clinical outcomes [[28]](#28) etc.


#### __id_dist__: <small>Low rank matrix approximation</small>
Low rank matrix approximation aims to find more compact representations of data while
limiting loss of information. SciPy vendors the low-rank matrix approximation package
ID [[29]](#29) as id_dist and exposes it through the module
[scipy.linalg.interpolative](https://docs.scipy.org/doc/scipy/reference/linalg.interpolative.html). Some of the numerous applications of low rank matrix approximation in biomedicine
include:

- __Omics analysis__: completion of gene expression data from partial measures [[30]](#30),
predicting biomarker-disease associations [[31]](#31), finding clusters of
co-expressed genes from gene expression microarray or RNASeq data  [[32]](#32),
- __Medical imaging__: separating signal from noise in image reconstruction [[33]](#33).
- __Machine learning__: feature extraction/dimensionality reduction as a preprocessing
step for machine learning models applied to biomedical problems [[34]](#34).
- __Systems biology__ Finding important interactions within biological networks,
e.g. protein-protein interactions, gene regulatory networks [[35]](#35).



#### __odrpack__: <small>Orthogonal distance regression</small>
Orthogonal distance regression is designed to handle problems where there are
measurement errors in the predictor variables. Ordinary least squares regression
assumes the predictor variables are fixed. ODRPACK is a software package for
weighted orthogonal distance regression [[36]](#36) which can handle linear and
non-linear fitting functions. SciPy exposes ODRPACK through
the [scipy.odr](https://docs.scipy.org/doc/scipy/reference/odr.html) module.
Orthogonal distance regression has a range of biomedical applications, including
calibration of biomedical instruments [[37]](#37), dose-response analysis [[38]](#38),
and other noisy problems [[39]](#39).



#### __cobyla__: <small>Derivative free constrained optimization</small>
The Constrained Optimization By Linear Approximation (COBYLA) algorithm, developed
by M.J.D Powell is a numerical optimization method for constrained problems where
the derivative of the objective function is not known [[40]](#40). Such problems
arise when trying to optimize a quantity which is noisy and/or difficult to compute.
A classic example is molecular design [[41]](#41), for instance trying to optimize
the binding affinity of a drug to a target under physical and biological constraints
[[42]](#42).

Some applications from recent biomedical articles that cite COBYLA specifically
include generation of realistic human anatomical models [[43]](#43), as part of a
pipeline for identifying functionally relevant brain regions from functional magnetic
resonance imaging data [[44]](#44), as part of a pipeline for removing artifacts
in x-ray cone beam computed tomography images [[45]](#45),
and for pre-operative planning for liver cryosurgery [[46]](#46).


#### __L-BFGS-B and slsqp__: <small>Gradient aware constrained optimization</small>
The derivative or gradient gives information on how a function is changing at a
given point. Optimization methods which take gradient information into account can
converge more rapidly to better solutions for smooth functions. L-BFGS-B (Limited-memory
Broyden–Fletcher–Goldfarb–Shanno algorithm) [[47]](#47) and slsqp
(sequential least squares programming) [[48]](#48) are quasi-Newton methods which
generate and work with approximations of the gradient and Hessian (higher dimensional
analog of the second derivative) to guide where the solver checks next. They differ
in that L-BFGS-B, while faster and more memory efficient, allows only simple bound
constraints like $$a \leq x_i \leq b$$ while slsqp allows for more complicated linear
and nonlinear equality and inequality constraints. Recent applications from papers
citing L-BFGS-B include aperture shape optimization for radiation therapy [[49]](#49),
planning of non-pharmaceutical interventions for the COVID-19 pandemic while accounting
for social end economic costs of lockdown measures [[50]](#50), and optimal
control problems related to bioproduction, the production of clinically and
commercially important biological products and chemicals from living cells
[[51]](#51). Recent applications from papers citing slsqp include estimating gene
regulatory networks from time series data [[52]](#52), predicting the distribution
of fitness effects of new mutations [[53]](#53), and causal inference of the impact
of the microbiome on disease states [[54]](#54).



#### __minpack__: <small>nonlinear equations and least squares minimization</small>
minpack is a collection of Fortran subroutines for solving systems of nonlinear
equations and for solving linear and nonlinear least squares fitting problems
[[55]](#55).  Such problems are of fundamental importance in statistics and mathematical
modeling. A small sample of applications from papers citing Minpack include
the Tinker 8 library for molecular design and simulation, where nonlinear least
squares problems appear for local search [[56]](#56), a weighted exact-test for
mutually exclusive mutations in cancer [[57]](#57), and determining parameters
from data in an ODE model of cellular spread of HIV-1 [[58](#58).



#### __ARPACK and PROPACK__: <small>sparse linear algebra</small>
ARPACK is a numerical library for solving eigenvalue problems for large sparse
or structured matrices [[59]](#59). Similarly, PROPACK is a library for singular value decomposition problems for large sparse or structured matrices [[60]](#60). Sparse eigenvalue and
singular value decomposition problems are essential for dealing with large,
sparse matrices (where most elements are zero) which are common in biomedical data.
Recent applications from papers citing ARPACK include identification of genetic loci
associated with risk of major depression [[61]](#61), coping with uncertainty when
making inferences from low depth next generation sequencing data [[62]](#62), and
establishing a link between clinical outcome of cancer and the immune contexture
[[63]](#63). Recent applications from papers citing PROPACK include medical imaging
resolution enhancement [[64]](#64), assembly of haplotypes (clusters of genes which
tend to vary together) from next generation sequence data [[65]](#65), and dimensionality
reduction of single cell RNASeq data [[66]](#66).


#### __AMOS, cdflib, and specfun__: <small>special functions</small>
AMOS computes Bessel functions [[67]](#67), cdflib inverse cumulative
distribution functions (CDFs) of common statistical distributions [[68]](#68),
and specfun [[69]](#69) is used in SciPy for special functions of a complex
variable. Inverse CDFs have clear statistical applications. Bessel functions and
other special functions of mathematical physics appear frequently in biophysical
modeling. These are fundamental functions with a wide range of applications. The
Fortran implementations SciPy relies on have a host of numerical issues, both
subtle and obvious, as can be seen by searching for issues tagged with `special`
on the SciPy GitHub repo.


#### __mvndist__: <small>multivariate normal distribution function</small>
Multivariate normal probability computation software from Alan Genz [[70]](#70). The
normal, or Gaussian distribution is of critical importance in statistics, it maximizes
the entropy among distributions with a given mean and covariance matrix and due to the
Central Limit Theorem, its presence within the sciences is ubitiquous.

### Conclusion
Even after a relatively short search, I've managed to unearth a vast number of applications
of these libraries within biomedicine. On the way, I've found much more that I simply do
not have time to condense and write down. My impression is that an exhaustive list at
a similar level of detail could fill a book length tome.


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

<a id="21">[21]</a>
Piessens R, deDoncker-Kapenga E, Uberhuber C, Kahaner D. Quadpack: A Subroutine Package for Automatic Integration. Series in Computational Mathematics v.1. Berlin, Germany: Springer-Verlag; 1983. 515.43/Q1S 100394Z.

<a id="22">[22]</a>
Dierckx P. Curve and Surface Fitting with Splines. Oxford, UK: Oxford University Press; 1993.

<a id="23">[23]</a>
Abdul, Ehab & Hussein, Razzaq & Abdulridha, Hayder & N. Hassan, Ashwaq. (2015). Feature Extraction of ECG Signal using Cubic Spline Technique. 128. 97-107. 


<a id="24">[24]</a>
Unser M. Splines: A Perfect Fit for Medical Imaging. Proc SPIE Int Soc Opt Eng. 2002;4684. doi:10.1117/12.467162.

<a id="25">[25]</a>
Kirby S, Colman P, Morris M. Adaptive modelling of dose-response relationships using smoothing splines. Pharm Stat. 2009;8(4):346-355. doi:10.1002/pst.363

<a id="26">[26]</a>
Unser M. Splines: A Perfect Fit for Medical Imaging. Proc SPIE Int Soc Opt Eng. 2002;4684. doi:10.1117/12.467162.

<a id="27">[27]</a>
Noorkojuri H, Hajizadeh E, Baghestani A, Pourhoseingholi M. Application of smoothing methods for determining of the effecting factors on the survival rate of gastric cancer patients. Iran Red Crescent Med J. 2013;15(2):166-172. doi:10.5812/ircmj.8649

<a id="28">[28]</a>
Gauthier, J., Wu, Q.V. & Gooley, T.A. Cubic splines to model relationships between continuous variables and outcomes: a guide for clinicians. Bone Marrow Transplant 55, 675–680 (2020). https://doi.org/10.1038/s41409-019-0679-x

<a id="29">[29]</a>
Martinsson P-G, Rokhlin V, Shkolnisky Y, Tygert M. ID: A Software Package for Low-Rank Approximation of Matrices via Interpolative Decompositions, Version 0.2. 2008.

<a id="30">[30]</a>
Kapur A, Marwah K, Alterovitz G. Gene expression prediction using low-rank matrix completion. BMC Bioinformatics. 2016;17:243. doi:10.1186/s12859-016-1106-6.

<a id="31">[31]</a>
Lu C, Yang M, Luo F, et al. Prediction of lncRNA-disease associations based on inductive matrix completion. Bioinformatics. 2018;34(19):3357-3364. doi:10.1093/bioinformatics/bty327


<a id="32">[32]</a>
Li J, Lai Y, Zhang C, Zhang Q. TGCnA: temporal gene coexpression network analysis using a low-rank plus sparse framework. J Appl Stat. 2019;47(6):1064-1083. Published 2019 Sep 16. doi:10.1080/02664763.2019.1667311


<a id="33">[33]</a>
Mohd Sagheer SV, George SN. Ultrasound image despeckling using low rank matrix approximation approach. Biomed Signal Process Control. 2017;38:236-249. doi:10.1016/j.bspc.2017.06.011.

<a id="34">[34]</a>
Markovsky I. Low Rank Approximation: Algorithms, Implementation, Applications. Springer; 2014.


<a id="35">[35]</a>
Li L-P, Wang Y-B, You Z-H, Li Y, An J-Y. PCLPred: A Bioinformatics Method for Predicting Protein–Protein Interactions by Combining Relevance Vector Machine Model with Low-Rank Matrix Approximation. International Journal of Molecular Sciences. 2018; 19(4):1029. https://doi.org/10.3390/ijms19041029

<a id="36">[36]</a>
Boggs PT, Donaldson JR, Byrd RH, Schnabel RB. Algorithm 676: ODRPACK: Software for Weighted Orthogonal Distance Regression. ACM Trans Math Softw. 1989;15(4):348-364. doi:10.1145/76909.76913.

<a id="37">[37]</a>
https://blog.minitab.com/en/orthogonal-regression-for-instrument-calibration

<a id="38">[38]</a>
Walsh L, Shore R, Azizova TV, Rühm W. On the choice of methodology for evaluating dose-rate effects on radiation-related cancer risks. Radiat Environ Biophys. 2021;60(3):493-500. doi:10.1007/s00411-021-00920-y

<a id="39">[39]</a>
Pallavi, Joshi S, Singh D, et al. Comprehensive Review of Orthogonal Regression and Its Applications in Different Domains. Arch Computat Methods Eng. 2022;29:4027-4047. doi:10.1007/s11831-022-09728-5.

<a id="40">[40]</a>
M. J. D. Powell. A direct search optimization method that models the objective and constraint functions by linear interpolation. In S. Gomez and J. P. Hennart, editors, Advances in Optimization and Numerical Analysis, pages 51–67, Dordrecht, NL, 1994. Springer.

<a id="41">[41]</a>
Sun Y, Sahinidis N, Sundaram A, Cheon MS. Derivative-free optimization for chemical product design. Curr Opin Chem Eng. 2020;27:98-106. doi:10.1016/j.coche.2019.11.006.

<a id="42">[42]</a>
Boitreaud J, Mallet V, Oliver C, Waldispühl J. OptiMol: Optimization of Binding Affinities in Chemical Space for Drug Discovery. J Chem Inf Model. 2020;60(12):5658-5666. doi:10.1021/acs.jcim.0c00833.


<a id="43">[43]</a>
Demirel, D, Yu, A, Baer-Cooper, S, Halic, T, Bayrak, C. Generative Anatomy Modeling Language (GAML). Int J Med Robotics Comput Assist Surg. 2017; 13:e1813. https://doi.org/10.1002/rcs.1813 

<a id="44">[44]</a>
Whiteman AS, Bartsch AJ, Kang J, Johnson TD. Bayesian inference for brain activity from functional magnetic resonance imaging collected at two spatial resolutions. Ann Appl Stat. 2022;16(4):2626-2647. doi:10.1214/22-AOAS1606.

<a id="45">[45]</a>
Würfl T, Hoffmann M, Aichert A, Maier AK, Maaß N, Dennerlein F. Calibration-free beam hardening reduction in x-ray CBCT using the epipolar consistency condition and physical constraints. Med Phys. 2019;46(11):e810-e822. doi:10.1002/mp.13625.

<a id="46">[46]</a>
Jaberzadeh A, Essert C. Pre-operative planning of multiple probes in three dimensions for liver cryosurgery: comparison of different optimization methods. Math Meth Appl Sci. 2016;39:4764-4772. doi:10.1002/mma.3548.

<a id="47">[47]</a>
Morales JL, Nocedal J. Remark on Algorithm 778: L-BFGS-B, FORTRAN Routines for Large Scale Bound Constrained Optimization. ACM Trans Math Softw. 2011;38(1).

<a id="48">[48]</a>
Kraft D. A Software Package for Sequential Quadratic Programming. DFVLR-FB 88-28. 1988.

<a id="49">[49]</a>
Zhang LY, Gui ZG, Zhang PC, et al. Aperture shape optimization in intensity-modulated radiation therapy planning. NUCL SCI TECH. 2023;34:140. doi:10.1007/s41365-023-01300-5


<a id="50">[50]</a>
Siradjuddin I, Agiska IN, Ningrum BC, et al. A Non-Pharmaceutical Intervention Policy for Mitigating COVID-19 Pandemic Using Predictive Control Scheme and SEIR Compartmental Model. Paper presented at: 10th International Conference on Information Technology, Computer, and Electrical Engineering (ICITACEE); 2023; Semarang, Indonesia. doi:10.1109/ICITACEE58587.2023.10277389.

<a id="51">[51]</a>
Lunz D, Bonnans JF, Ruess J. Optimal control of bioproduction in the presence of population heterogeneity. J Math Biol. 2023;86(3):43. Published 2023 Feb 6. doi:10.1007/s00285-023-01876-x

<a id="52">[52]</a>
Hoffmann M, Fröhner C, Noé F. Reactive SINDy: Discovering Governing Reactions from Concentration Data. J Chem Phys. 2019;150(2):025101. Published January 14, 2019. doi:10.1063/1.5054310.

<a id="53">[53]</a>
Kim BY, Huber CD, Lohmueller KE. Inference of the Distribution of Selection Coefficients for New Nonsynonymous Mutations Using Large Samples. Genetics. 2017;206(1):345-361. Published May 1, 2017. doi:10.1534/genetics.116.197145.

<a id="54">[54]</a>
Wang C, Hu J, Blaser MJ, Li H. Estimating and Testing the Microbial Causal Mediation Effect with High-Dimensional and Compositional Microbiome Data. Bioinformatics. 2020;36(2):347-355. doi:10.1093/bioinformatics/btz565.


<a id="55">[55]</a>
Moré JJ, Garbow BS, Hillstrom KE. User Guide for MINPACK-1. Argonne, IL: Argonne National Laboratory; 1980. Report ANL-80-74.

<a id="56">[56]</a>
Rackers JA, Wang Z, Lu C, Laury ML, Lagardère L, Schnieders MJ, Piquemal J-P, Ren P, Ponder JW. Tinker 8: Software Tools for Molecular Design. J Chem Theory Comput. 2018;14(10):5273-5289. doi:10.1021/acs.jctc.8b00529

<a id="57">[57]</a>
Leiserson MDM, Reyna MA, Raphael BJ. A Weighted Exact Test for Mutually Exclusive Mutations in Cancer. Bioinformatics. 2016;32(17):i736-i745. doi:10.1093/bioinformatics/btw462

<a id="58">[58]</a>
Hataye JM, Casazza JP, Best K, Liang CJ, Immonen TT, Ambrozak DR, Darko S, Henry AR, Laboune F, Maldarelli F, Douek DC, Hengartner NW, Yamamoto T, Keele BF, Perelson AS, Koup RA. Principles Governing Establishment versus Collapse of HIV-1 Cellular Spread. Cell Host Microbe. 2019;26(6):748-763.e20. doi:10.1016/j.chom.2019.10.006

<a id="59">[59]</a>
Lehoucq R, Maschhoff K, Sorensen D, et al. ARPACK-NG: Large Scale Eigenvalue Problem Solver. Astrophysics Source Code Library. 2023. Ascl:2306.049.

<a id="60">[60]</a>
Larsen RM. Lanczos Bidiagonalization with Partial Reorthogonalization for Large Sparse SVD and Linear Least Squares Calculations. Tech Rep. Computer Science Department, Aarhus University; August 1998.

<a id="61">[61]</a>
Hyde C, Nagle M, Tian C, et al. Identification of 15 Genetic Loci Associated with Risk of Major Depression in Individuals of European Descent. Nat Genet. 2016;48:1031–1036. doi:10.1038/ng.3623

<a id="62">[62]</a>
Meisner J, Albrechtsen A. Inferring Population Structure and Admixture Proportions in Low-Depth NGS Data. Genetics. 2018;210(2):719-731. doi:10.1534/genetics.118.301336


<a id="63">[63]</a>
Foerster F, Hess M, Gerhold-Ay A, et al. The Immune Contexture of Hepatocellular Carcinoma Predicts Clinical Outcome. Sci Rep. 2018;8:5351. doi:10.1038/s41598-018-21937-2

<a id="64">[64]</a>
Liu H, Guo Q, Wang G, et al. Medical Image Resolution Enhancement for Healthcare Using Nonlocal Self-Similarity and Low-Rank Prior. Multimed Tools Appl. 2019;78:9033-9050. doi:10.1007/s11042-017-5277-6

<a id="65">[65]</a>
Hashemi A, Zhu B, Vikalo H. Sparse Tensor Decomposition for Haplotype Assembly of Diploids and Polyploids. BMC Genomics. 2018;19(Suppl 4):191. doi:10.1186/s12864-018-4551-y

<a id="66">[66]</a>
Linderman GC. Dimensionality Reduction of Single-Cell RNA-Seq Data. In: Picardi E, ed. RNA Bioinformatics. Methods in Molecular Biology; vol 2284. New York, NY: Humana; 2021. doi:10.1007/978-1-0716-1307-8_18


<a id="67">[67]</a>
Amos DE. Algorithm 644: A Portable Package for Bessel Functions of a Complex Argument and Nonnegative Order. ACM Trans Math Softw. 1986;12(3):265-273. doi:10.1145/7921.214331.

<a id="68">[68]</a>
Brown B, Lovato J, Russell K. CDFLIB: Library of Fortran Routines for Cumulative Distribution Functions, Inverses, and Other Parameters. February 1994.

<a id="69">[69]</a>
Zhang S, Jin J. Computation of Special Functions. Wiley; 1996. ISBN: 0-471-11963-6. LC: QA351.C45.

<a id="70">[70]</a>
Genz A. MVNDST: Software for the Numerical Computation of Multivariate Normal Probabilities. 1998. Accessed November 28, 2023. Available from: https://www.sci.wsu.edu/math/faculty/genz/homepage.

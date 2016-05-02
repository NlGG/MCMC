# Mamba.jl

このレポジトリはJuliaにおけるMCMC用パッケージであるMamba.jlの紹介・説明の日本語訳のためのものである。
公式サイトは[Mamba.jl](https://mambajl.readthedocs.org/en/latest/tutorial.html#mcmc-simulation)である。
-----------------------------------
## The Mamba Package
Mambaは、MCMCを介して一般的なベイジアンモデルを求めるためにデザインされたjuliaパッケージである。OpenBUGSやJAGSのように幅広いモデルと分布を扱うことができ、モデル特定のためのsyntaxを提供している。

以下、このレポジトリの各項目の説明である。
### Tutorial

参考文献
[NUTSとハミルトニアンモンテカルロ法](https://drive.google.com/file/d/0Bw-J75fYQ33NaWxjb2VwMDU4cjg/view)



## The Mamba Package
Mamba [86] is a julia [4] package designed for general Bayesian model fitting via MCMC. Like OpenBUGS and JAGS, it supports a wide range of model and distributional specifications, and provides a syntax for model specification. Unlike those two, and like PyMC, Mamba provides a unified environment in which all interactions with the software are made through a single, interpreted language. Any julia operator, function, type, or package can be used for model specification; and custom distributions and samplers can be written in julia to extend the package. Conversely, interactions with and extensions to OpenBUGS and JAGS can involve three different programming environments — R wrappers used to call the programs, their DSLs, and the underlying implementations in Component Pascal and C++. Advantages of a unified environment include more flexible model specification; tighter integration with supplied functions for convergence diagnostics and posterior inference; and faster development, testing, and debugging of extensions. Advantages of the BUGS DSLs include more concise model specification and facilitation of automated sampling scheme formulation. Indeed, sampling schemes must be selected manually in the initial release of Mamba. Nevertheless, Mamba holds other distinct advantages over existing offerings. In particular, it provides arbitrary blocking of model parameters and designation of block-specific samplers; samplers that can be used with the included simulation engine or apart from it; and command-line access to all package functionality, including its simulation API. Likewise, advantages of the julia language include its familiar syntax, focus on technical computing, and benchmarks showing it to be one or more orders of magnitude faster than R and Python [3]. Finally, the intended audience for Mamba includes individuals interested in programming in julia; who wish to have low-level access to model design and implementation; and, in some cases, are able to derive full conditional distributions of model parameters (up to normalizing constants).

Mamba allows for the implementation of an MCMC sampling scheme to simulate draws for a set of Bayesian model parameters (\theta_1, \ldots, \theta_p) from their joint posterior distribution. The package supports the general Gibbs [29][34] scheme outlined in the algorithm below. In its implementation with the package, the user may specify blocks \{\Theta_j\}_{j=1}^{B} of parameters and corresponding functions \{f_j\}_{j=1}^{B} to sample each \Theta_j from its full conditional distribution p(\Theta_j | \Theta \setminus \Theta_{j}). Simulation performance (efficiency and runtime) can be affected greatly by the choice of blocking scheme and sampling functions. For some models, an optimal choice may not be obvious, and different choices may need to be tried to find one that gives a desired level of performance. This can be a time-consuming process. The Mamba package provides a set of julia types and method functions to facilitate the specification of different schemes and functions. Supported sampling functions include those provided by the package, user-defined functions, and functions from other packages; thus providing great flexibility with respect to sampling methods. Furthermore, a sampling engine is provided to save the user from having to implement tasks common to all MCMC simulators. Therefore, time and energy can be focused on implementation aspects that most directly affect performance.

_images/gibbs.png
Mamba Gibbs sampling scheme

A summary of the steps involved in using the package to perform MCMC simulation for a Bayesian model is given below.

Decide on names to use for julia objects that will represent the model data structures and parameters (\theta_1, \ldots, \theta_p). For instance, the Tutorial section describes a linear regression example in which predictor \bm{x} and response \bm{y} are represented by objects x and y, and regression parameters \beta_0, \beta_1, and \sigma^2 by objects b0, b1, and s2.
Create a dictionary to store all structures considered to be fixed in the simulation; e.g., the line dictionary in the regression example.
Specify the model using the constructors described in the MCMC Types section, to create the following:

A Stochastic object for each model term that has a distributional specification. This includes parameters and data, such as the regression parameters b0, b1, and s2 that have prior distributions and y that has a likelihood specification.
A vector of Sampler objects containing supplied, user-defined, or external functions \{f_j\}_{j=1}^{B} for sampling each parameter block \Theta_j.
A Model object from the resulting stochastic nodes and sampler vector.
Simulate parameter values with the mcmc() function.
Use the MCMC output to check convergence and perform posterior inference.
Next  Previous



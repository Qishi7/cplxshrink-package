# cplxshrink

Bayesian shrinkage regression for complex data with a shared local scale for
each real/imaginary coefficient pair. Scalar real regression is also available.
No intercept is added automatically; include a column of ones if needed
(the intercept coefficient is then subject to the same shrinkage prior).

## Install

A working R installation (>= 4.1) and C++ toolchain are required.
In R, install the dependencies:

```r
install.packages(c("Rcpp", "RcppArmadillo"))
```

From a terminal, install the supplied source archive:

```sh
R CMD INSTALL cplxshrink_0.1.0.tar.gz
```

Alternatively, run `R CMD INSTALL cplxshrink` from the directory containing
this source folder. To run its tests, also install `testthat` and `jsonlite`.

## Fit and summarize

```r
library(cplxshrink)
set.seed(42)
X <- matrix(rnorm(500) + 1i * rnorm(500), 100, 5)
beta <- c(1 + 1i, -0.5 + 0.5i, 0, 0, 0)
y <- drop(X %*% beta) + rnorm(100) + 1i * rnorm(100)
fit <- cplx_shrink(y, X, prior = "gdp")
coefficient_summary(fit)
coef(fit)
```

`coef(fit)` returns posterior mean coefficients, complex for a complex fit
and real for a real fit. `coefficient_summary()` gives means, standard
deviations, and marginal central intervals for each real and imaginary
coordinate. Draws remain available in `fit$beta` (real columns followed by
imaginary columns), `fit$w`, `fit$s`, and `fit$hyper`.

Choose `prior = "student"` (Student-t, the default), `"lasso"` (Bayesian
lasso), or `"gdp"` (generalized double Pareto). Use `model = "real"` for
real data. Optional `relation`, `hyper`, `iterations`, `warm`, `thin`, and
`start` arguments retain their existing meanings; see `?cplx_shrink` for
parameter definitions. Set a seed immediately before fitting for repeatability.
After installation, run the bundled demonstration:

```r
source(system.file("examples", "demo.R", package = "cplxshrink", mustWork = TRUE))
```

## Limits

The likelihood relation is fixed, with modulus below one; coefficient priors
are proper. The noise scale `s` is real-component variance, so complex noise
covariance is `2*s*I`. Gamma distributions use shape and rate. Hyperparameter
defaults depend on the model; they do not automatically implement the paper's
matched-prior comparisons. The model conventions are detailed in the function help.

A short run demonstrates code use; it does not establish MCMC convergence or
reproduce every manuscript simulation or the complete fMRI analysis. The
package does not implement likelihood-relation learning or the full structural
ablation workflows. Assess convergence and Monte
Carlo error with suitable multiple-chain diagnostics for substantive inference.
Marginal coefficient intervals are not joint selection regions. This minimal
package contains no human data, empirical outputs, or full replication workflows.

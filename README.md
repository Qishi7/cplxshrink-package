# cplxshrink

Bayesian shrinkage regression for complex data with a shared local scale for
each real/imaginary coefficient pair. Scalar real regression is also available.
No intercept is added automatically; include a column of ones if needed
(the intercept coefficient is then subject to the same shrinkage prior).

## Install

A working R installation (>= 4.1) and C++ toolchain are required.
In R, install from GitHub:

```r
install.packages("remotes")  # if needed
remotes::install_github("chenghanyustats/cplxshrink-package")
```

For source installation, first install `Rcpp` and `RcppArmadillo`.

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
n <- 100
p <- 5
X <- matrix(rnorm(n * p), n, p)
beta <- c(1 + 1i, -0.5 + 0.5i, 0, 0, 0)
y <- drop(X %*% beta) + rnorm(100) + 1i * rnorm(100)
fit <- cplx_shrink(y, X, prior = "gdp")
coef(fit)
coefficient_summary(fit)
```

`coef(fit)` returns posterior mean coefficients, complex for a complex fit
and real for a real fit. `coefficient_summary()` gives means, standard
deviations, and marginal central intervals for each real and imaginary
coordinate. Draws remain available in `fit$beta` (real columns followed by
imaginary columns), `fit$w`, `fit$s`, and `fit$hyper`.

Choose `prior = "student"` (Student-t, the default), `"lasso"` (Bayesian
lasso), or `"gdp"` (generalized double Pareto). Use `model = "real"` for
real data. Optional `relation`, `hyper`, `iterations`, `warm`, `thin`, and
`start` arguments control fixed error relation, prior hyperparameters, total
iterations, warmup, thinning, and initial values; see `?cplx_shrink` for details. Set a seed immediately before fitting for repeatability.
After installation, run the bundled demonstration:

```r
source(system.file("examples", "demo.R", package = "cplxshrink", mustWork = TRUE))
```

## Manuscript calibration (separate from the generic demo)

The primary simulation comparison uses real `X`, proper errors (`relation = 0`),
and `a_s = b_s = 0.5` for each error variance. Specify the following `hyper`
lists explicitly; package defaults are unchanged.

| Prior | Paired complex fit (`model = "complex"`) | Each scalar fit (`model = "real"`) |
| --- | --- | --- |
| Student-t | `list(a_s=0.5, b_s=0.5, nu=1)` | `list(a_s=0.5, b_s=0.5, a_tau=1, b_tau=1)` |
| Lasso | `list(a_s=0.5, b_s=0.5, r=0.5, delta=0.5)` | `list(a_s=0.5, b_s=0.5, r=0.5, delta=0.8966996574)` |
| GDP | `list(a_s=0.5, b_s=0.5, alpha=1, eta=1)` | `list(a_s=0.5, b_s=0.5, alpha=1, eta=1.3403844863)` |

For example, using `y` and real `X` from the generic demo, the calibrated
independent scalar GDP fits use:

```r
paper_gdp <- list(a_s=0.5, b_s=0.5, alpha=1, eta=1.3403844863)
fit_R <- cplx_shrink(Re(y), X, prior="gdp", model="real", hyper=paper_gdp)
fit_I <- cplx_shrink(Im(y), X, prior="gdp", model="real", hyper=paper_gdp)
```

M-ReIm requires two independent fits. M-Scalar-Pooled instead requires an
explicit block-diagonal real design and stacked response, sharing one error
variance and, for lasso, one global rate. A single ordinary fitting call does
not construct these simulation strategies. These parameter examples do not
supply the paper's datasets, seed mapping, four-chain schedules, diagnostics,
scoring, or full ablation and fMRI workflows. For the H1 sensitivity only,
the scalar lasso uses `delta=0.5` and scalar GDP uses `eta=1`.

## Parameter notation

The package's noise variance `s` is the manuscript's sigma-squared (\(\sigma^2\)),
the variance of each real error component, not total complex variance.
`a_s` and `b_s` correspond to \(a_\sigma\) and \(b_\sigma\), with
`s ~ IG(a_s, b_s)` and density proportional to `s^(-a_s-1)*exp(-b_s/s)`.
`w` denotes the local scale. Complex Student `nu` is \(\nu\); scalar Student
`a_tau` and `b_tau` are the shape and scale of the inverse-gamma local scale
(`a_tau=b_tau=1` gives the paper's scalar t with two degrees of freedom).
Lasso `delta` is \(\delta_L\), the rate in `L ~ Gamma(r, delta)`;
GDP `alpha` and `eta` are the shape and rate in `lambda[j] ~ Gamma(alpha, eta)`.

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

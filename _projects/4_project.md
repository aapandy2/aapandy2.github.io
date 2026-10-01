---
layout: page
title: Arbitrage-free local volatility calibration for SPX options
description:
img: assets/img/localvol_project_cover.png
importance: 4
category: work
related_publications: false
---

### Background

The celebrated Black-Scholes equation[^bs] was a major breakthrough in understanding how to price financial derivatives.
The equation is a partial differential equation (PDE) of the form, for a European option:

$$
\frac{\partial V}{\partial t} + \frac{1}{2}\sigma^2 S^2 \frac{\partial^2 V}{\partial S^2} + rS\frac{\partial V}{\partial S} - rV = 0 \tag{1}
$$

where \\(V(S,t)\\) is the option price as a function of the underlying price \\(S\\) and time \\(t\\), \\(\sigma\\) is the (here, constant) volatility, and \\(r\\) is the risk-free rate.

Despite all the fanfare, it turns out that (1) as written does not fit real market data for European options.
To make it fit, one must consider a slightly different model, namely one where the volatility is not constant. Instead, we require it to vary as a function of option strike \\(K\\) and time to expiration \\(T\\).
Making these modifications leads to a new equation, often attributed to Dupire[^dupire].

The so-called Dupire forward PDE takes the form:

$$
\frac{\partial C}{\partial T} - \frac{1}{2}\sigma(K,T)^2 K^2 \frac{\partial^2 C}{\partial K^2} + rK\frac{\partial C}{\partial K} = 0 \tag{2}
$$

where \\(C(K,T)\\) is the price of a European call as a function of strike \\(K\\) and time to maturity \\(T\\) (rather than of \\(S\\) and \\(t\\)), and \\(\sigma(K,T)\\) is the local volatility function ("local vol") we are solving for. (A dividend yield \\(q\\) would add a \\(+qC\\) term and change \\(r \to r-q\\) in the drift term above; we instead fold dividends directly into the forward price in the log-moneyness reformulation used later, rather than carrying them explicitly here.) A full derivation from (1) to (2) is available online.[^blog]

Note that the Dupire PDE is formulated in option-chain coordinates, strike \\(K\\) and maturity \\(T\\), rather than the underlying's coordinates \\((S, t)\\) that (1) uses. (1) and (2) are consistent in the sense that the local volatility surface \\(\sigma(K,T)\\) that solves (2) can, in principle, be substituted into (1) (now letting \\(\sigma\\) depend on \\(S\\) and \\(t\\) rather than holding it constant) to price any individual option, reproducing the same price implied by the calibrated \\(\sigma(K,T)\\). This project implements and solves only the forward equation (2); it does not independently re-derive the equivalence by also running a backward solve of (1). Instead, correctness is checked two other ways: against the closed-form Black-Scholes price in the constant-volatility limit, and against the no-arbitrage conditions described later in this project. Note also that the Black-Scholes equation (1) is a _backward_ PDE, in that we know the payoff function (price as a function of strike) exactly at maturity, and must solve backward in time for the price before then. In (2)'s option-chain coordinates, we instead solve forward in the time parameter \\(T\\), the time to maturity. This difference introduces sign changes in some terms between (1) and (2).

We also treat every quote in the chain uniformly as a call (which is possible thanks to put-call parity, though achieved by repricing each quote at its own solved implied volatility rather than a direct conversion), and restrict to out-of-the-money strikes relative to the forward price (dropping intrinsic-value-dominated, parity-redundant ITM quotes — liquidity itself is enforced separately, by the delta-band filter below).

The goal of this short project is to fit real market data for a European-style option to this model, in this case for the S&P 500 index option (ticker SPX).

### Fitting local vol

In order to calibrate the Dupire model to real market data for SPX, we effectively need to construct the function \\(\sigma(T, K)\\) which, when plugged into (2), produces options prices \\(C\\) in agreement with market data, at least for liquid strikes (that is, strikes that are actively being traded and thus have accurate prices), to within the bid-ask spread.
In fact, (2) can be solved for \\(\sigma\\) directly:

$$
\sigma(K,T) = \sqrt{\frac{2\left(\dfrac{\partial C}{\partial T} + rK\dfrac{\partial C}{\partial K}\right)}{K^2 \dfrac{\partial^2 C}{\partial K^2}}}
$$

So, given the form of (2), the most natural approach to this might be to directly fit a smooth surface to the observed option prices \\(C(K,T)\\) (or the implied volatility surface, as (2) can equivalently be written in terms of IV instead of direct prices), and evaluate this formula directly on the fitted surface.

The challenge with these direct-fitting methods is that it is very easy to generate a fit which violates arbitrage constraints. In plain terms: *calendar arbitrage* means an option's price would need to decrease as its maturity gets longer, which can't happen in a sane market; *butterfly arbitrage* means the implied probability of the underlying ending up at some price would need to be negative, which is nonsensical. Numerical errors in the fitted surface are also magnified by the need to take derivatives, often leading to an ill-behaved local vol surface despite having well-behaved market prices.

There are many tradeoffs in options calibration, most notably between parametric and nonparametric fits, which ultimately boil down to a bias-variance tradeoff: parametric fits are designed to be smooth and asymptotically well-behaved at extreme strikes, but can be a poor fit to the data, whereas nonparametric fits can match the data cleanly but suffer from numerical blow-ups and violations of arbitrage-freeness.

For these reasons, in this work we implement a robust method which is harder to fit, but guarantees arbitrage-freeness and can fit the data (at least for the test case of SPX options) to within the bid-ask spread for liquid strikes.

### Andreasen-Huge method

The Andreasen-Huge method[^ah] effectively integrates the forward Dupire PDE starting from the known payoff function at \\(T = 0\\), fitting the advanced timelevel (n+1) prices in our implicit solver. The PDE evolution ensures arbitrage-freeness in the progression from one time slice to the next. In practice, the equation is reformulated in log-moneyness coordinates for numerical stability, following Andreasen & Huge's formulation:

$$
\frac{\partial c}{\partial t} = \frac{1}{2}\sigma(y,t)^2\left(\frac{\partial^2 c}{\partial y^2} - \frac{\partial c}{\partial y}\right)
$$

where \\(y = \ln(K/F)\\) is the log-moneyness (\\(F\\) being the forward price), \\(c(y,t) = C(K,T)e^{rT}/F\\) is the call price re-expressed in forward, undiscounted units as a function of \\(y\\) and \\(t\\), and \\(\sigma(y,t)\\) is the local volatility, now written as a function of moneyness and time rather than strike and maturity.

In the Andreasen-Huge method we fit the local volatility at each maturity using classical least squares in between fitted knots (placed at each strike) in the region where those strikes are sufficiently liquid. The fitted surface in price space is then evolved sequentially from the previous maturity's solved price grid, and the PDE evolution ensures we avoid violating the arbitrage constraints. The fitting procedure has a regularization parameter, \\(\lambda\\), which ensures that adjacent knots do not differ too much from one another (effectively an L2 penalty).

Figure 1, below, shows the region of liquid strikes — defined as strikes whose Black-76 (the standard Black-Scholes-style formula for options on a forward price rather than spot) option delta falls between 0.10 and 0.90, excluding deep in- or out-of-the-money strikes as "periphery" because they are thinly traded and their quoted prices are less trustworthy — along with our fit to data in that region. This delta-based liquidity filter is a separate, complementary check from the bid-ask spread: the delta band decides which strikes are trusted enough to calibrate against at all, while the bid-ask spread separately measures fit quality (the model price must land within half the real bid-ask spread) for the strikes that pass the filter. The middle panel shows the residuals (error in the fit) along with the bid-ask spread, and the right panel shows the fitted local vol curve. The top row of plots illustrates a very close fit with low regularization — in other words, low bias, high variance, meaning that we fit the data precisely but also inherit noise in our fit. The second row of plots illustrates a large amount of regularization: the point when we start incurring errors larger than the bid-ask spread to produce a very smooth the fitted local-vol curve. The precise choice of \\(\lambda\\) in a production system is a modeling decision outside the scope of this project.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include video.liquid path="/assets/plotly/localvol_slice_slider.html" class="rounded z-depth-1" width="100%" height="800" %}
  </div>
</div>
<div class="caption">
  Per-expiry calibration fit: implied volatility smile, price residual vs. half the real bid-ask spread, and the calibrated local volatility curve itself. Top row is the low-regularization (near-exact) fit; bottom row applies 10,000&times; more smoothing when fitting the same data.
</div>

Our fit is tightly restricted to the most liquid strikes because strikes beyond that band carry little real information about volatility: away from the money, an option's price is dominated by intrinsic value, or for deep OTM strikes shrinks toward zero, rather than by the time value that's actually sensitive to \\(\sigma\\). Including those strikes in the fit would mostly add noise rather than signal to the calibrated local vol curve.

We also verify that our fit is arbitrage-free, checking this directly against the calibrated price surface rather than assuming it from the model's construction. For calendar arbitrage: at fixed moneyness, option prices must be non-decreasing in maturity; we check this across a grid of 120 moneyness points by 25 maturities (3,000 points total) and find zero violations. For butterfly arbitrage: the implied risk-neutral density (proportional to the second derivative of price with respect to strike) must be non-negative everywhere; we check this across all 10,624 grid points and again find no violations, with the minimum observed density comfortably above zero.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include video.liquid path="/assets/plotly/localvol_arbitrage_proof.html" class="rounded z-depth-1" width="100%" height="590" %}
  </div>
</div>
<div class="caption">
  Left: price at fixed moneyness across the whole term structure (monotone everywhere &mdash; 0 violations across the full grid). Right: the butterfly/density condition evaluated directly on the calibrated price grid.
</div>

### Conclusion

In this project we present a calibrated, arbitrage-free fit to real SPX options chain data, producing a similarly well-behaved local vol surface. This local vol surface could, in principle, be used to price more exotic derivatives products[^exotic] or to directly derive live trading strategies for the options or the underlying.

### References

[^bs]: Black, F. and Scholes, M. (1973), "The Pricing of Options and Corporate Liabilities," *Journal of Political Economy*, 81(3), 637–654. See also: [Wikipedia — Black-Scholes equation](https://en.wikipedia.org/wiki/Black%E2%80%93Scholes_equation).
[^dupire]: Dupire, B. (1994), "Pricing with a Smile," *Risk Magazine*, 7(1), 18–20.
[^blog]: [Derivation of the Dupire PDE from Black-Scholes](https://quantdev.blog/posts/dupire-pde/).
[^ah]: Andreasen, J. and Huge, B. (2011), "Volatility Interpolation," *Risk Magazine* / *Wilmott*. [SSRN link](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1694700).
[^exotic]: [Wikipedia — Exotic option](https://en.wikipedia.org/wiki/Exotic_option).

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

A <a href="https://en.wikipedia.org/wiki/Local_volatility">local volatility</a> model is the minimal way to price and hedge options that don't already trade in a liquid market of their own (barriers, cliquets, autocallables, and other path-dependent structures). Rather than guessing at a volatility process, local vol is calibrated directly from real, observed option prices, producing a single model that reprices every listed option exactly while also pricing anything else consistently with the market.

This project builds a full local volatility calibration pipeline for SPX index options, from raw market data through a PDE-based calibration (following <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1694700">Andreasen &amp; Huge</a>) to a directly-verified, arbitrage-free result. Along the way, the project surfaced and fixed a genuine data bug: some monthly SPX expiries silently mix two distinct, non-fungible option series (AM-settled SPX and PM-settled SPXW) under one nominal expiration date, corrupting the smile unless separated.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    <strong>100%</strong> of liquid quotes reprice within half their bid-ask spread, across all 26 real market expiries, with <strong>zero</strong> calendar or butterfly arbitrage violations &mdash; checked directly against the calibrated surface, not assumed.
  </div>
</div>

### Calibration fit, by expiry

Each real SPX expiry is calibrated independently (one PDE solve per expiry, bootstrapped sequentially for calendar consistency). The slider below shows the fit quality for every expiry, plus a second row showing what happens to the same fit under much heavier regularization &mdash; the top row is the production configuration (exact fit to every liquid quote); the bottom row uses a smoothness penalty 10,000&times; larger, trading some fit quality for a visibly smoother local vol curve.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include video.liquid path="/assets/plotly/localvol_slice_slider.html" class="img-fluid rounded z-depth-1" width="100%" height="950" %}
  </div>
</div>
<div class="caption">
  Per-expiry calibration fit: implied volatility smile, price residual vs. half the real bid-ask spread, and the calibrated local volatility curve itself. Top row is the production (near-exact) fit; bottom row applies 10,000&times; more smoothing to the same data.
</div>

### Verifying the surface is actually arbitrage-free

"Arbitrage-free" is a checkable claim, not a hoped-for property. The two conditions that make a local vol surface valid &mdash; prices non-decreasing in maturity (no calendar arbitrage), and a non-negative implied risk-neutral density (no butterfly arbitrage) &mdash; are verified directly against the calibrated PDE output below, rather than assumed from the model's construction alone.

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include video.liquid path="/assets/plotly/localvol_arbitrage_proof.html" class="img-fluid rounded z-depth-1" width="100%" height="550" %}
  </div>
</div>
<div class="caption">
  Left: price at fixed moneyness across the whole term structure (monotone everywhere &mdash; 0 violations across 3,000 checked points). Right: the butterfly/density condition evaluated directly on the calibrated price grid (0 violations across 10,624 checked points).
</div>

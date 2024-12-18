---
layout: page
title: Modeling dissipation in relativistic fluid systems
description:
img: assets/img/hydro_project_cover.webp
importance: 1
category: work
related_publications: false
---

### Background

Fluids are materials that continuously deform (_flow_) when acted on by an external force.  Such materials are central to the human experience, which has no doubt been shaped by the flow of water (e.g. tides, ocean currents) and air (wind, storms).  As a result, modeling fluids has been a major goal of theoretical physics research for millenia.

Fluid models are typically made with two central ingredients, namely:
1. conservation of energy and momentum
2. consistency with the laws of thermodynamics.

Incorporating these two features in their most basic form yields the Euler equations, which were written down by their namesake in the 1750's and assume a fluid which is constantly in thermodynamic equilibrium.  This turns out to mean that the fluid cannot generate entropy through processes like viscosity or heat conduction (which are present in real fluids).  It wasn't until the 1800's that the Navier-Stokes equations were put forth, which incorporated these effects.

A remarkable feature of modern physics is that the same recipe for constructing a fluid model works even for exotic materials like the matter composing neutron stars or quark-gluon plasma.  In these cases, however, one must add an additional ingredient: relativity.  The table below lists four fluid theories along with the ingredients in each.


| Equations          | Conserves energy & momentum? | Consistent with relativity? | Can generate entropy? |
| ---                | ---                          | ---                         | ---                   |
| Euler              | ✔                            | ✘                           | ✘                     |
| Navier-Stokes      | ✔                            | ✘                           | ✔                     |
| Rel. Euler         | ✔                            | ✔                           | ✘                     |
| Rel. Navier-Stokes | ✔                            | ✔                           | ✔                     |

<br>

Based on the table, it would seem that the relativistic Navier-Stokes equations should be the model of choice for the aforementioned exotic fluid systems, so long as entropy-generating effects (viscosity, heat conduction) are relevant.  Unfortunately, the story is not so simple, as _"the" relativistic Navier-Stokes equations do not exist_.

### Where is relativistic Navier-Stokes?

It turns out that when building a fluid theory with the three ingredients
1. conservation of energy and momentum
2. consistency with the laws of (non-equilibrium) thermodynamics
3. consistency with relativity

one runs into a number of theoretical issues.  The first "relativistic Navier-Stokes" theories, written down in the 1950's, were prone to severe pathologies (to give a specific example: they predict that a cup of water at room temperature would explode in a tiny fraction of a second).

Another set of "relativistic Navier-Stokes" equations was written down by Müller, Israel, and Stewart in the 60's-70's.  This theory, though empirically more well-behaved than the theories of the 50's, still suffers from some pathologies (specifically, shockwave solutions do not exist in some cases) which would render it unusable in some astrophysics contexts.  That said, offshoots of the Müller-Israel-Stewart theory have been successfully used to model quark-gluon plasma, but rigorous proofs that these theories are well-behaved have remained elusive due to the complexity of the theory's equations.

### BDNK theory

In the early 2020's, a series of works put forward a theory which served to fix the issues with the theories of the 1950's.  This theory is now known as BDNK theory, which is a promising framework for modeling relativistic fluids with dissipation in astrophysical contexts, where shockwaves are prevalent and the Müller-Israel-Stewart theory is likely to run into problems.

My work (done with collaborators Frans Pretorius and Elias Most) sought to be the first to solve the BDNK equations in "realistic" scenarios (that is, in cases where the equations cannot be solved with pen-and-paper methods -- cases where computer simulations are required).

### Numerical exploration of BDNK theory

Paper: <a href="https://arxiv.org/abs/2104.00804">A numerical exploration of first-order relativistic hydrodynamics</a> <br>
Authors: _Alex Pandya, Frans Pretorius_

In this paper, we were the first to numerically solve the BDNK equations in scenarios with variation both in space and time, albeit restricted to "slab" symmetry (variation only in one space dimension) and with simplified microphysics (i.e., the fluid was something like a photon gas wherein the stress-energy tensor is trace-free).

To briefly summarize the main results, we found:
- dissipation in BDNK theory behaves as expected (within the regime of validity of the theory)
- BDNK theory agrees with Müller-Israel-Stewart theory (within the regime of validity)
- BDNK theory shockwaves exist for appropriate choices of model parameters.

**TODO: insert figures**

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/pexels-engin-akyurt-6069112-960x540-30fps.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    A simple, elegant caption looks good between video rows, after each row, or doesn't have to be there at all.
</div>

### Conservative finite volume method for BDNK theory

Paper: <a href="https://arxiv.org/abs/2201.12317">Conservative finite volume scheme for first-order viscous relativistic hydrodynamics</a> <br>
Authors: _Alex Pandya, Elias R. Most, Frans Pretorius_

This work is the first to develop a numerical method for the BDNK equations which is suitably stable and accurate for supercomputer simulations of neutron stars and other astrophysical systems.  The method builds upon the conservative finite volume method commonly used for the relativistic Euler equations.

The main results include a detailed description of the numerical scheme as well as its performance on a suite of challenging test problems.

**TODO: insert figures**

### BDNK theory with more realistic microphysics

Paper: <a href="https://arxiv.org/abs/2209.09265">Causal, stable first-order viscous relativistic hydrodynamics with ideal gas microphysics</a> <br>
Authors: _Alex Pandya, Elias R. Most, Frans Pretorius_

In this project, we work out the first set of BDNK model parameters ("hydrodynamic frame") consistent with the BDNK "well-behavedness" constraints for a non-conformal fluid.  Specifically, we build upon the "gamma-law" ideal gas equation of state often employed in initial studies of neutron stars and other astrophysical fluid systems.

The main results include:
- the ideal gas model parameters ("transport coefficients")
- an explanation for "frame dynamics" in non-equilibrium relativistic hydrodynamics
- a demonstration that shockwaves exist in BDNK theory
- the first investigation of heat conduction in BDNK theory.

**TODO: insert figures**

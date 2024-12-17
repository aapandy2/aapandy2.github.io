---
layout: page
title: Modeling dissipation in relativistic fluid systems
description:
img:
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



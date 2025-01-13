---
layout: page
title: Numerical explorations of strong-field gravity
description:
img: assets/img/gravity_project_cover.webp
importance: 1
category: work
related_publications: false
---

### Background

Einstein's theory of general relativity has been remarkably successful over the past century, correctly predicting the existence of black holes, gravitational waves, the cosmic microwave background, and many other fascinating exotic phenomena.  GR's ability to describe these systems derives in part from the complexity of its equations, which makes extracting future predictions quite tricky.

In the sections below I briefly outline a couple of projects which aim to explore how gravity works (according to GR) in the _strong-field regime_, where the equations are pushed to their limits.  To be specific, below we consider the structure of spacetime inside of black holes, and various formal similarities between the nonlinear behavior of gravity and turbulence in fluid mechanics.

### What's inside of black holes?

Paper: <a href="https://arxiv.org/abs/2002.07130">The rotating black hole interior: Insights from gravitational collapse in AdS_3 spacetime</a> <br>
Authors: _Alex Pandya, Frans Pretorius_

It is widely believed that black holes have an _exterior_ geometry well-described by the famous Kerr solution to the Einstein equations of GR[^1].  This notion is closely tied to the fact that any special features (e.g. a 'bump' in the horizon) either fall in, or, if they're moving fast enough not to fall in, rapidly escape and fly off to infinity.

The same behavior does not occur inside the black hole.  There, escape is by definition impossible, so the interior geometry is sensitive to the stuff that fell in.  The Kerr solution is very special (it describes a black hole which never formed -- by assumption it was always just there, spinning along in an otherwise empty universe) and thus does not describe the interior geometry of a real black hole formed by the messy collapse of a large star.

In order to investigate the interior structure of a black hole formed from the gravitational collapse of matter, I (along with my advisor Frans Pretorius) simulated such a collapse numerically in the context of a particular toy model[^2].  We found that the structure of the black hole interior in this case depends strongly on the black hole's spin; the precise dependence is summarized in the figure below.

<div class="row">                                                               
    <div class="col-sm mt-3 mt-md-0">                                           
        {% include figure.liquid loading="eager" path="assets/img/bh_interior.webp" title="Penrose diagrams for BH interior" class="img-fluid rounded z-depth-1" %}
    </div>                                                                      
</div>                                                                          
<div class="caption">                                                           
    Penrose diagrams demonstrating the different black hole interior structures observed as a function of the dimensionless spin parameter.
</div>

We also found the development of a "gravitational shock-wave", where an infalling observer would be crushed by a very rapid increase in spacetime curvature before they reach the center of the black hole (which in this context may not contain a singularity if the hole is spinning rapidly enough).

### Can gravity act like a fluid? 

Paper: <a href="https://arxiv.org/abs/2206.08854"></a>Dynamics of a nonminimally coupled scalar field in asymptotically AdS_4 spacetime<br>
Authors: _Alex Pandya, Justin L. Ripley_

GR and relativistic fluid mechanics share a similar mathematical structure: namely, they are both systems of coupled hyperbolic partial differential equations.  A direct result of this fact is that both frameworks can lead to the formation of singularities: in GR, they arise in the center of black holes; in fluid mechanics, they arise in droplet formation or in inviscid shocks.

In this paper we investigated a formal similarity between the dynamics of a scalar field coupled to gravity and a viscous fluid.  It turns out that both can be written with stress-energy tensors of the same form, provided one identifies a particular derivative of the scalar field with the fluid's flow velocity.  We also study the turbulent instability of anti-de Sitter (AdS) spacetime viewed through this perspective.

We find that the mapping from scalar field to viscous fluid does imply fluid-like behavior in the scalar; in fact, simple metrics indicate that the 'scalar fluid' is highly pathological (i.e. the energy density can dynamically change sign).  That said, we do observe interesting dynamics in the turbulent instability of AdS as a function of the non-minimal coupling between the scalar field and gravity.

#### Footnotes

[^1]: This is known as the Kerr hypothesis, and is closely related to the ongoing effort to prove the nonlinear stability of the Kerr solution (in other words, that a black hole relaxes to the Kerr geometry if you prod it).
[^2]: To be specific, we simulated gravitational collapse in an asymptotically anti-de Sitter (AdS) spacetime with three spacetime dimensions.  This toy model is useful because black holes are "flat" (i.e., two-dimensional) and using some mathematical tricks one can incorporate angular momentum while keeping PDEs which vary only in time and one spatial coordinate (the radius).

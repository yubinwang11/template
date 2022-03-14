---
layout: page
title: Learning Observer
description: A learning-based observer for state estimation of Direct Contact Membrane Distillation
img: /assets/img/dcmd/dcmd.png
importance: 0.2
category: [deep learning,observer design,state estimation]
---

## Introduction
The demand of world’s freshwater is constantly growing urgent as freshwater sources are getting exhausted and expected to exceed the available amount by 2040. Considering that seawater is a theoretically inexhaustible resource, seawater desalination is a promising solution to freshwater scarcity issue.

Direct Contact Membrane distillation (DCMD) is placed as a high potential desalination technique due to its impressive solutes’
rejection factor approximating 100%. MD represents also an emerging sustainable desalination method, combining
both thermal and membrane-based separation techniques. Thermal energy is used to change the phase of water, while
a hydrophobic membrane is built to isolate the vapor of distilled water from the feed aqueous solution. The transmembrane pressure induces the distilled water vapor to pass through the membrane from the hot feed side to the cooler permeate side. Better than other desalination processes, DCMD is operated with a lower hydrostatic pressure and lower temperatures, making this energy-friendly technique a
promising and sustainable method for water desalination.

<div  align="center">    
<img src="/assets/img/dcmd/dcmd.png" width = "500" height = "500" alt="DCMD module configuration" align=center />
</div>
<div class="caption">
    DCMD module configuration 
</div>

In this case, state estimation in DCMD system, which is modeled by nonlinear Differential Algebraic Equations (DAE) is crucial for controller design and system's monitoring.

## Methodology
A novel learning-based observer is proposed for state estimation for DCMD
system. The method consists of an encoder and decoderstructure. The encoder allows to transform the DAE system into
a linear form modulo an output injection in the latent spaceand the decoder helps in recovering the state estimate from
the latent state.

<div  align="center">    
<img src="/assets/img/dcmd/model.png" width = "500" height = "300" alt="DCMD module configuration" align=center />
</div>
<div class="caption">
    Auto-encoder-decoder model structure 
</div>

## Simulation Results
**code:** [dcmd_obs][dcmd_obs]

After training, the state estimations rise or drop rapidly to go beyond the real value temporarily and then start converging to the real value in finite time. After about 15s, all states estimates converge to the real values with minor steady state error, and we consider that the state estimation for DCMD system is completed.
<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/dcmd/x1.png' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/dcmd/x2.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    state estimation with the proposed Deep-Learning-Based Observer and real value
</div>

The figure recording relative estimation error shows that there exists a phase usually called ”oscillation phase” where big peaks appear resulting in high estimation errors. One can notice that the biggest peaks are associated with heat transfer rates. This can be explained by the fact that this type of attributes (heat transfer rate) takes values of the order of 1000, and starting the learning observer
with a zero initial condition is effort-costing for the proposed observer to reach these high values in short time. After the mentioned phase (approximately 5s), all relative estimation error curves start decreasing rapidly to zero. The convergence
is achieved after 15s.

<div  align="center">    
<img src="/assets/img/dcmd/relative error.png" width = "500" height = "300" alt="relative estimation error" align=center />
</div>
<div class="caption">
    relative estimation error
</div>



[dcmd_obs]: https://github.com/yubinwang11/obs_dcmd
---
layout: page
title: Vertical Takeoff and Landing Aircraft
description: Developing a fully autonomous passenger air vehicle 
img: /assets/img/vtol/cover2.jpg
importance: 1
category: [aerospace,robotics] 

---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/vtol/cover2.jpg' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
   Conceptual design of our eVTOL prototype 
</div>

[Electric vertical take-off and landing aircrafts (eVTOL)][vtol], which are powered electrically and have the capability to hover, take-off and land vertically, have been receiving increasing attention in the quest for fully autonomous passenger air vehicles (PAV). **The primary objective of our research team was to design and develop a scaled down novel eVTOL prototype** .

### Structural Design

First, structural design of the prototype was completed on SolidWorks. Carbon fibre rods were used to construct the frame and care was taken to prevent the localisation of stresses. The primary aim of the design was the minimization of weight, which has an inverse relation with endurance. To accomplish this, custom 3D-printed joints were designed which could combine the functionality of multiple standard joints. Additionally, CFRP was chosen as the primary load- bearing material due to its high strength-to-weight ratio. 

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/vtol/cad1.png' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/vtol/cad2.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Left: CAD drawings of frame structure, Right: CAD drawings of outer body
</div>

### Control System Design 

Next, the control system for the prototype was designed using [Pixhawk][pixhawk]. The airframe of our prototype was a distorted version of the default OctoX pixhawk architecture. The control system relied on a multitude of sensors for proper operation. Two inbuilt Inertial Measurement Units (IMU) provided orientation data to the flight controller. An externally connected GPS and compass were used for localization of the VTOL. A downward facing LiDAR was used to provide altitude data. A telemetry unit provided essential flight and battery status information in real time. The eVTOL prototype had functionalities of both a multi- copter and a fixed wing aircraft. Thus, the control algorithm for the eVTOL was not constant but dependent on its flight mode – Hover, Transition or Forward Flight. PID controllers were used as the low-level controllers. 

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/vtol/cs1.png' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/vtol/cs2.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Left: Octocopter layout of Pixhawk, Right: High level control system layout
</div>

### Testing 

A 6S Lithium Polymer (LIPO) battery was selected as the power source and was projected to provide a 10-minute hover endurance.
Initial test flights were highly unstable and this was attributed to a badly tuned flight controller. Subsequent flights involved progressive tuning of Pixhawk parameters and minor adjustments in structural design until level flight was achieved in Altitude- Hold mode. While initial testing was done in a UAV cage with the VTOL tethered to prevent damage in case of loss of control, later flights were conducted outdoors in an unconstrained environment.**The prototype was finally put through endurance tests and was successfully able to achieve the 10-minute projected milestone!**

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/vtol/old-assembly.JPEG' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/vtol/old-flight.jpg' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Left: Initial assembly without body, Right: Initial prototype in unstable tethered flight 
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/vtol/stable-assembly.jpeg' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/vtol/stable-flight.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Left: Final assembly with body and wings, Right: Prototype in stable flight after proper tuning  
</div>

### Summary 

With rising populations and traffic congestion, the interest and demand in unconventional means of transport is only expected to grow and VTOL’s are leading the way in Urban Air Mobility. This project was a deep-dive into the world of VTOL's!    
**In a duration of just 1 year, we were able to conceptualize, design, assemble, tune and test an eVTOL prototype. This was made possible through an amazing team , where each member had an important and specalized role. Finally, the mentorship provided by our supervisor, Prof Ng Bing Feng was invaluable!**

_This project was done under NTU's undergraduate research experience on campus (URECA) programme_ 
**Undergraduate Research Paper:** [Design and Development of VTOL](../../assets/pdf/ureca.pdf)     
**Cool Videos of eVTOL in flight:** [Videos](https://drive.google.com/drive/folders/1QImPjqJek4xfY9Z9K-MsnLM0R4nG40Qn?usp=sharing) 


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/vtol/happy.jpeg' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    The eVTOL team! 
</div>


[vtol]: https://evtol.news
[pixhawk]: https://pixhawk.org

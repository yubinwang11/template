---
layout: archive
title: "Research Interests"
permalink: /research/
author_profile: true
header:
  og_image: "research/ecdf.png"
---

My research interests are broadly in control and learning.

## Machine Learning + Control Theory
My ongoing work at EMANG @ KAUST is focused on learning-based observer design to estimate the state of nonlinear systems. With the Deep Learning techniques, it would not be challenging to compute the transformations of KKL observer which is big trouble to cope with. The encoder trained can push states into latent space where the nonlinear systems can be deemed as linear one, which is beneficial to enforce the observer dynamics.

<center class="half">
    <img src="/images/research/Model Structure.png" width="50%"/><img src="/images/research/errors.jpg" width="50%"/>
</center>

Recently, I am figuring out the advanced learning-based observer for nonlinear systems. Please keep your eyes on this if you're interested.

## Multi-Agent Reinforcement Learning
My work at MARMot @ NUS was releted to evader-puruit game via Multi-Agent Reinforcement Learning with communication. This implementation was on the basis of OpenAI Gym and MPE. The mission of pursuers is to cage the evader via multi-agent colaboration, to ensure the evader cannot escape from cage for enough long time in the bounded world. Please note the policy of evder is not greedy but trained which means the evader is smart enough to plan the best evadision route. The learning framework and algorithm is MAAC from our friends @ USC.

<iframe height=498 width=510 src="//player.bilibili.com/player.html?aid=250257286&bvid=BV1jv411P75D&cid=403689306&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

## Multi-Robot Systems
I conducted and participated in several projects broadly related to Robotics and Control at ANCL @ NEU:

### Distributed Multi-Robot Systems Source Hunting
This Project aimed to guide multi-robot explore the potential space with source and hunt unknown source via distributed optimization algorithm. This description of project is under construction. 

### Multi-UGV Formation Control
This work aims to serve as display show of ANCL UGV platform. I utilized potential field function to guide robots to reach each nearest destination at each timestep. And all desitinations can construct the shape of "ANCL".
<iframe height=498 width=510 src="//player.bilibili.com/player.html?aid=632798895&bvid=BV1Wb4y1U7xp&cid=403696803&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

### Multi-UAV Foramtion Display
This work was a UAV show when I was freshman with the collaboration of my colleagues. The testbed is crazyfile2.0 with assistance of motion capture systems OptiTrack. This video is provided by my collegue Zhongkun Liu. Thanks a lot!

<iframe height=498 width=510 src="https://ancl.com.cn/videos/crazySwarm.mp4">

<nbsp>

{% include base_path %}

{% assign ordered_pages = site.research | sort:"order_number" %}

{% for post in ordered_pages %}
  {% include archive-single.html type="grid" %}
{% endfor %}

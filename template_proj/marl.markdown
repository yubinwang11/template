---
layout: page
title: Evader Pursuit Game
description: Multi Agent Reinforcement Learning
img: /assets/img/flatland/cover.png
importance: 1
category: [machine learning,robotics] 

---

### Introduction     
The [Flatland Challenge][flatland] was a competition to foster progress in multi-agent reinforcement learning for the vehicle re-scheduling problem (VRSP).The “Flatland” Competition aimed to address the vehicle rescheduling problem by providing a simplistic 2D grid world environment to represent railway networks. The challenge addressed a real-world problem faced by many transportation and logistics companies around the world such as the Swiss Federal Railways and SBB. **It was an official competition of the [NeurIPS-2020][neurips] conference.** 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/flatland/trains.gif' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Toy problem in Flatland environment
</div>

### Problem Statement  
The overall goal of the challenge was to make all agents (trains) arrive at their target destination with a minimal travel time. However, unlike classic multi-agent pathfinding problems, Flatland posed singificant additional challenges in the form of a highly constrained, irreversible environment and a sparsity of important decisions. These challenges made it difficult to find solutions using standard RL approaches, and custom observation spaces, reward structures, network architectures needed to be contrived. 

### Our Solution
Our solution built upon the multi-agent decentralized RL framework for MAPF in grid environments proposed in our previous work, PRIMAL2. Taking inspiration from [PRIMAL2][primal], we constructed a rich feature based observation for agents and used a shallow fully connected neural network architecture. We used the popular A3C learning algorithm for training. Our solution drew inspiration from the use of traffic lights, which are effective in managing traffic at bottlenecks such as junctions and crossings. **Our solution placed fourth in the RL track of the compeition and we were also delighted to be coauthors of the Flatland paper at NeurIPS**! 

**Flatland Paper accepted at NeurIPS-2020 Compeition Track**: [Flatland Competition 2020: MAPF and MARL for Efficient Train Coordination on a Grid World][flatland_paper]     

**Code:** [Flatland Solution][code]

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/flatland/cover2.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
</div>

[flatland]: https://www.aicrowd.com/challenges/flatland-challenge 
[neurips]: https://neurips.cc/Conferences/2020/CompetitionTrack
[code]: https://github.com/marmotlab/flatland-challenge-neurips-2020 
[primal]: https://arxiv.org/abs/2010.08184
[flatland_paper]: https://arxiv.org/abs/2103.16511
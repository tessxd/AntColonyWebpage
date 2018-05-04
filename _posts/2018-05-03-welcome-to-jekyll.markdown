---
layout: post
title:  "Ant Colony Optimization"
date:   2018-05-03 16:56:34 -0700
categories: jekyll update
---
{% include math_support.html %}

## Background
{: .Background}

Ants, and other insects, provide insight into motion planning
for robots. The Ant Colony Optimization (ACO) algorithm was designed
to use the methods ants use to plan optimal paths. In this 
algorithm, "ants" deposit pheromones on their paths. Shorter paths
get more pheromones, leading the robot to the shortest path. 

## Problem Definition

In our final project, we implemented this algorithm on a dual drive
Romi robot with the goal: Determine the best path from the start or "nest" to a goal state or 
"food" using Ant Colony Optimization.

Specifically we wanted to:
 * Minimize path length from start cell to goal cell
 * Analyze the performance of ant colony optimization 

## Solution

Our implementation uses three steps introduced in M. Brand's paper Ant Colony Optimization
algorithm for robot path planning. 

The first step is to determine the most probable cell from around the robot. 
Using the equation shown below. 

![Probability of Steps](/assets/probablilityfunction.PNG)
*Equation 1: Probability [1]*

Where $$\tau$$ is the pheromone amount and $$\eta$$ is the heuristic information. 
The probability calculated for the the ant, $$k$$, moving to cell $$j$$ from 
cell $$i$$. $$N$$includes all the possible neighboring cells. 

The second step is to evaporate all the pheromones on the map.
This is to make sure that ants do not get stuck in one path, and 
continue to explore new paths. By exploring new paths, the ants
will be able to adjust dynamically to new obstacles. 

![Pheromone evaporation](/assets/pheromone.PNG)
*Equation 2: Evaporation [1]*

Where $$\tau$$ is once again the pheromone amount at a particular cell. And 
$$\rho$$ 

The final step in the ACO algorithm is to deposit pheromones
on the path taken by the ants. More pheromones are deposited
as an inverse function of how long the path is. The pheromone
deposition equation is shown below.

![Pheromone deposition](/assets/updateStep.PNG)  
*Equation 3: Deposition [1]*


Where $$C$$ is the length of the path found for each ant $$k$$.

# Software

To implement our solution in software, we had to first initialize
the environment. We choose to use a grid of cells, that mapped
to positions on our user interface. The algorithm takes in a current state
and goal state in the GUI coordinates. These are then transformed to 
cell coordinates. By discretizing the map, nearby cells are 
easily accessed, and moved to.

Using this setup, the main algorithm as described in the solution
section was written in an AntColonyPathPlanner() function. The code flow 
for this loop is shown below. 

![Code Flow for ACO](/assets/overall.PNG)
*[1]*


To dynamically update the path, the code flow below was used. 

![Code Flow for dynamic update](/assets/dynamicUpdate.PNG)
*[1]*


# Hardware

To implement this in hardware, we had to update our motion planner to use cells
instead of nodes. This transferred the most probable cell movement to 
instructions for each wheel motor. Using odometry, the wheel motions convert
back into our current state. Taking the current state allows us to once 
again calculate the most probable step in a loop until the goal cell is reached. 


## Results 

Below are a series of images showing our algorithm working. The blue 
represents the paths found, and the red path shows the best path found. 

![Working Simulation 1](/assets/bestPath.PNG)
*[1]*
![Working Simulation 2](/assets/behindObstacle.PNG)
*[1]*
![Working Simulation 3](/assets/workingSearchWithBuffers.PNG)
*[1]*
Dynamic updates were also successful in software. We added an add wall 
button to the GUI and after the robot started to move towards it's goal state a 
wall was added to the path. Below are pictures of the robot dynamically
updating it's path to account for a new obstacle. 

![Working Simulation 4](/assets/added wall.PNG)

Finally, here is a video of the robot working in hardware to navigate around
a wall specified by the environment.

<iframe width="560" height="315" src="https://www.youtube.com/embed/1tcBV76ae90" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


## Citations

[1] M. Brand, M. Masuda, N. Wehner and Xiao-Hua Yu, "Ant Colony Optimization algorithm for robot path planning," 2010 International Conference On Computer Design and Applications, Qinhuangdao, 2010, pp. V3-436-V3-440. doi: 10.1109/ICCDA.2010.5541300


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

Where $$\tau$$ is the pheromone amount and $$\eta$$ is the heuristic information. 
The probability calculated for the the ant, $$k$$, moving to cell $$j$$ from 
cell $$i$$. $$N$$includes all the possible neighboring cells. 

The second step is to evaporate all the pheromones on the map.
This is to make sure that ants do not get stuck in one path, and 
continue to explore new paths. By exploring new paths, the ants
will be able to adjust dynamically to new obstacles. 

![Pheromone evaporation](/assets/pheromone.PNG)

Where $$\tau$$ is once again the pheromone amount at a particular cell. And 
$$\rho$$ 

The final step in the ACO algorithm is to deposit pheromones
on the path taken by the ants. More pheromones are deposited
as an inverse function of how long the path is. The pheromone
deposition equation is shown below.

![Pheromone deposition](/assets/updateStep.PNG)  

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


To dynamically update the path, the code flow below was used. 

![Code Flow for dynamic update](/assets/dynamicUpdate.PNG)

# Hardware

## Results 

## Citations

[1] M. Brand, M. Masuda, N. Wehner and Xiao-Hua Yu, "Ant Colony Optimization algorithm for robot path planning," 2010 International Conference On Computer Design and Applications, Qinhuangdao, 2010, pp. V3-436-V3-440. doi: 10.1109/ICCDA.2010.5541300


You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To addsimply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/

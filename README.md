# Optimal-Control
> These are solution for the algorithm course of Princeton
## Table of Contents
* [Dynamics Review](#dynamics-review)
* [Dynamics Discretization and Stability](#dynamics-discretization-and-stability)
* [Lecture 3: Optimization Pt. 1](#lecture-3-optimization-pt-1)
* [8 Puzzle](#8-puzzle)
* [Kd-Trees](#kd-trees)
* [WordNet](#wordnet)
* [Seam Carving](#seam-carving)
* [Baseball Elimination](#baseball-elimination)
* [Boggle](#boggle)
* [Burrows Wheeler](#burrows-wheeler)
* [Contact](#contact)
<!-- * [License](#license) -->


## Dynamics Review
## Dynamics Discretization and Stability
To Transform from continuity to discretization, some methods are following:<br/>
### 1. Discret-time sim/dynamics  <br/>

### 2. Stability of discrete-time system<br/>

### 3. Forward/Backward Euler<br/>

Forward Euler(Emplicit): add energy.  <br/>
Backward Euler(Impicit): add dampping, use in game and physical simulation.  <br/>
- Impilicit methods are often "more" stable than explicit counterparts
- For forward simulation, solving implicit equation can be more expensive
- In many "direct" traj opt methods, they're not any more expensive to use

### 4. Zero/First-order hold on controls <br/>

## Lecture 3: Optimization Pt. 1
### 1. Notation

giving $g(x):\mathbb{R}^{(m)}\rightarrow\mathbb{R}^{(n)}$

$$\frac{ \partial g }{ \partial x }\in \mathbb{R}^{n \times m}$$

because

$$ g(x+\delta x)\approx \frac{ \partial g }{ \partial x }\delta x$$


### 2. Root Finding
- given $f(x)$, find $x*$ such that $f(x*)=0$ 

-- example: equilibrium point of continuous-time dynamics

- Closely realted fixef point such that $f(x*)=x*$

-- example: equilibrium for disrete-time dynamics
- Fixed point solution method
- N ewton's method(better than the above)

### 3. Minimization
$min f(x),f: \mathbb{R}^{n} \rightarrow \mathbb{R}$

convert it to root finding problem

Newton is a local root-finding method. Will converge to the cloest min, max or saddle

sufficient conditions

## 8 Puzzle
## Kd-Trees
## WordNet
## Seam Carving
## Baseball Elimination
## Boggle
## Burrows Wheeler
<!-- You don't have to answer all the questions - just the ones relevant to your project. -->

## Contact
changjingliu@sjtu.edu.cn


<!-- Optional -->
<!-- ## License -->
<!-- This project is open source and available under the [... License](). -->

<!-- You don't have to include all sections - just the one's relevant to your project -->

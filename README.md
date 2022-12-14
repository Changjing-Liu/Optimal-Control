# Optimal-Control
> These are solution for the algorithm course of Princeton
## Table of Contents
* [Lecture 1: Dynamics Review](#lecture-1-dynamics-review)
* [Lecture 2: Dynamics Discretization and Stability](#lecture-2-dynamics-discretization-and-stability)
* [Lecture 3: Optimization Pt. 1](#lecture-3-optimization-pt-1)
* [Lecture 4: Optimization Pt. 2](#lecture-4-optimization-pt-2)
* [Lecture 5: Optimization Pt. 3](#lecture-5-optimization-pt-3)
* [WordNet](#wordnet)
* [Seam Carving](#seam-carving)
* [Baseball Elimination](#baseball-elimination)
* [Boggle](#boggle)
* [Burrows Wheeler](#burrows-wheeler)
* [Contact](#contact)
<!-- * [License](#license) -->


## Lecture 1: Dynamics Review
## Lecture 2: Dynamics Discretization and Stability
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
- Given $f(x):\mathbb{R}^2\rightarrow\mathbb{R}$
$$\frac{\partial{f}}{\partial{x}}\in\mathbb{R}^{1\times n}$$ is a row vector

Giving $g(x):\mathbb{R}^{m}\rightarrow\mathbb{R}^{n}$

$$\frac{ \partial g }{ \partial x }\in \mathbb{R}^{n \times m}$$

because

$$ g(x+\Delta x)\approx \frac{ \partial g }{ \partial x }\Delta x$$

We will also define:

$$\nabla f(x)=(\frac{\partial{f}}{\partial{x}})^T\in\mathbb{R}^{n \times 1}$$

$$\nabla^2f(x) = \frac{\partial}{\partial{x}}(\nabla f(x))=\frac{\partial^22{f}}{\partial{x}^2}\in\mathbb{R}^{m \times n}$$

### 2. Root Finding
- given $f(x)$, find $x*$ such that $f(x*)=0$ 

- - example: equilibrium point of continuous-time dynamics

- Closely realted fixef point such that $f(x*)=x*$

- - example: equilibrium for disrete-time dynamics
- Fixed point solution method

Newton's method(better than the above)
- Fit a linear appr0ximation to $f(x)$:
$$f(x+\Delta{x})\approx f(x)+\frac{\partial{f}}{\partial{x}}\Delta{x}$$
- Set approximation to zero and solve for $\Delta x$:
$$f(x)+\frac{\partial{f}}{\partial{x}}\Delta{x}=0 \qquad \Longrightarrow \qquad \Delta{x}=-(\frac{\partial{f}}{\partial{x}})^{-1}f(x)$$
- Apply correction:
$$x \leftarrow x+\Delta{x}$$
- Repeat until convergence

Take-Away Message

- Quadratic convergence
- Can achieve machine precision
- Most expensive part: solving linear system
- Can impreove complexity by taking advantage of preblem structure
### 3. Minimization

$$ \min_{x} f(x),\qquad f: \mathbb{R}^{n} \rightarrow \mathbb{R} $$

convert it to root finding problem

Take Away Message:

- Newton is a local root-finding method. Will converge to the Cloest Fined point to the initial guess (min, max or saddle)

sufficient conditions

### 4. Regularization
use some tricks to fix the decent, and go downhill

## Lecture 4: Optimization Pt. 2
### 1. Line Search
Genernal intuition
- Often $\Delta{X}$ step from Newton is too big and overshoots the minimization
- To fix this check ${f(x+\Delta{x})}$ and "backtrack" until we get a "good" reduction
- Many stratagics exist
- A simplest effective one is "Armijo rule"
- Make sure step agrees with linearization within some tolerance.

Take Away Messages

- Nweton with simple, cheap modifications(" globalization strategies") is extremely effetive at finding local minima.

### 2. Constrained Minimization

#### 2.1 equality constraints
First-Order Necessary Conditions
$$\min_{x} \quad f(x)$$
$${s.t.\quad c(x)=0}$$

1) Need ${\nabla f(x)=0}$ in free directions
2) Need $c(x)=0$
![image](https://user-images.githubusercontent.com/43432899/197389536-3ada9b19-c901-4ab3-808e-61c379cd3376.png)


* Any non-zero component of ${\nabla f}$ must be in normal to the constraints surface/manifold
${\nabla{f}+ \lambda \nabla{c}=0}$ for some ${\lambda \in \mathbb{R}}$, where ${\lambda}$ is "Lagrange multipliter"/"Dual variable"

In genral:
$$\frac{\partial{f}}{\partial{x}}+\lambda^{T} \frac{\partial{c}}{\partial{x}} = 0,\lambda \in \mathbb{R}^{M}$$

- Based on this gradient condition, we define:
$L(x,\lambda)=f(x)+\lambda^{T}c(x)$, where ${L}$ denotes the "Lagrangian"

- Such that:
$$\nabla_{x}L(x,\lambda)=\nabla{f}+(\frac{\partial{c}}{\partial{x}})^T\lambda=0$$
$$\nabla_{\lambda}L(x,\lambda)=c(x)=0$$

We can solve this with Newton:
$$\nabla_x L(x+\delta x,\lambda +\delta\lambda)\approx\nabla_xL(x,\lambda)+\frac{\partial^{2}L}{\partial{x}^2}+\frac{\partial*2{L}}{\partial{x}\partial{\lambda}}\Delta\lambda=0$$

$$\nabla_\lambda L(x+\delta x,\lambda)\approx c(x)+\frac{\partial{c}}{\partial{x}}\Delta{x}=0$$

Gauss-Newton Method:

$$\frac{\partial^2{L}}{\partial{x^2}} = \nabla^2f + \frac{\partial{}}{\partial{x}}[(\frac{\partial{c}}{\partial{x}})^T\lambda]$$

- We often drop the second-order "constraint " term for its expensive to compute
- Called "gauss-newton"
- Slightly slower converagence than full newton(more itterations) but much cheaper per itteration

Take away message:
-May still need to regularize $\frac{\partial^2{L}}{\partial{x^2}}$ in Newton, even if $\nabla^2{f}>0$
-Gauss-Newton is often used in practice

#### 2.2 Inequality constraints
$$\min_{x} \quad f(x)$$
$$s.t. c(x) \quad \geq 0$$
-We'll just look at inequalities for now
- In general, these methods for now with the privious ones for mixed equility/inequality constraints

First-Order Necessary Conditions:
1) $\nabla f = 0 $ in the free directions
2) $c(x) \geq 0$

KKT Conditions:
$$\nabla{f}-(\frac{\partial{c}}{\partial{x}})^T\lambda=0\qquad stationarity$$
$$c(x) \geq 0 \qquad primal \quad feasibility$$
$$\lambda \geq 0 \qquad dual \quad fesibility$$
$$\lambda^{T}c(x) = 0 \qquad complementarity$$

Intuition:
- If constraint is "active" $\lambda > 0$ (same as equality case)
- If constraint is "inactive" $\lambda = 0$ (same as unconstrainted)
- Complementerity encodes "on/off" switching

Algorithms:

- Mush harder than equality case
- can't directly apply Newton to KKT
- Many options will trade offs

Active-Set:
- Have some way of guessing active/inactive constraints
- Just solve equality-constrainted problems
- Very fast if you can guess well
- Very bad otherwise

Barrier/Interier-POINT

- Replace inequalities with "barrier function" in objective that goes to infinity at constraint boundary.
$${min \quad f(x)}$$
$$s.t.\quad c_i(x)\geq 0$$
convert the above to the fllowing???
$$min_{x} f(x)-\sum^{m}_{i=1}\frac{1}{p}log(c_i(x))$$

- Gold standard for small~medium convex problems
- Requies lots of tricks/hacks for non-convex problems

Penalty
- Replace inequality with objective term to pernalize violations
$${min \quad f(x)}$$
$$s.t.\quad c_i(x)\geq 0$$
convert the above to the fllowing???
$$min f(x)+\frac{p}{2}[min(0,C(x))]^{2}$$

- Easy to implement
- Has issues with numerical ill-conditioning
- Difficult to achieve high accuracy




## Lecture 5: Optimization Pt. 3
### Penalty Method
- Replace inequalities with objective term that penalizes violations:
$$\min_{x} \quad f(x)$$
$$s.t. \quad c(x)\geq0$$
The above can be rewritten:
$$\min_{x} \quad f(x)+\frac{p}{2}[min(0,c(x))]^2$$
- Easy to implement
- Large penalties->ill-conditioning
- Difficult to achieve high accrracy
### 1 Augmented Lagrangian
- Add Lagrange nultiplter estimate to penalty method:
$$\min_{x} L_p(x,\hat{\lambda})=f(x)-\hat{\lambda}^{T}c(x)+\frac{P}{2}[min(0,c(x))]^2$$
-Update $\hat{\lambda}$ by "off loading" penalty into multiplier at each iteration
- Repeat until convergence
1) $\min_{x} \quad L_p(x,\hat{\lambda})$
2) $\hat{\lambda}\leftarrow max(0,\hat{\lambda}-pc(x))$
3) $p \leftarrow \alpha p \quad (optinal)$

- Fixed ill-conditioning of penalty method
- Convergencs fast(super linear)
- Works well on non-linear problems

### 2 Quadratic Programs
$$\min_{x} \frac{1}{2}X^{T}QX+q*Tx,\quad Q>0$$
$$s.t. \quad Ax\leq b$$
$$s.t. \quad Cx\leq d$$
- Very useful in control
- Can be solved very fast

### 3 Regularization+duality
$$\min_{x} \quad f(x)$$
$$s.t. \quad c(x)=0$$
- We might line to trun this into :
$$\min_{x}\quad f(x)+P_{\infty}(c(x)), P_{\infty}(c(x))\geq 0$$
- Practically terrible, but we can get the same effect by solving:
$$\min_{x} \quad \max_{\lambda} \quad f(x)+\lambda^Tc(x)$$
- Whatever $c(x)\neq 0$, inner problem blows up
- Similarly for inequalities!
$$\min_{x} \quad f(x)$$
$$s.t. \quad c(x) \geq 0$$
trun this into:
$$\min_{x}\quad f(x)+P_{\infty}(c(x)), P_{\infty}^{+}(c(x))\geq 0$$
we can get the same effect by solving:
$$\min_{x} \quad \max_{\lambda\geq 0} \quad L(x,\lambda)=f(x)+\lambda^Tc(x)$$
- Asside for convex problems, we can switch the order of min/max and get the same answer(duality). Not true ub general.
- Interpretation: KKT conditions define a sddle point in $(x,\lambda)$
- KKT system should have $dim(x)$ positive eigenvalues and $dim(\lambda)$ negative eigenvalue at an optimum. Called "Quasi-definite"

When rugularizing a KKT system, the lower-right block should be negative:
![image](https://user-images.githubusercontent.com/43432899/198868643-341167ac-c0a8-4844-adb8-29e359dba958.png)
* Exapmle:
-still heve overshoot -> need line search

### Merit Function
- How do we do a lone search on a root-finding problem?

find $x^{\*} \quad s.t. \quad c(x^{\*})=0$
- Define a scalar "merit function" $P(x)$ that measures distance from a solution
- standard Choices!
$$P(x)=\frac{1}{2}c(x)^{T}c(x)=\frac{1}{2}||c(x)||^2_2$$
$$P(x)=\Vert c(x)\Vert_1 \quad (any \quad norm \quad works)$$

- Now just do Armijo on $P(x)$
$$\alpha=1$$
$$while\quad P(x+\Delta x)>P(x)+c\alpha\nabla P(x)^T\Delta{x}$$
$$\qquad \alpha \leftarrow c\alpha$$
$$end$$
$$\qquad x \leftarrow x+ \alpha \Delta x$$$
- How about constrained minimization?
$$min_{x}\quad f(x)$$
$$s.t.\quad c(x)\geq 0$$
$$s.t.\quad d(x)= 0$$
turn into:
$$L(x,\lambda,\mu)=f(x)-\lambda^{T}c(x)+\mu^{T}d(x)$$
- Lot's of options for merit function:
- 
![image](https://user-images.githubusercontent.com/43432899/198869024-2dc90962-8e9a-4db3-8c6a-a0c8ced0b130.png)

Take away message:
- $P(x)$ based on KKT residual is expensive
- Exessively large constraint penalty can cause problems
- AL methods come with a merit fuction for free
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

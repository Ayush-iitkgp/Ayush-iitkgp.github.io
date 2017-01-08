.. title: 
.. slug: juliacon-slides
.. date: 2016-07-25 15:01:32 UTC+05:30
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->
# Enabling Complex-Domain Convex Optimization in Julia

## Ayush Pandey | JuliaCon 2016

[https://github.com/Ayush-iitkgp/JuliaCon2016Presentation](https://github.com/Ayush-iitkgp/JuliaCon2016Presentation)

## About Me 

#### Integrated Master of Science student at IIT Kharagpur pursuing Mathematics and Computing Sciences

#### GitHub - [https://github.com/ayush-iitkgp](https://github.com/ayush-iitkgp)

#### Google Summer of Code - 2016 student under mentorship of Madeleine Udell and Dvijotham Krishnamurthy

#### Blogging my GSoC'16 experience at [http://ayush-iitkgp.rhcloud.com](http://ayush-iitkgp.rhcloud.com/) 

<!--- ## CVX.jl team

* [CVX.jl](https://github.com/cvxgrp/CVX.jl): Madeleine Udell, Karanveer Mohan, David Zeng, Jenny Hong
<!---* [ParallelSparseMatMul.jl](https://github.com/madeleineudell/ParallelSparseMatMul.jl): Madeleine Udell
--->

# What is Convex Optimization?

### What is Convex function? 
$f$ is **convex** if for all $\theta \in [0,1]$
$$
f(\theta x + (1-\theta)y ) \leq \theta f(x) + (1-\theta) f(y)
$$

equivalently, 

* $f$ has nonnegative (upward) curvature
* $f'' \geq 0$ (if $f$ is differentiable)
* Geometrically, the line joining any 2 points on $f$ always lies above the graph of $f$

<!---* sublevel sets $\{x: f(x) \leq \alpha\}$ are convex sets --->

![chords](chord.png)

**Note - A function is called affine iff it is both convex and concave.**

## What is Convex optimization ?

### Functional Form

$$
\begin{array}{ll} 
\mbox{minimize}  & f_0(x) \\
\mbox{subject to} & f_i(x) \leq 0, \quad i=1, \ldots, m_1\\
& h_j(x) = 0, \quad j=1, \ldots, m_2\\
\end{array}
$$

* variable $x\in \mathbf{R}^n$
* $f_i$ are all convex
* $h_j$ are all affine

**Note 1- If $x\in \mathbf{R}^n$ then we refer the problem as real-domain convex optimization and if $x\in \mathbf{C}^n$ then we refer it as complex-domain optimization problem. Also if the coefficients of the variablex are complex numbers then it is complex-doamin optimization problem.** 

**Note 2- It is important to note that if $x\in \mathbf{C}^n$ then we no longer have inequality constraint.**

## Why do we need another package if we have available solvers like SCS, Mosek?

### Convex optimization (conic form)

$$
\begin{array}{ll} 
\mbox{minimize}  & c^T x \\
    \mbox{subject to} & Ax = b\\
    & x \in \mathcal K\\
\end{array}
$$

where $\mathcal K$ is a **convex cone**

$$ x \in \mathcal K \iff rx \in \mathcal K, \quad \forall r>0$$

examples:

* positive orthant 

$$\mathcal K_+ = \{x: x\geq 0\}$$
    
* second order cone 

$$\mathcal K_{\mathrm{SOC}} = \{(x,t): \|x\|_2 \leq t\}$$
    
* semidefinite cone 

$$\mathcal K_{\mathrm{SDP}} = \{X: X = X^T,~ v^T X v \geq 0,~ \forall v \in \mathbf{R}^n\}$$

**Does the previous slide look complicated? It turns out that the above solvers only understand the conic form which is hard for us to write and this is the reason why we need a package for convex optimization where we can express our problem in functional form and let the package manage the difficult part of converting the problem to conic form.**

# Quick Tutorial


```julia
using Convex
```

    WARNING: Method definition ctranspose(Convex.Constant) in module Convex at /home/hduser/.julia/v0.5/Convex/src/atoms/affine/transpose.jl:73 overwritten at /home/hduser/.julia/v0.5/Convex/src/atoms/affine/transpose.jl:74.
    WARNING: Method definition norm(Convex.AbstractExpr, Symbol) in module Convex at /home/hduser/.julia/v0.5/Convex/src/atoms/norm.jl:45 overwritten at deprecated.jl:49.


## Variables


```julia
# Scalar variable
x = Variable()
# (Column) vector variable
y = Variable(4)
# Matrix variable
z = Variable(4, 2)
```




    Variable of
    size: (4, 2)
    sign: Convex.NoSign()
    vexity: Convex.AffineVexity()



# Expressions

* We can operate on variables to form *convex expressions*


```julia
# indexing, multiplication, addition
e1 = y[1] + 2*x

# expressions can be affine, convex, or concave
e3 = sqrt(x)
```




    AbstractExpr with
    head: geomean
    size: (1, 1)
    sign: Convex.Positive()
    vexity: Convex.ConcaveVexity()




# Constraints


```julia
# affine equality constraint
A = randn(3,4); b = randn(3)
constraint1 = A*y == b
```




    Constraint:
    == constraint
    lhs: AbstractExpr with
    head: *
    size: (3, 1)
    sign: Convex.NoSign()
    vexity: Convex.AffineVexity()
    
    rhs: [-1.6048077944321295,2.1044876445367,0.6535176954608789]
    vexity: Convex.AffineVexity()




```julia
# convex inequality constraint
constraint2 = norm(y,2) <= 2
```




    Constraint:
    <= constraint
    lhs: AbstractExpr with
    head: norm2
    size: (1, 1)
    sign: Convex.Positive()
    vexity: Convex.ConvexVexity()
    
    rhs: 2
    vexity: Convex.ConvexVexity()



# Problems

* Now let's combine the above 3 steps to define our problem


```julia
objective = 2*x + 1 - sqrt(sum(y))
p = minimize(objective, constraint1, constraint2)
```




    Problem:
    minimize AbstractExpr with
    head: +
    size: (1, 1)
    sign: Convex.NoSign()
    vexity: Convex.ConvexVexity()
    
    subject to
    Constraint:
    == constraint
    lhs: AbstractExpr with
    head: *
    size: (3, 1)
    sign: Convex.NoSign()
    vexity: Convex.AffineVexity()
    
    rhs: [-1.6048077944321295,2.1044876445367,0.6535176954608789]
    vexity: Convex.AffineVexity()
    		Constraint:
    <= constraint
    lhs: AbstractExpr with
    head: norm2
    size: (1, 1)
    sign: Convex.Positive()
    vexity: Convex.ConvexVexity()
    
    rhs: 2
    vexity: Convex.ConvexVexity()
    current status: not yet solved




```julia
# solve the problem
solve!(p)
p
```

    ----------------------------------------------------------------------------
    	SCS v1.1.8 - Splitting Conic Solver
    	(c) Brendan O'Donoghue, Stanford University, 2012-2015
    ----------------------------------------------------------------------------
    Lin-sys: sparse-direct, nnz in A = 34
    eps = 1.00e-04, alpha = 1.80, max_iters = 20000, normalize = 1, scale = 5.00
    Variables n = 8, constraints m = 15
    Cones:	primal zero / dual free vars: 4
    	linear vars: 3
    	soc vars: 8, soc blks: 2
    Setup time: 8.36e-05s
    ----------------------------------------------------------------------------
     Iter | pri res | dua res | rel gap | pri obj | dua obj | kap/tau | time (s)
    ----------------------------------------------------------------------------
         0|      inf       inf      -nan      -inf       inf       inf  2.49e-05 
        20|      inf       inf      -nan      -inf      -inf       inf  5.35e-05 
    ----------------------------------------------------------------------------
    Status: Unbounded
    Timing: Solve time: 5.92e-05s
    	Lin-sys: nnz in L factor: 69, avg solve time: 6.34e-07s
    	Cones: avg projection time: 2.07e-07s
    ----------------------------------------------------------------------------
    Certificate of dual infeasibility:
    dist(s, K) = 8.2611e-21
    |Ax + s|_2 * |c|_2 = 2.3690e-06
    c'x = -1.0000
    ============================================================================


    WARNING: Problem status Unbounded; solution may be inaccurate.





    Problem:
    minimize AbstractExpr with
    head: +
    size: (1, 1)
    sign: Convex.NoSign()
    vexity: Convex.ConvexVexity()
    
    subject to
    Constraint:
    == constraint
    lhs: AbstractExpr with
    head: *
    size: (3, 1)
    sign: Convex.NoSign()
    vexity: Convex.AffineVexity()
    
    rhs: [-1.6048077944321295,2.1044876445367,0.6535176954608789]
    vexity: Convex.AffineVexity()
    		Constraint:
    <= constraint
    lhs: AbstractExpr with
    head: norm2
    size: (1, 1)
    sign: Convex.Positive()
    vexity: Convex.ConvexVexity()
    
    rhs: 2
    vexity: Convex.ConvexVexity()
    current status: Unbounded




```julia
p.optval
```




    -0.9999999999999999




```julia
x.value
```




    -0.49999943073238445



# Why do we need complex-domain optimization convex package?

## Applicationf of complex-domain convex optimization

1. **Phase retreival from sparse signals** used in MRI Imaging of the Brain

**Mathematical Formulation**

*find x*

*satisfying A(x) = A($x_0$) = b*

*where $x_0$ in $x\in \mathbf{C}^n$*

*A($x_0$) = {|$<a_k, x_0>$|^2 : k = 1, 2, . . . , m }*

![MRI Imaging](Brain_MRI.jpg)


**Used in demodulation of the mutually interfering digital streams of information** that occur in areas such as wireless communications, high-speed data transmission, DSL, satellite communication, digital television, and magnetic recording. 

Here the problem is detecting interference between multiple users in a wireless communication network.

![Wireless Communication](wirelesscommunication.jpg)

**Optimization in power grids**: The problem of finding the most efficient dispatch of generators in a power grid subject to meeting the demand for power and respecting network constraints. Here the complex variable x represents steady-state voltages in an AC power network.

Here one of the variables which represents steady-state voltages in an AC power network is a complex variable so it again becomes complex-domain optimization problem.


![Power Grids](powergrid.jpg)

# How Convex.jl handles the complex-domain optimization problem?

## Things to keep in mind

* The objective function must be real
* The inequality constraints must be real
* The last entry of the Second Order Conic Constraint must be real.
* All variables in exponential cones must be real.


```julia
A complex equality constraint could be transfored to corresponding 2 equality constraints.
```

# My Work

I have started with implemeting the support for complex coefficients which means we keep the variable (say x) as real but the coefficients as complex. 


```julia
# Creating a real variable
using Convex
x = Variable()
```




    Variable of
    size: (1, 1)
    sign: Convex.NoSign()
    vexity: Convex.AffineVexity()




```julia
# Creating an objective function
objective = 2*x
```




    AbstractExpr with
    head: *
    size: (1, 1)
    sign: Convex.NoSign()
    vexity: Convex.AffineVexity()





```julia
# Creating complex equality constraints
c1 = (2+3im)*x == (6+9im)
c2 = x<=5
```




    Constraint:
    <= constraint
    lhs: Variable of
    size: (1, 1)
    sign: Convex.NoSign()
    vexity: Convex.AffineVexity()
    rhs: 5
    vexity: Convex.AffineVexity()




```julia
p = minimize(objective, c1, c2)
```




    Problem:
    minimize AbstractExpr with
    head: *
    size: (1, 1)
    sign: Convex.NoSign()
    vexity: Convex.AffineVexity()
    
    subject to
    Constraint:
    == constraint
    lhs: AbstractExpr with
    head: *
    size: (1, 1)
    sign: Convex.ComplexSign()
    vexity: Convex.AffineVexity()
    
    rhs: 6 + 9im
    vexity: Convex.AffineVexity()
    		Constraint:
    <= constraint
    lhs: Variable of
    size: (1, 1)
    sign: Convex.NoSign()
    vexity: Convex.AffineVexity()
    rhs: 5
    vexity: Convex.AffineVexity()
    current status: not yet solved




```julia
solve!(p,verbose=false)
x.value
p.optval

# Solution
# x.value = 3.0001651341687965
# p.optval = 6.000269523275286


```

    ----------------------------------------------------------------------------
    	SCS v1.1.8 - Splitting Conic Solver
    	(c) Brendan O'Donoghue, Stanford University, 2012-2015
    ----------------------------------------------------------------------------
    Lin-sys: sparse-direct, nnz in A = 5
    eps = 1.00e-04, alpha = 1.80, max_iters = 20000, normalize = 1, scale = 5.00
    Variables n = 2, constraints m = 4
    Cones:	primal zero / dual free vars: 3
    	linear vars: 1
    Setup time: 5.35e-05s
    ----------------------------------------------------------------------------
     Iter | pri res | dua res | rel gap | pri obj | dua obj | kap/tau | time (s)
    ----------------------------------------------------------------------------
         0|      inf       inf      -nan       inf      -inf       inf  2.26e-05 
        60| 7.69e-06  5.72e-05  3.14e-05  6.00e+00  6.00e+00  9.46e-17  1.02e-04 
    ----------------------------------------------------------------------------
    Status: Solved
    Timing: Solve time: 1.04e-04s
    	Lin-sys: nnz in L factor: 11, avg solve time: 3.59e-07s
    	Cones: avg projection time: 1.99e-07s
    ----------------------------------------------------------------------------
    Error metrics:
    dist(s, K) = 1.0750e-16, dist(y, K*) = 0.0000e+00, s'y/m = 2.9317e-17
    |Ax + s - b|_2 / (1 + |b|_2) = 7.6946e-06
    |A'y + c|_2 / (1 + |c|_2) = 5.7200e-05
    |c'x + b'y| / (1 + |c'x| + |b'y|) = 3.1364e-05
    ----------------------------------------------------------------------------
    c'x = 5.9999, -b'y = 6.0003
    ============================================================================





    5.999939822156568



# Future work

* Implementing complex variables
* Implemeting Complex SemiDefinite Programming

# Questions

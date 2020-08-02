---
title: "Foundations of Robot Motion"
section: 2.1
---

## Terms:

**Configuration of a Robot:**

- Specification of the positions of all points on a robot

**C-Space:**

- The space of all configurations

**Degrees of Freedom (DoF):**

- The dimention of the C-space

- (Minimum number of real numbers you need to represent the configuration)

**Rigid Body:**

- A solid body in which deformation is zero or negligible

- Ex. A box, not a pillow

**Topology:**

- The study of the properties of geometric forms that remain invariant under certain transformations such as bending or stretching

---

## Degrees of Freedom of a Rigid Body

Before giving a robot commands, the first thing you need to understand  is the configuration space of the robot, or all of the possible positions that the robot can take. 

Instead of having a grid representing all possible spaces, it is simpler to just understand the degrees of freedom of a robot. But how do you find the DoF? Fortunately, it's easier than you might think:

> Consider an airplane. The entire body has 6 degrees of freedom based on the movements that can take. 
> 
> - It can move along all 3 axis in the cartersian plane (x, y, z)
> 
> - It can also rotate around all 3 axis in the cartesian plane (x, y, z)
> 
> - Thus the airplane has 6DoF

However, we must also be able to derive it *mathemagically*

We have two ways to find the DoF:

1) $DoF = \sum(freedom\ of\ points) - number_{independent\ constraints}$
2. $DoF = \sum(freedom\ of\ bodies) - number_{independent\ constraints}$

Really, these two equations are the same thing, just using differing defintions. Now, let's apply this formula for a basic example. Consider a normal 3D rectangle in space:

<img title="" src="https://raw.githubusercontent.com/CSharpRon/Notes/images/modern-robotics/rectangle.png" alt="" data-align="center">

To begin, we first pick an arbitrary point called Point A. Since we have not defined any other constraints, it is free to move along any of the 3 axes (x, y, and z). Therefore, that point is said to have *3 freedoms.*

| point | coords |     | constraint |     | freedoms |
|:-----:|:------:|:---:|:----------:|:---:|:--------:|
| A     | 3      | -   | 0          | =   | 3        |

Next, we pick an additional point called B and observe that is too can move freely along any of the 3 axes. However, it has a constraint on it location due to its distance to A which we established already has 3 freedoms. Therefore A acts as a single constraint to B. Updating the table we get:

| point | coords |     | constraint |     | freedoms |
|:-----:|:------:|:---:|:----------:|:---:|:--------:|
| A     | 3      | -   | 0          | =   | 3        |
| B     | 3      | -   | 1          | =   | 2        |

Next, we observe C. Just like A and B, it can move freely along any of the 3 axes. Now, it is constrained by it's distance to both B and A. With this it is easy to see how we will update the table:~~~~

| point | coords |     | constraint |     | freedoms |
|:-----:|:------:|:---:|:----------:|:---:|:--------:|
| A     | 3      | -   | 0          | =   | 3        |
| B     | 3      | -   | 1          | =   | 2        |
| C     | 3      | -   | 2          | =   | 1        |

Last, we observe a 4th point D and realize that the outcome will be the same for all subsequent points. While they can all move along any axis, they are all constrained to Points A, B, and C and therefore have no freedom:

| point  | coords |     | constraint |     | freedoms |
|:------:|:------:|:---:|:----------:|:---:|:--------:|
| A      | 3      | -   | 0          | =   | 3        |
| B      | 3      | -   | 1          | =   | 2        |
| C      | 3      | -   | 2          | =   | 1        |
| D, etc | 3      | -   | 3          | =   | 0        |

Therefore, the DoF of this rectangle is 6.

## Commonly Used Joints

![](https://raw.githubusercontent.com/CSharpRon/Notes/images/modern-robotics/0fa875b341053c5769a93f0e623f9bbd.jpg)

## Grubler's Formula

In the case of a robot, constraints on motion often come from joints. Joints come in all shapes and sizes and have their own freedoms. Here is a table of common joints on a robot:

Since robots are often made up of joints with known formulas, there is a much more straightforward way to derive the DoF of an entire robot without needing to analyze every single point like we did in the rectangle example. **Grubler's formula** is created exactly for this. All we need to know is:

- N = number of links (bodies) on the robot, including ground

- J = number of joints on the robot

- m = the degrees of freedom of a single body (6 for spatial robots, 3 for planar robots)

Assuming all constraints are independent, Grubler's Formula is:

>  $DoF = m(N - 1 - J) + \sum_{i=1}^J f_i$, 
> 
> where $f_i$ is the freedoms for joint i.

## Applying Grubler's Formula

<img title="" src="https://raw.githubusercontent.com/CSharpRon/Notes/master/images/fc92e9a7b00e2517dbbd5340c6fec3e6.jpg" alt="" width="150">

Problem: Use Grubler's Formula to calculate the DoF of a Stewart Platform.

> $DoF = m(N-1-J) + \sum_{i=1}^Jf_i$

m = 6 (since this is a spatial body)

N = 14 (6 x 2 plus ground and top)

J =  18 (6U + 6P + 6S joints)

---

$DoF = 6(14 - 1 - 18) + (6\times dof(f_U) + 6\times dof(f_P) + 6\times dof(f_S)$

$DoF = -30 + (6\times2 + 6\times1 + 6\times3)$

$DoF = -30 + 36$

$DoF = 6$

---

## Calculating Constraints

Consider a joint between two rigid bodies. Each rigid body has m degrees of freedom (m = 3 for a planar rigid body and m = 6 for a spatial rigid body) in the absence of any constraints. The joint has f degrees of freedom. How many constraints does the joint place on the motion of one rigid body relative to the other?

From proposition 2.2 in the book:

$$
f + c = m\ , therefore...\newline
c = m - f
$$

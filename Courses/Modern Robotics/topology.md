---
title: "Topology"
section: 2.3
---

## Terms

**Topology**:

- Shape of a robot

**Constraints:**

**Holonomic Constraint:**

- Constraints that reduce the dimension of the C-space

**Non-holonomic Constraints:**

- Constraints that reduce the space of possible velocities

- I.e., A car cannot slide directly to the side due to non-holonomic, but this constraint does not reduce the space of configuration. The car can still move sideways via another force.

**Pfaffian Constraints:**

- Constraints that are linear in velocity

**Task Space**:

- The space in which the robot's task is naturally expressed

**Workspace**:

- A specification of the reachable configurations of the end effector (has nothing to do with the original task)

---

## Configuration Space Topology

Two spaces are **topologically equivalent** if one can be smoothly deformed to the other <u>without</u> cutting and gluing.

Believe it or not, a taurus (left) can be smoothly deformed into a coffee mug (right). However, they cannot be deformed into a plane.

![](https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/2020-07-15image.png) 

> The **topology** of an object is a fundamental property that is <u>independent</u> of how we choose to define the space with coordinates.

Note: To visualize topology, picture a point going all around the surface of a cylinder.

![](https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/baa76f8e9d8b643c2479c2f18d3709db.jpg)

## Configuration Space Representation

Remember that configuration space allows us to define the space that a robot can operate in. To represent a C-space using real numbers and in more than just a 2D space, we have to make some arbitrary choices...

To represent points on a 2D plane, we first choose the origin and 2 orthogonal origin axes (x,y). Any point can then be represented as the pair of x and y. This one is easy to understand, it's just an x,y axis.

Say you want to track a 3rd property of an object moving in this 2D space: velocity. We know that this is just the time derivative of the coordinates and we can represent it as a vector on this axis:

![](https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/c548d461947149caa204327e35e59278.jpg)

Great! So what's the problem? Well what happens when the configuration space is defined as a curve (like a sphere)?

### Explicit Parameterization

![](https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/3debb6c539e8d2d772e21073aac90c1e.jpg)

One solution we are already familiar with: using latitude and longitude. This is **explicit parameterization**. It works by defining every point along the sphere.

**Pros**:

- Minimum Number of coordinates

- Less complex as a result

**Cons:**

- Since the topology of the space is different from the Euclidean space, the representation will have poor behavior at some points. For example, longitude changes dramatically when the cross the north pole.

- This solution is also unable to deal with discontinuities in the c-space.

### Implicit Parameterization

![](https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/ba12cf4e01fe6e00727aa6e0dcdb91cb.jpg)

Another solution (the one used in this course) is to convert the space into a higher-dimensional space (e.g., x,y,z) such that $x^2+y^2+z^2=1$. This is known as **implicit parameterization**.

**Pros:**

- No problems with discontinuities

**Cons:**

- More complex

## Relationship between dimensions and constraints

The number of degrees of freedom is equal to the number of coordinates minus the number of independent constraints of those coordinates.

> If a K-dimensional space is represented by 7 coordinates subject to 3 independent constraints, what is K? K = (7 - 3) = 4

## Holonomic Constraints

Implicit parameterization involves representing movement from one dimension into a higher order dimension. Let's take a look at the four-bar linkage:

<img title="" src="https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/db8a7fd10a92e6052ad197865396435b.jpg" alt="" data-align="center">

After applying Grubler's formula, we know that this has one DoF and should be representable by one variable. However, this will have singularities. Instead we will (thanks to implicit parameterization) represent the C-space as a 1-dimensional space embedded in the 4-dimensional space of joint angles.

How do we know what these joint angles are? Well that depends on what happens to the linkage. What we can do is define the *constraints* that define the joint angles. In this case, these constraints are the loop closure equations:

$$
L_1cos(\theta_1) + L_2cos(\theta_1 + \theta_2) + 
L_3cos(\theta_1 + \theta_2 + \theta_3) + 
L_4cos(\theta_1 + \theta_2 + \theta_3 + \theta_4) = 0,\newline
L_1sin(\theta_1) + L_2sin(\theta_1 + \theta_2) + 
L_3sin(\theta_1 + \theta_2 + \theta_3) + 
L_4sin(\theta_1 + \theta_2 + \theta_3 + \theta_4) = 0,\newline
\theta_1+\theta_2+\theta_3+\theta_4-2\pi = 0

$$

These constraints effectively reduce the dimension of the C-space. The left side of these equations represent the final positions and the right side represents the initial position (which in this case is 0).

Now, we can generalize these equations if we define the vector of joint angles as $\theta = [\theta_1 ... \theta_n]^T$ where n is the number of angles in the system.

We can then represent the loop closure equations as *g* where:

$$
g = 
\begin{bmatrix}
g_1(\theta_1,...,\theta_n) \cr
g_k(\theta_1,...,\theta_n) 
\end{bmatrix}
= 0
$$

*k* is a **holonomic constraint**. Each $g_k$ is a holonomic constraint. That is, these are constraints that reduce the dimension of the C-space. Here we can derive the following intuition:

> if $\theta \in \real^n$ 
> 
> and $g(\theta) \in \real^k$
> 
> then $DoF = n - k$

## Pfaffian Constraints

Continuing with the 4-bar closed chain: if the robot is moving, we could ask how the holonomic constraints restrict the *velocity* of the robot. Since g($\theta$) has to be 0 at all times, the time rate of change (derivative) of g must also be 0 at all times.

$$
\frac{d}{dt}g(\theta(t)) = 0
$$

<img title="" src="https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/b3bc29f6c8ada332c0bba14a658a9d14.jpg" alt="" data-align="center">

Generalizing one last time means that we can rewrite these constraints as:

$$
A(\theta)\dot{\theta} = 0
$$

where $A(\theta)\in\real^{k \times n}$

Velocity constraints of this form are called **Pfaffian constraints** and are characterized by being linear in velocity. One example of Pfaffian constraints is rolling without slipping in a wheeled robot.

## Nonholonomic Constraint

We just established that Pfaffian constraints come from the derivative of holonomic constraints. Reversing this means that holonomic constraints are the <u>integral</u> of these Pfaffian constraints. Holonomic constraints are sometimes referred to as **integratable constraints** for this very reason.

However, in some cases a set of velocity constraints cannot be integrated to equivalent configuration constraints. Consider the chassis of a car riding on a plane. if you pick a reference point $q = (\theta,x,y)$ then the derivatives are easy to compute:

$$
\dot{x} = vcos(\theta)\newline
\dot{y} = vsin(\theta)\newline
\dot{x}sin(\theta)-\dot{y}cos(\theta) = 0
$$

This final equation can be written as the Pfaffian constraint:

$$
A(q)\dot{q} = A(q)
\begin{bmatrix}
\dot{\theta} \cr
\dot{x} \cr
\dot y
\end{bmatrix}
$$

$$
A(q) = [0 sin(\theta)-cos(\theta)], \in \real^{1 \times 3}
$$

This final equation is not integratable. Such constraints are **nonholonomic.** That is, they are only constraints on velocity. An example would be the nonholonomic constraints that prevent a car from sliding directly to the side. These same constraints do not ultimately reduce the spce of configurations. I.e., sideways motion can still be achieved.

## Task Space and Workspace

Now that we have started to define the configuration of a robot, let's end by defining the task space and workspace. Both of these pieces relate to the end-effector of the robot (usually the arm or gripper) rather than the entire robot.

**Task space** is used to define the space in which the robot's task can be naturally expressed. For example, if the robot's task is to control the position of the tip of a marker on a board, then the task space is the Euclidean plane $\real^2$. If the task is to control the position and orientation of a rigid body, then the task space is the 6-dimensional space of rigid body configurations.

> Notice that we haven't done any math here. To identify the task space, you only have to know about the nature of the task. You do not need to know about the robot as a whole.

Similarly, the **workspace** of the robot is the specification of the configurations that the end-effector of a robot can reach. In this case, the workspace is defined primarily by the robot's structure, independently of the task. For examples see the following diagram. In the upper two examples, the area within the circle represents all of the possible configurations for that robot arm. In the lower left example, the arm can move  in a coordinate fashion around a sphere. In the bottom right example, the arm can move in the x, y, and z direction and can rotate:

<img title="" src="https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/cb4ad628ef4316e7be27044e2b806bbd.jpg" alt="" data-align="center">

It is helpful to define the workspace as a **shape** just like I did when describing the first 3 examples.

> The workspace is often defined in terms of the Cartesian points that can be reached by the end-effector, but it is also possible to include the orientation.

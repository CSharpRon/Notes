---
title: "Angular Velocities"
section: 3.2
keywords:
- SO(3)
- Angluar
- Velocity
---

## Terms

---

## Angular Velocities

Knowing the configuration of a robot helps us understand the space in which the robot can operate. Usually this is made up of positions and rotations. 

Next we will look for a representation of angular velocity, again using 3 variables like we did before. We defined the frame in coordinates $[\hat{x}, \hat{y}, \hat{z}]^T$ so we should be able to take the change in these positions over time. $\triangle{t}$ 

To determine angular velocity, we also need to identify the angles present in a frame. If we examine the body frame at times $t$ and $t + \triangle{t}$, the change in frame orientation can be described as a rotation of angle $\triangle\theta$ about some unit axis $\hat{w}$ passing through the origin. Now for the final step.

As $\triangle{t}$ approaches zero, the ratio $\frac{\triangle{\theta}}{\triangle{t}}$ becomes the rate of rotation $\dot{\theta}$ and moves by a factor of the unit axis we defined before. Therefore, the angular velocity can be defined as:

$$
w = \hat{w}\dot{\theta}
$$

> This equation works because any regular velocity can be represented by a rotation axis and the speed of rotation about it.

Following along with this, we can also get the angular velocity along any single axis:

$$
\dot{\hat{x}} = w\times\hat{x}\newline
\dot{\hat{y}} = w\times\hat{y}\newline
\dot{\hat{z}} = w\times\hat{z}
$$

Isn't math fun? :)

### Skew Symmetric Matrices

Let's go back to the Rotation Matrix R. We previously stated that any regular velocity can be represented by a rotation axis and the speed of rotation about it. Well since w is a matrix, let's refer to it as [w] in equations. If we want to define the angular velocity via the rotation matrix we would then say:

$$
\dot{R} = R[w]R^T
$$

which will yield a coordinate matrix for the angular velocities. But that's a lot of work going on in the right hand side. Thankfully a useful math property is here to save the day. If we can define w to be a **skew-symmetric** matrix, then we can rewrite the equation as:

$$
[\dot{R}]= [w]R
$$

Indeed, we can achieve this. Since w is made up of $[\dot{\hat{x}}, \dot{\hat{y}},\ and\ \dot{\hat{z}}]$, we need to prove that each of these is skew-symmetric. To do that we only have to focus on one example. 

A matrix is said to be **skew-symmetric** if the transpose of the matrix is equal to the negative of the matrix. This is true if:

$$
x = 
\begin{bmatrix}
x_1 \cr x_2 \cr x_3
\end{bmatrix} \in \real^3,
[x] =
\begin{bmatrix}
0 & -x_3 & x_2 \cr
x_3 & 0 & -x_1 \cr
-x_2 & x_1 & 0
\end{bmatrix}
$$

In otherwords, we need to make [x] be a 3x3 matrix representation of the 3-vector x. The math works out so we can acomplish this. We also get a new definition out of this, so(3).

> so(3) refers to all of the 3x3 skew-symmetric real matrices. It is related to SO(3), the space of all rotation matrices.

### Angular Velocity based on Frame

Typically, we write the angular velocity rotation matrix as:

$$
\dot{R}=[w_s]R_{sb}
$$

The ordering is very important as is the subscript ordering. Here are the rules on expressing this matrix in different frames.

1) The angular velocity vector can be expressed in other frames, not just the {s} frame. We can also write it in the body frame.
   
   >  $w_b = R_{bs}w_s = R_{sb}^{-1}w_s = R_{sb}^Tw_s$

2) R usually indicates the body frame relative to the space frame, so we can drop the subscripts and write the relationship between the body angular velocity and spatial angular velocity as:
   
   > $w_b = R_{w_s}^{-1} = R_{w_s}^T$

3) We can isolate the angular velocity:
   
   > $[w_s] = \dot{R}R^{-1}$

## Exponential Coordinates Representation of Rotation

The last thing we are going to focus on is a three-parameter representation for rotations, known as the **exponential coordinates for rotation**. What's

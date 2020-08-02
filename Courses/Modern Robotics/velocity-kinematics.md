---
title: "Velocity Kinematics"
section: 5.1

keywords:
  - Open-Chain
  - Velocity
  - Kinematics
  - End-Effector
  - Jacobian
  - Robotics
---

## Terms

**Jacobian**

- A determinate which describes the transformation factor of a matrix
  
  - I.e., after the determinate is applied, the area of the original matrix will change by a factor of what?

- Represented by the time derivative of the matrix

**Singularity**

- Robot configurations where $J_1(\theta)$ and $J_2(\theta)$ will be co-linear, resulting in a singular matrix for the Jacobian J(\theta)

**Space Jacobian**

- Jacobian in fixed (space) frame coordinate

**Body Jacobian**

- Jacobian in the end-effector (or body) frame coordinates

**End-Effector Velocity**

- Also referred to as endpoint velocity (v_{tip})

- Since the end effector can have any x, y, and theta value depending on its configuration, the time derivative of this vector will yield a Jacobian matrix that can be used to calculate the end-effector's $v_{tip}$

---

## Velocity Kinematics

- The Jacobian matrix provides the relation between joint velocities and end-effector velocitities of a robot manipultor. 
- Since the joints move with certain velocities, it is useful to know the velocity of the end-effector velocity: $v_{tip}$
- Here, we are calculating the velocity with Twists.
- Since the Jacobian is a matrix, we need to understand what all of the values mean:

$$
J(\theta) = 
\begin{bmatrix}
\dot{x}_{j1} & \dot{x}_{j2} & ... & \dot{x}_{jn} \cr
\dot{y}_{j1} & \dot{y}_{j2} & ... & \dot{y}_{jn} \cr
\dot{z}_{j1} & \dot{z}_{j2} & ... & \dot{z}_{jn}
\end{bmatrix}
$$

- Each column represents the <u>effect on $v_{tip}$</u> due to variation in each joint velocity

## Manipulability Ellipsoids - Joint Velocities

The joint velocities are typically graphed in a 2D grid, where the possible joint velocities are represented as a square in the $\dot{\theta_1}-\dot{\theta_2}$ space. 

![](https://raw.githubusercontent.com/CSharpRon/Notes/images/modern-robotics/joint-velocities.png)

This set of values can then be passed to the Jacobian to return the parallelogram of **possible end-effector velocities.** :

![](https://raw.githubusercontent.com/CSharpRon/Notes/images/modern-robotics/end-effector-velocities.png)

Note: Typically, the joint velocities are shown as a sphere. When they are, an ellipsoid appears to show the end-effector velocities instead of a parallelogram.

- These ellipses are called **Manipulability Ellipsoids**

![](/home/ronald/.var/app/com.github.marktext.marktext/config/marktext/images/manipulability-ellipsoid.png)

## Force Ellipsoids - Joint Torques

You can also map the joint torques onto a 2D grid. When passed through the Jacobian, this will return the limits of the end-effector forces. 

![](https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/0f3c3717dba900bdd1bdde251abd326d.jpg)

- The resulting ellipsoid is called the **force ellipsoid.**

Here's how to find Tau ($\tau$)

- Let  $\tau$ = vector of joint torques (forces)
  
                                            $power = \dot{\theta}^T\times\tau$ = $v_{tip}^T \times f_{tip}$

$$
= \dot{\theta} \times \tau = (J(\theta)\dot{\theta})^T \times f_{tip} \newline
= \dot{\theta} \times \tau = \dot{\theta} \times J^T(\theta) \times f_{tip} \newline
\tau = J^T(\theta) \times f_{tip}
$$

- Tau is useful for **force control:** If we want the robot to generate the force $f_{tip}$ as its end-effector, the motors must generate joint torques and forces equal to $J^T(\theta) \times f_{tip}$

- If $J^T(\theta)$ is invertible: $J^{-T}(\theta)\tau = f_{tip}$
  
  - This formula is used to produce the force ellipsoids

## Space Jacobian

Other than representing the end-effector velocity as $v_{tip}$, we can represent it by the twist $V_s$

$$
V_s = J_s(\theta)\dot{\theta} = [J_{s1}(\theta)J_{s2}(\theta)...J_{sn}(\theta)]\dot{\theta} \newline
$$

Generalized the **Space Jacobian** is defined as:

$$
J_s(\theta)\in\real^{6 \times n}, n = 2
$$

Since the space jacobian is dependend on joints and angle rotations, we can derive the following example for a RRRP robot:

![](https://raw.githubusercontent.com/CSharpRon/Notes/images/modern-robotics/RRRP.png)

Since the Jacobian is a matrix, we will need to fill in every ith column:

- Denote the ith column of $J_S(\theta)$ by $J_{si} = (\omega_{si},v_{si})$

- Observe that $\omega_{s1}$ is constant and in the $\hat{z}_s$ direction: $\omega_{s1} = (0,0,1)$. Choosing q1 as the origin, $v_{s1} = (0,0,0)$

- $\omega_{s2}$ is also constant in the $\hat{z}_s$ direction, so $\omega_{s2} = (0,0,1)$. Choose q2 as the point ($L_1c_1,L_1s_1,0$), where $c_1 = cos\theta_1, s_1 = sin\theta_1$. Then $v_{s2} = -\omega_2 \times q_2 = (L_1s_1, -L_1c_1,0).$

- The direction of $\omega_{s3}$ is always fixed in the $\hat{z}_s$ direction regardless of the values of $\theta_1$ and $\theta_2$, so $\omega_{s3} = (0,0,1).$ Choosing $q_3 = (L_1c_1 + L_2c_{12},L_1s_1 + L_2s_{12},0),$ where $c_{12} = cos(\theta_1 + \theta_2), s_{12} = sin(\theta_1 + \theta_2)$, it followsthat $v_{s3} = (L_1s_1 + L_2s_{12}, -L_1c_1 - L_2c_{12}, 0).$

- ![](https://raw.githubusercontent.com/CSharpRon/Notes/images/modern-robotics/space-jacobian.png)

## Body Jacobian

The Body Jacobian transforms joint veocities into the body twist:

![](https://raw.githubusercontent.com/CSharpRon/Notes/images/modern-robotics/body-twist.png)

Here is a 5R Robot. The end effector frame is denoted as ${b^{\prime\prime}}$ since the frame is shifted twice due to the angle changes from the rotations of joints 4 and 5.

To find $J_{b3}$, find $T_{bb^{\prime\prime}}$

$$
J_{b3} = [Ad_{T_{b^{\prime\prime}b}}]\beta_3 \newline
  = [Ad_{T_{bb^{\prime\prime}}^{-1}}]\beta_3 \newline
= [Ad_{e^{-[\beta_5]\theta_5}e^{-[\beta_4]\theta_4}}]\beta_3
$$

**Generalized:** the Body Jacobian $J_b(\theta)$ is defined by 

$V_b = J_b(\theta)\dot{\theta}$

**where** $J_b(\theta) = [J_{b1}(\theta)...J_{b(n-1)}(\theta)J_{bn}] \in \real^{6 \times n}$

**with** $J_{bn} = B_n$

**and** $J_{bi}(\theta) = [Ad_{e^{-[B_n]\theta_n}...e^{-[B_{i+1}]\theta_{i+1}}}]$

### Relationship between Space Jacobian and Body Jacobian

$J_b(\theta) = [Ad_{T_{bs}}]J_s(\theta)$

$J_s(\theta) = [Ad_{T_{sb}}]J_b(\theta)$

## Statics of Open Chains

$\tau_{motion}(t): Inverse\ Dynamics$

To resist a wrench $-F_*$ applied tothe end-effector at configuration $\theta$, the joint torques/forces must be at:

$\tau = J_*^T(\theta)F_*$

where * = b indicates th Jacobian and the wrench represented in {b} and * = s indicates the Jacobian and wrench represented in {s}. This can be used in force control of the robot.

<u>Remember, the Jacbian shows the relationships for te end-effector velocities. These relationships can be used to determine the **corrective force** needed to maintain stability when an opposing force is applied. </u>

## Kinematic Singularities

Since the Jacobian is a matrix, we can determine its rank:

### Ranking Jacobians

The rank of a matrix is the number of independent rows contained in the matrix. A row is independent if it matches all of the following criteria:

- The row must contain at least one non-zero value

- The row must be unique

- The row must not be a multiple of another

- The row must not be a linear combination of another row

Remember that:

- $J(\theta) \in \real^{6 \times n}$

Therefore:

- $rank\ J(\theta) <= min(6,n)$

if $J(\theta) = min(6,n)$, then the Jacobian is at **full rank**.

If $J(\theta) < max\ rank\ J(\theta)$, then the Jacobian is in **singularity at** $\theta$

### Classifing Jacobians

For $J(\theta) \in \real^{6 \times n}$:

- When n < 6: The Jacobian is **tall** and **kinematically deficient**.

- When n = 6: The Jacobian is **square** (capable of general 6-dimenional body motions)

- When n > 6: The Jacobian is **fat** (it has more columns than rows like a 7R robot) and is considered **redundant**.

## Manipulability

A robot configuration is either singular, or it is not. However, it is useful to describe "how close" the robot is to being singular.

We can assign a measure ofjust how close the robot is to being singular according to how close the ellipsoid is to collapsing.

**Eigenvector:**

- A vector when after a transformation is applied to it only scaled instead of rotating

**Eigenvalue:**

- The amount that the vector scaled

![](https://raw.githubusercontent.com/CSharpRon/Notes/images/modern-robotics/eigenvalue.jpg)

We can then use a single number to represent how close the ellipsoid is to singularity: **This is called Manipulabiliy**

![](https://raw.githubusercontent.com/CSharpRon/Notes/images/modern-robotics/manipulability.jpg)

It's more useful to visualize the manipulability ellipsoid using the 
body Jacobian than the space Jacobian, since the body Jacobian measures 
linear velocities at the origin of the end-effector frame, which has a 
more intuitive meaning than the linear velocity at the origin of the 
space frame. If the robot has nnn joints, then the body Jacobian $J_b$ is 6 x n. We can break $J_b$ into two sub-Jacobians, the angular and linear Jacobians:

$$
J_b = 
\begin{bmatrix}
J_{bw} \cr
J_{bv} 
\end{bmatrix}
$$

The dimension of $J_{bv}J_{bv\prime}^T$, which is used to generate the linear component of the manipulability ellipsoid is:

- 3x3

As a Full Rank Jacobian approaches singular configuration, the following occurs to the **manipulability ellipsoid**:

- The length of one principal axis approaches zero

- The interior volume of the ellipsoid approaches zero

As a Full Rank Jacobian approaches singular configuration, the following occurs to the **force ellipsoid**

- The length of one principal axis approaches infinity.

- The interior volume of the ellipsoid approaches infinity.

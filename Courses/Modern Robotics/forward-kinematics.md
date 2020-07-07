---
title: "Forward Kinematics"
keywords:
  - Open-Chain
  - Forward
  - Kinematics
  - End-Effector
  - Robotics
...
# 4.1.1: Product of Exponentials Formula in the Space Frame

## Terms:

**Forward Kinematics**

- Refers to the use of the kineamtic equations of a robot to compute the position of the **end-effector** from specified values for the joint parameters. 

**End Effector**

- Device at the end of a robotic arm designed to interact with the environment.
  
  - End effectors are typically the **grippers** of a robot

**Denavit-Hartenberg Parameters**

- Referred to as D-H Parameters

- Commonly used convention for selecting frames of reference to the links of a spatial kinematic chain

**Product of Exponentials**

- Referred to as PoE

- Adjacent frames do not need to be considered.

- Only 2 frames are needed: Space Frame {s} and Tool Frame {b}

- 6n numbers are needed to describe n screw axes

---

## Forward Kinematics

- Forward kinematics refers to the use of the kinematic equations of a robot to compute the <u>position of the end-effector</u> from specified values for the joint parameters.

## Problem

> Define a frame {s} (often fixed at the base of a robot) and a frame {b} at the end effector of the robot arm.
> 
> Calculate the Forward Kinematics of the robot: I.e., 
> $Find \newline T(\theta)$

<img src="/home/ronald/.var/app/com.github.marktext.marktext/config/marktext/images/2020-06-02-13-40-38-image.png" title="" alt="" data-align="center">

### Representing Forward Kinematics in the {s} Frame

**Procedure:**

1. Let M be the transformation matrix of the end-effector frame {b} when $\theta$ = 0
   
   In other words, M is the position and orientation of the end effector when all joint angles are set to zero:
   
   $$
   x_b\quad y_b\quad z_b\quad
   $$
   
   $$
   M = 
\begin{bmatrix}
\hat{x_s} & \hat{x_s} & \hat{x_s} & \sum{L}_{x_s} \\
\hat{y_s} & \hat{y_s} & \hat{y_s} & \sum{L}_{y_s} \\
\hat{z_s} & \hat{z_s} & \hat{z_s} & \sum{L}_{z_s} \\
0 & 0 & 0 & 1
\end{bmatrix}
   $$
   
   - The first three 3-vectors (x,y,z) denote which axis in the {s} frame corresponds to a value in the {b} frame. For example, a value of $[0, 1, 0]^T$ in the first column means that the x axis of the {b} frame is aligned with the positive y axis of the {s} frame
   
   - The bottom row is added to simplify matrix operations
   
   - The $\sum{L}$ represents the sum of all of the links from the space frame {s} to the end effector.

2. Find the {s} frame Screw Axes $S_{1}, ..., S_n$ for each of the n joint axes when $\theta = 0$ 
   
   $$
   S_n = 
\begin{bmatrix}
\omega \\
v
\end{bmatrix}
= 
\begin{bmatrix}
\hat{x_s} \\
\hat{y_s} \\
\hat{z_s} \\ \\
\dot{x_s} \\
\dot{y_s} \\
\dot{z_s}
\end{bmatrix}
   $$
   
   - The first 3-vector (angular velocity) represents which axis the space frame {s} is rotating about. For example, a value of $[0,0,1]^T$ means that this joint in the {s} frame is rotating about the positive z-axis
   
   - The linear velocity v can be found by identifying which axis is tangental to the turntable created at the center of the joint n and then multiplying that number with the distance that the joint is from the origin of the {s} frame. 
   
   - ![](/home/ronald/.var/app/com.github.marktext.marktext/config/marktext/images/2020-06-03-21-39-12-image.png)

3. Given $\theta$, calculate the product of exponentials (PoE) formula in the space frame:
   
   > $T(\theta) = e^{[S_1]\theta_1}e^{[S_2]\theta_2}..e^{[s_n]\theta_n}M$

**Example:**

Given a 4 joint RRRP Robot Arm, find the Forward Kinematics:

![](/home/ronald/.var/app/com.github.marktext.marktext/config/marktext/images/2020-06-03-20-28-33-image.png)

<u>1st, Find M:</u>

$$
M = 
\begin{bmatrix}
    0 & -1 & 0 & 19 \\
    -1 & 0 & 0 & 0 \\
    0 & 0 & -1 & -3 \\
    0 & 0 & 0 & 1
\end{bmatrix}
$$

<u>2nd, Find all Screw Axes:</u>

$$
S_1 = 
\begin{bmatrix}
    0 \\
    0 \\
    1 \\ \\
    0 \\
    0 \\
    0 \\
\end{bmatrix}, 
S_2 = 
\begin{bmatrix}
    0 \\ 0 \\ 1 \\ \\
    0 \\ -10 \\ 0 
\end{bmatrix},
S_3 = 
\begin{bmatrix}
    0 \\ 0 \\ 1 \\ \\
    0 \\ -19 \\ 0
\end{bmatrix},
S_4 = 
\begin{bmatrix}
    0 \\ 0 \\ 0 \\ \\ 
    0 \\ 0 \\ 1
\end{bmatrix}
$$

> Note, for $S_4$, the angular velocity is 0 because joint 4 is a prismatic joint which can only slide

<u>3rd, that's it!</u>

We now have the elements necessary to calculate the forward kinematic of the robot T($\theta$) for any given theta

---

### Representing Forward Kinematics in the {b} Frame

**Procedure:**

1. **Unchanged:** Let M be the transformation matrix of the end-effector frame {b} when $\theta$ = 0
   
   $$
   x_b\quad y_b\quad z_b\quad
   $$
   
   $$
   M = 
\begin{bmatrix}
\hat{x_s} & \hat{x_s} & \hat{x_s} & \sum{L}_{x_s} \\
\hat{y_s} & \hat{y_s} & \hat{y_s} & \sum{L}_{y_s} \\
\hat{z_s} & \hat{z_s} & \hat{z_s} & \sum{L}_{z_s} \\
0 & 0 & 0 & 1
\end{bmatrix}
   $$

2. Find the {b} frame Screw Axes $\beta$_{1}, ..., $\beta$_n for each of the n joint axes when $\theta$ = 0
   
   $$
   \beta_n = 
\begin{bmatrix}
\omega \\
v
\end{bmatrix}
= 
\begin{bmatrix}
\hat{x_b} \\
\hat{y_b} \\
\hat{z_b} \\ \\
\dot{x_b} \\
\dot{y_b} \\
\dot{z_b}
\end{bmatrix}
   $$
   
   - The first 3-vector (angular velocity) represents which axis the space frame {b} is rotating about. For example, a value of [0,0,1]^T means that this joint in the {b} frame is rotating about the positive z-axis
   
   - The linear velocity v can be found by identifying which axis is tangental to the turntable created at the center of the joint n and then multiplying that number with the distance that the joint is from the origin of the {b} frame.

3. Given $\theta$, calculate the product of exponentials (PoE) formula in the space frame:
   
   > $T(\theta) = e^{[S_1]\theta_1}e^{[S_2]\theta_2}..e^{[s_n]\theta_n}M$

**Example:**

Given a URRPR spatial open chain robot, determin the screw axis $\beta_i$ in {b} when the robot is in its zero position. **L = 1.**

![](/home/ronald/.var/app/com.github.marktext.marktext/config/marktext/images/2020-06-06-13-11-13-image.png)

<u>1st, Find M:</u>

$$
M = 
\begin{bmatrix}
    1 & 0 & 0 & 3.73 \\
    0 & 1 & 0 & 0 \\
    0 & 0 & 1 & 2.73 \\
    0 & 0 & 0 & 1
\end{bmatrix}
$$

<u>2nd, Find all Screw Axes:</u>

$$
\beta_1 = 
\begin{bmatrix}
    0 \\ 0 \\ 1 \\ \\
    0 \\ 2.73 \\ 0 \\
\end{bmatrix}, 
\beta_2 = 
\begin{bmatrix}
    0 \\ 1 \\ 0 \\ \\
    2.73 \\ 0 \\ -2.73 
\end{bmatrix},
\beta_3 = 
\begin{bmatrix}
    0 \\ 1 \\ 0 \\ \\
    3.73 \\ 0 \\ -1
\end{bmatrix},
\beta_4 = 
\begin{bmatrix}
    0 \\ 1 \\ 0 \\ \\ 
    2 \\ 0 \\ 0
\end{bmatrix}
\beta_5 = 
\begin{bmatrix}
    0 \\ 0 \\ 0 \\ \\ 
    0 \\ 0 \\ 1
\end{bmatrix}
\beta_6 = 
\begin{bmatrix}
    0 \\ 0 \\ 1 \\ \\ 
    0 \\ 0 \\ 0
\end{bmatrix}
$$

> Calculating the linear velocity looks complicated but is still quite simple. Take a look at $\theta_1$. If you were to map that rotation onto the body frame {b}, you would notice that axis y is tangential to the rotation. It is also **2.73 units away from the {b} frame** which is why it gets the value assigned. 
> 
> Next, take a look at $\theta_2$. It's rotation is tangential to both the x and z frames, so those will be assigned values. It is translated along the $z_b$ axis by a unit of 2.73 and along the $x_b$ axis by a unit of 2.73. The sign is produced from applying the multiplication formula: $v_i = -\omega_i \times q_i$. Here, -$\omega_i$ is (0,-1,0), which will invert the sign of the z axis.
> 
> Alternatively, you could just apply the formula for each vector to determine the linear velocity. Less error prone even though it takes more time. 

<u>3rd, that's it!</u>

We now have the elements necessary to calculate the forward kinematic of the robot $T(\theta)$ for any given theta

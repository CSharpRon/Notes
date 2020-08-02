---
title: "Rigid Body Motion"
section: 3.1
keywords:
- rotation-matrices
- space-frame
- frames
- SO(3)
---

## Terms

**Rigid Body**:

**RHR**:

**Determinate**:

---

## Rigid Body Motion

Since we are focused on rigid bodies, we can represent them implicitly using **frames**. In three-dimensional kinematics, we use a frame which consists of an origin and orthogonal x, y, and z coordinate areas. Visualizing this frame is done with the **right-hand rule**:

![](https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/a4c35dde45cca2a6b36e89d36f8fba65.jpg)

When the frame is stationary, we can use it as a point of reference. This is known as the **space frame** and often appears as:

<img title="" src="https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/4ef6be958cfc77737738c2eec13991ab.jpg" alt="" data-align="center">

To represent the position and orientation of a body in space, just fix a frame to the body (known as the **body frame**) and fix a frame in space.

![](https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/51db149f8ea3f6b5873f489842eedc80.jpg)

## Rotation Matrices

Continuing on with representing configuration, let's take a look at orientation.

As we mentioned before, we have a space frame (reference) and a body frame that is mapped onto the body we are interested in analyzing. Let's take an example:

<img title="" src="https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/805151830b9a10cdb9ce9cf1ef7178b2.jpg" alt="" data-align="center">

We define a **rotation matrix** to express the orientation of {b} relative to {s} by writing the unit coordinate axes of {b} in the coordinates of frame {s}. For this example:

$$
\hat{x}_b=\begin{bmatrix}0 \cr 1 \cr 0\end{bmatrix},
\hat{y}_b=\begin{bmatrix}-1 \cr 0 \cr 0\end{bmatrix},
\hat{z}_b=\begin{bmatrix}0 \cr 0 \cr 1\end{bmatrix}
$$

We can then write these column vectors side by side to form the rotation matrix $R_{sb}$:

$$
R_{sb} = [\hat{x}_b\hat{y}_b\hat{z}_b] = 
\begin{bmatrix}
0 & -1 & 0 \cr
1 & 0 & 0 \cr
0 & 0 & 1
\end{bmatrix}
= R
$$

Since we know from chapter 2 that the space of orientations of a rigid body is only 3-dimensional, but we have 0 numbers in the rotational matrix, we know that the 9 entries must be subject to 6 constraints.

- Three of these constraints are that the column vectors are also unit vectors. 

- The other three constraints are that the dot product of any two of the column vectors is 0.

- We can compactly write these constraints as $R^TR = I$, (identity matrix I), where $I = \begin{bmatrix}1 & 0 & 0 \cr 0 & 1 & 0 \cr 0 & 0 & 1\end{bmatrix}$

- These constraints ensure that the **determinate** of R is either
  
  - +1 (corresponding to right-handed frames)
  
  - -1 (corresponding to left handed frames)

## Properties of Rotation Matrices

From the above constraints, we can describe the **orthogonal group** that all rotational matrices belong to: SO(3). **Special Orthogonal Group SO(3)** is the set of all 3x3 real matrices R such that

1) $R^TR = I$ and

2) $det(R) = 1$

Now that we know rotational matrices are SO(3), we can easily derive several properties about them.

| **Identity**      | Rule                        |
|:----------------- | --------------------------- |
| inverse           | $R^{-1} = R^T \in SO(3)$    |
| closure           | $R_1R_2 \in SO(3)$          |
| associative       | $(R_1R_2)R_3 = R_1(R_2R_3)$ |
| not communicative | $R_1R_2\ne R_2R_1$          |

From these identities we can realize a pretty important rule:

The inverse of a rotation matrix $R^{-1}_{ab} = R^T_{ab} = R_{ba}$

## Other ways to represent orientation

Rotational matrices work great throughout this course and in fact in practice. However it is useful to list other ways to represent orientation. Two promenent ways are through

- Quaternions: 4 variable representation (provides double cover -> 2 quaternions for every rotation)

- Euler Angles (RPY): More intuitive than Quaternions and adds Gimble lock.

## Three Common Uses for a Rotational Matrix

### 1) Represent an Orientation

We already showed how we can represent a body frame {b} in the {s} frame with rotational matrix $R_{sb}$. But we can also represent the space frame in the *body* frame. Applying the inverse rule means that we can say:

$$
R_{bs} = R^T_{sb} = R_{sb}^{-1} =
\begin{bmatrix}
0 & 1 & 0 \cr
-1 & 0 & 0 \cr
0 & 0 & 1
\end{bmatrix}
$$

### 2) Change Reference Frame

Say you were given a rotation matrix 

$$
R_{bc} = 
\begin{bmatrix}
0 & 0 & -1 \cr
0 & 1 & 0 \cr
1 & 0 & 0
\end{bmatrix}
$$

where {c} is being represented in the {b} frame, but you wanted to represent {c} in the {s} frame instead. You can do that thanks to the subscribe cancellation rule:

$$
R_{sc} = R_{sb}R_{bc} = R_{sb}R_{bc} = 
\begin{bmatrix}
0 & -1 & 0 \cr
0 & 0 & -1 \cr
1 & 0 & 0
\end{bmatrix}
$$

> In the third term, note that $R_{bc}$ is "pre-multiplied" to $R_{sb}$

### 3) Rotate a vector or frame

Going back to the {b} frame example, we can see that {b} is obtained from {s} by rotating {s} about the $\hat{z}$ axis $90\degree$. 

$$
R_{sb} = R = Rot(\hat{z}, 90\degree)
$$

We can also define a vector p to show the displacement of the {b} frame from the {s} frame.

![](https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/db36b33ffff7de8dbf28d860d2e88cbe.jpg)

If we premultiply a vector $p_b$ by this rotation operator, we get a change of reference frame to {s} coordinates, as we saw before.

But if the vector is $p_{s}$ in {s} coordinates, then there is no subscript cancellation and instead we get a new vector: $p^\prime_s = R_{p_s}$

- The vector has been rotated, but it is still represented in the same frame {s}.

We can also rotate the frame {c} by premultiplying or postmultiplying $R_{sb}$ by the rotation operator R. 

<img title="" src="https://raw.githubusercontent.com/CSharpRon/Notes/master/images/modern-robotics/11c33793820345d946590f0c118cb64d.jpg" alt="" data-align="center">

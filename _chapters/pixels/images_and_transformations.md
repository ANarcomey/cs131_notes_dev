---
title: Images and transformations
keywords: (insert comma-separated keywords here)
order: 2 # Lecture number for 2020
---

## Geometric Image Formation

**Motivation**. In this section we will explore geometric image formation. Images are projections of 2D projections of the 3D world. For this reason, it is important to establish an understanding of projective geometry and how it can be applied to help us solve problems in computer vision. We begin by looking at geometric primitives in 2-Dimensions and 3-Dimensions.  

# Geometric Primitives

The simplest possible type of geometric primitive is a point. We can represent points as vectors e.g.
In 2d,
$$\textbf{x} = (x, y) \in \mathbb{R}^2$$
In 3d,
$$\textbf{x} = (x, y, z) \in \mathbb{R}^3$$
Many of the operations we wish to perform on points in our image for computer vision are comparatively easier to do with homogenous coordinates as opposed to the heterogeneous ones above. Hence, we need a way to project a point into the projective space:
The projective space is an extension of euclidean space in which two lines always meet at a point (In some cases this point can be at point at infinity). $$$$
**Method**: We can go from heterogenous to homogeneous coordinates by appending a 1 to end of our vector i.e. $(x, y)$ would go to $(x, y, 1)$.
<br>
We can return to heterogenous coordinates by dividing by the last component of the vector i.e. 
$$\begin{bmatrix}
x \\
y \\
w
\end{bmatrix} = \begin{bmatrix}
x/w\\
y/w
\end{bmatrix}$$ $$$$
Appending the 1 allows us to switch from Euclidean space (2 dimensions) into the projective space (3 dimensions). <br> In this case, we refer to the projective space as $\mathbb{P^2} := \mathbb{R^3} - (0, 0, 0)$ 
We can do this because points in the projective space that only differ by scale are equivalent in euclidean space i.e. 
$$(x, y, w) \sim \lambda(x, y, w)$$ $$$$
**Consequences**: We want to work with points in the projective space because it simplifies the linear algebra we have to performe e.g.
1. We can represent lines by $\textbf{l} = (\hat{n}_x, \hat{n}_y, d)$, where $(\hat{n}_x, \hat{n}_y)$ is the normal vector to the line and d is the distance from the origin
2. Line point duality: If we have two points then we can find the line between the two points via the cross product of the two products
$$\textbf{l} = \textbf{x}_1 \times \textbf{x}_2$$
If we have two lines then we can find their intersection point via the cross product
$$\textbf{x} = \textbf{l}_1 \times \textbf{l}_2$$
3. In homogeneous coordinates, we can represent points at infinity with $(x, y, 0)$ and lines. at infinity with $(0, 0, 1)$
4. A projective transformation constitutes a linear transformation on points in homogeneous coordinates. Therefore, the set of projective transformations on three dimensional space is the set of all four by four matrices operating on the homogeneous coordinate representation of 3D space.


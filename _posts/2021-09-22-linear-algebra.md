---
layout: post
title: "Linear Algebra"
description: "Course notes from MIT 18.06SC, Fall 2011."
author: "Nathan Tsai"
toc: true
comments: false
hide: false
search_exclude: false
show_tags: false
categories: [course-notes, mathematics, linear-algebra]
permalink: /linear-algebra
---

# MIT 18.06SC: Linear Algebra, Fall 2011

## Course Resources
* Professor Gilbert Strang
* [Course Lectures]()
* [Course Website](https://ocw.mit.edu/courses/mathematics/18-06sc-linear-algebra-fall-2011/index.htm)
* [Course Syllabus](https://ocw.mit.edu/courses/mathematics/18-06sc-linear-algebra-fall-2011/syllabus/)

## 1. The Geometry of Linear Equations

* [Lecture Summary](https://ocw.mit.edu/courses/mathematics/18-06sc-linear-algebra-fall-2011/ax-b-and-the-four-subspaces/the-geometry-of-linear-equations/MIT18_06SCF11_Ses1.1sum.pdf)

The fundamental problem of linear algebra is to *solve a system of equations*. This is represented by $$A\boldsymbol{x} = \boldsymbol{b}$$.

2-variable example:

$$\begin{aligned} 2x-y &= 0 \\ -x+2y &= 3 \\ \end{aligned}$$

In the **Row Picture** method, we view the system of equations as separate lines on the xy-plane. The solution is $$(1, 2)$$, the point where the two lines intersect.

In the **Column Picture** method, we view the system of equations as a **linear combination** of column vectors. The solution is $$x=1,y=2$$, the coefficients of column vectors that linearly combine to create $$\boldsymbol{b}$$.

$$x \begin{bmatrix} 2 \\ -1 \\ \end{bmatrix} + y \begin{bmatrix} -1 \\ 2 \\ \end{bmatrix} = \begin{bmatrix} 0 \\ 3 \\ \end{bmatrix}$$

In the **Matrix Form** method, we view the system of equations as a matrix multiplication.

$$A\boldsymbol{x} = \boldsymbol{b}$$
$$\begin{bmatrix} 2 & -1 \\ -1 & 2 \\ \end{bmatrix} \begin{bmatrix} x \\ y \\ \end{bmatrix} = \begin{bmatrix} 0 \\ 3 \\ \end{bmatrix}$$

where $$A = \begin{bmatrix} 2 & -1 \\ -1 & 2 \\ \end{bmatrix}$$ is the **coefficient matrix**, $$\boldsymbol{x}=\begin{bmatrix} x \\ y \\ \end{bmatrix}$$, and $$\boldsymbol{b}=\begin{bmatrix} 0 \\ 3 \\ \end{bmatrix}$$

3-variable example:

$$\left\{ \begin{array}{rl} 2x-y &= 0 \\ -x+2y &= -1 \\ -3y+4z &= 4 \end{array} \right.$$

## 2. Elimination with Matrices


## References
[^1]: Footnote
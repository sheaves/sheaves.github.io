---
title: Finding Inverses
---
**4 May 2020**

Recall that the inverse of a matrix $M$ is a matrix $M^{-1}$ such that:
- $M M^{-1} = I$, and
- $M^{-1} M = I$

Here, $I$ is the identity matrix of the appropriate size.

In these exercises, you will compute the inverses of some matrices by hand. I will also introduce some Python commands for defining some special matrices, but these are just for your convenience.

You don't have to type your answers. You can write them on paper and just take a photo. 

## 1. Identity and Scalar Matrices

You can define the $n \times n$ identity matrix $I_n$ in Python using the command ```np.eye(n)```:


```python
import numpy as np

I = np.eye(4)
print(I)
```python

    [[1. 0. 0. 0.]
     [0. 1. 0. 0.]
     [0. 0. 1. 0.]
     [0. 0. 0. 1.]]
    

**Question 1(a)**: What is the inverse of the identity matrix? (for some dimension $n$)

The identity matrix is a special case of a *scalar matrix*, which is any matrix of the form $c I = c \times I$ for some real number $c$.
Such a matrix looks like this:

$$
     \begin{pmatrix}
        c & 0 & 0 & 0
        \\
        0 & c & 0 & 0
        \\
        0 & 0 & c & 0
        \\
        0 & 0 & 0 & c
    \end{pmatrix}
$$

**Question 1(b)**: What happens when you multiply on the left or on the right by a scalar matrix? (This explains why they're called *scalar* matrices).

**Question 1(c)**: What is the inverse of a scalar matrix $cI$? Does the inverse exist for all $c$?

## 2. Diagonal Matrices

Scalar matrices are special cases of *diagonal matrices*, which are square matrices that can have anything along the diagonal, but zero entries everywhere else. Here's an example of a diagonal matrix:

$$
    M = \begin{pmatrix}
        1 & 0 & 0 & 0
        \\
        0 & 2 & 0 & 0
        \\
        0 & 0 & 3 & 0
        \\
        0 & 0 & 0 & 4
    \end{pmatrix}
$$

Note that a diagonal matrix could have $0$ on the diagonal too.

You can define a diagonal matrix using ```np.diag```, followed by a list of entries that you want to put along the diagonal. 
*(Note that you should only have one set of square brackets here!)*


```python
M = np.diag([1,2,3,4])
print(M)
```python

    [[1 0 0 0]
     [0 2 0 0]
     [0 0 3 0]
     [0 0 0 4]]
    

**Question 2(a)**: What happens when you multiply on the left by a diagonal matrix? What about multiplying on the right? 

To help you, you can use the following matrix $N$ to compute $MN$ and $NM$ to help you spot the pattern:


```python
N = np.ones((4,4))
print(N)
```python

    [[1. 1. 1. 1.]
     [1. 1. 1. 1.]
     [1. 1. 1. 1.]
     [1. 1. 1. 1.]]
    

**Question 2(b)**: What is the inverse of a diagonal matrix? Does the inverse always exist?

## 3. Triangular matrices

An *upper triangular matrix* (also called a *right* triangular matrix) is a matrix with anything *along and above* the diagonal, and zeros below the diagonal, like this:

$$
    R = \begin{pmatrix}
        1 & 2 & 3 & 4
        \\
        0 & 2 & 1 & 0
        \\
        0 & 0 & 3 & 5
        \\
        0 & 0 & 0 & 4
    \end{pmatrix}
$$

Similarly, a *lower triangular matrix* (also called a *left* triangular matrix) is a matrix with anything *along and below* the diagonal, and zeros above the diagonal, like this:

$$
    L = \begin{pmatrix}
        1 & 0 & 0 & 0
        \\
        4 & 2 & 0 & 0
        \\
        9 & 0 & 3 & 0
        \\
        2 & 4 & -1 & 4
    \end{pmatrix}
$$

Triangular matrices are great for solving equations:

**Question 3(a)**: Solve for $x_1, \dots, x_4$ in this equation, starting from $x_1$ first, then $x_2$ and so on:

$$
    \begin{pmatrix}
        1 & 0 & 0 & 0
        \\
        4 & 2 & 0 & 0
        \\
        9 & 0 & 3 & 0
        \\
        2 & 4 & -1 & 4
    \end{pmatrix}
    \begin{pmatrix}
        x_1 \\ x_2 \\ x_3 \\x_4
    \end{pmatrix}
    =
    \begin{pmatrix}
        2 \\ 10 \\ 15\\ 13
    \end{pmatrix}
$$

This is called *forward substitution*, since we solve for $x_1$, then substitute $x_1$ into the system to solve for $x_2$, and so on.

**Question 3(b)**: Solve for $x_1, \dots, x_4$ in this equation, but this time start from $x_4$ first, then $x_3$ and so on:

$$
    \begin{pmatrix}
        1 & 2 & 3 & 4
        \\
        0 & 2 & 1 & 0
        \\
        0 & 0 & 3 & 5
        \\
        0 & 0 & 0 & 4
    \end{pmatrix}
    \begin{pmatrix}
        x_1 \\ x_2 \\ x_3 \\x_4
    \end{pmatrix}
    =
    \begin{pmatrix}
        17 \\ 0 \\ 21\\ 12
    \end{pmatrix}
$$

This is called *backward substitution*, since we start from the back this time.

**Question 3(c)**: Compute the inverse of this matrix:

$$
R = \begin{pmatrix}
    1 & 2 & 3
    \\
    0 & 1 & -1
    \\
    0 & 0 & 1
\end{pmatrix}
$$

Do this by using backward substitution to solve for $x$ in $R x = v$, for each of the following $v$:

$$
v = 
\begin{pmatrix}
    1 \\ 0 \\ 0
\end{pmatrix}
,\,
\begin{pmatrix}
    0 \\ 1 \\ 0
\end{pmatrix}
,\,
\begin{pmatrix}
    0 \\ 0 \\ 1
\end{pmatrix}
$$

Then combine your three solutions together to form a matrix.

## 4. Functions as matrices

During our lecture, we learned about how matrices can be treated as function. In this last exercise, we will turn things around, and treat functions as matrices.

For this exercise, we will use $[n]$ to denote the set $\{1,2,3,\dots, n\}$. Any function $f : [n] \to [m]$ can be represented as an $m \times n$ matrix $F$ whose $(i,j)$ entry is given by:

$$
    F_{ij} = \begin{cases}
        0 &\mbox{ if } f(j) \neq i
        \\
        1 &\mbox{ if } f(j) = i
    \end{cases}
$$

For example, suppose we have the function $f: [3] \to [4]$ given by 
$$
    \begin{matrix}
        f(1) & = & 2
        \\
        f(2) & = & 1
        \\
        f(3) & = & 4
    \end{matrix}
$$

Then $F$ is the matrix 
$$
\begin{pmatrix}
    0 & 1 & 0
    \\
    1 & 0 & 0
    \\
    0 & 0 & 0
    \\
    0 & 0 & 1
\end{pmatrix}
$$

Each column has exactly one $1$, and its row is given by $f$ applied to the column number.

**Question 4(a)**: Let $f: [4] \to [4]$ be the invertible function given by
$$
    \begin{matrix}
        f(1) & = & 2
        \\
        f(2) & = & 4
        \\
        f(3) & = & 1
        \\
        f(4) & = & 3
    \end{matrix}
$$

Write down the matrix $F$ corresponding to $f$.

**Question 4(b)**: Let $g$ be the inverse of $f$ from 4(a). What are the values of $g(i)$ for $i = 1,2,3,4$? 

**Question 4(c)**: Let $G$ be the matrix representation of $g$. What does $G$ look like? Do you notice any special relationship between $F$ and $G$? (apart from the fact that $FG = GF = I_4$)

## 5. Non-invertible matrices

**Question 5**: Write up some non-invertible functions $f:[n] \to [m]$ and their corresponding matrices. Try to have at least one with $n < m$ and one with $n > m$. Sum up the number of $1$s in each *row* of those matrices, then do this also for the invertible $f$ in 4(a).

How can you tell if a function is invertible, just by looking at its matrix?

## the end

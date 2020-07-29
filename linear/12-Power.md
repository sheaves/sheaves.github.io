---
title: The Power Method
---
*29 July 2020*

In this post, you will learn about the power method for finding the eigenvector corresponding to the largest eigenvalue.

Throughout, let $A$ denote an $n \times n$ matrix. The power method is given by the following iteration:

- Start with a random vector $v_0 \in \mathbb{R}^n$.
- Define future $v_i$'s via the recurrence relation:

$$
v_{i+1} = \frac{Av_i}{\|Av_i\|},
$$

where $\|w\|$ denotes the *length* or *norm* of the vector $w$.

Under appropriate conditions on $A$ (that you will discover in the homework), the sequence $v_0, v_1, v_2, \dots$ converges to some vector $v$ which satisfies

$$
v = \frac{Av}{\|Av\|}.
$$

# Homework

Questions 1 and 2 are due on 11 Aug (Tuesday). Question 3 is due on 17 Aug (Monday). Feel free to email me if you get stuck or have any questions.

## Question 1

For this question, the following formulas might be helpful:
- Scalars can be moved in and out of matrix products: $A\,(cv) = c\, Av$
- Scalars can be moved in and out of dot products: $v \cdot (cw) = c (v \cdot w) = (cv) \cdot w$
- Formula for the norm: $\|v\| = \sqrt{v \cdot v}$

**a)** Show that any $v$ satisfying $v = \frac{Av}{\|Av\|}$ is an eigenvector of $A$. What is the corresponding eigenvalue?

**b)** Let $v$ be the vector from (a). Use the formula for the norm and the fact that $v$ is an eigenvector to show that $\|Av\| = v \cdot Av$. 

**c)** Returning to the power method, give a formula for $v_{i+1}$ in terms of only $A$ and $v_0$. (This should also tell you why the method is called the *power* method).

## Question 2

Now suppose that $u_1, u_2, \dots u_n$ are all the eigenvectors of $A$, with corresponding eigenvalues $\lambda_1, \lambda_2, \dots, \lambda_n$. (i.e. if $d,E = eig(A)$ is the eigendecomposition of $A$, then $u_i$'s are the columns of $E$ and $\lambda_i$'s are the entries of $d$).

Suppose also that $\|\lambda_1\| > \|\lambda_i\|$ for all other $i$.

Let $v_0 = c_1 u_1 + c_2 u_2 + \dots + c_n u_n$ for some scalars $c_1,\dots, c_n$. 

**a)** For any $i \neq 1$, what is  $\left(\frac{\lambda_i}{\lambda_1}\right)^k$ as $k$ goes to infinity?

**b)** Express $A^k v_0$ in terms of $k$ and all the $c_i, \lambda_i, u_i$'s.

**c)** Use your answer in (b) to express $\frac{A^k v_0}{\lambda_1^k}$. What happens as $k$ goes to infinity?

**d)** Now use $v_0$ as the starting vector in the power method. Show that $v_k$ converges to $u_1$ as $k$ goes to infinity.

**e)** Can you guess what might go wrong if $\lambda_2 = \lambda_1$?

## Question 3

For this coding question, you may use the following, unless stated otherwise:
- ```np.linalg.norm(v)``` returns the norm of a vector
- ```np.dot(v,w)``` returns the dot product of two vectors (note that this is the same as ```v @ w```)
- ```np.sqrt(c)``` returns the square root of a number
- ```np.random.rand(n)``` returns an $n$-dim vector, with entries randomly chosen from the interval [0,1].
- ```np.random.rand(n,m)``` returns an $n \times m$ array, with entries randomly chosen from the interval [0,1].
- ```np.allclose(v,w)``` returns ```True``` if ```v``` and ```w``` are close to each other. It is best to use this instead of ```v == w```, since there might be some differences due to precision.

**a)** Write your own function ```my_norm(v)``` to compute the norm of a vector $v$, without using ```np.linalg.norm```. Compare the results of your function with ```np.linalg.norm``` to make sure it is correct.

**b)** Write a function ```power_method(A,v0,k)``` that returns ```k``` iterations of the power method, applied to the matrix ```A``` and the starting vector ```v0```. You may now use ```np.linalg.norm``` if you want.

**c)** Apply your function to any matrix and starting vector of your choice, for a large number of iterations (50 should be more than enough).

**d)** Verify that the result is indeed an eigenvector, and compute the corresponding eigenvalue (Question 1 might be useful).

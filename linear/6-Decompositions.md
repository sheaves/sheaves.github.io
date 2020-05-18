---
title: Special matrices and matrix decompositions

---
**18 May 2020**

We have already see some special classes of matrices:

- Identity matrices, usually denoted $I$
- Upper or right triangular matrices, usually denoted $R$ or $U$
- Lower or left triangular matrices, usually denoted $L$
- Diagonal matrices, sometimes denoted $D$
- Permutation matrices (i.e. matrices of invertible functions), sometimes denoted $P$

We have also learned about the LU decomposition, which says that any square matrix $M$ can be decomposed as:

$$ M = PLU $$

Today, we will learn about a few more decompositions. But first, let's talk about transposes.

## Tranposes
If $A$ is an $m \times n$ matrix, whose $(i,j)^{th}$ entry is $a_{ij}$, the **transpose** of $A$ is the $n \times m$ matrix $A^\top$ whose $(i,j)^{th}$ entry is $a_{ji}$. For example:

$$
A = \begin{pmatrix} 1 & 2 & 3 & 4 \\ 1 & 2 & 3  & 4 \end{pmatrix}, \quad \quad
A^\top = \begin{pmatrix} 1 & 1 \\ 2 & 2 \\ 3 & 3 \\ 4 & 4 \end{pmatrix}
$$

You can think of $A^\top$ as flipping $A$ across its diagonal. 

In Python, the transpose of a matrix ```A``` is given by ```A.T``` or ```A.transpose()```. *Warning: Take note of the round brackets at the end of ```transpose```!*

## Symmetric and Orthogonal matrices

A square matrix is **symmetric** if $A  = A^\top$. 

A square matrix is **orthogonal** if $AA^\top = A^\top A = I$

**Question 1:** Among the classes of matrices $I, R,L,D,P$, which are symmetric?

**Question 2:** What is the inverse of an orthogonal matrix $Q$?

**Question 3:** Among the classes of matrices $I, R,L,D,P$, which are orthogonal?

## The LU Decomposition

I mentioned in the lecture that if we can write a matrix $A$ as $A = LU$, then we can solve for $x$ in $Ax = b$. But I didn't show you how to do this.

For the next question, assume that we have a fixed matrix $A$, decomposed into  $A = LU$.
For any $y$, let $l(y)$ be the solution of $Lx = y$ (i.e. $Ll(y) = y$), and let $u(y)$ be the solution of $Ux = y$.

(You have already seen how to compute $l(y)$ and $u(y)$ in a previous homework, using forward and backward substitution!)

**Question 4:** Express the solution of $Ax = b$, using $b$ and the functions $l$ and $u$.

## The QR decomposition

The **QR decomposition** is a decomposition of a matrix $A$ into $A = QR$ where $Q$ is orthogonal and $R$ is upper triangular. 

(We usually $Q$ instead of $O$ to avoid any confusion with $0$. Recall also that an upper triangular matrix is also called *right* triangular. This explains the notation $R$.)

Now suppose $A = QR$, and for any $y$, let $r(y)$ be the solution of $Rx = y$.

**Question 5:** Express the solution of $Ax = b$, using $b$, the function $r$ and the matrix $Q$ and/or its tranpose.

## Powers of diagonal matrices

Since diagonal matrices are also triangular, they share the benefits of triangular matrices, so it's very easy to solve equations of the form $Dx = b$ where $D$ is diagonal.

But diagonal matrices have an extra benefit that triangular matrices do not have.

**Question 6:** Let $D$ be a diagonal matrix, with entries $d_1, d_2, \dots, d_n$ on the diagonal.
What are the entries of $D^k$, for any integer $k$? 

**Question 7:** Suppose that $A = M D M^{-1}$, where $D$ is a diagonal matrix, and $M$ is some invertible matrix. 
Express $A^k$ in terms of $M$, $M^{-1}$ and $D^k$.

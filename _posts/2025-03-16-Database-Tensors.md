---
layout: post
title: Tensor associated to a database
draft_tag: 
- Tensors and Language Models
---
In this post, we will be interested in collections of tables or dataframes that look something like:

| NAME   | born_in    | lives_in |
|------ | --------   | ------- |
|Astrid| Singapore  | Malaysia  |
|Bernard| Singapore | Singapore  |
|Colin| Malaysia   | Malaysia  |

| COUNTRY | currency|
|-------|---------|
|Malaysia|Ringgit|
|Singapore|Dollar|

We will call these *databases*. 
This example comes from the paper, [Paying attention to facts: quantifying the knowledge capacity of attention layers]
(arXiv:2502.05076}{:target="_blank"}.

By the end of this post, we will see how to define a *tensor* associated to a database.
But first, we will begin by defining a *matrix* associated to a function $f: X \to Y$.

<!--more-->

### Matrix representation of a function
Suppose we have a set $X = \{a,b,c\}$, another set $Y = \{m,s\}$ and a function $f \colon X \to Y$ given by

$$
\begin{align}
  f \colon X &\to Y \\
  a &\mapsto s \\
  b &\mapsto s \\
  c & \mapsto m
\end{align}
$$

For such a function, we may define an $|X| \times |Y|$ matrix $M_f$ given by:
$$
  (M_f)_{xy} =
		\begin{cases}
			1 \mbox{ if } f(x) = y,\\
			0 \mbox{ otherwise}.
		\end{cases}
$$

For the function $f$ above, we would have

$$
		M_f = 
		\begin{array}{c@{}c}
			&
			\begin{array}{cc}
				m & s
			\end{array}
		\\
			\begin{array}{c}
				a \\ b \\ c
			\end{array}
			&
			\left[
				\begin{array}{cc}
					0 & 1 
					\\ 
					0 & 1 
          \\
          1 & 0
				\end{array} 
			\right]
		\end{array}
$$
The matrix $M_f$ *represents* the function $f$ in the following sense: if we represent elements of $X$ as vectors $e_a = (1,0,0),\, e_b = (0,1,0),\, e_c=(0,0,1)$ in $\mathbb{R}^{|X|}$ and elements of $Y$ as vectors $e_m = (1,0),\, e_s = (0,1)$ in $\mathbb{R}^{|Y|}$ (i.e. one-hot encoding), then we have
$$
  e_x \, M_f = e_{f(x)}.
$$
For example, $e_a\, M_f = e_s = e_{f(a)}$. 
In other words, $M_f$ stores the values of $f$, and can be used to compute $f$.

### The rank of $M_f$
Since $M_f$ is a matrix, we can compute its [rank](https://en.wikipedia.org/wiki/Rank_(linear_algebra)){:target="_blank"}.
There are a few [equivalent definitions](https://en.wikipedia.org/wiki/Rank_(linear_algebra)#Alternative_definitions){:target="_blank"} for the rank of a matrix, but we will use the [one that generalizes most easily to tensors](https://en.wikipedia.org/wiki/Rank_(linear_algebra)#Tensor_rank_%E2%80%93_minimum_number_of_simple_tensors){:target="_blank"}, since tensors are where we're headed next.

In order to state this definition, we first define a rank 1 matrix to be a non-zero matrix that can be expressed as a matrix product $c \cdot r$ of a column vector $c$ and a row vector $r$. The following example shows a rank 1 matrix and its decomposition into $c$ and $r$:

$$ 
\left[
  \begin{array}{cc}
    0 & 0 
    \\ 
    0 & 0 
    \\
    1 & 0
  \end{array} 
\right]
= 
\left[ \begin{array}{c} 0 \\ 0 \\ 1 \end{array} \right] 
\left[ \begin{array}{cc} 1 & 0 \end{array} \right]
$$
The rank of a matrix $M$ is then the smallest $k$ such that $M$ can be expressed as a sum of $k$ rank 1 matrices.

From this, we can see that an upper bound for the rank of $M$ is simply the number of non-zero entries it has, since $M$ can be expressed as a sum of matrices that contain only 1 non-zero entry.  
For example, the matrix $M_f$ above can be written as the sum:

$$
M_f = 
\left[
  \begin{array}{cc}
    0 & 1 
    \\ 
    0 & 0 
    \\
    0 & 0
  \end{array} 
\right]
+
\left[
  \begin{array}{cc}
    0 & 0 
    \\ 
    0 & 1 
    \\
    0 & 0
  \end{array} 
\right]
+
\left[
  \begin{array}{cc}
    0 & 0 
    \\ 
    0 & 0 
    \\
    1 & 0
  \end{array} 
\right]
$$
and thus $\mathrm{rank}(M_f) \leq 3$. 

But in fact, the rank of $M_f$ is 2, because this is also a rank 1 matrix:
$$
    \left[
      \begin{array}{cc}
        0 & 1 
        \\ 
        0 & 1 
        \\
        0 & 0
      \end{array} 
    \right]
  \end{array}
  = 
  \left[ \begin{array}{c} 1 \\ 1 \\ 0 \end{array} \right] 
  \left[ \begin{array}{cc} 0 & 1 \end{array} \right]
$$
and we can decompose $M_f$ as
$$
M_f = 
\left[
  \begin{array}{cc}
    0 & 1 
    \\ 
    0 & 1 
    \\
    0 & 0
  \end{array} 
\right]
+
\left[
  \begin{array}{cc}
    0 & 0 
    \\ 
    0 & 0 
    \\
    1 & 0
  \end{array} 
\right].
$$

More generally, TODO: rank inequality

Note: while our rank 1 matrices above had many zeros and only a few non-zero entries, keep in mind that this need not be the case. 
For example, here is another example of a rank 1 matrix that has no zeros:
$$
    \left[
      \begin{array}{cc}
        1 & 2 
        \\ 
        2 & 4 
        \\
        3 & 6
      \end{array} 
    \right]
  \end{array}
  = 
  \left[ \begin{array}{c} 1 \\ 2 \\ 3 \end{array} \right] 
  \left[ \begin{array}{cc} 1 & 2 \end{array} \right]
$$

### Why matrices? Why rank?
The rank of $M_f$ gives us some notion of the size of $f$.
But why go through all that trouble? 
Why not just represent $f$ as a dictionary or hashmap (e.g. ```f = {'a': 's', 'b':'s', 'c':'m'}```), and define its size to be the number of entries in this dictionary (which coincides with the number of non-zero entries of $M_f$)?

Because linear algebra is the language of neural networks.
Indeed, an $m \times n$ rank 1 matrix $M$ in can be represented as a neural network with 1 hidden neuron, where the activation functions are all just the identity function:
![Rank 1 matrix as a neural network](/images/rank_1_matrix.png "Rank 1 matrix")
If $c \dot r$ is the decomposition of $M$, the weights on the left are the entries of $c$, while the weights on the right are the entries of $r$.

A rank $k$ matrix is then a neural network with $k$ hidden neurons:
![Rank k matrix as a neural network](/images/rank_k_matrix.png "Rank k matrix")

So the rank of $M_f$ tells us how many hidden neurons we need to represent the function $f$ when using a linear network (i.e. with identity activation functions).

In practice, neural networks tend to have non-linear activation functions (such as ReLu or various sigmoids), which complicates the relationship with rank.
However, even in those settings, the number of hidden neurons continues to be a useful measure of the size or complexity of a network.

### From functions to databases
Let's move on from functions to databases.
Going back to the example database at the top of this post, we see that every column in a database is a function.
For example, the column ```born_in``` is the same as the example function $f$ if we let $X$ be the set of Names, $Y$ the set of countries, and replace Astrid with $a$, Bernard with $c$, and so on.

As we have seen, every function can be represented as a matrix.
Since we have multiple functions, we have multiple matrices, which we can stack into a 3-dimensional tensor.


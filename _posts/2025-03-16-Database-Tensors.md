---
layout: post
title: Tensor associated to a database
draft_tag: 
- Tensors and Language Models
---
In this post, we will be interested in collections of tables or dataframes that look something like:

![Example of a database](/images/database_example.png "Example of a database")

We will call these *databases*. 
This example comes from the paper, [Paying attention to facts: quantifying the knowledge capacity of attention layers]
(https://arxiv.org/abs/2502.05076){:target="_blank"}.

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
Functions are columns in databases. For example, $f$ above is the column ```born_in``` if we replace $a$ with Astrid,  $b$ with Bernard, and so on.

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

For example, one can verify that $e_a\, M_f = e_s = e_{f(a)}$. 
Thus $M_f$ stores the values of $f$, and can be used to compute $f$.

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

From this, we can see that an upper bound for the rank of $M$ is simply the number of non-zero entries it has, since $M$ can be expressed as a sum of matrices that contain only 1 non-zero entry.  For example, the matrix $M_f$ above can be written as the sum:

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

In fact, for any function $f : X \to Y$ between finite sets, we can always decompose $M_f$ into a sum of $k$ rank 1 matrices, one for each element in the [range](https://en.wikipedia.org/wiki/Range_of_a_function){:target="_blank"} of $f$, so that we have:

$$
\mathrm{rank}(M_f) = |\mathrm{range}(f)|.
$$

We thus see that the rank of $M_f$ gives us some notion of the size of $f$.

### Why matrices? Why rank?
But why go through all that trouble? Why not just represent $f$ as a dictionary or hashmap (e.g. ```f = {'a': 's', 'b':'s', 'c':'m'}```), and define its size to be the number of entries in this dictionary? (which coincides with the number of non-zero entries of $M_f$)

The reason is that linear algebra is the language of neural networks. Indeed, a rank 1 matrix $M$ can be represented as a neural network with 1 hidden neuron, where the activation functions are all just the identity function:
![Rank 1 matrix as a neural network](/images/rank_1_matrix.png "Rank 1 matrix")
If $c \dot r$ is the decomposition of $M$, the weights on the left are the entries of $c$, while the weights on the right are the entries of $r$.

A rank $k$ matrix is then a neural network with $k$ hidden neurons:
![Rank k matrix as a neural network](/images/rank_k_matrix.png "Rank k matrix")

So the rank of $M_f$ tells us how many hidden neurons are needed to represent the function $f$ when using a linear network (i.e. a network with identity activation functions).

Of course, "real" neural networks usually have non-linear activation functions (such as ReLu or various sigmoids). But as papers such as [this](https://arxiv.org/abs/2012.14913){:target="_blank"} show, such networks also store functions (or "key-value memories").
In this setting, the number of hidden neurons continues to be a useful measure of the size or capacity of a network, even though it no longer equals rank due to non-linearity.

### From functions to databases
So far, we have seen that functions (or key-value pairs, associative memories, dictionaries, hashmaps etc.) can be stored in matrices/neural networks. But functions or key-value pairs are not yet facts. Here's what I mean: consider the key-value pair ```(Michael Jordan, Basketball)```, which is sometimes given as an example of a "fact". Indeed, Michael Jordan is most strongly associated to basketball, and if you asked someone to imagine "Michael Jordan", chances are they would imagine him playing basketball or in a basketball jersey. 

But what is happening here is that we are implicitly filling in a relation between Michael Jordan and basketball, such as ```played``` (for most of his career), or ```is most often associated with```. And the facticity of the pair depends on this relation. If we change the relation to something like ```played in 1994```, then the correct fact should mention ```baseball``` instead of ```basketball```.

To put it another way, a function or a set of key-value pairs is only *one* column in a database, and a fact consists not only of a key-value pair, but also the name of the column, which indicates the relationship between the key and the value. Going back to the example database at the top of this post, the pair ```(Astrid, Singapore)``` is an incomplete fact. The full fact is ```(Astrid, born_in, Singapore)```. 




---
title: Matrices
---

**Lecture 2 : 20 April 2020**

In this lecture, we will introduce the main characters in our story: matrices.

A **matrix** is a just a table of real numbers, like the following:

$$
    \begin{pmatrix}
        1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \\ 0.5 & -1 & 6
    \end{pmatrix}
$$

Above we have a **4 × 3 matrix** or **(4,3)-matrix**, pronounced "4-by-3 matrix".

4 × 3 or (4,3) is called the **shape** of the matrix.

More generally, we can talk about m × n or (m,n)-matrices, where m and n are integers.

We always write the number of *rows* followed by the number of *columns*. To help you remember this, think of **R**emote **C**ontrol : **R**ows and **C**olumns.



# Operations on matrices

We can add two matrices of the same shape, by adding them entrywise:

$$
    \begin{pmatrix}
        1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \\ 0.5 & -1 & 6
    \end{pmatrix}
    + 
    \begin{pmatrix}
        1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \\ 1 & 1 & 1
    \end{pmatrix}    
    =
    \begin{pmatrix}
        2 & 2 & 3 \\ 4 & 6 & 6 \\ 7 & 8 & 10 \\ 1.5 & 0 & 7
    \end{pmatrix}       
$$

We can multiply a matrix by a real number:

$$
    2.5\,
    \begin{pmatrix}
        1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \\ 0.5 & -1 & 6
    \end{pmatrix}
    =
    \begin{pmatrix}
        2.5 & 5 & 7.5 \\ 10 & 12.5 & 15 \\ 17.5 & 20 & 22.5 \\ 1.25 & -2.5 & 15
    \end{pmatrix}       
$$

This is known as **scalar multiplication**. The scalar refers to the real number that we are multiplying by.

Finally, we can multiply an m × n matrix with an n × p matrix to produce an m × p matrix: 

$$
    \begin{pmatrix}
        1 & 2 & 3
        \\
        4 & 5 & 6
    \end{pmatrix}
    \begin{pmatrix}
        1 & 2 & 3
        \\
        4 & 5 & 6
        \\
        7 & 8 & 9
    \end{pmatrix}
    = 
    \begin{pmatrix}
       30 & 36 & 42
       \\
       66 & 81 & 96
    \end{pmatrix}
$$

This is known as **matrix multiplication** to distinguish it from scalar multiplication.

In summary, we have the following operations on matrices:
- **Addition** : Add two matrices of the *same shape*
- **Scalar multiplication** : Multiply any matrix by a scalar
- **Matrix multiplication** : Multiply an m × n matrix with an n × p matrix

# Matrices in Python

We will now go through some basic matrix examples in Python. Let's define the following matrix and find its shape:

$$
    A = 
    \begin{pmatrix}
        1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \\ 0.5 & -1 & 6
    \end{pmatrix}
$$

<div class="python">
  <script type="text/x-sage">
import numpy as np

A = np.array([[  1,   2,   3],
              [  4,   5,   6],
              [  7,   8,   9],
              [0.5,  -1,   6]])

print(A.shape)
  </script>
</div>

Note the following:
- We need to import the numpy (Numerical Python) library before using it. We import it and use the short-form "np" to refer to it.
- We use numpy arrays to represent matrices.
- Nested square brackets: We create a matrix by feeding a *list of lists* into the ```np.array``` function. Observe the order in which we enter the items.
- Commas: we need commas between entries, and also between rows.

Now let's carry out the operations on matrices that we saw above.

## Addition

Addition of matrices is simple: just use ```+```


<div class="python">
  <script type="text/x-sage">
import numpy as np

A = np.array([[  1,   2,   3],
              [  4,   5,   6],
              [  7,   8,   9],
              [0.5,  -1,   6]])

B = np.array([[1,0,0], [0,1,0], [0,0,1], [1,1,1]])

print(A + B)
  </script>
</div>


Note that I defined matrix $B$ slightly differently from matrix $A$. Both methods work: $A$ is clearer, but $B$ is more compact. Just make sure to put square brackets and commas in the right places!

## Scalar multiplication

Use ```*```:

<div class="python">
  <script type="text/x-sage">
import numpy as np

A = np.array([[  1,   2,   3],
              [  4,   5,   6],
              [  7,   8,   9],
              [0.5,  -1,   6]])

print(2.5 * A)
  </script>
</div>


Note that ```A * 2.5``` works as well.

## Matrix multiplication

Use ```@```:

<div class="python">
  <script type="text/x-sage">
import numpy as np

A = np.array([[1,2,3], [4,5,6]])

B = np.array([[1,2,3],[4,5,6],[7,8,9]])

print(A @ B)
  </script>
</div>


Older versions of Python don't let you use ```@``` for matrix multiplication. Instead you have to use ```np.dot```:

<div class="python">
  <script type="text/x-sage">
import numpy as np

A = np.array([[1,2,3], [4,5,6]])

B = np.array([[1,2,3],[4,5,6],[7,8,9]])

print(np.dot(A,B))
  </script>
</div>

You should be able to use ```@``` if you've downloaded the latest version of Python, but it's good to keep ```np.dot``` in mind as you'll see it quite often when looking at other people's code.

## Warning!

Numpy allows you to carry out some other operations with ```+``` and ```*```. Can you guess what happens with each of the following?

<div class="python">
  <script type="text/x-sage">
import numpy as np

A = np.array([[1,2,3], [4,5,6]])

print(A + 1)
  </script>
</div>


<div class="python">
  <script type="text/x-sage">
import numpy as np

A = np.array([[1,2,3], [4,5,6]])
B = np.array([[1,0,2], [-2,1,0]])

print(A * B)
  </script>
</div>

It's ok to use them, but make sure it's what you want!

# Homework


## Part 1: Find the error
(The cells cannot be evaluated in the slides)
Each of the code cells below contains some errors. Can you modify the code in each cell so that it works? 

Feel free to delete  or make up some entries if you need.

#### Question 1
<div class="python">
  <script type="text/x-sage">
A = np.array([[1,2,3], [4,5,6]])

print(A)
  </script>
</div>

#### Question 2
<div class="python">
  <script type="text/x-sage">
import numpy as np

A = np.array([1,2,3], [4,5,6])

print(A)
  </script>
</div>

#### Question 3
No errors, but is this the output that you expected?
<div class="python">
  <script type="text/x-sage">
import numpy as np

A = np.array([[1,1,1], [1,1]])

print(A.shape)
  </script>
</div>

#### Question 4

The code works, but is this the right way to compute AB?

<div class="python">
  <script type="text/x-sage">
import numpy as np

A = np.array([[1,2], [3,4]])
B = np.array([[1,0],[0,1]])

print(A * B)
  </script>
</div>


## Part 2: 

1. Download and install Anaconda from [https://www.anaconda.com/distribution/](https://www.anaconda.com/distribution/)
1. Follow the instructions [here](https://www.edureka.co/blog/spyder-ide/) to open Spyder, and familiarize yourself with the **Editor**, the **Console** and the **Variable Explorer**.

Try to define some of the matrices we've seen above in Spyder, and play around with some matrix operations. If there are any errors, try to figure out what's wrong. 

Feel free to email me if you have any questions!

# the end

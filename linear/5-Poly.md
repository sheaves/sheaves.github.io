---
title: Functions and loops in Python
---
**11 May 2020**

By the end of this homework, you should be able to do the following things in Python:

- Define functions
- Create lists from other lists
- Combine the two things above to quickly define arrays

In particular, you will understand all the parts of the following template to create an array:


```python
import numpy as np

# Function that gives the (i,j)th entry of our matrix
def a(i,j):
    return i + j

# Define the matrix A
A = np.array([[ a(i,j) for j in range(4) ] for i in range(5) ])

print(A)
```

    [[0 1 2 3]
     [1 2 3 4]
     [2 3 4 5]
     [3 4 5 6]
     [4 5 6 7]]
    

The above code produces a $4 \times 5$ matrix whose $(i,j)^{th}$ entry is $i - j$. If you understand everything in the above code, you can skip to the homework at the end. 

Otherwise, let's go through the code and explain the new features.

## Functions: ```def```...```return```

We use the keywords **def** and **return** to define a new function in Python. The general template for functions is:


```python
def name_of_function(input1, input2, input3, ...):
    # Compute the output
    return output
```

In our example, the name of our function is ```a```, and it takes two inputs, ```i``` and ```j```. Since the output is so simple (```output = i+j``` in this case), we could return it immediately. But more complicated functions usually involve some extra steps before returning the output.

## Ranges

The next new thing we encounter is the ```range(n)``` function. This gives us a quick way of producing a list of length $n$.

**Important note**: Like many other programming languages, Python practices zero-indexing, which means counting starts from 0, not 1. So ```range(n)``` gives the list ```[0,1,2,...,n-1]``` which has length $n$.

To produce the list ```[1,2,3,..., n]```, we can use ```range(1,n+1)``` instead. 
In general, we can use ```range(m,n)``` for any $m < n$, to produce the list of integers from $m$ to $n-1$. If $m$ is not specified (i.e. if you write ```range(n)```), Python assumes that $m = 0$.

Note that if you try to print a range, it doesn't actually give you the list. That's because the entries of range are only created when you need them. To force Python to produce the whole list, you can use ```list```.


```python
my_range = range(4)
my_list  = list(range(4))

print("range(4):", my_range)
print("list(range(4)):", my_list)
```

    range(4): range(0, 4)
    list(range(4)): [0, 1, 2, 3]
    

## Loops: ```for``` ... ```in```

The last new thing we encounter is the pattern ```for```...```in```. This pattern is used to run through all elements of a list. By running through the list ```range(4)```, we can produce a loop of length $n$:


```python
for i in range(4):
    print(i)
```

    0
    1
    2
    3
    

The pattern  ```[ ___ for ___ in ______ ]``` combines square-brackets for lists with ```for```...```in``` . This is a very powerful way of creating a new list from an existing list. The general syntax is:


```python
new_list = [ some_function(x) for x in existing_list ]
```

This creates a ```new_list``` with the same length as ```existing_list```, but with ```some_function(x)``` in place of each entry ```x``` in the ```existing_list```.

Here are some examples, where ```existing_list``` is ```range(4)```:


```python
do_nothing = [   i for i in range(4)]
all_ones   = [   1 for i in range(4)]
i_plus_2   = [ i+2 for i in range(4)]
listify    = [ [i] for i in range(4)]

print("do_nothing:", do_nothing)
print("all_ones:", all_ones)
print("i_plus_2:", i_plus_2)
print("listify:", listify)
```

    do_nothing: [0, 1, 2, 3]
    all_ones: [1, 1, 1, 1]
    i_plus_2: [2, 3, 4, 5]
    listify: [[0], [1], [2], [3]]
    

As we see in the last example above, lists can also be nested in other lists. This is very useful for creating arrays. Let's see some examples:


```python
# All ones
all_ones = [[ 1 for i in range(4)] for j in range(5)]
row_num =  [[ j for i in range(4)] for j in range(5)]
col_num =  [[ i for i in range(4)] for j in range(5)]

print(np.array(all_ones))
print(np.array(row_num))
print(np.array(col_num))
```

    [[1 1 1 1]
     [1 1 1 1]
     [1 1 1 1]
     [1 1 1 1]
     [1 1 1 1]]
    [[0 0 0 0]
     [1 1 1 1]
     [2 2 2 2]
     [3 3 3 3]
     [4 4 4 4]]    
    [[0 1 2 3]
     [0 1 2 3]
     [0 1 2 3]
     [0 1 2 3]
     [0 1 2 3]]     

These examples are all quite simple, so there was no need to define the function ```a(i,j)``` separately. Here's a slightly more complicated example that uses logic statements:

```python
# Identity matrix

def id(i,j):
    if i == j:
        return 1
    else:
        return 0
    
I4 = np.array([[ id(i,j) for j in range(4) ] for i in range(4) ])
print(I4)
```

    [[1 0 0 0]
     [0 1 0 0]
     [0 0 1 0]
     [0 0 0 1]]
    

We'll get to logic statements and the ```if```...```else``` pattern another time. But the meaning should be clear in this example.

# Homework

**Question 1** In this question, we will find the value of $p(4)$, where $p$ is a polynomial such that

$$
    \begin{matrix}
        p(0) & = & 1
        \\
        p(1) & = & 0
        \\
        p(2) & = & 9
        \\
        p(3) & = & 46
    \end{matrix}
$$

Do this in the following steps:

1. Modify the array template given at the top of the page to create "matrix of powers" that we saw in the lecture. Before you start, you might want to think about how big your matrix should be. Note that $a^b$ is written ```a**b``` in Python.
1. Use this matrix (and its inverse) to solve for the coefficients of $p$. These coefficients will be the entries of $v$ that solve the equation $Av = b$, where $A$ is the matrix you created. You'll have to decide what $b$ is. I've chosen the entries of $v$ to be integers, so you can use ```np.round(v)``` to round the answer.
1. Find a vector $u$ such that ```u @ v``` gives the value $p(4)$. (Here Python will treat ```u``` as a row vector, and ```v``` as a column vector). The vector ```v``` is the solution you just found, so you just have to decide what the entries of $u$ are.

**Question 2** Do this only if you have time. Now you are given

$$
    \begin{matrix}
        q(0) & = & -4
        \\
        q(1) & = & -2
        \\
        q(2) & = & 2
        \\
        q(4) & = & 40
    \end{matrix}
$$
and you want to compute $q(3)$. Repeat what you did above, with the necessary modifications.

*(Hint: You might want to replace one of the ranges with your own list)*

# the end   

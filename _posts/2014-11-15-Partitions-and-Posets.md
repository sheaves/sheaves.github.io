---
layout: post
title: Partitions and Posets
tag: Combinatorics
---

In this post, we generate the Hasse diagram of a set partition.

![Partitions of a 4 element set](/images/partitions.png "Partitions of a 4 element set")

<!--more-->

The following piece of code generates the image above.

<div class="sage">
  <script type="text/x-sage">
N = 4
  
def Partition_Poset(X):
    return Poset((SetPartitions(X),lambda q,p: q in p.refinements()))

def p_label(p):
    out = ""
    for block in p:
        for elm in block:
            out += str(elm)
        out += "|"
    return out[:-1]

Po = Partition_Poset(N)
Po.plot(element_labels = {x:p_label(x) for x in Po},vertex_size=500,vertex_shape=None)
  </script>
</div>

Read on for an explanation of the code.

# Partitions

A [**partition of a set**](http://en.wikipedia.org/wiki/Partition_of_a_set) $X$ is a collection $p$ of non-empty subsets of $X$ such that $X$ is the disjoint union of these sets.

In SAGE, you can get the set of all partitions of the $N$ element set {$1,2,\dots,N$} using [**`SetPartitions`**](http://www.sagemath.org/doc/reference/combinat/sage/combinat/set_partition.html):

<div class="sage">
  <script type="text/x-sage">
N = 3
P = SetPartitions(N)
for p in P:
  print(p)
  </script>
</div>

### Refinements
Each item in the preceding list is a partition of $X$. The elements of each partition are called **blocks**. At the top of the list, we see the *trivial partition* consisting of just one block, $X$. At the other end, we see the *singleton partition* consisting of $|X|$ blocks, where each block contains a single element of $X$. All other partitions of $X$ fall somewhere in-between these two partitions.

We can make this notion of "in-betweenness" precise by defining a relation on the partitions of $X$. We say that $q$ is a [refinement](http://en.wikipedia.org/wiki/Partition_of_a_set#Refinement_of_partitions) of $p$ if each block of $q$ is contained in some block of $p$. 

In Sage, you can see the refinements of a partition using the method `refinements()`:

<div class="sage">
  <script type="text/x-sage">
N = 3
P = SetPartitions(N)

p = P[2] # take some partition
print('p = ' + str(p))
print('Its refinements are:')
for q in p.refinements():
  print(q)
  </script>
</div>

# Posets
For a fixed $X$, the set $\mathcal{P}$ of all partitions of $X$ has the structure of a [**poset**](http://en.wikipedia.org/wiki/Partially_ordered_set) given by $q \leq p$ if $q$ is a refinement of $p$. 

In Sage, we can construct a [**`Poset`**](http://www.sagemath.org/doc/reference/combinat/sage/combinat/posets/posets.html) by specifying an underlying set $P$ along with a function $f:P\times P \to$  {$\text{True},\text{False}$} where

$$
f(q,p)=
\begin{cases}
\text{True} &\text{ if } q \leq p \\
\text{False} &\text{ otherwise}.
\end{cases}
$$

The resulting poset can be visualized via its [**Hasse diagram**](http://en.wikipedia.org/wiki/Hasse_diagram), which is a directed graph with paths from $q \to p$ if $q \leq p$. We can generate a Hasse diagram of a poset using the `show()` method.

<div class="sage">
  <script type="text/x-sage">
N = 3
P = SetPartitions(N)
f = lambda q,p: q in p.refinements()

Po = Poset((P,f))
Po.show()
  </script>
</div>

The final piece of code (at the top of the page) combines everything above to produce the Hasse diagram of a set. The function `Partition_Poset` first generates the set of partitions of an $N$ element set, then converts it to a poset. The function `p_label` relabels the partitions so that they look prettier. I've also tweaked some options in the `show()` method to make things look nicer.

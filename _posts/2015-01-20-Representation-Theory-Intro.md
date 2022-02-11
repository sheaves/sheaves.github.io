---
layout: post
title: Representation Theory in Sage - Basics
tag: 
- Representation Theory
---

This is the first of a series of posts about working with group representations in Sage.

<!--more-->

## Basic Definitions

Given a group $G$, a linear representation of $G$ is a group homomorphism $\rho: G \to \mathrm{GL}(V)$. 

For our purposes, we will assume that $G$ is a finite group and $V$ is an $n$-dimensional vector space over $\mathbb{C}$. Then $\mathrm{GL}(V)$ is isomorphic to the invertible $n \times n$ matrices over $\mathbb{C}$, which we will denote $\mathrm{GL}_n \mathbb{C}$. 

So a representation is just a function that takes group elements and returns invertible matrices, such that

$$
\rho(g h) = \rho(g) \rho(h) \,\, \forall g,h \in G,
$$

where we have matrix multiplication on the right-hand side.

Various authors refer to the map $\rho$, the vector space $V$, or the tuple $(V,\rho)$ as a representation; this shouldn't cause any confusion, as it's usually clear from context whether we are referring to a map or a vector space. When I need to be extra precise, I'll use $(V,\rho)$.

## Some simple examples

### Trivial representation
The simplest representation is just the trivial representation that sends every element of $G$ to the identity matrix (of some fixed dimension $n$). Let's do this for the symmetric group $S_3$:

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
G = SymmetricGroup(3)
n = 3

def triv(g):
    return matrix.identity(n)

g = G.an_element()

show(triv(g))
  </script>
</div>

We can verify that this is indeed a group homomorphism (warning: There are 6 elements in $S_3$, which means we have to check $6^2 = 36$ pairs!):

<div class="linked">
  <script type="text/x-sage">
for g in G:
    for h in G:
        print(triv(g*h) == triv(g)*triv(h))
  </script>
</div>

### Permutation representation
This isn't very interesting. However, we also know that $S_3$ is the group of permutations of the 3-element set {$1,2,3$}. We can associate to each permutation a [permutation matrix](http://mathworld.wolfram.com/PermutationMatrix.html){:target="_blank"}. Sage already has this implemented for us, via the method `matrix()` for a group element `g`:

<div class="linked">
  <script type="text/x-sage">
def perm(g):
    return g.matrix()

g = G.an_element()

show(perm(g))
  </script>
</div>

*Qn: From the permutation matrix, can you tell which permutation $g$ corresponds to?*

We can again verify that this is indeed a representation. Let's not print out all the output; instead, we'll only print something if it is *not* a representation. If nothing pops up, then we're fine:

<div class="linked">
  <script type="text/x-sage">
for g in G:
    for h in G:
        if triv(g*h) != triv(g)*triv(h):
            print("This is not a representation!")
  </script>
</div>

### Defining a representation from generators
We could define permutation representations so easily only because Sage has them built in. But what if we had some other representation that we'd like to work with in Sage? Take the [dihedral group](http://en.wikipedia.org/wiki/Dihedral_group){:target="_blank"} $D_4$. Wikipedia tells us that this group has [a certain matrix representation](http://en.wikipedia.org/wiki/Dihedral_group#Matrix_representation){:target="_blank"}. How can we recreate this in Sage?

We could hard-code the relevant matrices in our function definition. However, typing all these matrices can be time-consuming, especially if the group is large.

But remember that representations are group homomorphisms. If we've defined $\rho(g)$ and $\rho(h)$, then we can get $\rho(gh)$ simply by multiplying the matrices $\rho(g)$ and $\rho(h)$! If we have a [set of generators](http://en.wikipedia.org/wiki/Generating_set_of_a_group){:target="_blank"} of a group, then we only need to define $\rho$ on these generators. Let's do that for the generators of $D_4$:

<div class="linked">
  <script type="text/x-sage">
D4 = DihedralGroup(4)

D4.gens()
  </script>
</div>

We see that $D_4$ has a generating set of 2 elements (note: the method `gens()` need not return a *minimal* generating set, but in this case, we do get a minimal generating set). Let's call these $r$ and $s$. We know that elements of $D_4$ can be written $r^is^j$, where $i = 0,1,2,3$ and $j = 0,1$. We first run through all such pairs $(i,j)$ to create a [dictionary](https://docs.python.org/2/tutorial/datastructures.html#dictionaries){:target="_blank"} that tells us which group elements are given by which $(i,j)$:

<div class="linked">
  <script type="text/x-sage">
r,s = D4.gens()  
D4_dict = {}

# Populate dictionary
for i in range(4):
    for j in range(2):
        D4_dict[r^i * s^j] = (i,j)

# Check!
for g in D4:
    show(D4_dict[g])
  </script>
</div>

Now for $g = r^i s^j \in D_4$, we can define $\rho(g) = \rho(r)^i \rho(s)^j$ and we will get a representation of $D_4$. We need only choose the matrices we want for $\rho(r)$ and $\rho(s)$.

$r$ and $s$ correspond to $R_1$ and $S_0$, resp., in the [Wikipedia example](http://en.wikipedia.org/wiki/Dihedral_group#Matrix_representation){:target="_blank"}, so let's use their matrix representations to generate our representation:

<div class="linked">
  <script type="text/x-sage">
def wiki_rep(g):
    i,j = D4_dict[g]
    return matrix([[0,-1],[1,0]])^i * matrix([[1,0],[0,-1]])^j

# Check!
for g in D4:
    show(wiki_rep(g))
  </script>
</div>

One can verify that this does indeed give the same matrices as the Wikipedia example, albeit in a different order.

## We can do better!
All the representations we've defined so far aren't very satisfying! For the last example, we required the special property that all elements in $D_4$ have the form $r^i s^j$. In general, it isn't always easy to express a given group element in terms of the group's generators (this is known as the [word problem](http://en.wikipedia.org/wiki/Word_problem_for_groups){:target="_blank"}).

We've also been constructing representations in a rather ad-hoc manner. Is there a more general way to construct representations? And how many are representations are there?

In the [next post]({% post_url 2015-01-24-Representation-Theory-Sums-Products%}), I'll run through two simple ways of combining existing representations to get new ones: the direct sum and the tensor product. I'll also define *irreducible* representations, and state some results that will shed some light on the above questions.

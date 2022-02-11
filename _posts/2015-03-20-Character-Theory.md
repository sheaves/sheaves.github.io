---
layout: post
title: Character Theory Basics
tag: 
- Representation Theory
---

This post illustrates some of SageMath's character theory functionality, as well as some basic results about characters of finite groups. 

<!--more-->

## Basic Definitions and Properties

Given a representation $(V,\rho)$ of a group $G$, its [**character**](http://en.wikipedia.org/wiki/Character_theory){:target="_blank"} is a map $ \chi: G \to \mathbb{C}$ that returns the [trace](http://en.wikipedia.org/wiki/Trace_(linear_algebra)){:target="_blank"} of the matrices given by $\rho$:

$$
\chi(g) = \text{trace}(\rho(g)).
$$

A character $\chi$ is **irreducible** if the corresponding $(V,\rho)$ is [irreducible]({% post_url 2015-01-26-Representation-Theory-Irreducibility-Indecomposability%}){:target="_blank"}.

Despite the simplicity of the definition, the (irreducible) characters of a group contain a surprising amount of information about the group. Some [big theorems](http://en.wikipedia.org/wiki/Character_theory#Applications){:target="_blank"} in group theory depend heavily on character theory.

Let's calculate the character of the permutation representation of $D_4$. For each $g \in G$, we'll display the pairs:

$$[\rho(g),\chi(g)]$$

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
# Define group and its permutation representation
G = DihedralGroup(4)

def rho(g):
    return g.matrix()

# Define a function that returns the character of a representation
def character(rho):
    def chi(g):
        return rho(g).trace()
    return chi

# Compute the character
chi = character(rho)

for g in G:
    show([rho(g),chi(g)])
  </script>
</div>

Many of the following properties of characters can be deduced from properties of the trace:

1. The **dimension** of a character is the dimension of $V$ in $(V,\rho)$. Since $\rho(\text{Id})$ is always the identity matrix, the dimension of $\chi$ is $\chi(\text{Id})$.
1. Because the trace is [invariant under similarity transformations](http://en.wikipedia.org/wiki/Similarity_invariance){:target="_blank"}, $\chi(hgh^{-1}) = \chi(g)$ for all $g,h \in G$. So characters are constant on conjugacy classes, and are thus [**class functions**](http://en.wikipedia.org/wiki/Class_function){:target="_blank"}.
1. Let $\chi_V$ denote the character of $(V,\rho)$. Recalling the definitions of [direct sums and tensor products]({% post_url 2015-01-24-Representation-Theory-Sums-Products%}){:target="_blank"}, we see that

$$
\begin{align*}
  \chi_{V_1 \oplus V_2} &= \chi_{V_1} + \chi_{V_2} \\
  \chi_{V_1 \otimes V_2} &= \chi_{V_1} \times \chi_{V_2}
\end{align*}
$$

## The Character Table

Let's ignore the representation $\rho$ for now, and just look at the character $\chi$:

<div class="linked">
  <script type="text/x-sage">
print("chi  g")
table([[chi(g),g] for g in G])
  </script>
</div>

This is succinct, but we can make it even shorter. From point 2 above, $\chi$ is constant on conjugacy classes of $G$, so we don't lose any information by just looking at the values of $\chi$ on each conjugacy class:

<div class="linked">
  <script type="text/x-sage">
print("chi  conjugacy class")
table([[chi(C[0]),C.list()] for C in G.conjugacy_classes()])
  </script>
</div>

Even shorter, let's just display the values of $\chi$:

<div class="linked">
  <script type="text/x-sage">
[chi(g) for g in G.conjugacy_classes_representatives()]
  </script>
</div>

This single row of numbers represents the character of *one* representation of $G$. If we knew all the irreducible representations of $G$ and their corresponding characters, we could form a table with one row for each character. This is called the [**character table**](http://en.wikipedia.org/wiki/Character_table){:target="_blank"} of $G$.

Remember how we had to define our representations by hand, one by one? We don't have to do that for characters, because  SageMath has the [character tables of small groups built-in](http://www.sagemath.org/doc/constructions/rep_theory.html){:target="_blank"}:

<div class="linked">
  <script type="text/x-sage">
char_table = G.character_table()
char_table
  </script>
</div>

This just goes to show how important the character of a group is. We can also access individual characters as a functions. Let's say we want the last one:

<div class="linked">
  <script type="text/x-sage">
c = G.character(char_table[4])

print("c(g) for each g in G:")
print([c(g) for g in G])

print("c(g) for each conjugacy class:")
print([c(g) for g in G.conjugacy_classes_representatives()])
  </script>
</div>

Notice that the character we were playing with, $[4,2,0,0,0]$, is not in the table. This is because its representation $\rho$  is not irreducible. At the end of the post on [decomposing representations]({% post_url 2015-02-02-Representation-Theory-Decomposing-Representations%}){:target="_blank"}, we saw that $\rho$ splits into two $1$-dimensional irreducible representations and one $2$-dimensional one. It's not hard to see that the character of $\rho$ is the sum of rows 1,4 and 5 in our character table:

<div class="linked">
  <script type="text/x-sage">
c1 = G.character(char_table[0])
c4 = G.character(char_table[3])
c5 = G.character(char_table[4])

c = c1 + c4 + c5

print("c1 + c4 + c5:")
print([c(g) for g in G.conjugacy_classes_representatives()])

print("chi:")
print([chi(g) for g in G.conjugacy_classes_representatives()])
  </script>
</div>

Just as we could decompose every representation of $G$ into a sum of irreducible representations, we can express any character as a sum of irreducible characters. 

The next post discusses how to do this easily, by making use of the [Schur orthogonality relations](http://en.wikipedia.org/wiki/Schur_orthogonality_relations). These are really cool relations among the rows and columns of the character table. Apart from decomposing representations into irreducibles, we'll also be able to prove that the character table is always square!

**Edit:** The promised "next post" about these topics never happened. Maybe sometime in the far future, I might come back to these topics, but no promises for now!









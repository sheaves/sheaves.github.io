---
layout: post
title: Decomposing Representations
tag: 
- Representation Theory
---

In this post, we'll implement an algorithm for decomposing representations that [Dixon published in 1970](http://www.ams.org/journals/mcom/1970-24-111/S0025-5718-1970-0280611-6/S0025-5718-1970-0280611-6.pdf){:target="_blank"}.

<!--more-->

As a motivating example, I'll use the permutation matrix representation of $D_4$ that we saw in an [earlier post]({% post_url 2015-01-20-Representation-Theory-Intro%}){:target="_blank"}. To make the code more generally applicable, let's call the group $G$ and the representation $\rho$:

*(The Sage cells in this post are linked, so things may not work if you don't execute them in order.)*

<div class="linked">
  <script type="text/x-sage">
G = DihedralGroup(4)

# Defining the permutation representation
def rho(g):
    return g.matrix()

g = G.an_element()
show(rho(g))
  </script>
</div>

We'll see that this is decomposable, and find out what its irreducible components are.

### Unitary representations

A short remark before we begin: The algorithm assumes that $\rho$ is a [unitary representation](http://en.wikipedia.org/wiki/Unitary_representation){:target="_blank"}

i.e. for all $g \in G$,

$$ \rho(g)^* \rho(g) = \rho(g) \rho(g)^* = I,$$

where $A*$ is the [conjugate transpose](http://en.wikipedia.org/wiki/Conjugate_transpose){:target="_blank"} of a matrix $A$. 
For $G$ a finite group, all representations can be made unitary under an appropriate change of basis, so we need not be too concerned about this. In any case, permutation representations are always unitary, so we can proceed with our example.

## Finding non-scalar, commuting matrices

At the end of the [previous post]({% post_url 2015-01-26-Representation-Theory-Irreducibility-Indecomposability%}){:target="_blank"} we saw that in order to decompose a representation $(V,\rho)$, it is enough to find a non-scalar matrix $T$ that commutes with $\rho(g)$ for every $g \in G$.  This first step finds a [Hermitian](http://en.wikipedia.org/wiki/Hermitian_matrix){:target="_blank"} non-scalar $H$ that commutes with $\rho(G)$ (if there is one to be found).

Let $E_{rs}$ denote the $n \times n$ matrix with a $1$ in the $(r,s)$th entry and zeros everywhere else. Here $n$ is the dimension of $V$ in the representation $(V,\rho)$. Define

$$
H_{rs} = \begin{cases}
E_{rr} &\text{if } r = s \\
E_{rs} + E_{sr} &\text{if } r > s \\
i(E_{rs} - E_{sr}) &\text{if } r < s,
\end{cases}
$$

then the set of matrices $H_{rs}$ forms a Hermitian basis for the $n \times n$ matrices over $\mathbb{C}$.

Now for each $r,s$, compute the sum

$$
H = \frac{1}{|G|} \sum_{g \in G} \,\, \rho(g)^* \, H_{rs} \, \rho(g).
$$

Observe that $H$ has the following properties:

- it is hermitian
- it commutes with $\rho(g)$ for all $g \in G$

If $\rho$ is irreducible, then $H$ is a scalar matrix for all $r,s$. Otherwise, it turns out that there **will** be some $r,s$ such that $H$ is non-scalar (this is due to the fact that the $H_{rs}$ matrices form a basis of the $n \times n$ matrices$).

Let's test this algorithm on our permutation representation of $D_4$:

<div class="linked">
  <script type="text/x-sage">
def is_irreducible(rho,G, n= None):
  """
  If rho is irreducible, returns (True, I)  where I is the n-by-n identity matrix, n = dimension of rho.
  Otherwise, returns (False, H) where H is a non-scalar matrix that commutes with rho(G).
  """
  # Compute the dimension of the representation
  if n is None:
      n = rho(G.identity()).dimensions()[0]
  
  # Run through all r,s = 1,2,...,n
  for r in range(n):
      for s in range(n):
          # Define H_rs
          H_rs = matrix.zero(QQbar,n)
          if r == s:
              H_rs[r,s] = 1
          elif r > s:
              H_rs[r,s] = 1
              H_rs[s,r] = 1
          else: # r < s
              H_rs[r,s] = I
              H_rs[s,r] = -I
          
          # Compute H
          H = sum([rho(g).conjugate_transpose()*H_rs*rho(g) for g in G])/G.cardinality()
          
          # Check if H is scalar
          if H[0,0]*matrix.identity(n) != H:
              return False,H
  
  # If all H are scalar
  return True, matrix.identity(n)

is_irred,H = is_irreducible(rho,G) 

show(is_irred)
show(H)
  </script>
</div>

We get a non-scalar $H$! So the permutation representation of $D_4$ is reducible!

## Using $H$ to decompose $\rho$

Our next step is to use the eigenspaces of $H$ to decompose $\rho$. At the end of the [previous post]({% post_url 2015-01-26-Representation-Theory-Irreducibility-Indecomposability%}){:target="_blank"}, we saw that $\rho(g)$ preserves the eigenspaces of $H$, so we need only find the eigenspaces of $H$ to decompose $\rho$. 

Since $H$ is hermitian, it is [diagonalizable](http://en.wikipedia.org/wiki/Diagonalizable_matrix){:target="_blank"}, so its eigenvectors form a basis of $V$. In fact, the eigenbasis can be chosen to be orthonormal.

We can find this basis by computing the [Jordan decomposition](http://en.wikipedia.org/wiki/Jordan_normal_form){:target="_blank"} of $H$, and then orthonormalizing it:

<div class="linked">
  <script type="text/x-sage">
# Compute J,P such that H = PJP^(-1)
J,P = H.jordan_form(QQbar,transformation=True)
P = P.transpose().gram_schmidt(orthonormal=True)[0].transpose()

show(P)
  </script>
</div>

It's important that we orthonormalize $P$ so that $P$ becomes unitary. This will ensure that the subrepresentations remain unitary.

Observe that $P^{-1} \rho(g) P$ has the same block-diagonal form for each $g \in G$:

<div class="linked">
  <script type="text/x-sage">
# Compute block subdivisions (just for aesthetics)
edges = []
for g in G:
    edges += (P.conjugate_transpose()*rho(g)*P).nonzero_positions()
graph = Graph(edges, multiedges = False, loops = True)
subrep_indices = graph.connected_components()
subdivisions = graph.vertices()[1:]
for l in subrep_indices:
    for i in l[1:]:
        subdivisions.remove(i)
      
# Display rho in block-diagonal form
for g in G:
    M = P.inverse()*rho(g)*P
    M.subdivide(subdivisions, subdivisions)
    show(M)
  </script>
</div>

We have thus decomposed $\rho$ into two 1-dimensional representations and one 2-dimensional one! 

## Decomposing into irreducibles

Finally, to get a decomposition into irreducibles,  we can apply the algorithm recursively on each of the subrepresentations to see if they further decompose. 

Here's a stand-alone script that decomposes a representation into its irreducible components:

<div class="sage">
  <script type="text/x-sage">
# Define group and representation here
G = DihedralGroup(4)
def rho(g):
    return g.matrix()
    
# Algorithms
import numpy as np

def is_irreducible(rho,G, n= None):
  """
  If rho is irreducible, returns (True, I)  where I is the n-by-n identity matrix, n = dimension of rho.
  Otherwise, returns (False, H) where H is a non-scalar matrix that commutes with rho(G).
  """
  # Compute the dimension of the representation
  if n is None:
      n = rho(G.identity()).dimensions()[0]
  
  # Run through all r,s = 1,2,...,n
  for r in range(n):
      for s in range(n):
          # Define H_rs
          H_rs = matrix.zero(QQbar,n)
          if r == s:
              H_rs[r,s] = 1
          elif r > s:
              H_rs[r,s] = 1
              H_rs[s,r] = 1
          else: # r < s
              H_rs[r,s] = I
              H_rs[s,r] = -I
          
          # Compute H
          H = sum([rho(g).conjugate_transpose()*H_rs*rho(g) for g in G])/G.cardinality()
          
          # Check if H is scalar
          if H[0,0]*matrix.identity(n) != H:
              return False,H
  
  # If all H are scalar
  return True, matrix.identity(n)

def decompose(rho,G,H):
    """
    Uses the eigenspaces of H to decompose G into subrepresentations.
    Returns a change of basis matrix P and the indices of the block-decomposition of rho in this basis.
    """
    
    # Compute J,P such that H = PJP^(-1)
    J,P = H.jordan_form(QQbar,transformation=True)
    P = P.transpose().gram_schmidt(orthonormal=True)[0].transpose()

    # Compute block subdivisions
    edges = []
    for g in G:
        edges += (P.conjugate_transpose()*rho(g)*P).nonzero_positions()
    graph = Graph(edges, multiedges = False, loops = True)
    subrep_indices = sorted(graph.connected_components(), key=lambda x: x[0])    
    
    return P,subrep_indices  

def irr_decompose(rho,G,index = None):
    """
    Decomposes rho into irreducible representations of G.
    Returns a change of basis matrix P and the indices of the block-decomposition of rho in this basis.
    """
    n = rho(G.identity()).dimensions()[0]
    if index is None:
        index = range(n)
        
    # Test for irreducibility
    is_irred, H = is_irreducible(rho,G,n)
    
    if is_irred:
        subrep_indices = list(np.array(index)[range(n)])
        return H, [subrep_indices]
    else:
        P, subrep_indices = decompose(rho,G,H)
        print([list(np.array(index)[subrep_index]) for subrep_index in subrep_indices])

        new_subrep_indices = []
        new_P_list = []
        
        for subrep_index in subrep_indices:
            
            def subrep(g):
                return (P.inverse()*rho(g)*P)[subrep_index,subrep_index]
            new_P, new_indices = irr_decompose(subrep,G, list(np.array(index)[subrep_index]))
            
            new_subrep_indices += new_indices
            new_P_list += [new_P]
        
        return P*block_diagonal_matrix(new_P_list), new_subrep_indices

def show_irreps(rho,G,P,irrep_indices):
    subdivisions = [i for subrep_index in irrep_indices for i in subrep_index][1:]
    for subrep in irrep_indices:
        for i in subrep[1:]:
            subdivisions.remove(i)

    # Display rho in block-diagonal form
    for g in G:
        M = P.inverse()*rho(g)*P
        M.subdivide(subdivisions, subdivisions)
        show(M)

# Execute!
P,irrep_indices = irr_decompose(rho,G)
show_irreps(rho,G,P,irrep_indices)    
  </script>
</div>

## Getting all irreducible representations

Now we know how to test for irreducibility and decompose reducible representations. But we still don't know how many irreducible representations a group has. 

It turns out that finite groups have finitely many irreducible representations! In the [next post]({% post_url 2015-02-15-Group-Ring-Regular-Representation%}), we'll construct a representation for any finite group $G$ that contains *all* the irreducible representations of $G$.








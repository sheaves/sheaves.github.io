[Distributive Laws](https://golem.ph.utexas.edu/category/2017/02/distributive_laws.html)

The Kan Extension Seminar II continues and this week, we discuss Jon Beck's ``Distributive Laws'', which was published in 1969 in the proceedings of the [Seminar on Triples and Categorical Homology Theory, LNM vol 80](http://www.tac.mta.ca/tac/reprints/articles/18/tr18abs.html). In [the previous Kan II post](https://golem.ph.utexas.edu/category/2017/02/the_category_theoretic_underst.html), Evangelia described the relationship between Lawvere theories and finitary monads, along with two ways of combining them (the sum and tensor) that are very natural for Lawvere theories but less so for monads. Distributive laws give us a way of *composing* monads to get another monad, and are more natural from the monad point of view.

Beck's paper starts by defining and characterizing distributive laws. He then describes the category of algebras of the composite monad, and shows how it fits into a `distributive square of adjunctions' containing the categories of algebras of the original monads. Finally, he ends off with a series of examples.

Before we dive into the paper, I would like to thank Emily Riehl, Alexander Campbell and Brendan Fong for allowing me to be a part of this seminar, and the other participants for their wonderful virtual company. I would also like to thank my advisor [James Zhang](https://www.math.washington.edu/~zhang/) and his group for their insightful and encouraging comments as I was preparing for this seminar.

---

First, some differences between this post and Beck's paper: 

* I'll use a different convention than Beck for writing the composition of two functors. Given $F:\mathbf{X} \to \mathbf{Y}$ and $G: \mathbf{Y} \to \mathbf{Z}$, their composite will be denoted $GF$ in this post ($FG$ in Beck's paper). 

* I'll use the terms `monad' and `[monadic](https://ncatlab.org/nlab/show/monadic+functor)' instead of `triple' and `tripleable'.

* I'll rely quite a bit on string diagrams instead of commutative diagrams. These are to be read from *right to left* and *top to bottom*. You can learn about string diagrams through [this video](https://www.youtube.com/view_play_list?p=50ABC4792BD0A086) or [this paper](https://arxiv.org/pdf/1401.7220.pdf) (warning: they read string diagrams in different directions!).

* All constructions involving the category of $S$-algebras, $\cX^S$, will be done in an `object-free' manner involving only the universal property of $\cX^S$.

The last two points have the advantage of making the resulting theory applicable to $2$-categories other than $\mathbf{Cat}$, by replacing `categories/ functors/natural transformations' with `0/1/2-cells'.

### Motivating examples

**Example 1**: Let $S$ be the free monoid monad and $T$ be the free abelian group monad the over $\mathbf{Set}$. Then the elementary fact that `multiplication distributes over addition' means we have a map $STX \to TSX$ sending $(a+b)(c+d)$, say, to $ac+ad+bc+bd$. Further, the composition of $T$ with $S$ is the free ring monad, $TS$.

**Example 2**: Let $A$ and $B$ be monoids in a *braided* monoidal category $(\mathcal{V}, \otimes, 1)$. Then $A \otimes B$ is also a monoid, with multiplication:
$$
A \otimes B \otimes A \otimes B \xrightarrow{A \otimes tw_{BA} \otimes B} A \otimes A \otimes B \otimes B \xrightarrow{\mu_A \otimes \mu_B} A \otimes B,
$$
where $tw_{BA}: B \otimes A \to A \otimes B$ is provided by the braiding in $\mathcal{V}$.

In example 1, the monoidal category $\left(\mathbf{End}(\mathbf{Set}), \circ, \text{Id}\right)$ is not braided -- this is why we need distributive laws!

### Distributive laws, composite and lifted monads

Let $(S,\eta^S, \mu^S)$ and $(T,\eta^T,\mu^T)$ be monads on a category $\cX$. I'll use *S*carlet and *T*eal strings to denote $S$ and $T$, resp. 
A \emph{distributive law of $S$ over $T$} is a natural transformation $\ell:ST \Rightarrow TS$

> <img src="http://sheaves.github.io/distributive/dist_def.png" alt="distributive law" width="150"/>

satisfying the following equalities:

> <img src="http://sheaves.github.io/distributive/dist_axioms.png" alt="axioms for a distributive law" width="600"/>

A distributive law looks somewhat like a braiding in a braided monoidal category. In fact, it is a *local pre-braiding*: `local' in the sense of being defined only for $S$ over $T$, and `pre' because it is not necessarily invertible. 

As the above examples suggest, a distributive law allows us to define a multiplication $m:TSTS \Rightarrow TS$:

> <img src="http://sheaves.github.io/distributive/comp_mult.png" alt="multiplication for composite monad" width="150"/>

It is easy to check visually that this makes $TS$ a monad, with unit $\eta^T \eta^S$. For instance, the proof that $m$ is associative looks like this:

> <img src="http://sheaves.github.io/distributive/comp_assoc.png" alt="associativity for composite monad" width="600"/>

Not only is $TS$ a monad, we also have monad maps $T \eta^S: T \Rightarrow TS$ and $\eta^T S: S \Rightarrow TS$:

> <img src="http://sheaves.github.io/distributive/comp_maps.png" alt="monad maps to composite monad" width="450"/>

Asserting that $T \eta^S$ is a monad morphism is the same as asserting these two equalities:

> <img src="http://sheaves.github.io/distributive/comp_maps_check.png" alt="check monad maps" width="600"/>

Similar diagrams hold for $\eta^T S$. Finally, the multiplication $m$ also satisfies a *middle unitary law*:

> <img src="http://sheaves.github.io/distributive/comp_middle_unit.png" alt="middle unitary law" width="300"/>

To get back the distributive law, we can simply plug the appropriate units at both ends of $m$:

> <img src="http://sheaves.github.io/distributive/comp_inverse.png" alt="recovering the distributive law" width="150"/>

This last procedure (plugging units at the ends) can be applied to any  $m':TSTS \Rightarrow TS$. It turns out that if $m'$ happens to satisfy all the previous properties as well, then we also get a distributive law. Further, the (distributive law $\to$ multiplication) and (multiplication $\to$ distributive law) constructions are mutually inverse:

> **Theorem** &nbsp; *The following are equivalent: (1) Distributive laws $\ell:ST \Rightarrow TS$; (2) multiplications $m:TSTS \Rightarrow TS$ such that $(TS, \eta^T \eta^S, m)$ is a monad, $T\eta^S$ and $S \eta^T$ are monad maps, and the middle unitary law holds.*

In addition to making $TS$ a monad, distributive laws also let us lift $T$ to the category of $S$-algebras, $\mathbf{X}^S$. Before defining what we mean by `lift', let's recall the universal property of $\mathbf{X}^S$: Let $\mathbf{Y}$ be another category; then there is an equivalence of categories between $\mathbf{Funct}(\mathbf{Y}, \mathbf{X}^S)$, the category of functors $\tilde{G}: \mathbf{Y} \to \mathbf{X}^$ and natural transformations between them, and $S-\mathbf{Alg}(\mathbf{Y})$, the category of functors $G: \mathbf{Y} \to \mathbf{X}$ equipped with an $S$-action $\sigma: SG \Rightarrow G$ and natural transformations that commute with the $S$-action. The equivalence sends $\tilde{G}$ to $G = U^S \tilde{G}$ and $\tilde{\phi}: \tilde{G} \Rightarrow \tilde{G}'$ to $\phi = U^S \tilde{\phi}: G \Rightarrow G'$:

> <img src="http://sheaves.github.io/distributive/lift_univ.png" alt="universal property of S-algebras" width="300"/>

Further, $\tilde{G}$ and $\tilde{\phi}$ are called *lifts* of $G$ and $\phi$, resp. A *monad lift of $T$ to $\mathbf{X}^S* is a monad $(\tilde{T}, \tilde{\eta}^T,\tilde{\mu}^T)$ on $\mathbf{X}^S$ such that $\tilde{T}, \tilde{\eta}^T, \tilde{\mu}^T$ are lifts of $TU^S, \eta^T U^S,\mu^T U^S$, resp. Thus, if $\tilde{T}$ is a monad lift of $T$, 

$$
U^S \tilde{T} = T U^S, \qquad U^S \tilde{\eta}^T = \eta^T U^S, \qquad U^S \tilde{\mu}^T = \mu^T U^S.
$$

By the universal property of $\mathbf{X}^S$, to get a monad lift of $T$, it suffices to produce an $S$-action on $TU^S$ that is compatible with $\eta^T U^S$ and $\mu^T U^S$. We may simply combine the distributive law with the canonical $S$-action on $U^S$ to obtain the desired action on $TU^S$:

> <img src="http://sheaves.github.io/distributive/lift_action.png" alt="S-action on TU^S" width="200"/>

Conversely, suppose we have a monad lift $\tilde{T}$ of $T$. Then the equality $U^S \tilde{T} = T U^S$ can expressed by saying that we have an *invertible* natural transformation $\chi: U^S \tilde{T} \Rightarrow TU^S$. Using $\chi$ and the unit and counit of the adjunction $F^S \dashv U^S$ that gives rise to $S$, we obtain a distributive law of $S$ over $T$:

> <img src="http://sheaves.github.io/distributive/lift_dist.png" alt="getting a distributive law from a lift" width="300"/>

The essence of the proofs that these constructions are mutually inverse is contained in the following two equalities:

> <img src="http://sheaves.github.io/distributive/lift_inverse.png" alt="constructions are mutually inverse" width="450"/>

The first shows that the resulting distributive law in the (distributive law $\to$ lift $\tilde{T}$ $\to$ distributive law) construction is the same as the original distributive law we started with. The second shows that in the (lift $\tilde{T}$ $\to$ distributive law $\to$ another lift $\tilde{T}'$) construction, the $S-action$ on $U^S \tilde{T}'$ (LHS) is the same as the original $S$-action on $U^S \tilde{T}$ (RHS; note that both composites are equal to $TU^S$, by virtue of being lifts, so they can only differ in their $S$-actions). By the universal property of $\mathbf{X}^S$, this implies that $\tilde{T} = \tilde{T}'$. We thus have another characterization of distributive laws:

> **Theorem** &nbsp; *The following are equivalent: (1) Distributive laws $\ell:ST \Rightarrow TS$; (3) monad lifts of $T$ to $\mathbf{X}^S$.*

In fact, the converse construction did not rely on the universal property of $\mathbf{X}^S$, and hence applies to any adjunction giving rise to $S$ (with a suitable definition of a monad lift of $T$ in this situation). In particular, it applies to the Kleisli adjunction $F_S \dashv U_S$, $F_S: \mathbf{X} \to \mathbf{X}_S$. Since $\mathbf{X}_S$ is equivalent to the subcategory of *free* $S$-algebras (in the classical sense) in $\mathbf{X}^S$, this means that to get a distributive law of $S$ over $T$, it suffices to lift $T$ to a monad over just the free $S$-algebras! (Thanks to [Jonathan Beardsley](http://www.math.washington.edu/~jbeards1/) for pointing this out!) The resulting distributive law may be used to get another lift of $T$, but we should not expect this to be the same as the original lift unless the original lift was `monadic' to begin with, in the sense of being a lift to $\mathbf{X}^S$.

There are two further characterizations of distributive laws that are not mentioned in Beck's paper, but whose equivalences follow easily. Eugenia Cheng in [Distrbutive laws for Lawvere theories](https://arxiv.org/abs/1112.3076) states that distributive laws of $S$ over $T$ are also equivalent to *extensions* of $S$ to a monad $\tilde{S}$ on the Kleisli category of $T$, $\mathbf{X}_T$. This follows by duality from the previous statement, since $\mathbf{X}_T = (\mathbf{X}^{op})^T$ (i.e. read all the above diagrams from *left to right*, swap $T$ and $S$, and replace superscripts by subscripts). Finally, Ross Street's [Formal theory of monads](http://www.sciencedirect.com/science/article/pii/0022404972900199) (which was also covered in [a previous Kan Extension Seminar post](https://golem.ph.utexas.edu/category/2014/01/formal_theory_of_monads_follow.html)) says that distrbutive laws in a $2$-category $\mathbf{K}$ are precisely monads in $\mathbf{Mnd}(\mathbf{K})$. It is a fun and easy exercise to draw string diagrams for what the objects of $\mathbf{Mnd}(\mathbf{Mnd}(\mathbf{K}))$ look like; it becomes visually obvious that these are the same as distributive laws.

### Algebras for the composite monad

After characterizing distributive laws, Beck characterizes the algebras for the composite monad $TS$. Just as a morphism of rings $R \to R'$ induces a `restriction of scalars' functor $R'-\mathbf{Mod} \to R-\mathbf{Mod}$, the monad maps $T \eta^S: T \Rightarrow TS$ and $\eta^T S: S \Rightarrow TS$ induce functors $\hat{U}^{TS}: \mathbf{X}^{TS} \to \mathbf{X}^T$ and $\tilde{U}^{TS}: \mathbf{X}^{TS} \to \mathbf{X}^S$. 

Equivalently, we have $S$- and $T$-actions on $U^{TS}$, which we call $\sigma: SU^{TS} \Rightarrow U^{TS}$ and $\tau: T U^{TS} \Rightarrow U^{TS}$. Let $\varepsilon: TS\, U^{TS} \Rightarrow U^{TS}$ be the canonical $TS$-action on $U^{TS}$. The middle unitary law then implies that $\varepsilon = T\sigma \cdot \tau$:

> <img src="http://sheaves.github.io/distributive/alg_actions.png" alt="actions of T, S and TS" width="600"/>

Further, $\sigma$ `distributes over' $\tau$ in the following sense:

> <img src="http://sheaves.github.io/distributive/alg_dist.png" alt="S-action distributes over T-action" width="400"/>

The properties of these actions allow us to characterize $TS$-algebras:

> **Theorem** &nbsp; *The category of $TS$-algebras coincides with that of $\tilde{T}$:*

$$
	\mathbf{X}^{TS} \cong (\mathbf{X}^S)^{\tilde{T}}
$$

To prove this, Beck constructs $\Phi: (\mathbf{X}^{S})^{\tilde{T}} \to \mathbf{X}^{TS}$ and its inverse $\Phi^{-1}: \mathbf{X}^{TS} \to (\mathbf{X}^S)^{\tilde{T}} $. These constructions are best summarized in the following diagram of lifts:

> <img src="http://sheaves.github.io/distributive/alg_lifts.png" alt="required lifts" width="300"/>

On the left half of the diagram, we see that to get $\Phi^{-1}$, we must first produce a functor $\tilde{U}^{TS}: \mathbf{X}^{TS} \to \mathbf{X}^S$ with a $\tilde{T}$-action. We already have $\tilde{U}^{TS}$ as a lift of $U^{TS}$, given by the $S$-action on $U^{TS}$. But we saw above that we have a $T$-action on $U^{TS}$ which the $S$-action distributes over, and this is precisely what is required to get a lift of the $T$-action on $U^{TS}$ to a $\tilde{T}$-action on $\tilde{U}^{TS}$.

On the right half of the diagram, to get $\Phi$ we need to produce a functor $(\mathbf{X}^{S})^{\tilde{T}} \to \mathbf{X}$ with a $TS$-action. The obvious functor is $U^S U^{\tilde{T}}$, and we get an action by using the canonical actions of $S$ on $U^S$ and $\tilde{T}$ on $U^{\tilde{T}}$:

> <img src="http://sheaves.github.io/distributive/alg_TS.png" alt="required lifts" width="200"/>

All that's left to prove the theorem is to check that $\Phi$ and $\Phi^{-1}$ are inverses. In a similar fashion, we can prove the dual statement (again found in Cheng's paper but not Beck's):

> **Theorem** &nbsp; *The Kleisli category of $TS$ coincides with that of $\tilde{S}$:*

$$
	\mathbf{X}_{TS} \cong (\mathbf{X}_T)_{\tilde{S}}
$$

### Distributivity for adjoints

From now on, we identify $\mathbf{X}^{TS}$ with $(\mathbf{X}^S)^{\tilde{T}}$. Under this identification, it turns out that $\tilde{U}^{TS} \cong U^{\tilde{T}}$, and we obtain what Beck calls a *distributive adjoint situation* containing of 3 pairs of adjunctions:

> <img src="http://sheaves.github.io/distributive/adj_square.png" alt="required lifts" width="300"/>

For this to qualify as a distributive adjoint situation, we also require that both composites from $\mathbf{X}^{TS}$ to $\mathbf{X}$ are naturally isomorphic, and both composites from $\mathbf{X}^S$ \to $\mathbf{X}^T$ are naturally isomorphic. This can be expressed in this following diagram by requiring both blue circles to be mutually inverse, and both red circles to be mutually inverse:

> <img src="http://sheaves.github.io/distributive/adj_string.png" alt="required lifts" width="600"/>

This diagram is very similar to the diagram for getting a distributive law out of a lift $\tilde{T}$, and it is easy to believe that any such distributive adjoint situation (with 3 pairs of adjoints - not necessarily monadic - and the corresponding natural isomorphisms) leads to a distributive law.

Finally, suppose the `restriction of scalars' functor $\hat{U}^{TS}$ has an adjoint. This adjoint behaves like an `extension of scalars' functor, and Beck fittingly calls it $(\,) \otimes_S F^T$ at the start of his paper. I'll use $\hat{F}^{TS}$ instead, to  highlight its relationship with $\hat{U}^{TS}$.

In such a situation, we get an adjoint square consisting of 4 pairs of adjunctions. By drawing these 4 adjoints in the following manner, it becomes clear which natural transformations we require in order to get a distributive law: 

> <img src="http://sheaves.github.io/distributive/adj_cross.png" alt="distributive adjoints" width="600"/>

It turns out that given $u$, we can get $f$ as the `adjoint' of $u$, which is invertible if $u$ is. We may use $u$ or $f$, along with the units and counits of the relevant adjunctions, to construct $e$:

> <img src="http://sheaves.github.io/distributive/adj_e.png" alt="e from u or f" width="450"/>

But $e$ is in the `wrong direction', so we have to further require that $e$ is invertible, to get $e^{-1}$. We get $e'$ from $u^{-1}$ or $f^{-1}$ in a similar manner. Since $e'$ will turn out to be in the `right direction', we will not require it to be invertible. Finally, given any 4 pairs of adjoints that look like the above, along with natural transformations $u,f,e,e'$ satisfying the above properties, we will get a distributive law!

### What next?

Beck ends his paper with some examples, two of which I've already mentioned at the start of this post. Instead of repeating those examples, I'd like to end by pointing to some related works:

* Since we've been talking about Lawvere theories, we can ask what distributive laws look like for Lawvere theories. Cheng's [Distributive laws for Lawvere theories](https://arxiv.org/abs/1112.3076), which I've already referred to a few times, does exactly that. But first, she comes up with 4 settings in which to define Lawvere theories! She also has a very readable introduction to the correspondence between Lawvere theories and finitary monads. 

* As Beck mentions in his paper, we can similarly define distributive laws between *co*monads, as well as *mixed* distributive laws between a monad and a comonad. Just as we can define bimonoids/bialgebras, and thus Hopf monoids/algebras, in a braided monoidal category, such distributive laws allow us to define bimonads and Hopf monads. There are in fact two distinct notions of Hopf monads: the first is described in [this paper](https://arxiv.org/abs/math/0604180) by Alain Bruguières and Alexis Virelizier (with a [follow-up paper](https://arxiv.org/abs/1003.1920) coauthored with Steve Lack, and a [diagrammatic approach](https://arxiv.org/abs/0807.0658) with amazing *surface* diagrams by Simon Willerton); the second is [this paper](https://arxiv.org/abs/0710.1163) by Bachuki Mesablishvili and Robert Wisbauer. The difference between these two approaches is described in the Mesablishvili-Wisbauer paper, but both involve mixed distributive laws. Gabriella Böhm also recently gave a talk entitled [The Unifying Notion of Hopf Monad](http://www1.maths.leeds.ac.uk/~matzk/Modelling%20TPM%20Resources_files/Bohm_Leeds.pdf), in which she shows how the many generalizations of Hopf algebras are just instances of Hopf monads (in the first sense) in an appropriate monoidal bicategory!

* We also saw that distributive laws are monads in a category of monads. Instead of thinking of distributive laws as merely a means of composing monads, we can study distributive laws as objects in their own right, just as monoids in a category of monoids (a.k.a. abelian monoids) are studied in their own right! The story for monoids terminates at this step: monoids in abelian monoids are just abelian monoids. But for distributive laws, we can keep going! See Cheng's paper on [Iterated Distributive Laws](https://arxiv.org/abs/0710.1120), where she shows the connection between iterated distributive laws and $n$-categories. We also require a kind of Yang-Baxter equation between 3 monads:

> <img src="http://sheaves.github.io/distributive/yang_baxter.png" alt="Yang-Baxter condition" width="300"/>

* Finally, there seems to be a strange connection between distributive laws and factorization systems. Just search for `distributive law' and `factorization system' together, to find a list of papers, including a few by some of the authors above. I can't say more because I don't know much about factorization systems, but hopefully someone can say something illuminating about this in the comments!












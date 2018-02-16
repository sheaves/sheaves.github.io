---
layout: post
title: Are word embeddings fibered categories?
draft_tag: 
- Category theory
- Applications
---

Links [Kan Extension Seminar II](http://www.math.jhu.edu/~eriehl/kanII/){:target="_blank"}
Images ![](/distributive/comp_assoc.png "Associativity of the composite monad")

This post explores the question "What structure can we extract from a word embedding?" and posits that the answer is a fibered category, or a database!

<!--more-->

*Disclaimer: Everything I know about word embeddings come from a couple of blog posts mentioned below, so this article should not be taken too seriously. On the other hand, if this post leads to an actual paper, it would be nice if you mentioned that you first saw it here :D*


> You shall know a word by the company it keeps.
>
> - [John Firth](https://en.wikipedia.org/wiki/John_Rupert_Firth){:target="_blank"}

> You shall know an object (of a category) by the company it keeps.
>
> - [Corollary](https://ncatlab.org/nlab/show/Yoneda+lemma#corollary_ii_uniqueness_of_representing_objects){:target="_blank"} of the Yoneda Lemma

## Word embeddings

Methods: word2vec and GloVe

Features
- Clustering of closely related words
- Difference vectors encode relationship information
- Can add and subtract to form analogies!

## Fibered Categories and Databases

Discrete fibrations
The Grothendieck construction
Database schemas (ologs)

## Extracting a Fibered Category

Not-even-pseudo code

- Compute all differences
- Cluster them, and select the largest/tightest k clusters
- Form the base graph/category from this data
- Form a discrete fibration

Benefits
- Labelling arrows
- Compositionality, and overcoming "drifting"
- Sparsity of representation
- A structured database from unstructured text

## Problems

- Noise, need fine tuning
- Complexity of computing difference pairs
- Collision of disparate difference pairs
- Incomplete datasets: partial functions?
- Can't handle inclusions. More generally, objects that belong under multiple headings
- Can't identify direction of arrows

## Speculation

- Can we learn the category directly?
- Clustering data points gives objects (word2vec), difference vectors gives arrows (Glove). What about higher "simplices"?
- Given a category, can we compute what it is fibered over? Any measure for what constitutes a good representation?

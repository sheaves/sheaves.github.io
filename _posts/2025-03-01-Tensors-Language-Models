---
layout: post
title: Tensors and Language Models
draft_tag: 
- Tensors and Language Models
---
This is the first in a series of posts explaining and expanding on the ideas introduced in the following papers:

- Paying attention to facts: quantifying the knowledge capacity of attention layers  
  \[[arXiv:2502.05076](https://arxiv.org/abs/2502.05076){:target="_blank"}\]

- 'Generalization is hallucination' through the lens of tensor completions  
  \[[arXiv:2502.17305](https://arxiv.org/abs/2502.17305){:target="_blank"}\]

The overarching theme behind these papers is that tensors and tensor completions are a useful theoretical framework for thinking about language models, specifically generative models based on [transformers](https://en.wikipedia.org/wiki/Transformer_(deep_learning_architecture)){:target="_blank"}.  
As I've only started doing this in 2025, everything is still quite preliminary.
Nevertheless, this line of inquiry has already yielded some interesting findings. 
For example, the [Paying attention to the facts](https://arxiv.org/abs/2502.05076){:target="_blank"} suggests that we can increase model capacity without increasing the number of parameters by increasing the dimensions of the output-value weights at the expense of the key-query weights. 

In this series of blog posts, I plan to explain and expand on some of the ideas in these papers, in the hopes of attracting more research in this direction.
Starting from the next post, I will introduce tensors and their rank, relate them to language datasets and language models, talk about databases and what it means to generate `random databases', and highlight the rank-increasing effects of argmax and softmax.
But this post will be less technical, and will instead explain some of the motivations behind these papers.

### Why am I doing this?
By now, it *what* large language models (LLMs) can or cannot do, but less on *how* they do these tasks.

need for theoretical frameworks that move away from human analogies

Interpretability research is hard, and can sometimes feel like looking for a needle in a haystack, with the additional difficulty of not knowing what the needle looks like!
By approaching from theory on small examples, I hope to show what the needle looks like



### Why tensors?
One might ask, "Why do you feel the need to introduce yet another theoretical framework? What's wrong with existing frameworks? Do you secretly have a tensor-theoretic agenda? Who is paying you???"

No, I am not in the pocket of 'Big Tensor', and I have not previously done research on tensors or tensor completion (although I do love linear algebra).
My reason for introducing tensors is simple: I took a good, hard look at the situation, and saw tensors there.

This is not an instance of the proverb, ["if all you have is a hammer, everything looks like a nail"](https://en.wikipedia.org/wiki/Law_of_the_instrument){:target="_blank"}.
Indeed, when I first wrote these papers, I did not have tensors in mind at all. 
I was simply trying to find a way to think about databases of [RDF triples](https://en.wikipedia.org/wiki/Semantic_triple){:target="_blank"}.
I wanted a setting that allowed me to talk about an RDF triple being "in the span" of other triples in a precise linear-algebraic sense.
Since each RDF triple has 3 parts (subject, predicate and object), it seemed natural to use a 3-tensor rather than a matrix to encode this information.
From there, it was a natural extension to $n$-tensors for more general $n$-grams.

I do not think that tensors are *the* way to understand language models, merely *one* way. 
Each framework (be it embeddings, or neurons, or circuits, or...) sheds light on language models from a different angle, and all I hope to do is to introduce another angle to the story.
I think a lot can be gained by also trying to understand the interplay between different frameworks. 
For example, can we relate linear-algebraic properties of a tensor to geometric properties of embeddings that result from decomposing the tensor?

### What next?
I've talked a lot about tensors without really defining them. In the next post, I will 

### Getting involved
If this line of work interests you, please reach out in the comments below, or email me at ```liangze DOT wong AT gmail DOT com```. 
If time permits, I would be happy to collaborate on topics related to interpretability and theory of language models, even if it does not involve tensors.
If you are hiring, or know of anyone who might be interested in hiring, research scientists with my background, I am currently **open to work**!

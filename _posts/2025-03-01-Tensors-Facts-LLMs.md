---
layout: post
title: Tensors and Factual Recall in Language Models
tag: 
- Tensors and Language Models
---
This is the first in a series of posts explaining and expanding on the ideas introduced in the following papers:

- Paying attention to facts: quantifying the knowledge capacity of attention layers  
  \[[arXiv:2502.05076](https://arxiv.org/abs/2502.05076){:target="_blank"}\]

- 'Generalization is hallucination' through the lens of tensor completions  
  \[[arXiv:2502.17305](https://arxiv.org/abs/2502.17305){:target="_blank"}\]

The overarching theme behind these papers is that tensors and tensor completions are a useful theoretical framework for thinking about language models, specifically generative models based on [transformers](https://en.wikipedia.org/wiki/Transformer_(deep_learning_architecture)){:target="_blank"}.
As I've only started doing this in 2025, everything is still quite preliminary. In particular, the results of these papers only hold for very simple toy models and synthetic datasets.

Nevertheless, this line of inquiry has already yielded some interesting findings.
For example, the ['Paying attention to the facts' paper](https://arxiv.org/abs/2502.05076){:target="_blank"} shows that transformers can store facts in their attention heads and not just their MLP layers. It also shows that we can increase model capacity without increasing the number of parameters by increasing the dimensions of the output-value weights at the expense of the key-query weights. 
Meanwhile, the ['Generalization is hallucination' paper](https://arxiv.org/abs/2502.17305){:target="_blank"} identifies a common mechanism that shows how training data gives rise to certain types of generalizations and hallucinations.

In this series of blog posts, I plan to explain and expand on some of the ideas in these papers, in the hopes of encouraging more research in this direction.
In the next few posts, I will introduce tensors and their rank, relate them to language datasets and language models, talk about databases and ways of generating `random databases', and highlight the non-linear effects of argmax and softmax on rank.

But this post will be less technical. These papers may broadly be described as using *tensors* to understand *factual recall* in language models, and so in this post I hope to explain some of the motivations behind these papers by answering two questions, "Why facts?" and "Why tensors?"

### Why facts?
Factual recall in language models has some nice properties as a research topic. 
Firstly, facts are simple. 
At their core, facts can be represented as subject-predicate-object (or subject-relation-object) triples, such as ```(France, capital_is, Paris)```, although they may be represented in other ways in natural language.

Not only are facts easy to represent, it is usually the case that there is one right answer: given the pair ```(France, capital_is)```, you would expect a model to output ```Paris```. 
This turns factual recall into a supervised task, and removes the complication of having multiple acceptable outputs, which are common in many other next-token prediction tasks.
On a related note, databases of facts (be it in traditional tabular databases, or graph databases, or RDF triples) are readily available as supervised datasets, and are also easy to synthetically generate.

In addition to the nature of facts themselves, it seems to be generally accepted that LLMs can store facts, and that this is a desirable behaviour.
It is thus important to study factual recall, because a better understanding will enable us to better control this behaviour (to increase the storage capacity, or reduce factual hallucinations, for example).

I think it is in part because of these reasons that the question of how LLMs memorize facts has received a fair amount of attention, as these examples demonstrate:

- [Summing up the facts: Additive mechanisms behind factual recall in LLMs](https://arxiv.org/abs/2402.07321){:target="_blank"} 
- [Scaling laws for fact memorization of large language models](https://aclanthology.org/2024.findings-emnlp.658/){:target="_blank"} 
- [Understanding factual recall in transformers via associative memories](https://openreview.net/forum?id=hwSmPOAmhk){:target="_blank"}
- [Physics of language models: Part 3.1,
knowledge storage and extraction](https://arxiv.org/abs/2309.14316){:target="_blank"} and [Part 3.3,
knowledge capacity scaling laws](https://arxiv.org/abs/2404.05405){:target="_blank"}
- [Transformer Feed-Forward Layers Are Key-Value Memories](https://aclanthology.org/2021.emnlp-main.446.pdf){:target="_blank"}

Despite the simplicity of facts, however, and despite the research that has gone into it, we still do not have a full picture of how facts are stored in language models.
For example, papers about scaling laws of memorization capacity tend to quantify the size of a database of facts in terms of the number of facts contained. 
However, this ignores the structure of facts and patterns among facts that might make one database easier or harder to memorize than another database with the same number of facts.

My sense is that the number of facts in a database is just a proxy for another measure of database size that better reflects the underlying properties of the database.
While I have not uncovered this measure yet, I think my approach (in terms of tensors) has brought us a step closer to it.

Which leads us to the next question...

### Why tensors?
When I first started this line of investigation, I did not have tensors in mind at all.
I was simply trying to find a way to think about facts represented as [RDF triples](https://en.wikipedia.org/wiki/Semantic_triple){:target="_blank"}.
I wanted a setting that allowed me to talk about an RDF triple being "in the span" of other triples in a precise sense.
Since each RDF triple has 3 parts (subject, predicate and object), it seemed natural to use a 3-tensor rather than a matrix to encode this information.
From there, it was a natural extension to $n$-tensors for more general $n$-grams.

I do not think that tensors are *the* way to understand language models, merely *one* way.
Each framework (be it embeddings, or neurons, or dictionary learning, or ...) sheds light on language models from a different angle, and all I hope to do is to introduce another angle to the story.
I think a lot can be gained by also trying to understand the interplay between different frameworks.
For example, can we relate linear-algebraic properties of a tensor to geometric properties of embeddings that result from decomposing the tensor?
Can we relate linear-algebraic properties of tensors to neuron activations?

### Up next
I've talked a lot about tensors without really defining them. In the next post, I will define tensors and relate them to facts represented as RDF triples.

#### Getting involved
If this line of work interests you, please reach out in the comments below, or email me at [liangze.wong@gmail.com](mailto:liangze.wong@gmail.com).
I would be happy to collaborate on topics related to interpretability and theory of language models, even if it does not involve tensors or factual recall.
If you are hiring (or know of anyone who might be interested in hiring) research scientists with my background, I am currently open to work, and am open to re-location!

---
title: Possible Projects
---

Here are some possible types of projects that we can work on. All of them use graphs and eigenvectors in some way or other. 

There are multiple aspects to each class of projects:

- Representation as a graph/matrix
- Datasets
- Algorithms

For some projects, the representations and the datasets are already present, but we can try to improve the algorithms. For others, the representation and algorithms are well-known, and the novelty would come in collecting your own data (e.g. by carrying out a survey among your classmates, for example).

Each section below describes one class of projects (not a specific topic).  

I have also included some references that I felt were quite readable, but it's not compulsory to read them (some are quite long). If you do read them, don't feel like you need to understand everything. Just get a general idea of the kinds of questions people are interested in, and the methods they use to solve them.

# 1. Transport networks

In this class of projects, networks are used to model various transport-related networks. This could mean things like roads, bus networks, MRT networks, or even things like electrical networks, or wired/wireless networks.

In all these problems, we usually want to know about the connectivity of the network, and how things (cars, buses, electrical currents, data packets) flow through the network.

In some cases, the network representation is already known (e.g. roads, electrical networks). But in other cases, current network representations are not satisfactory (e.g. MRT networks, bus networks). 

### Possible projects
Possible projects could involve:
- finding new representations, or
- finding ways to improve an existing representation to make it more expressive 

For example, can we find a way to include commuter preferences like fewer transfers or fewer stops?.

We could also aim to answer questions such as: 

- How much will connectivity improve if we close up the circle line? 
- What's the best way to route 5 buses to connect 20 stations?
- Is it better to have a hub-and-spoke network or a network with concentric circles?

### References and datasets
Here's a nice intro the topic: [Linear Algebra in Geography: Eigenvectors of Networks](https://pdfs.semanticscholar.org/b261/1632c1a7687e613b1fcbb02209a5d9e59cf3.pdf).

In Singapore, LTA also has public transport network data available for download at the [LTA DataMall](https://www.mytransport.sg/content/mytransport/home/dataMall.html).

# 2. Community Detection

Community detection in networks is a huge area of research with many applications. In such projects, the network usually represents some kind of relationship between nodes, and could be an ordinary graph or a bipartite graph, for example:

- Friends network
- Movie-actor network
- Product-consumer network

See the [Stanford Large Network Dataset Collection](http://snap.stanford.edu/data/index.html) for an idea of the many different kinds of networks out there.

### Possible projects
In many of these settings, we usually want to identify *communities*, which are nodes that are similar to each other in one way or another. Further, we want to be able to do this just by looking at the graph. 

This can be used to answer questions like:
- How many types of movie-goers are there?
- How well-connected are different people in a network? Can we identify hubs that connect many people? (e.g. a celebrity account)

Community detection also has applications for things such as:
- Classification
- Recommendation systems

### References and datasets
For more on this, you can look at Chapter 4.4 of [this textbook](http://vmls-book.stanford.edu/vmls.pdf). (The rest of the textbook is also very good, so you might want to save it for your own reference).

Datasets: [Stanford Large Network Dataset Collection](http://snap.stanford.edu/data/index.html)

# 3. Ranking

Although Google is commonly known as a _search_ engine, what really allowed it to leap ahead of its competition is its _ranking_ system. Finding a search term in a webpage is easy, and early search engines like Yahoo, AltaVista, Lycos etc. were able to do all that even before Google existed. But the reason you've probably never heard of them is that they tended to serve up many low-quality search results that just happen to contain the search term but were not really relevant to the term.

When you run a Google search, you might end up with _hundreds of thousands_ of hits. And yet, Google is somehow able to put the most relevant webpage within the top 3 or top 5 hits. To do this, it needs to _rank_ the hits according to some notion of quality or relevance. How is it able to do this automatically, without actually understanding what's on the webpage?

The main idea is this: _a webpage's rank should depend on the ranks of the webpages that link to it_.

So the effect of many low-scoring webpages (like personal blogs) linking to a page might be less than the effect of a high-scoring webpage (like a reputable news site) linking to a page.

But this seems like a circular argument. How do we get started determining scores in the first place?

In fact, you've already seen something similar if you did Question 2 on Ranking Graphs in this week's homework. In that situation, we want to rank teams based on teams that they defeated. 

The main idea there is: _A team's rank should depend on the ranks of the teams that it defeats_.

So defeating many weak teams may not contribute as much to a team's rank as defeating one strong team.

### References
Here's a nice article on [Searching the web with eigenvectors](https://www.math.upenn.edu/~wilf/website/KendallWei.pdf), that also includes the example of ranking teams. (In fact, the example in Question 2 was taken from this paper). 

Another nice write-up on [How Google finds your needle in the web's haystack](http://www.ams.org/publicoutreach/feature-column/fcarc-pagerank).

### Projects
The problem with this topic is that it is hard to find something new to rank. Most situations where people are interested in rankings have already been ranked, so the challenge is to find interesting situations that have not been ranked.

Here are some possibilities:
- Instead of the whole web, look at a smaller network like a subset of Wikipedia, or some other fan-Wikis for other subjects
- If any of you have competitive hobbies or CCAs (like sports or board games), and are able to get match data for matches played among a group of people, it might be interesting to rank these players using linear algebra.
- Any network where each node has a score, which depends on the scores of its neighbours, which in turn depends on the scores of their neighbours, and so on. The 'score' need not represent a rank.


# 4. Dynamical systems

Dynamical systems are quantities that change over time, according to some pre-set rules. For example, the quantities could be the population of foxes and rabbits in an ecosystem, or the spread of a disease through a population.

You have already seen an example of a simple dynamical system when working with the Fibonacci series. Here's a fun account of analyzing how diseases spread through a population, using zombies instead of diseases: [When Zombies Attack!](https://loe.org/images/content/091023/Zombie%20Publication.pdf) 

### Possible projects
We could try to model things like:
- The spread of Covid-19 in a network, where we draw a link if two people interacted with each other. We can then investigate the effect of different segregation plans or quarantine schemes by changing the structure of the network (cutting certain links)
- The spread of information or misinformation (fake news) in an online network.


### References
A more detailed introduction: [Dynamical systems and matrix algebra](http://www.math.ubc.ca/~behrend/math223/DynSys.pdf)



# 5. Other ideas

If you have any other ideas or areas that you think you would like to explore, do let me know!

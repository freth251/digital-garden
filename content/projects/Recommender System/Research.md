#notes
## Explore existing real-time recommendation systems.

### 1. Scaling the Instagram Explore recommendations system
 ![[Screen Shot 2023-11-03 at 3.56.40 PM.png]]

#### 1.1 Retrieval
- This stage retrieves the content that will be ranked in later stages
- Narrows down the search from billions to hundreds
- There are four types of candidate sources: 

![[Screen Shot 2023-11-03 at 5.10.23 PM.png]]

Two tower model: 

![[Screen Shot 2023-11-03 at 5.22.03 PM.png]]

Two tower model for retrieval: 

![[Screen Shot 2023-11-03 at 5.23.28 PM.png]]

Two tower model for history: 

![[Screen Shot 2023-11-03 at 5.23.45 PM.png]]

#### 1.2 First stage ranking

- Lightweight model that is less precise and less computationally intensive and can recall thousand of candidates. 
- Train the first stage ranker to predict the output of the second stage ranker. 
![[Screen Shot 2023-11-03 at 5.27.58 PM.png]]
#### 1.3 Second stage ranking

- Multi task multi label neural network
- consumes user-item interactions 
#### 1.4 Final ranking
- apply some rules to comply with business rules

### 2. How to design and implement an MVP 

 from the article : 
 
>**To train item embeddings, we adopt the simple but effective word2vec approach**, specifically, the skip-gram model. (This is also used by [Instagram](https://ai.facebook.com/blog/powered-by-ai-instagrams-explore-recommender-system/), [Twitter](https://blog.twitter.com/engineering/en_us/topics/insights/2018/embeddingsattwitter.html), and [Alibaba](https://arxiv.org/abs/1803.02349).) I’ve [previously written](https://eugeneyan.com/writing/recommender-systems-graph-and-nlp-pytorch/#natural-language-processing-nlp-and-graphs) about how to create embeddings via word2vec and DeepWalk and won’t go into details here.

>**To generate candidates, we apply k-nearest neigbours** (à la YouTube’s implementation). However, exact kNN is slow and we don’t really need the precision at this stage. Thus, we’ll use [_approximate_ nearest neighbours](https://en.wikipedia.org/wiki/Nearest_neighbor_search#Approximate_nearest_neighbor) (ANN) instead.


![[Screen Shot 2023-11-03 at 5.41.29 PM.png]]


## Identify potential algorithms suitable for cold-start recommendations 

### User cold start

Strategies to address user cold start problem: 
- Show popular or nearby items.
- Ask user to pick a few interesting items during first time visit. 
- Use sequence of item IDs instead of user ID as your query.

### Item Cold start 

Strategies to address item cold start problem: 
- Illicit user interaction with the item as fast as possible
- Content based filtering 
	- Item metadata
	- Expert Knowledge
- Random exploration

### System cold start

Zero interaction between user and items.

Strategies to address item cold start problem: 
- Use Multi-Armed Bandits to bootstrap
- Transfer learning form an already existing system (same store in different geolocation)
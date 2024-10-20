# Retrieval Augmented Generation (RAG)

## Index

### 1. Introduction
   - What is a Language Model?
   - How do we train and inference a Language Model?
   - The cons of fine-tuning
   - Prompt Engineering
   - QA with Prompt Engineering
   - The pros of fine-tuning
   - QA with Retrieval Augmented Generation

### 2. Embedding Vectors
   - Why do we use vectors to represent words?
   - Word embeddings: the ideas
   - Word embeddings: the Cloze task
   - How do we train embedding vectors in BERT?
   - Sentence Embeddings
   - Sentence Embeddings with BERT
   - Sentence Embeddings: comparison
   - Introducing Sentence BERT
   - Sentence BERT: architecture
   - Strategies to teach new concepts to LLM

### 3. Vector DB
   - Vector DB: introduction
   - K-NN: a naïve approach
   - Similarity Search: let's trade precision for speed
   - HNSW in the real world
   - HNSW: idea #1
   - Navigable Small Worlds
   - Navigable Small Worlds: searching for K-NN
   - Navigable Small Worlds: inserting a new vector
   - HNSW: idea #2
   - HNSW: Hierarchical Navigable Small Worlds
   - HNSW: Hierarchical Navigable Small Worlds

## Retrieval Augmented Generation (RAG)

### Intro to Large Language Models

A language model is a probabilistic model that assigns probabilities to sequences of words. 

In practice, a language model allows us to compute the following: 

```
P [ "China" | "Shanghai is a city in" ]
```

We usually train a neural network to predict these probabilities. A neural network trained on a large corpus of text is known as a Large Language Model (LLM). 

### RAG Pipeline

The RAG pipeline consists of the following steps:

1. **Split into chunks:** The input document is split into smaller chunks.
2. **Embeddings:** Each chunk is converted into an embedding vector using Sentence BERT.
3. **Store:** The embedding vectors are stored in a vector database.
4. **Search:** When a query is given, its embedding vector is calculated and used to search the vector database for the most similar chunks.
5. **Top-K:** The top-K most similar chunks are retrieved from the vector database.
6. **LLM:** The retrieved chunks are fed into a large language model (LLM) along with the query.
7. **Prompt:** The LLM generates a response based on the query and the retrieved context.

### Embedding Vectors

#### Why do we use vectors to represent words?

Given the words "cherry", "digital" and "information", if we represent the embedding vectors using only 2 dimensions (X, Y) and we plot them, we hope to see something like this: the angle between words with similar meaning is small, while the angle between words with different meaning is big. So, the embeddings "capture" the meaning of the words they represent by projecting them into a high-dimensional space. 

We commonly use the cosine similarity, which is based on the dot product between the two vectors.

#### Word embeddings: the ideas

- Words that are synonyms tend to occur in the same context (surrounded by the same words). 
- For example, the word "teacher" and "professor" usually occur surrounded by the words "school", "university", "exam", "lecture", "course", etc..
- The inverse can also be true: words that occur in the same context tend to have similar meanings. This is known as the **distributional hypothesis**.
- This means that to capture the meaning of the word, we also need to have access to its context (the words surrounding it).
- This is why we employ the Self-Attention mechanism in the Transformer model to capture contextual information for every token. The Self-Attention mechanism relates every token to all the other tokens in the sentence.

#### Word embeddings: the Cloze task

- Imagine I give you the following sentence: 
  Rome is the _______  of Italy, which is why it hosts many government buildings. 
  Can you tell me what is the missing word?
- Of course! The missing word is "capital", because by looking at the rest of the sentence, it is the one that makes the most sense.
- This is how we train BERT: we want the Self-Attention mechanism to relate all the input tokens with each other, so that BERT has enough information about the "context" of the missing word to predict it.

#### How do we train embedding vectors in BERT?

```
Input  (14 tokens):  Output  (14 tokens):  Target  (1 token): capital
Rome is the [mask]  of Italy, which is why it hosts many government buildings.
TK1 TK2 TK3 TK4 TK5 TK6 TK7 TK8 TK9 TK10 TK11 TK12 TK13 TK14
```

We run backpropagation to update the weights and minimize the loss.

#### Sentence Embeddings

We can use the Self-Attention mechanism also to capture the "meaning" of an entire sentence. We can use a pre-trained BERT model to produce embeddings of entire sentences. 

#### Sentence Embeddings with BERT

```
Input  (13 tokens):  Output  (13 tokens):
Our professor always gives us lots of assignments to do in the weekend.
TK1 TK2 TK3 TK4 TK5 TK6 TK7 TK8 TK9 TK10 TK11 TK12 TK13
```

The output of the Self-Attention is a matrix of shape (13, 768). We can then apply mean-pooling to take the average of all the vectors, resulting in a single vector with 768 dimensions - our sentence embedding.

#### Sentence Embeddings: comparison

How can we compare Sentence Embeddings to see if two sentences have similar "meaning"? We could use the cosine similarity, which measures the cosine of the angle between the two vectors. A small angle results in a high cosine similarity score. 

But there's a problem: nobody told BERT that the embeddings it produces should be comparable with the cosine similarity, that is, two similar sentences should be represented by vectors pointing to the same direction in space. How can we teach BERT to produce embeddings that can be compared with a similarity function of our choice? 

### Introducing Sentence BERT

Sentence BERT is a variation of BERT that is specifically trained to produce sentence embeddings that are comparable using cosine similarity. It does this by using a Siamese network.

#### Sentence BERT: architecture

Sentence BERT uses two identical BERT models, one for each input sentence. The outputs of the BERT models are then fed into a pooling layer to produce sentence embeddings. The sentence embeddings are then compared using cosine similarity, and the loss is calculated based on the difference between the cosine similarity and the target similarity score.

```
Sentence A
BERT
Pooling
Sentence Embedding A
Sentence B
BERT
Pooling
Sentence Embedding B
Cosine Similarity
Target  (Cosine Similarity)
Loss (MSE)
```

We can apply a linear layer to reduce the size of the embedding vector, for example from 768 to 512. We can use mean-pooling, max-pooling or just use the [cls] token as sentence embedding.

#### Strategies to teach new concepts to LLM

There are two main strategies for teaching new concepts to an LLM:

1. **Fine-tuning:** The LLM is fine-tuned on a custom dataset that contains the desired knowledge. This can result in higher quality results compared to prompt engineering, but it can be expensive and may not be additive.
2. **RAG:** The LLM is combined with a vector database that contains relevant context. This allows the LLM to access information from a wider range of sources, but it may not be as effective as fine-tuning for specific tasks.

### Vector DB

#### Vector DB: introduction

A vector database stores vectors of fixed dimensions (called embeddings) such that we can then query the database to find all the embeddings that are closest (most similar) to a given query vector using a distance metric, which is usually the cosine similarity, but we can also use the Euclidean distance. The database uses a variant of the KNN (K Nearest Neighbor) algorithm or another similarity search algorithm. Vector DBs are also used for finding similar songs (e.g. Spotify), images (e.g. Google Images) or products (e.g. Amazon).

#### K-NN: a naïve approach

Imagine we want to search for the query in our database: a simple way would be comparing the query with all the vectors, sorting them by distance, and keeping the top K.

If there are N embedding vectors and each has D dimensions, the computational complexity is in the order of O(N*D), too slow!🐌

#### Similarity Search: let's trade precision for speed

The naïve approach we used before, always produces accurate results, since it compares the query with all the stored vectors, but what if we reduced the number of comparison, but still obtain accurate results with high probability?

The metric we usually care about in Similarity Search is recall.

In this video we will explore an algorithm for Approximate Nearest Neighbors, called Hierarchical Navigable Small Worlds (HNSW). 

#### HNSW in the real world

It is the same algorithm that powers Qdrant, the open source Vector DB used by Twitter’s (X) Grok LLM, which can access tweets in real time.

#### HNSW: idea #1

HNSW is an evolution of the Navigable Small Worlds algorithm for Approximate Nearest Neighbors, which is based on the concept of **Six Degrees of Separation**.

Milgram's experiment aimed to test the social connections among people in the United States. The participants, who were initially located in Nebraska and Kansas, were given a letter to be delivered to a specific person in Boston. However, they were not allowed to send the letter directly to the recipient. Instead, they were instructed to send it to someone they knew on a first-name basis, who they believed might have a better chance of knowing the target person.

At the end of Milgram’s small-world experiment, Milgram found that most of the letters reached the final recipient in five or six steps, creating the concept that people all over the world are all connected by six degrees of separation.

Facebook found in 2016 that its 1.59 billion active users were connected on average by 3.5 degrees of separation: https://research.facebook.com/blog/2016/02/three-and-a-half-degrees-of-separation/

This means that you and Mark Zuckerberg are only 3.5 connections apart!

#### Navigable Small Worlds

The NSW algorithm builds a graph that – just like Facebook friends – connects close vectors with each other but keeping the total number of connections small. For example, every vector may be connected to up to 6 other vectors (to mimic the Six Degrees of Separation).

```
01  02
     \
      03
     / \
    05  04
   /  / \
  06  09  08
       \ /
        13
       / \
      11  12
     /  \  /
    15  10 07
        \ /
         14
```

#### Navigable Small Worlds: searching for K-NN

Given the following query: “How many Encoder layers are there in the Transformer model?”

How does the algorithm find the K Nearest Neighbors?

```
01  02
     \
      03
     / \
    05  04
   /  / \
  06  09  08
       \ /
        13
       / \
      11  12
     /  \  /
    15  10 07
        \ /
         14

Node Text
01 […] The Transformer is a model […]
02 […] Diagnose cancer with AI […]
03 […] A transformer-based model […]
04 […] The Transformer has 6 layers […]
05 […] An MRI machine that costs 1$ […]
06 […] The dot-product is a […]
07 […] Big-Pharma is not so big […]
08 […] Cross-Attention is a great […]
09 […] To solve an ODE […]
10 […] We are aging too fast […]
11 […] Open-source models like […]
12 […] MathBERT : a new model […]
13 […] AI to control aging […]
14 […] Attention is all you need […]
15 […] LLaMA 2 has 7B params […]
```

We start from a randomly chosen node and then we keep moving to the closest neighbor. 

#### Navigable Small Worlds: inserting a new vector

We can insert a new vector by searching the top KNN with the searching algorithm described before and making an edge between the vector and the top K results.

#### HNSW: idea #2

To go from NSW (Navigable Small Worlds) to HNSW (Hierarchical Navigable Small Worlds), we need to introduce the algorithm behind the data structure known as Skip-List.

The skip list is a data structure that maintains a sorted list and allows search and insertion with an average of 𝑂(log𝑁) time complexity.

```
H3
H2
H1
H0 EEEE
1 5 3 9 12 17 23
```

Let’s search the number 9

#### HNSW: Hierarchical Navigable Small Worlds

HNSW uses a hierarchical structure of graphs, where each level is sparser than the previous one.

```
Q
Layer 0 (dense)
Layer 1
Layer 2
Layer 3 (sparse)
```

Let’s search!

We repeat the search with randomly chosen starting points (on the top layer) and then keep the top K among all the visited nodes.

## Thanks for watching!

Don’t forget to subscribe for more amazing content on AI and Machine Learning!

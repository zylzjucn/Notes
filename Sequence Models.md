# Sequence Data

![Sequence Models](resources/SequenceModels1.jpg)

In some cases, both X and Y are sequences. For others, maybe X not or Y not.

# Representing words

> x: Harry Potter and Hermione Granger invented a new spell.

Come up with a vocabulary(dictionary):

> Vocabulary: [a, aaron, ..., and, ..., harry, ..., potter, zulu]

Each word is represented as a vector with only one 1 and others 0 (one-hot vector)

> length of vector = length of vocabulary

# Recurrent Neural Network

CNN can not use for sequence learning problem:

1. Inputs, outputs can be different lengths in different examples

2. Doesn't share features learnt across different position of text

![RNN](resources/RNN.jpg)

- Parameters are shared in different steps

- Need to make up for a<0> (always all 0)

- Use the information before the stpe but not after (solution: BRNN)

# Forward Propagation

notation:

Wax: multiply **x-like** to compute **a-like**

![Forward Propagation](resources/ForwardPropagation.jpg)

- g1 might be tanh or ReLu

- g2 might be sigmoid

# Backward Propagation through time

![BPTT](resources/BPTT.jpg)

# Different types of RNNs

![Different types of RNNs](resources/DifferentRNNs.jpg)

#### one to many:

> music generation(sequence generation)

#### many to one:

> sentiment classification

#### many to many:

> ##### Tx == Ty:
> 
> name entity recognition
> 
> ##### Tx != Ty: (encoder and decoder)
> 
> machine translation

# RNN models

![RNN models](resources/RNNModels.jpg)

If meeting a word not in the vocabulary, use \<UNK> to represent.

# Sampling novel sequence

- Sampling a sequence from a trained RNN

![Sampling novel sequence](resources/Sampling.jpg)

- Character-level language model

![Character](resources/Character.jpg)

#### Character-wise comparing to the word-wise:

- Can handle with \<UNK>
- End with much longer sequences, high qualily hardware required


# Vanishing gradients

- problem for **long-range dependencies:**

> The cat, which ... , was full.

> The cats, which ... , were full.

while generating 'was' or 'were', the model needs to remember the far earlier nouns, 'cat' and 'cats', which is difficult.

- Gradients explosion is also a problem:

> 1. Has "NAN"
> 
> 2. Solution: **Gradient Clipping** (If the gradient vector is bigger than the threshold, re-scale some vector so that it is not too big)

# Gated Recurrent Unit(GRU)

![GRU](resources/GRU.jpg)

> c to keep some information for long time later(singular or plural)
> 
> gamma-u to decide while updating c or not(sigmoid)
> 
> gamma-u might be close to 0, so the value of c could be kept, and the problem of vanishing gradients solved.

# Long short term memory(LSTM)
 
> - GRU: simpler, faster, bigger models (2 gates)
> 
> - LSTM: mode powerful and effective (3 gates)

![LSTM](resources/LSTM.jpg)

> red line indicates preventing of the vanishing gradients

> The * stands for element multiply

# Bidirectional RNN

![BRNN](resources/BRNN.jpg)

> - pros: considers both previous and later content
> 
> - cons: need the whole content(so not for real-time project)

# Deep RNNs

> a[l]\<t>: activation of layer l and time t

> wa[l] and ba[l] for the layer. Same in the layer and different among layers

![Deep RNN](resources/DeepRNN.jpg)

> 3 layers is quite big in RNN. May stack more layers with no horizontal connection

# Word representation

> One-hot vectors can't show the connection between the words(man and woman, apple and orange)

![Word Representation](resources/repre.jpg)

- We can use different features to represent words so that the product between 2 relative words would not be 0(so they are relative). Better than One-hot.

![Word Embedding](resources/3D.jpg)

# Word Embeddings

![Word Embedding](resources/Embedding.jpg)

# Properties of word embeddings

![Properties](resources/property.jpg)

> If "man" is corresponding to "woman", what's "king" for?
> 
> The big elements in the subtracted vector(-2) represents the different property(gender). They should have similar subtracted vector

![word vectors](resources/wordvectors.jpg)

> To find a word that has a similar vector. To maximize the similarity function

![Cosine](resources/cosine.jpg)

> Cosine of 2 vectors, a good function

# Embedding matrix

![Embedding Matrix](resources/matrix.jpg)

> In the big embedding matrix(E), different words in a row while different features in a column. 
> 
> E * one-hot vector = the word's feature. Same with extract the column directly, which is the actual practice
> 
> The goal: **learn E**

# Context/target pairs

![Context](resources/context.jpg)

> skip-gram is to pick up context to figure out the target word.

# Negative Sampling

![Negative](resources/negative.jpg)

> context-target pair? (the goal train to learn)
>
> 1: positive example
> 
> 0: negative example (word randomly chosen from the dictionary)

![Model](resources/negmodel.jpg)

- Only need to update for k+1 instead of 10000 pairs for each iteration

How to generate negative examples:

![Select](resources/select.jpg)

# GloVe

![GloVe](resources/glove.jpg)

- Simply check the times i and j been in a context(customized) globally

![GloVeModel](resources/glovemodel.jpg)

![Featurization](resources/featurization.jpg)

- The features machine learnt are may not explain by human. The are not the same coordinate system and even not orthogonal

# Sentiment Classification

![Sentiment](resources/sentiment.jpg)

- simply average or sum the features of different words

- did not consider the words' order

![Sentiment Model](resources/sentmodel.jpg)

# Debias word embeddings

![Bias](resources/bias.jpg)

- use for school and job admissions, loan applications, criminal justice system

- want to eliminate this kind of bias

![Bias2](resources/bias2.jpg)

1. Use subtraction average to figure out a cordinate system with bias(x) and non-bias(y)
2. For the words which should be unbias, project them onto y axis
3. For the others(should be bias), they should be symmetry to y

# Image captioning

Many to many models:

- translation:

![sequence to sequence](resources/stos.jpg)

- image captioning:

![Catpion](resources/caption.jpg)

# Picking the most likely sentence

**Machine translation** and **language model** is similar. Difference:

![Comparison](resources/translation.jpg)

- Language model starts at vector zeros

- Machine translation starts at a representation from encoding network

So Machine translation is a **conditional language model**:

- Modeling the probability of the output of English translation, conditions on some input French sentence.

Greedy search doesn't work here.

# Beam search

![Beam](resources/beam.jpg)

> Set a beam width(say 3, if 1, it is greedy search)
> 
> For each step, find out 3 words combination with highest possibilities out of 10,000 * 3, and save these 3 results to next step.

> beam width bigger: bigger memory space required and slower
> 
> beam width smaller: worse result
> 
> 3~100. up tp 3000 for research

![Length Normalization](resources/lengthnorm.jpg)

> problem: The product of probabilities could be too small

- solution: Use log()

> problem: Finding max may lead to a shorter result(less word, bigger product or sum)

- solution: Devided by Tx to normalize

> A normalized log likelihood objective

alpha is a hyperparameter to tune, often 0.7

> Unlike exact search algorithms like BFS or DFS, Beam Search runs faster but is not guarenteed to find exact maximum for arg max P(y|x)

# Error analysis in beam search

![Error Analysis](resources/error.jpg)

# Bleu score

![Biagrams](resources/biagrams.jpg)

![Bleu](resources/bleu.jpg)

# Attention Model

![Attention](resources/attention.jpg)

For short sentences, not good. For long sentences, attention can improve the performance. As the human beings, we don't expect the machine to remember all the sentence.

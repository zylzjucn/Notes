### Sequence Data

![Sequence Models](resources/SequenceModels1.jpg)

In some cases, both X and Y are sequences. For others, maybe X not or Y not.

### Representing words

> x: Harry Potter and Hermione Granger invented a new spell.

Come up with a vocabulary(dictionary):

> Vocabulary: [a, aaron, ..., and, ..., harry, ..., potter, zulu]

Each word is represented as a vector with only one 1 and others 0 (one-hot vector)

> length of vector = length of vocabulary

### Recurrent Neural Network

CNN can not use for sequence learning problem:

1. Inputs, outputs can be different lengths in different examples

2. Doesn't share features learnt across different position of text

![RNN](resources/RNN.jpg)

- Parameters are shared in different steps

- Need to make up for a<0>

- Use the information before the stpe but not after (solution: BRNN)




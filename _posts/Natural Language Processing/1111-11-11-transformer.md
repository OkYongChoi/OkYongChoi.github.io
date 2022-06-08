---
# permalink: /posts/
title: "Transformer: Attention Is All You Need"
toc: true
categories:
  - natural language processing
tags:
  - attention mechanism
  - transformer
  - masked language model

---
Attention Is All You Need.  
Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Lukasz Kaiser, Illia Polosukhin

# Seq2seq + Attention?
## The problems of the seq2seq model and the attention as a solution
### Problems:  
- The seq2seq compresses all the information into one fixed-size vector (context vector), resulting in information loss.
- Using RNNs induces vanishing gradient.  

### Attention as a solution:  
- Attention mechanism in the paper where it was introduced also uses the RNNs (seq2seq + attension), but the Attention refers the entire input senstence at the decoder at each time step the decoder predicts the output


But rather than using Attention as a solution to your RNN problem, why not make your encoder and decoder with **attention**-only?

# Transformer Architecture
With the attention mechanism instead of RNNs, it can capture longer dependencies in a sentence.

Not using the recurrence method of RNN can significantly speed up the training time.


- In the seq2seq structure, each RNN in the encoder and decoder had t time steps, but the Transformer is a structure consisting of $N$ encoders and decoders. 

- Each encoder and decoder layer is sometimes referred to as *Transformer block*.   

- N is referred to as the number of layers (or Transformer blocks)

- In the Transformer paper, the number of Transformer blocks, N, is 6, that is, 6 encoders and 6 decoders each are stacked. In the BERT, N is 12 for BERT-BASE and 24 for BERT-LARGE

## Encoder
![transformer-block](/assets/images/transformer/transformer-block.svg)
 - $d_{model}$ is the fixed input and output size of the encoder and decoder of the transformer. 
 - And at the same time, it equals to the dimension of the embedding vector.
 - In the figure, $d_{model}$ is expressed as 4, but in the original paper, 512. The dimension of the embedding vector is also $d_{model}$.

This encoder block is repeated $N$ times.

<center><img src="/assets/images/transformer/transformer-encoder.svg" width="70%" height="70%" itle="transformer-encoder"/> </center>


### Positional Encoding 
The reason why RNNs were useful in NLP was RNNs receive and process words sequentially depending on the order of the word, so that they could have the position information. Transformer uses the following two functions to create a value with positional information.
$$
PE\left(pos, 2i\right) = \sin\left(pos/10000^{2i/d_{model}}\right) \\
PE\left(pos, 2i+1\right) = \cos\left(pos/10000^{2i/d_{model}}\right)
$$
$pos$ denotes the position of the embedding vector in the input sentence, and $2i$ and $2i+1$ denotes the index of the dimension within the embedding vector. According to the above expression, the value of the sine function is used if the index of each dimension within the embedding vector is even, $(pos, 2i)$, and the cosine function is used for odd, $(pos, 2i+1)$.

$d_{ff} = 2048$

### Self-Attention
Conducts attention on itself, which calculates the similarity between all of the words in the input sentence.

### Multi-Head Attention
 - Multi-Head Attention gives the *multiple representation subspaces*. That is, it collects information from *different perspectives* by performing attention in parallel.
 - h: the number of attentions heads
 - With multi-head attention, multiple sets of Q, K, V weight matrices are randomly initialized. 
 - Then, after training, each set is used to project the input (embedding vectors or vectors from lower encoders/decoders into a different representation subspace).


#### Query, Key and Value
 - Q: all word vectors in the input sentence
 - K: all word vectors in the input sentence
 - V: all word vectors in the input sentence

Transformer determines the dimension of each $\mathbf{Q}$, $\mathbf{K}$, and $\mathbf{V}$ vectors (actually they are matrices because the word vectors are stacked) by dividing $d_{model}$ by the number of attention heads (h). In the paper, h is set to 8. Thus the size of the $\mathbf{Q}, \mathbf{K}, \mathbf{V}$ vectors are $d_{model}/h = 64$. These vectors are obtained by multiplying the ($d_{model}$, $d_{model}/h$) size of matrices, $\mathbf{W}_i^Q, \mathbf{W}_i^K, \mathbf{W}_i^V$, to the input vector. ($i \in range(h)$)

We denote the dimension of $\mathbf{Q}, \mathbf{K}, and \mathbf{V}$ vectors, $d_{model}/h$ by $d_k$.

While, in the attention paper, $\mathbf{V}$ vector was each hidden state of the RNN encoder, in this paper, it is obtained by multiplying the matrices, $\mathbf{W}_i^Q, \mathbf{W}_i^K, \mathbf{W}_i^V$.

 - Attention scores: $\mathbf{Q} \times \mathbf{K}^\intercal$
 - Attention value: $Attention(\mathbf{Q}, \mathbf{K}, \mathbf{V})$ = $softmax\left(\frac{\mathbf{Q} \times \mathbf{K}^\intercal}{\sqrt{d_k}}\right)\mathbf{V}$
  
The matrix concatenating attention heads is multiplied by another weight matrix, $\mathbf{W^O}$, and the resulting matrix is the final result of multi-head attention.
$$ MultiHead(\mathbf{Q}, \mathbf{K}, \mathbf{V}) = Concat(head_1, ..., head_h) \mathbf{W^O} \\
\text{where } head_i = Attention(\mathbf{Q}\mathbf{W}_i^Q, \mathbf{K}\mathbf{W}_i^K, \mathbf{Q}\mathbf{V}_i^V) $$

### Masked Attention





http://jalammar.github.io/illustrated-transformer/
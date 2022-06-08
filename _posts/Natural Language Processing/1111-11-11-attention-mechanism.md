---
# permalink: /posts/
title: "Attention"
toc: true
categories:
  - natural language processing
tags:
  - seq2seq
  - attention mechanism

---
Neural Machine Translation by Jointly Learning to Align and Translate  
Dzmitry Bahdanau, Kyunghyun Cho, Yoshua Bengio

# Seq2seq Model
## Problems of seq2seq
There are some problems with the seq2seq model based on these RNNs. (RNN Language Model)
1. The seq2seq compresses all the information into one fixed-size vector (context vector), resulting in information loss.
 - Attention mechanism make use of not only the 
2. Using RNNs induces vanishing gradient.
 - Attention mechanism in the paper where it was introduced also uses the RNNs (seq2seq + attension), but the Attention refers the entire input senstence at the decoder at each time step the decoder predicts the output

## Attention
Seq2seq uses the hidden state of the decoder at time step t as is. However, the attention uses extra
Attention score is a score that determines how similar the hidden state $d_t$ of the decoder at current time t is to each hidden state $e_i$ of the encoder in order to predict a word at current time t of the decoder.
 - Attention score: $ \mathbf{s}_t = \left[\mathbf{d}_t^{\intercal}\mathbf{e}_1,...,\mathbf{d}_t^{\intercal}\mathbf{e}_n \right] $
 - Attention distribution: $ \mathbf{p}_t = softmax(\mathbf{s}_t) $
 - Attention weight $w_i$: $ \mathbf{p}_t = \left[ w_1^t,... w_n^t \right] $, each element of the attention distribution.
 - Attention value: $ \mathbf{a}_t = \sum_i w_i^t \mathbf{e}_i$ , attention value is a vector. It is also refered to as a context vector in that it includes the context of the sentence
## Dot-product Attention
1. Concatenate the attention value $a_t$ and the hidden state of the decoder at current timestep t $d_t$. When the $a_t$ and $d_t$ are colum vectors,
$$ \mathbf{c}_t = \left[\mathbf{a}_t; \mathbf{d}_t \right]. $$
2. Add a neural network before sending it directly to the output layer. After multiplying with the weight matrix $ \mathbf{W}_c $ and adding with the bias $\mathbf{b}_c$, it goes through the hyperbolic tangent function to get a new vector $\tilde{\mathbf{d}_t}$ for the output layer operation.
$$ \mathbf{\tilde{d}}_t = \tanh \left(\mathbf{W}_c[\mathbf{c}_t] + \mathbf{b}_c\right) $$
3. Use $\mathbf{\tilde{d}}_t$ as an input of the output layer
$$ {\mathbf{\hat{y}}_t} = softmax\left( \mathbf{W}_y \mathbf{\tilde{d}}_t + \mathbf{b}_y \right) $$ 



## Question
1. How are the seq2seq and seq2seq + attention different?

2. What is the attention score and attention value? What is the differencec between the two?
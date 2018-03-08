### Title

End-To-End Memory Networks

### tl;dr

Introduces a neural network with external memory, helping in tasks like question answering and language modelling. Builds upon Memory Networks introduced by FAIR, but it is trained end-to-end, making it more scalable and less supervision is needed.

### Describe the method

The architecture is shown below

#### Architecture

<img width="789" alt="architecture" src="https://user-images.githubusercontent.com/8344320/37136190-8685fea4-22c6-11e8-90d7-64878675dcce.png">


The major insights of this paper are introduction of computational hops in memory and the fact that all functions are smooth, hence backpropagation is easy. This makes the MemN2N look like an RNN/LSTM and better than the MemNN. 

We compute embeddings for each of the input vectors x<sub>i</sub>(in external memory-think of it as facts in a QA task) to compute their corresponding memory vectors m<sub>i</sub>(using a dxV embedding matrix A). Also we compute the embedding for the query(say in the QA task) q(using dxV embedding matrix B). The input vectors x<sub>i</sub> also have corresponding output vector embeddings o<sub>i</sub>(using dxV embedding matrix C). Details on how to embed a full sentence are given in the paper(two approaches - summing up BoW or incorporating position encoding PE as well). To ensure temporality of the data(i.e order of events), we also use a temporal matrix T<sub>k</sub> corresponding to each of the matrices A,B,C. This is used in generating the memory vector embeddings. Matrices A,B,C and T<sub>A</sub>,T<sub>B</sub>, T<sub>C</sub> are learnt during training. 

The MemN2N also makes use of the attention mechanism by computing similarity between the query embedding and each of the memory embeddings. It computes the softmax score of the dot products(soft attention). The weighted sum of these attention weights and corresponding memory output vector embedding results in the output vector o<sub>i</sub>.

The above accounts for a single step MemN2N. The paper introduces a concept of multiple hops, so once we get an output vector o from the previous layer, we combine it to produce a new query embedding c<sup>k+1</sup> as follows. We can use weight tying to use the same A,B,C matrices for all layers(like an RNN/LSTM) or consider the previous layer's output embedding matrix C as the new input embedding matrix. Both approaches are considered in the paper.

A cross-entropy loss is used to train model by backpropagating the error through multiple memory layers.

#### Dataset, Results and Reasons

The paper focuses on two tasks - Question Answering and Language Modelling

*Question Answering*

The paper uses the bAbI dataset consisting of 20 different QA tasks. The dataset has a small vocabulary(177) and each task consists of multiple problems, each having <= 320 sentences, a query and the answer. Two variants are used - one with 1000 training problems and another with 10000. The task is said to be failed if error is greater than 5%. 

To train, 10% of the dataset is kept as validation set. The paper introduces Linear Start(LS) training, which does not initially use any softmax layers except the final output softmax. After a few epochs, it incorporates the softmax and proceeds to normal training. Details wrt learning rate annealing, weight initialization etc can be found in the training details section of the paper.

The performance of MemN2N is close to the strongly supervised MemNN(error - 12.6% vs 6.7% in 1k dataset and 4.2% vs 3.2% in 10k). LS training and joint training on all tasks seem to help, with the former helping avoid local minimas. As expected, position encoding does better than BoW due to remembering order of words in a sentence. 
 
*Language Modelling*

Two standard datasets are used - Penn Tree Bank and Text8 datasets. Penn Tree Bank consists of 929k/73k/82k train/validation/test words, distributed over a
vocabulary of 10k words. Text8 is a a pre-processed version of the first 100M million characters, dumped from Wikipedia. This is split into 93.3M/5.7M/1M character train/validation/test sets. All word occurring less than 5 times are replaced with the <UNK> token, resulting in a vocabulary size of ∼44k. (refer paper)

Changes are made according to sections described in the MemN2N paper.
* There are no query. We try to find the next word. Not a response to a query. Hence, we do not need embedding B and we just fill u with a constant say 0.1.
* We use multiple layers but we use the same embedding A for all layers. We use a separate embedding B for all layer.
* To aid training, we apply ReLU operations to half of the units in each layer. (Section 5)

The MemN2N achieves lower perplexity on both datasets (111 vs 115 for RNN/SCRN on Penn and 147 vs 154 for LSTM on Text8). Further, it has lesser parameters than an LSTM for the same number of hidden units and has only ~1.5 times the number of params in an RNN.
 
As mentioned before, the MemN2N uses multiple hops. It has been observed that some hops focus on recent sentences only, while others have attention over all memory vectors. This is consistent with the notion of a cache language model.

>To understand why it is a good idea for a statistical language model to contain a cache component one might consider someone who is dictating a letter about elephants to a speech recognition system. Standard (non-cache) N-gram language models will assign a very low probability to the word "elephant" because it is a very rare word in English. If the speech recognition system does not contain a cache component the person dictating the letter may be annoyed: each time the word "elephant" is spoken another sequence of words with a higher probability according to the N-gram language model may be recognized (e.g., "tell a plan"). These erroneous sequences will have to be deleted manually and replaced in the text by "elephant" each time "elephant" is spoken. If the system has a cache language model, "elephant" will still probably be misrecognized the first time it is spoken and will have to be entered into the text manually; however, from this point on the system is aware that "elephant" is likely to occur again – the estimated probability of occurrence of "elephant" has been increased, making it more likely that if it is spoken it will be recognized correctly. Once "elephant" has occurred several times the system is likely to recognize it correctly every time it is spoken until the letter has been completely dictated. This increase in the probability assigned to the occurrence of "elephant" is an example of a consequence of machine learning and more specifically of pattern recognition.

The paper also highlights better performance with higher number of computation hops.

### Any further details

* [Paper](https://arxiv.org/pdf/1503.08895.pdf)
* [A good blog](https://jhui.github.io/2017/03/15/Memory-network/)

### My two cents

I find this paper interesting as it reduces the need for strong supervision(in an MemNN) and also the need for multiple computation hops. However, I feel the fact that number of computation hops is a fixed parameter may not work for cases where the logic flow is more complicated. Also, a small tweak in the architecture we could do is eliminate the need for different input and output embeddings for the memory by tying their weights.

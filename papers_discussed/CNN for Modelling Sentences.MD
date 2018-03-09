### Title

A Convolutional Neural Network for Modelling Sentences

### tl;dr
The paper describes a Dynamic convolutional neural network for semantic modelling of sentences. This network takes input sentences of varying length and induces a feature graph explicitly for capturing short and long-range relations.

### Describe the method

#### Architecture:

The network is stacked with alternate convolution layers and dynamic pooling layers and a few folding operations in the deeper layers of the network. The input to the network is a sequence of embeddings corresponding to each word in the input sentence.Each embedding is a vector of fixed length which captures the semantic information of the correlated word.

##### wide convolution:
The convolution operation used in DCNN is one-dimensional since the columns of the input matrix are embeddings of a word and the elements in a column vector are not independent.Let the filter is a vector of weights m ∈ R^m and the input sentence is viewed as a sequence s ∈ R^s and if c is the sequence of outputs of the convolution, c_{j} = m^T s_{j−m+1:j}. The wide type of convolution does not have
requirements on s or m and yields a sequence c ∈ R^s+m−1 where the index j ranges from 1 to s+m−1.

<img width="300" style="horizantal-align:middle" alt="architecture" src="https://user-images.githubusercontent.com/13769685/37209481-7caa5fb4-23cb-11e8-81cd-0ab26329a83f.png">

The dynamic pooling layer used in this network is an improvement over the simple max pool layers and it uses kmax operation.As the network takes input sentences of varying length, width of a feature map at an intermediate layer varies depending on the length of the input sentence.

##### kmax-pooling:
The kmax-pooling is a generalisation of the max-pool operation. Given a value k and a sequence p ∈ R^p of length p ≥ k, kmax
pooling selects the subsequence p^k_{max}of the k highest values of p. The order of the values in p^k_{max} corresponds to their original order in p.

The value of k is chosen dynamically depending on the length of input to the convolution layer and the fraction of the depth of the present convolution layer in the complete network.

##### Folding
Folding operation is performed after a convolutional layer and before (dynamic) k-max pooling. It is adding every two rows in a feature map component-wise. For a map of d rows, folding returns a map of d/2 rows, thus halving the size of the representation.

As in case of simple CNNS, multiple feature maps are used with different filters each, to obtain multiple raw features in the input sentence. Unlike CNNs the weights are predefined to capture specific relations among words and are not learned in the network.

![dcnn](https://user-images.githubusercontent.com/13769685/37209360-11a01d8a-23cb-11e8-86cf-3a707b7b91a2.png)

##### datasets used for evaluation
The above network is evaluated on various datasets like  small-scale binary and multi-class sentiment prediction, six class question classification and Twitter sentiment prediction by distant supervision.

> Comparing DCNNS with past models like neural models using bag of n-grams, This model can capture the relation among words as far apart as the length of the filter vector(m) in the next layer though going into deeper layers, DCNN can recognize semantic relation between any pair of words.

### Any further details
> Similar work 
Ask Me Anything: Dynamic Memory Networks for Natural Language Processing

### My two cents

> I find that the model can be extended to any length of input sentence very useful and i believe that this method can also be extended to replace all the neural models which had to use fixed size representation of sentences earlier like "CNNs for Matching Natural Language Sentences" and "Effective Use of Word Order for Text Categorization with CNNs"

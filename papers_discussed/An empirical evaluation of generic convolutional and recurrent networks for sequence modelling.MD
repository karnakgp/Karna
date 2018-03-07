### Title

An Empirical Evaluation of Generic Convolutional and Recurrent Networks for Sequence Modeling

### tl;dr

Shows how temporal convolutional networks are better than recurrent networks for seq-to-seq tasks.

### Describe the method

> The results suggest that TCNs convincingly outperform baseline recurrent architectures across a broad range of sequence modeling tasks

This is the reason they give to just compare TCNs with basic architectures like LSTMs and GRUs:
> Greff et al. (2017) benchmarked the performance of eight LSTM variants on speech recognition, handwriting recognition, and polyphonic music modeling. They also found that “none of the variants can improve upon the standard LSTM architecture significantly”. 

causal is basically zero padding
> The distinguishing characteristics of TCNs are:
  1. The convolutions in the architecture are causal, meaning that there is no information “leakage” from future to past;
  2. The architecture can take a sequence of any length and map it to an output sequence of the same length, just as with an RNN

> A simple causal convolution is only able to look back at a history with size linear in the depth of the network. This makes it challenging to apply the aforementioned causal convolution on sequence tasks, especially those requiring longer history. Our solution here, following the work of van den Oord et al. (2016), is to employ dilated convolutions that enable an exponentially large receptive field (Yu & Koltun, 2016)

Dilated convolutions:
![image](https://user-images.githubusercontent.com/9252491/37082425-8cac69e4-2212-11e8-9ab4-8054f9cb257a.png)

> This gives us two ways to increase the receptive field of the TCN: choosing larger filter sizes k and increasing the dilation factor d, where the effective history of one such layer is (k − 1)d. As is common when using dilated convolutions, we increase d exponentially with the depth of the network (i.e., d = O(2i ) at level i of the network). This ensures that there is some filter that hits each input within the effective history, while also allowing for an extremely large effective history using deep networks. 

Why use residual connections here:
> Since a TCN’s receptive field depends on the network depth n as well as filter size k and dilation factor d, stabilization of deeper and larger TCNs becomes important. For example, in a case where the prediction could depend on a history of size 2^12 and a high-dimensional input sequence, a network of up to 12 layers could be needed. Each layer, more specifically, consists of multiple filters for feature extraction. 

Features and comparisons against RNNs:
  - Parallelism (RNNs computations depend on previous time steps and hence cant be parallelized)
  - Flexible receptive field size (but same exists for RNNs)
  - Stable gradients (no vanishing/exploding gradients as TCNs have a backpropagation path different from the temporal direction of the sequence.
  - Low memory requirement for training (RNNs use lot of memory to store partial results during the training, TCNs dont)
  - Variable length input (TCNs are basically 1D CNNs - realte to the DSSM variable input size case)

Disadvantages:
  - Data storage during evaluation (TCNs take more data - *how??*)
  - Potential parameter change for a transfer of domain (Different domains can have different requirements on the amount of history the model needs in order to predict. Therefore, when transferring a model from a domain where only little memory is needed (i.e., small k and d) to a domain where much longer memory is required (i.e., much larger k and d), TCN may perform poorly for not having a sufficiently large receptive field.)
  
### Any further details

> Other recent works have aimed to combine aspects of RNN and CNN architectures. This includes the Convolutional LSTM (Shi et al., 2015), which replaces the fully-connected layers in an LSTM with convolutional layers to allow for additional structure in the recurrent layers; the Quasi-RNN model (Bradbury et al., 2017) that interleaves convolutional layers with simple recurrent layers; and the dilated RNN (Chang et al., 2017), which adds dilations to recurrent architecture 

>  showed that the “infinite memory” advantage of RNNs is largely absent in practice. TCNs exhibit longer memory than recurrent architectures with the same capacity

- http://www.inference.vc/dilated-convolutions-and-kronecker-factorisation/

### My two cents

I believe that TCNs are more practical than RNNs.

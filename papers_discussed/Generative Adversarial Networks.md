### Title

 Generative Adversarial Networks

### tl;dr

 2 models are being simultaneously trained :  a generative model that captures data and tries to imitate it and a discriminant 
model that tries to differentiate whether the data came from the training data or the generative model

### Describe the method

 An adversarial net is applied first. A prior on input noise variables and a second multilayer perceptron is defined. *D(x)* represents 
the probability that the data input *x* came from data rather than generator's distribution to the data. The *D* is trained to maximise
the probability of assigning correct labels to both data samples and the sample from generator(*G*). The *G* is simultaneously trained to 
minimise log(1 − D(G(z))).

![alt text](https://github.com/the-ethan-hunt/storage/blob/master/GANS.JPG)

Advantages- 

- Only backpropagation is needed to obtain gradients.

- Since there is no need to generate different entries in sample sequentially, it's sample generation is much faster than fully visible 
nets like WaveNet.

- Compared to VAEs, it is easier to use discrete latent variables. Additionally, VAEs produce deterministic bias to optimise a lower bound
on the log-likelihood.


### Any further details

[Laplacian Pyramid of Adversarial Networks](https://arxiv.org/pdf/1506.05751v1.pdf) is an interesting paper that has a network architecture 
generating high quality images mistaken 40% of the times when evaluated by humans.

### My two cents

I liked the idea of concept of *D* and *G* playing a two layer minimax game with the following value function *V(G,D)*

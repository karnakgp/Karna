### Title

Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift

### tl;dr

The method speeds up the training phase by stabilizing the distribution of each layer. Further, it also acts as regularizer eliminating the need for Dropout (in some cases) and use of higher learning rates.

### Describe the method

The technique normalizes the input along each feature to have `zero mean` and `unit variance`. In addition, the model learns two additional parameters, `scale` and `shift` for each of the normalized feature. The algorithm is presented below (i.e, application of batch-norm on an activation x over a mini-batch).

![transform](https://user-images.githubusercontent.com/15105941/37105982-945fa8ba-2256-11e8-867b-66588ad40eb0.png)

Internal Covariate shift refers to the change in the distributions of internal nodes of a deep network in the course of training.

Introduction of the parameters `beta` and `gamma` for every normalized features can give back the original activation x if that were the optimal values to be learnt for that activation in which case `beta` = 0 and `gamma` = 1.


The model computes the `mean` and `variance` along each feature of a layer and accumulates this over-time during training and saves it as `moving_mean` and `moving_variance` (decaying running average). These accumulated batch statistics are used at inference time. The algorithm to train a batch-normalized network is presented below.

![training](https://user-images.githubusercontent.com/15105941/37105828-2a39ba66-2256-11e8-8bd8-9ae4d5f30432.png)

For fully connected layers of shape `[B, N]`, where `B` is the batch-size and `N` is the number of features, batch-norm is applied along each feature over the entire mini-batch.

For convolutional layers, this normalization should further obey the convolutional property (translational invariance) i.e, different elements of the same feature map at different locations are normalized the same way. To accomplish this, for a feature map of shape `[H, W, C]` over `B` batches, the effective batch-size becomes `B*H*W`. In a nutshell, the parameters, `beta` and `gamma` are learnt per feature rather than per activation.

And of course, when one uses batch-normalization, the mini-batch size is greater than 1.

##### Advantages of using batch-norm:

- This enables the use of higher learning rates without the risk of divergence.
- Regularizes the model (How?)
   - When training with batch-normalization, we accumulate the `moving_mean` and `moving_variance` by looking at different examples and use these statistics at test time. Since these statistics are computed over different training examples, the model now no longer produces deterministic values for a given training example. And thus, the model generalizes (proved in the experments carried out by the author).
- Reduces the need for Dropout
- One can now use saturating nonlinearities by preventing the network from getting stuck in the saturated modes.


### Any further details

- Instance Normalization [paper](https://arxiv.org/abs/1607.08022)

### My two cents

I liked the idea of maintaining the distribution of the layers' activations and incorporating this idea into the network architecture itself.

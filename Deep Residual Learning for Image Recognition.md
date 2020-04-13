### Title
[**Deep Residual Learning for Image Recognition**](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf)

### tl;dr
The depth of a neural network is important for image/visual recognition tasks. But, adding multiple layers causes a degradation problem, where vanishing gradients cause information loss. Addition of 'Residual blocks' to networks allow addition of multiple layers by compensating lost information with information from preceding layers.

### Describe the method
This graphic from the paper shows the _degradation_ problem encountered. The training and test accuracies drop for deeper networks.  
<img src="https://raw.githubusercontent.com/rohan-varma/resnet-implementation/master/images/verydeep_network.png" alt="Plain Network Accuracy" width="480" height="240">  

##### Architecture:  
<img src="https://swethatanamala.github.io/assets/Images/ResNet/restnet-block.png" alt="A residual Block" width="318" height="180">

##### From the paper:
>We present comprehensive experiments on ImageNet to show the degradation problem and evaluate our method. We show that:  
> * Our extremely deep residual nets are easy to optimize, but the counterpart “plain” nets (that simply stack layers) exhibit higher training error when the depth increases.  
> * Our deep residual nets can easily enjoy accuracy gains from greatly increased depth, producing results substantially better than previous networks.
>
>Similar phenomena are also shown on the CIFAR-10 set, suggesting that the optimization difficulties and the effects of our method are not just akin to a particular dataset.

##### Mathematically: 
For the $l^{th}$ layer,
Let input into the $l^{th}$ layer be denoted by $A_l$, weights for layer $l$ be $W_{l}$ & $B_{l}$ and $\sigma_{l}$ be the activation for layer $l$,

1. $Z_{l} = W_{l}A_{l} + B_{l}$  
2. $A_{l+1} = \sigma_{l} (Z_{l})$  
3. $Z_{l+1} = W_{l+1}A_{l+1} + B_{l+1}$  
4. a. $A_{l+2} = \sigma_{l+1} (Z_{l+1} + A_{l})$  

Here the dimensions of $Z_{l+1}$ and $A_{l}$ should be the same.

Or the step 4. a. can be altered to:

4. b. $A_{l+2} = \sigma_{l+1} (Z_{l+1} + W_{s}A_{l})$  

Here $W_{s}$ is chosen such that the dimensions of $Z_{l+1}$ and $W_{s}A_{l})$ are the same, and $W_{s}$ can either be a set of trainable parameters or fixed parameters that pad $A_{l}$.

$W_{s}$ can also be square matrix that is multiplied to $A_{l}$ in equation 4. a. (called 'projection shortcuts') but further experimentation shows that identity mapping is sufficient for improvement in results. Even though having projection shortcuts throughout the network marginally improves results, the added parameters cause an increase in time and complexity of the network. Hence the final decision was to use "projection shortcuts" where dimensions had to be altered and the rest were identity shortcuts.

The above _Shortcut Connection_ skips over two layers. Multiple layer skips can also be used. Single layer skips are observed to have no advantage (They are basically linear connections).

Bottle-Neck Architecture used to reduce parameters in the deeper networks.
<img src="http://i.stack.imgur.com/kbiIG.png" alt="Bottle-Neck Architecture" width="400" height="200">

##### Advantages of using ResNets:
* Deeper Networks can be used, without any drop in performance.
* The experiment tested ResNets with 18 to 152 Layers, with best results being observed with 152 layers for the ImageNet dataset.
* The experiment tested ResNets with 20 to 1202 Layers (0.27M to 19.4M Parameters), with best results being observed with 110 layers for the CIFAR-10 dataset.

### Any further details
* From the Paper:  
> In this paper, we use no maxout/dropout and just simply impose regularization via deep and thin architectures by design, without distracting from the focus on the difficulties of optimization. But combining with stronger regularization may improve results, which we will study in the future.
* Making a few replacements in the equations gives you:  
$A_{l+2} = \sigma_{l+1} (W_{l+1}\sigma_{l} (W_{l}A_{l} + B_{l}) + B_{l+1} + A_{l})$  
Here skipping over two layers allows us to ignore the gradient vanishing effects, that occur with multiplications by small weights over multiple layers. Also, if the activation functions are "ReLU", $A_{l+2}$ acts as an identity function to $A_{l}$.

### My two cents
The paper gives a very simple and efficient method to skip over the layers that contribute to gradient vanishing and also use the more complex features learned with this much more deeper network to make better predictions in Image Classification.
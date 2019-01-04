### Title

A Simple Convolutional Neural Network for Object Detection

### tl;dr
This paper describes 13 layer Convolutional Neural Network that outperforms most of the deeper and complex architectures to date such as VGGNet, ResNet, and GoogleNet on several well-known benchmarks while having 2 to 25 times fewer number of parameters and operations.
### Describe the method

> Major CNNs such as AlexNet, VGGNet, Resnet include tens to hundred of millions of parameters which impose considerable computation. On contrary light weight models mainly suffers from low accuracy.
SimpleNet has acchieved state of the art result on CIFAR10 outperforming several heavier architectures and near state of the art on CIFAR100 and SVHN.

#### Architecture:

We propose a simple convolutional network with 13 layers. The network employs a homogeneous design utilizing 3 x 3 kernels for convolutional layer and 2 x 2 kernels for pooling operations.
The only layers which do not use 3 x 3 kernels are 11th and 12th layers, these layers, utilize 1 x 1 convolutional kernels. Feature-map down-sampling is carried out using nonoverlapping 2 x 2 max-pooling. In order to cope of vanishing gradient and also over-fitting, we used batch-normalization with moving average fraction of 0.95 before any ReLU non-linearity. We also used weight decay as regularizer. A second version of the architecture uses dropout to cope with over-fitting.

![simple_net_architecture](https://user-images.githubusercontent.com/25545489/38054046-66f028e8-32f3-11e8-8aa9-ef43fe0c15e6.png)


##### Gradual Expansion:
In order to utilize both depth and parameters more efficiently, design of the architecture is in a symmetric and gradual fashion, i.e. instead of creating a network with a random yet great depth, and large number of neurons per layer, start with a small and thin network then gradually add more symmetric layers.

##### Local Correlation Preservation:
This network preserve locality information throughout the network as much as possible by avoiding 1 x 1 kernels in early layers.
> It is suggested to replace 1 x 1 kernels with 2 x 2 if one plans on using them other than the end of the network. Using 2 x 2 kernals both help to reduce the number of parameters and also to retain neighborhood information.


##### Maximum Information Utilization
Larger feature-maps especially in early layers, provide more valuable information to the network than the
smaller ones. As more information will lead to more discriminative power.

> With the same depth and number of parameters, a network which utilizes bigger feature-maps achieves a higher accuracy. Therefore instead of increasing the complexity of a network by increasing its depth and number of parameters, one can leverage more performance/accuracy by simply using larger input dimensions or avoiding rapid early down-sampling.  This is a good technique to keep the complexity of the network in check and improve the network performance.

##### Datasets used for Evaluation
The above network is evaluated on various datasets like CIFAR10, CIFAR100, MNIST, SVHN.
Here is the performance on CIFAR dataset:
![persormance_simple_net](https://user-images.githubusercontent.com/25545489/38061662-cfbd2ffa-330d-11e8-9f05-40e2f9ec9b22.png)

The Arc1 achieves a new state of the art in CIFAR10 when no data-augmentation is used and the Arc2 achieves 95.32%. In addition to the normal architecture, model used a modified version on CIFAR100 and achieved 74.86% with data-augmentation.
### Any further details
This model tried to show the importance of simplicity and optimization using our experiments and also encourage more researchers to study the vast design space of convolutional neural network in an effort to find more and better guidelines to make or propose better performing architectures with much less overhead.
[Link](https://arxiv.org/pdf/1608.06037.pdf) to the paper.
### My two cents

> I found that model does not need to be always deep and complex to perform better, if we apply some hacks or techniques to tweak/create model specified to our objective then we can obtain better results than the deep/complex one.

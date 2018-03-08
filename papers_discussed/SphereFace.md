### Title 

Sphereface: Deep Hyperspace Embedding for face recognition 

### tl;dr

Comparison of numerous loss functions , and proposition of A-Softmax loss that enables the convolutional neural network to learn angular discriminative features .


### Describe the method

Typically the Face Recognition (FR) can be categorised as face identification and face verification. In terms of testing protocol, face recognition can be evaluated under closed-set or open-set settings, as illustrated in Fig. 1. For closed-set protocol, all testing identities are predefined in training set.

![open-close](https://raw.githubusercontent.com/Ramit-Pahwa/files/master/Screen%20Shot%202018-03-09%20at%201.55.55%20AM.png)

![Comparision](https://raw.githubusercontent.com/Ramit-Pahwa/files/master/Screen%20Shot%202018-03-09%20at%201.56.23%20AM.png)
### Functions

![original](https://raw.githubusercontent.com/Ramit-Pahwa/files/master/Screen%20Shot%202018-03-09%20at%201.58.24%20AM.png)

![Modified](https://raw.githubusercontent.com/Ramit-Pahwa/files/master/Screen%20Shot%202018-03-09%20at%201.58.30%20AM.png)

![angular](https://raw.githubusercontent.com/Ramit-Pahwa/files/master/Screen%20Shot%202018-03-09%20at%201.58.49%20AM.png)

#### Proposed Loss Function 
![A-softmax](https://raw.githubusercontent.com/Ramit-Pahwa/files/master/Screen%20Shot%202018-03-09%20at%201.58.55%20AM.png)

#### Properties of A-Sotmax Loss 

- Property 1: A-Softmax loss defines a large angular 
margin learning task with adjustable difficulty. With larger m, the angular margin becomes larger, the constrained region on the manifold becomes smaller, and the corresponding learning task also becomes more difficult.

##### (minimal m for desired feature distribution). Mmin is the minimal value.

- Property 2 :(lower bound of Mmin in binary-class case). In binary class case we have Mmin>=2+/3

- Property 3 (lower bound of mmin in multi-class case). Under the assumption that Wi , ∀i are uniformly spaced in the Euclidean space, we have Mmin ≥ 3.

### Discussions

#### Why angular margin?
 First and most importantly, angular margin directly links to discriminativeness on a manifold, which intrinsically matches the prior that faces also lie on a manifold. Second, incorporating angular margin to softmax loss is actually a more natural choice. As Fig. 2 shows, features learned by the original softmax loss have an intrinsic angular distribution. So directly combining Euclidean margin constraints with softmax loss is not reasonable.
#### Comparison with existing losses. 
In deep FR task, the most popular and well-performing loss functions include contrastive loss, triplet loss and center loss. First, they only impose Euclidean margin to the learned features (w/o normalization), while ours instead directly considers angular margin which is naturally motivated. Second, both contrastive loss and triplet loss suffer from data expansion when constituting the pairs/triplets from the training set, while ours requires no sample mining and imposes discriminative constraints to the entire mini-batches (compared to contrastive and triplet loss that only affect a few representative pairs/triplets).

### Training
![training](https://raw.githubusercontent.com/Ramit-Pahwa/files/master/Screen%20Shot%202018-03-09%20at%201.59.50%20AM.png)
### Concluding Remarks

This paper presents a novel deep hypersphere embedding approach for face recognition. In specific, we propose the angular softmax loss for CNNs to learn discriminative face features (SphereFace) with angular margin. A-Softmax loss renders nice geometric interpretation by constraining learned features to be discriminative on a hypersphere manifold, which intrinsically matches the prior that faces also lie on a non-linear manifold. This connection makes A-Softmax very effective for learning face representation. Competitive results on several popular face benchmarks demonstrate the superiority and great potentials of our approach



### My Two Cents 
- Angular Embedding seems promising in open FR problem, can be extended to identification of graphics/cartoons
- Metric Learning looks promising and be looked into. 

![metric learning ](https://raw.githubusercontent.com/Ramit-Pahwa/files/master/Screen%20Shot%202018-03-09%20at%202.31.25%20AM.png)
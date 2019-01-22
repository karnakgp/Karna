### Title

[Overcoming catastrophic forgetting in neural networks](https://arxiv.org/abs/1612.00796)

### tl;dr

The objective of the work is to design a methodology which can learn multiple tasks in a sequential manner on the same set of model parameters without having access to the previous task dataset while learning a new task. The challenge involved in this work is to reduce the rapid drop in performance of previously learned tasks(called catastrophic forgetting) while training a new one.

### Describe the method

In case of deep neural networks, when we train a model on some task A, the model parameters adapt themselves to optimize the loss L_A(θ). But, when we try to train a second task B after training our first task A on the same model, the parameters adapt themselves according to the new loss objective L_B(θ) and therefore losing its performance on task A.

To embed the neurobiological model of task-specific synaptic consolidation in neural networks, Kirkpatrick et al. has proposed an additional term L_pull(θ, θ^(∗)A) in the loss function L(θ) while training on task B, which ensures that important parameters stay close to their earlier values θ^(∗)A based on how important they are to the previously learned
tasks, resembling plastic nature of the synapses in the brain. The idea is clearly demonstrated in the figure below.

![img](https://www.pnas.org/content/pnas/114/13/3521/F1.large.jpg?width=800&height=600&carousel=1)

The importance of parameters w.r.t. the earlier task A is approximated as the diagonal of the Fisher information matrix F. The final loss function L that we minimize during training task B is:

![img](https://cdn-images-1.medium.com/max/800/1*tON3f3xLkaFswgOOzuUqng.png)

where L_B(θ) is the loss for task B only, λ sets how important the old task is compared to the new one, i labels each parameter and F_i in this case is the second derivative of the loss near a minimum(in this case θ^(∗)A_i wrt each parameter θ_i). When we have to train other tasks (say a new task C), we need to keep the network parameters close to both the task A and B by adding two separate penalties for both the tasks.

### Any further details

Another method to measure the importance of paramters wrt the earlier trained tasks is discussed in the paper below.
"Continual Learning Through Synaptic Intelligence" by Friedemann Zenke, Ben Poole, Surya Ganguli

### My two cents

I will add later.
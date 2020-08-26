# TITLE : Learning representations by Back-propagating errors

## tl;dr
This paper introduced the concept of back propagation for the first time. We compute the derivatives at each step and then store it to utilise it at the next pass of computation. We dynamically change the weights in order to arrive at the global minima more efficiently. This method, in short, adds an extra step of backward propagation to the existing step of forward propagation and is more efficient than Gradient descent. This was the first method which utilised the outputs obtained at the hidden layers.

## Describe the method
First of all we calculate the state of the neuron 

![.](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle+x_j+%3D+%5Csum_%7Bi%7D+y_%7Bi%7D+w_%7Bij%7D&bg=ffffff&fg=000&s=0)

Then we calculate the corresponding output of the neuron using any suitable activation function, for example the sigmoid activation function

![.](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle+y_j+%3D+%5Cfrac%7B1%7D%7B1+%2B+e%5E%7B-x_j%7D%7D&bg=ffffff&fg=000&s=0)

The error obtained is given as

![.](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle+E+%3D+%5Cfrac%7B1%7D%7B2%7D%5Csum_%7Bj%7D%28y_j+-+d_j%29%5E2&bg=ffffff&fg=000&s=0)

Where  is the computed output value of unit j in the output layer, and  is its desired state. Next we calculate the derivatives accordingly and then finally adjust the weights.

![.](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle+%5Cfrac%7B%5Cpartial+E%7D%7B%5Cpartial+y_j%7D+%5Cfrac%7B1%7D%7B2%7D%5Csum_%7Bj%7D%28y_j+-+d_j%29%5E2+%3D+y_j+-+d_j++&bg=ffffff&fg=000&s=0)

Then we can adjust the weights accordingly

![.](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle+%5CDelta+w+%3D+-+%5Cepsilon+%5Cfrac%7B%5Cpartial+E%7D%7B%5Cpartial+w%7D&bg=ffffff&fg=000&s=0)

## Any further details
The weights may converge slowly. In that case we can use an accelerated version of the algorithm. 

![.](https://s0.wp.com/latex.php?latex=%5Cdisplaystyle+%5CDelta+w%28t%29+%3D+-+%5Cepsilon+%5Cfrac%7B%5Cpartial+E%7D%7B%5Cpartial+w%7D%28t%29+%2B+%5Calpha+%5CDelta+w%28t-1%29&bg=ffffff&fg=000&s=0)

Here t is incremented in steps of 1.
Other than that, there might also be a very small probability that this gets stuck at a local minima in place of a global maxima. Well, the probability is extremely small and hence we initialise the weights randomly. Also we do not initialise all the weights as zeroes as that would be a futile attempt as all the derivatives would then be zeroes.
## My two cents
Back-propagation can be considered more fulfilling if combined with other methods. For instance, even in the present scenario we use club it with other algorithms. Other than that what I might suggest is that we can conduct several passes of the algorithm with different seeds of randomised initialisation so that the probability of getting stuck at a local minima is decreased even further.


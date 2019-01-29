# Title

Histograms of Oriented Gradients for Human Detection

# tl;dr

Making use of Histograms of Oriented Gradients for detecting humans in images. Makes use of Gamma normalisation, gradient computation (using a simple [-1, 0, 1] filter), making histograms having 9 bins, from 0 to 180 degress, generating feature vector and performing contrast normalisation on top of it. The final feature vector is classified in a binary fashion using linear SVM.

# Describe the method

## Architecture
An image is processed in the following manner.  

### Gamma normalisation
Square root operation is performed on the image, followed by a minmax normalisation

### Gradient computation

Gradient is computed horizontally and vertically using a [-1, 0, 1] filter and it's transposed. Again, this is followed by a minmax normalisation

### Binning

HOGs are genrated by placing angles in bins. No. of bins is set to 9, each 20 degrees apart. The paper instructs moving the values obtained at the edges to next bins, and this is achieved via bilinear interpolation.

### Contrast normalisation

The feature vectors are contrast normalised. L2-Hys, L2-norm and L1-sqrt
worked best using Bhattacharya distance. 64×128 detection window includes about 16 pixels of margin around the person on all four sides.

Then, classification is performed in a binary fashion using linear SVM.

## Dataset
MIT pedestrian database and INRIA dataset

# Any further details

[Bhattacharya Distance](https://en.wikipedia.org/wiki/Bhattacharyya_distance)

# My two cents

Making histograms from 0 - 360 degrees in place of 0 - 180 degrees helps making object detection algorithms instead of human detection.
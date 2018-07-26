### Title

You Only Look Once: Unified Real-time object detection

### tl;dr

  Goal of the algorithm: Detect objects in an image, and do it as fast as possible.

### Description

  YOLO divides the input image into an S×S grid.
  1. Each grid cell predicts only one object.
  2. Each grid cell predicts a fixed number of boundary boxes (specified beforehand)
  3. For each grid cell,
      + it predicts B boundary boxes
        + Each boundary box contains 5 elements: (x, y, w, h) and a box confidence score. The confidence score reflects how likely the box contains an object (objectness) and how accurate is the boundary box.
        + We normalize the bounding box width w and height h by the image
        width and height. x and y are offsets to the corresponding cell. Hence,
        x, y, w and h are all between 0 and 1.
      + it detects one object only regardless of the number of boxes B,
      + it predicts C conditional class probabilities (one per class for the likeliness of the object class).

  Loss function:
  
  YOLO uses sum-squared error between the predictions and the ground truth to calculate loss. The loss function composes of: 
  + the classification loss.
  + the localization loss (errors between the predicted boundary box and
the ground truth).
  + the confidence loss (the objectness of the box).

  Since two or more grid cells can detect the same object, YOLO applies non-maximal suppression to remove duplications with lower confidence.
  
  ### Any further details
  Benefits:
  + Fast. Good for real-time processing.
  + Predictions (object locations and classes) are made from one single network. Can be trained end-to-end to improve accuracy.
  + YOLO is more generalized. It outperforms other methods when generalizing from natural images to other domains like artwork.
  + YOLO accesses to the whole image in predicting boundaries. With the additional context, YOLO demonstrates fewer false positives in background areas.
  
### My two cents

Using techniques from other object-detection algorithms, like the usage of anchor boxes can improve performance as has been seen in the higher versions of YOLO.

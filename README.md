# IDAO 2021 First Stage 

Our solution for the first stage.



### Train Test Split 
The main difficulty of the problem was that the training classes and the classes for the final ranking wasn't the same.
Instead of doing a classic 80/20 split on the training data, we divided so we had unseen energy levels
and the two classes on the test set. For example : Train on [6,10,20,30]  and test on [1,3].


### Binary classification
We kept it quite simple, training with basic data augmentations.
To improve training speed we also cropped the images to 256x256.

We trained different models on different splits.
* Resnet18
* Resnext50
* squeezenet
* mobilenet_v2
* mobilenet_small

The final prediction used the harmonic mean of 40 of these.


### Regression
We used the same split strategy for the regression problem. That gave us quite bad MAE scores on test data, but
there was a separation between classes and the order was preserved. 

We used the Binary classification predictions to split the data. For each split we used Kmeans clustering on kev predictions
to make our three groups [1,6,20] & [3,10,30].

We used a blend of 15 models trained on different splits and used the mean to make our groups.

### Post processing
Our predictions were unbalanced and because we were quite confident about our Kmeans method to differentiate different
 levels of energy in one class, we thought that the unbalance was from classification errors.
 
For each energy level class with too much element we transferred exceeding elements to the other class.
We choose the elements with the lowest confidence in the binary classification. We then recompute our clusters.
We did that operation until all classes are ~ 2510 elements.


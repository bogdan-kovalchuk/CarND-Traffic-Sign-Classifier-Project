# **Traffic Sign Recognition** 


**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (links to the project [data set](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/traffic-signs-data.zip))
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[classes_img]: writeup_images/classes_images.png
[train_bar]: writeup_images/train_bar.png
[valid_bar]: writeup_images/valid_bar.png
[test_bar]: writeup_images/test_bar.png
[befor_grscl]: writeup_images/before_grayscl.png
[after_grscl]: writeup_images/after_grayscl.png
[5ts]: writeup_images/5ts.png
[image8]: ./examples/placeholder.png "Traffic Sign 5"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

You're reading it! and here is a link to my [project code](https://github.com/bogdan-kovalchuk/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Basic summary of the data set.

I used the python and numpy to calculate summary statistics of the traffic signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 1)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.
Here is an example of random image from each class:

![alt text][classes_img]

Here is an exploratory visualization of the data set. It is a bar charts showing how the data is distributed between classes for train/validation/test data sets.

![alt text][train_bar]
![alt text][valid_bar]
![alt text][test_bar]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I decided to convert the images to grayscale because it reduces the complexity of the calculations. Before we had (32, 32, 3) after grayscale we have (32, 32, 1).

To grayscale next formula was used:
```python
np.sum(original_image / 3, axis = 3, keepdims = True)
```
As a last step, I normalized the image data because it helps to avoid too big or too small values and so to avoid accumulating errors.

To normalise next code was used:
```python
a = 0.1
b = 0.9
grayscale_min = 0
grayscale_max = 255
return a + (((image_data - grayscale_min)*(b - a))/( grayscale_max - grayscale_min ))
```

Here is an example of a traffic signs images before and after grayscaling/normalising.

![alt text][befor_grscl]
![alt text][after_grscl]

#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.



My model based on LeNet pipeline and consiste of the following layers:

| Layer         		|     Description	        					|
|:---------------------:|:---------------------------------------------:|
| Layer 1         		|                               				|
| Input         		| 32x32x1 grayscale images      				|
| Convolution 3x3     	| 1x1 stride, valid padding, output = 28x28x6 	|
| Activation         	| Relu                                          |
| Max pooling         	| Input = 28x28x6, output = 14x14x6             |
|                       |                                               |
| Layer 2         		|                               				|
| Input         		| 14x14x6                       				|
| Convolution 3x3     	| 1x1 stride, valid padding, output is 10x10x16 |
| Activation         	| Relu                                          |
| Max pooling         	| Input = 10x10x16. Output = 5x5x16             |
| Flatten         	    | Input = 5x5x16. Output = 400                  |
|                       |                                               |
| Layer 3         		|                               				|
| Input         		| 400                       				    |
| Fully Connected       | Output = 120                                  |
| Activation         	| Relu                                          |
| Dropout            	|                                               |
|                       |                                               |
| Layer 4         		|                               				|
| Input         		| 120                       				    |
| Fully Connected       | Output = 84                                   |
| Activation         	| Relu                                          |
| Dropout            	|                                               |
|                       |                                               |
| Layer 5         		|                               				|
| Input         		| 84                       				        |
| Fully Connected       | Output is 43                                  |

#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used Adam Optimizer with learning rate = 0.001. Batch size is equal to 130 and the number of epochs is equal to 30 were used.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 1.000
* validation set accuracy of 0.952
* test set accuracy of 0.934

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][5ts]

The first image might be difficult to classify because ...

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			              |     Prediction	        			| 
|:---------------------------:|:-----------------------------------:| 
| Speed limit (30km/h)     	  | Speed limit (30km/h)  				| 
| No entry     			      | No entry 							|
| Speed limit (100km/h)       | Speed limit (100km/h)				|
| Go straight or right        | Go straight or right				|
| Dangerous curve to the left | Dangerous curve to the left      	|

The model was able to correctly guess 5 of the 5 traffic signs, which gives an accuracy of 100%. This compares favorably to the accuracy on the test set of 0.934.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 17th cell of the Ipython notebook.

For the first image, the model is relatively sure that this is a Speed limit (30km/h) (probability of 0.22), and the image does contain a Speed limit (30km/h). The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 21.933224	            | Speed limit (30km/h)                          |
| 16.71605	            | Speed limit (20km/h)                          |
| 5.169108	            | Speed limit (70km/h)                          |
| 0.7669643	            | Speed limit (50km/h)                          |
| 0.05123633            | Keep right                                    |

For the second image, the model is relatively sure that this is a No entry (probability of 0.36), and the image does contain a No entry. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 36.48883	            | No entry                                      |
| 11.203043	            | Stop                                          |
| 8.976204	            | Roundabout mandatory                          |
| 1.6796513	            | End of all speed and passing limits           |
| 0.7450319	            | Keep right                                    |

For the third image, the model is relatively sure that this is a Speed limit (100km/h (probability of 0.27), and the image does contain a Speed limit (100km/h. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 27.65406	            | Speed limit (100km/h)                         |
| 16.98507	            | Speed limit (80km/h)                          |
| 5.830722	            | Speed limit (120km/h)                         |
| 3.2366002	            | Speed limit (30km/h)                          |
| 2.0297427	            | Roundabout mandatory                          |

For the fourth image, the model is relatively sure that this is a Go straight or right (probability of 0.21), and the image does contain a Go straight or right. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 20.524559	            | Go straight or right                          |
| 8.717066	            | Keep right                                    |
| 4.3321085	            | End of all speed and passing limits           |
| -1.1727667	        | End of no passing                             |
| -2.173958	            | Children crossing                             |

For the fifth image, the model is relatively sure that this is a Dangerous curve to the left (probability of 0.13), and the image does contain a Dangerous curve to the left. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 12.558402	            | Dangerous curve to the left                   |
| 5.5321436	            | Slippery road                                 |
| 3.020726	            | Wild animals crossing                         |
| 2.9183102	            | Double curve                                  |
| 0.04683751	        | Bicycles crossing                             |

### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?



---
layout: post
title: Detection and Classification of Fish Species
---

Image processing using convolutional neural networks (or deep learning) was one of the key topics over the last couple of weeks in my Data Science boot camp at Metis in San Francisco. One of the major use cases for image processing is the classification of images, which has a broad range of applications. Examples include simple binary categorization tasks like deciding whether a given picture shows a cat or a dog and more advanced techniques helping to steer autonomous cars.

I was fascinated how these neural networks are able to classify images and therefore decided to play around with it for my last and final project at Metis.

# 1. Goal of the Project
  
Shortly before the start of this project, there was a new [competition](https://www.kaggle.com/c/the-nature-conservancy-fisheries-monitoring) posted on Kaggle which aims to detect and classify fish into 8 categories based on images taken on fishing vessels. The competition is sponsored by the Nature Conservancy in an effort to fight overfishing through efficient monitoring of fishing activities.

As my main goal for this project was to get to know the techniques of convolutional neural networks, I decided to give it a shot. Please see below for a couple of those fishing vessel pictures to get an idea of the task.

[Example 1: Albacore](/images/Final/ALB.jpg){:class="img-responsive"}  
[Example 2: Big eye tuna](/images/Final/BET.jpg){:class="img-responsive"}  
[Example 3: No fish](/images/Final/NoF.jpg){:class="img-responsive"}  
[Example 4: Other](/images/Final/Other.jpg){:class="img-responsive"}  
[Example 5: Shark](/images/Final/shark.jpg){:class="img-responsive"}  

# 2. Approach: Two step convolutional neural network
  
In short my approach works as follows:

* Preprocess all images into the same size
* Use a pre-trained VGG-16 model with regression head in Keras to localize the fish in the image
* Crop a rectangle containing the fish out of the image
* Use another pre-trained VGG-16 model with classification head in Keras to classify the fish using the cropped image

The first step is easy to accomplish using some basic functions (mainly resize) from OpenCV.

The last classification step uses the deep neural network "VGG-16" with weights trained on ImageNet. This network has performed very well on classifying images. For a detailed description see this [Blog](https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html). The basic idea is to download the weights for the convolutional and RELU layers from a pretrained model. Then use a training set of images from Kaggle to train the dense layers (Classification Head).

The second step is very similar to the last one only that it replaces the classification head with a regression Head using the following code: 

```model = Sequential()  
model.add(Flatten(input_shape=[512,2,5]))  
model.add(Dense(256, activation='relu'))  
model.add(Dropout(0.5))  
model.add(Dense(4, init='normal'))  
model.load_weights('help_files/bottleneck_fc_model.h5')  
model.compile(optimizer='adam', loss='mean_squared_error')
```

For more details, lecture 8 from this [Stanford class](https://archive.org/details/cs231n-CNNs) is a good resource.

The third step is once again an easy python exercise using OpenCV. For a graphical overview of the approach please see below.

[The Approach](/images/Final/Process.png){:class="img-responsive"}

# 3. Results

My algorithm achieved an accuracy of 0.5 on a test set and a log-loss of 1.5 on another unknown test set on Kaggle. There are other submissions scoring better than mine which made me think of potential ways to improve the approach. 

[The Results](/images/Final/Results.png){:class="img-responsive"}

# 4. Potential Next steps

I have several ideas on how to improve my scores which I might try in the future. These include:

* Increase the image resolution and run the algorithm on AWS
* Change the parameters in both convolutional neural nets, particularly the categorization one as this performed worse than the localization one
* Manually tag more fish locations in the training images to increase the number of training pictures
* Train a whole new neural net instead of using the pretrained VGG-16 model
* ...



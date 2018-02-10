---
layout: post
title: Building iceberg classification models with Keras
---

*This is my capstone project #1 in Springboard. And this is a on-going [Kaggle competition](https://www.kaggle.com/c/statoil-iceberg-classifier-challenge) when I worked on this project.*

Here is the link for the full report: [Link](https://github.com/yaqiongz/aws/blob/master/FinalReport/FinalR_AWS.ipynb)

---

There are so many new images generated every second, from the smart phones by our hand and from the radar in the sky. Letting a computer understand images is useful, yet not easy. It's interesting to recall the moments we teach our children apples and pears from image cards. Human's brains are certainly different, but they also need some teaching efforts. Now, is it the time to teach the computer about the contents in an image?

In this kaggle chanllenge project, the goal is to let the computer tell icebergs and ships from remote sensors' image data. The teaching materials is a training dataset contains about 1600 image data. Those data are marked with labels telling us if a image contains an iceberg or not. After building a predictive model using the training data, we will test the model with new images (about 8000), and the performance of the prediction will be evaluated. The result is evaluated by log loss.

### 1. Let's first take a look at how the training dataset looks like:

Each row in the json file contains a sample. There are two image bands for one sample, band_1 and band2. Actually, it's common to see image data have multiple bands. Colored images contains three bands: R, G and B bands. Here, band_1 is HH channel of a radar, which means transmit and receive horizontally polarized signal. Band_2 is HV channel of the radar, which means transmit horizontally polarized signal and receive vertially polarized signal. The difference between two bands contains important information on the object in the image since they can reflect the energy of the signal differently.

Good examples are shown here: Picture:

### 2. Data wrangling 

The data wrangling is fairly easy this time. The data is preprocessed and cleaned, except there are missing data in the inc_angle column.

Inc_angle column contains information about the incident angle that the image was taken. I attempted to use the inc_angle column in my model, but really no difference I can find in the results. This is also a hot topic in the discussion section of the competition, and it includes some data leakages. For my data science project, I would like to focus on the basics. So I will just skip this column.

Then, I reshaped the band_1 and the band_2 data to a 75x75 array, and I created a band_3 using the average of the band_1 and the band_2. I create the third band because later in the transfer learning, the pretrained VGG16 model takes input data with three channels.

Another trick I found while doing this image classification problem is *imageargumentation*. This means we can play with the image a little bit, such as rotate, magnify and shift the image to get more training images. I keras, there is a build in function named *ImageDataGenerator* to do the task. Before that, I have done some image rotation myself and combined all the rotated and unrotated images together as the training dataset. And I noticed an improvement of testing result by doing this (score from 0.3xx to 0.2xx).

### 3. Basic Convnet with Keras
**What is Convnet?** Convolutional neural networks (convnet) are neural networks that share their weigths across the space. This means that convnet scan images with small patches and each patch has the same weights. As the network grow, the height of the image gets bigger and the length and width of the image get smaller. It works quite well for image data since it catches small features in the image from those patches, which can be important for image classifying problems.  

The details of how convnet works can find in this video: [Udacity Deep Learning](https://www.youtube.com/watch?v=jajksuQW4mc)

Typically, there are a bunch of things need to be determined when building the Convent:
1. The architecture: I have found a [simple architecture](https://www.kaggle.com/toregil/welcome-to-deep-learning-cnn-99) that works well with MNIST. There are several different layers here:
   - *Conv2D layer:* convolutional neural network layer, have 64 filters and each filter have 3x3 = 9 weights. The weight of the filters will be shared across the whole image.
   - *Maxpooling layer:* a pooling layer is used to reduce the representation to reduce the computational load, as well as getting more robust results. Maxpooling is one kind of pooling method which takes the max of a certain region.
   - *[BatchNormalization layer](https://arxiv.org/abs/1502.03167):* one of the regularization method. Allow fast training rate and less careful about initialization.
   - *Drop out layer:* one of the regularization method. It randomly drop a portion of the weights to zero to avoid overfitting.

2. The hyperparameters
   - batch size
   - epoch
   - activation function in Convnet layer: ReLU (rectified linear unit), most commonly used.
   - optimizer of gradient descent: chosen Adam, tried SGD as well.
 
 Here is the training result of runing 30 epoch of this model: Picture: 


## 4. Transfer learning with VGG16
Training a new Convnet can be time_consuming, and there are a lot of hyperparameters to be decided as well as the architecture of it. Transfer learning, which means using the already trained Convnet weights, and adjust it to solve this problem. This is useful because those pretrained covnet contains features captures which maybe useful in this problem. VGG16 is a well known pre-trained convnet which won 2014 ImageNet challenge.
I followed the tutorial from [keras](https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html) to build my model. The score I got from this model is 0.35x. 

## 5. Cross validation and [hyperparameter tuning](https://github.com/yaqiongz/aws/blob/master/vgg16-finetuning.ipynb)
Cross validation (cv) 




## 6. Limitation and future work



### Acknowledgements: 
I would like to thank my mentor Aaron for his help, such as giving me suggestions what model to use, explaining CV to me, helping connecting with AWS and giving me suggestions for future work (building machine learning framework, building data pipeline, connecting with available API, etc.). 

I also want to thank the community in Kaggle. The most important part of my learning so far is reading and understing others' code. For example, some of the code are built nicely with structured functions, and some of them have used neat sytax for a perticular line of code. I wouldn't build my python skills as fast if I didn't read those nice posts in Kaggle kernels.

 

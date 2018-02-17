---
layout: post
title: Building iceberg classification models with Keras
---

*This is my capstone project #1 in Springboard. And this is a on-going [Kaggle competition](https://www.kaggle.com/c/statoil-iceberg-classifier-challenge) when I worked on this project.*

Here is the link for the full report: [Link](https://github.com/yaqiongz/Iceberg1/blob/master/FinalR_AWS.ipynb)

---

There are so many new images generated every second, from the smart phones in our hands and from the radar in the sky. Letting a computer understand images is useful, yet not easy. It's interesting to recall the moments we teach our children apples and pears from image cards. Human's brains are certainly different, but they also need some teaching efforts. Now, let's see how to teach a computer about the contents in an image.

In this kaggle challenge project, the goal is to let the computer tell icebergs from ships using remote sensors' image data. The teaching materials is a training dataset that contains about 1600 samples of image data. Each sample is marked with a label telling us if an image contains an iceberg or not. After building a predictive model using the training data, we will test the model with new images (about 8000), and the performance of the prediction will be evaluated. The result is evaluated by log loss, which penalized heavily on wrong predictions. The goal of this competition is to increase the model's power of prediction(minimize log loss).

In this blog, I will talk about my approach for building a basic convolutional neural network (convnet) model based on Keras. The mathematical details of a convnet is complex, and there are a lot of tricks when training a convnet. Here I will focus only on building and understanding a basic convnet. More detailed work will not be talked here but can be found in my github repository [Link](https://github.com/yaqiongz/aws/blob/master/FinalReport/FinalR_AWS.ipynb).

### 1. The training dataset looks like:

From kaggle, I downloaded two .json files. The first file is the train.json file, the training data. Each row in it contains a sample. There are two image bands for one sample, band_1 and band_2. Actually, it's common to see image data have multiple bands. Colored images contain three bands: R, G and B bands. Here, band_1 is the HH channel of a radar, which means transmit and receive horizontally polarized signal. Band_2 is the HV channel of the radar, which means transmit horizontally polarized signal and receive vertically polarized signal. The difference between the two bands contains important information on the object in the image since they can reflect the energy of the signal differently.

Good examples are shown here: [Link](https://www.kaggle.com/c/statoil-iceberg-classifier-challenge#Background) An iceberg's HV channel does not contain as much signals as from it's HH channel. But for a ship, it's HH and HV channel have similar amount of signal intensity. This is because the metal in ships, which has energy transfer from H polarization to V polarization when backscattering happens on its surface. This is one thing we can take into consideration when doing the classification task. The other thing is the shape of the object. Ship usually have clear edges and may have a rectangular shape. 

### 2. Data wrangling 

The data wrangling is fairly easy this time. The data is preprocessed and cleaned, except there are missing data in the inc_angle column.

Inc_angle column contains information about the incident angle that the image was taken. However, I didn't find this angle information helpful in my convnet at this stage. This is also a hot topic in the discussion section of the competition since it contains some data leakages. For this data science project, I would like to focus on the basics. So I will just skip this column.

One of the first time I did was reshaped the band_1 and the band_2 data to a 75x75 array, and created a band_3 using the average of the band_1 and the band_2. I create the third band because later in the transfer learning, the pretrained VGG16 model takes input data with three channels. So I just created a third band for convenience.

A simple but useful trick I found while doing this image classification problem is *imageargumentation*. This means we can play with the image a little bit, such as rotate, magnify and shift the image to get more training images. In keras, there is a build in function named *ImageDataGenerator* to do the task. Before that, I have done some image rotation myself and combined all the rotated and unrotated images together as the training dataset. And I noticed an improvement of testing result by doing this (log loss score from 0.35xx to 0.23xx).

### 3. Basic Convnet with Keras
**What is Convnet?** Convolutional neural networks (convnet) are neural networks that share their weights across the space. This means that convnet scans images with small patches and each patch has the same weights. As the network grows, the height of the image gets bigger and the length and width of the image get smaller. It works quite well for image data since it catches small features in the image from those patches, which can be important for image classifying problems. This video explains the concept very well: [Udacity Deep Learning](https://www.youtube.com/watch?v=jajksuQW4mc)

Typically, there are a bunch of elements that need to be determined when building a Convent:

**The architecture:** At the beginning, I was trying to find some clues about how to design an architecture for a specific problem. Frustratingly, I have not found anything practical rules yet. The best advice I get was to learn from what architecture already worked for certain problems and then optimize it. This simple [architecture](https://www.kaggle.com/toregil/welcome-to-deep-learning-cnn-99) that works well with MNIST, which is a handwriting digital image database, maybe a good one to start with.

There are several different layers here:
   - *Conv2D layer:* convolutional neural network layer, has 64 filters and each filter has 3x3 = 9 weights. The weight of the filters will be shared across the whole image.
   - *Maxpooling layer:* a pooling layer is used to reduce the representation to reduce the computational load, as well as getting more robust results. Maxpooling is one kind of pooling method which takes the max of a certain region.
   - *[BatchNormalization layer](https://arxiv.org/abs/1502.03167):* one of the regularization method. Allow fast training rate and less careful about initialization. Normalizes the weights so it helps with exploding gradients.
   - *Drop out layer:* one of the regularization method. It randomly drops a portion of the weights to zero to avoid overfitting.

**The hyperparameters**
   - batch size: to save computational resources, not all the samples are used during the training process. Batch size is how many samples are trained at the same time.
   - epoch: how many times there are for an all-sample training.
   - activation function in Convnet layer: ReLU (rectified linear unit), most commonly used.
   - optimizer of gradient descent: chosen Adam, tried SGD as well. Adam is a optimization method present not long ago (in 2015) and it's an extension to traditional stochastic gradient descent (SGD). Adam works well in this project while the traditional SGD have times that the score of the model keep the same (not improving at the beginning). I found this more obviously when I use transfer learning with VGG16. This blog explains what is Adam well [link](https://machinelearningmastery.com/adam-optimization-algorithm-for-deep-learning/).
 
 
Here is layers of the this convent:  IMG1

Here is the training result of running 30 epoch of this model: IMG2

## 4. Conclusion
By using this simple convnet combined with 90 degree and 180 degree rotated images, I get my score 0.2331 in private Leaderboard which is pretty good. The disadvantage of this model is that the training result fluctuate a lot as you can see from IMG2. And I spent time adding cross validation methods and conducting hyperparameter tuning (epoch, learning rate of optimizer, batch size, etc.). But the improvement of using an optimized hyperparameters is not obvious and I didn't find the way to minimize the fluctuation.

I then tired transfer learning with VGG16. The result from my transfer learning is not as good as this simple model and I doubt I missed something in it. 


## Notes for Transfer learning with VGG16
Training a new Convnet can be time_consuming, and there are a lot of hyperparameters to be decided as well as the architecture of it. Transfer learning, which means using the already trained Convnet weights, and adjust it to solve this problem. This is useful because those pretrained covnet contains features captures which maybe useful in this problem. VGG16 is a well-known pre-trained convnet which won 2014 ImageNet challenge.
I followed the tutorial from [keras](https://blog.keras.io/building-powerful-image-classification-models-using-very-little-data.html) to build my model. The score I got from this model is 0.350. 

## Notes for Cross validation and hyperparameter tuning
I did cross validation for hyperparameter tuning [link](https://github.com/yaqiongz/aws/blob/master/vgg16-finetuning.ipynb). In the future, I think it's useful to add cross validation for ensembling the results just like the author did in this kernel [link](https://www.kaggle.com/jirivrany/my-best-single-model-simple-cnn-lb-0-1541).


## Suggestion for first time kaggler
1. Learn from other's solution and other's best single model. Especially after the competition, there will have solutions coming out from top rankers.
2. Write well-structured code and consider making a machine learning framework that can be reused for future competitions.
3. While practicing in kaggle, keep in mind that the goal of solving Kaggle problem is DIFFERENT from solving real-life problems. In kaggle, we are so focused on improving the performance of the model. But in real-life, the first important thing is to find out where is valuable question to ask, and then comes to the best way to solve it. The process of finding the right and doable question may takes more effort. And of course, real-life data won't be as clean as the data in kaggle.


### Acknowledgements: 
I would like to thank my mentor Aaron for his help, such as giving me suggestions what model to use, explaining CV to me, helping connecting with AWS and giving me suggestions for future work (building machine learning framework, building data pipeline, connecting with available API, etc.). 

I also want to thank the community in Kaggle. The most important part of my learning so far is reading and understing others' code. For example, some of the code are built nicely with structured functions, and some of them have used neat sytax for a perticular line of code. I wouldn't build my python skills as fast if I didn't read those nice posts in Kaggle kernels.

 

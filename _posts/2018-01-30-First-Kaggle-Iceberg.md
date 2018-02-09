---
layout: post
title: Building iceberg classification models with Keras
---

*This is my capstone project #1 in Springboard. And this is a on-going [Kaggle competition](https://www.kaggle.com/c/statoil-iceberg-classifier-challenge) when I worked on this project.*

Here is the link for the full report: [Link](https://github.com/yaqiongz/aws/blob/master/FinalReport/FinalR_AWS.ipynb)

---

There are so many new images generated every second, from the smart phones by our hand and from the radar in the sky. Letting a computer understand images is useful, yet not easy. It's interesting to recall the moments we teach our children apples and pears from image cards. Human's brains are certainly different, but they also need some teaching efforts. Now, is it the time to teach the computer about the contents in an image?

In this kaggle chanllenge project, the goal is to let the computer tell icebergs and ships from remote sensors' image data. The teaching materials is a training dataset contains about 1600 image data. Those data are marked with labels telling us if a image contains an iceberg or not. After building a predictive model using the training data, we will test the model with new images (about 8000), and the performance of the prediction will be evaluated. 

### 1. Let's first take a look at how the training dataset looks like:

Each row in the json file contains a sample. There are two image bands for one sample, band_1 and band2. Actually, it's common to see image data have multiple bands. Colored images contains three bands: R, G and B bands. Here, band_1 is HH channel of a radar, which means transmit and receive horizontally polarized signal. Band_2 is HV channel of the radar, which means transmit horizontally polarized signal and receive vertially polarized signal. The difference between two bands contains important information on the object in the image since they can reflect the energy of the signal differently.

Good examples are shown here: Picture:

### 2. Data wrangling 

The data we get from this kaggle competition is fairly easy. The data is preprocessed and cleaned, except there are missing data in the inc_angle column.

Inc_angle column contains information about the incident angle that the image was taken. I have attempted to use the inc_angle column in my model, but really nothing significant difference I can find in the results. This is also a hot topic in the discussion section of the competition, and it includes some data leakages. For my first capstone project, I would like to focus on building the bacis model. So, I will just skip this column.

So, I reshape the band_1 and band_2 data to a 75x75 array, and I created band_3 using the average of band_1 and band_2. I create the third band because later in the transfer learning, the pretrained VGG16 take input with three channels.

Another trick I found while doing this image classification problem is *imageargumentation*. This means we can play with the image a little bit, such as rotate, magnify and shift the image to get more training images. I found there is a buildin function in Keras to do the task. Before that, I have done image rotation myself and combined all the rotated and unrotated images together to train the data. And I did notice an improvement of testing result by doing this.

### 3. Basic Convnet with Keras
**What is Convnet?** Convolutional neural networks (convnet) are neural networks that share their weigths across the space. This means that convnet scan images with small patches and each patch has the same weights. As the network grow, the height of the image gets bigger and the length and width of the image get smaller. It works quite well for image data since it catches small features in the image from those patches, which can be important for image classifying problems.  

The details of how convnet works can find in this video: [Udacity Deep Learning](https://www.youtube.com/watch?v=jajksuQW4mc)

Typically, there are a bunch of things need to be determined when building the Convent:
1. The architecture: I have found a [simple architecture](https://www.kaggle.com/toregil/welcome-to-deep-learning-cnn-99) that works well with MNIST. There are several different layers here:
   - Conv2D layer: this 
   - Maxpooling layer:
   - BatchNormalization layer:

2. The hyperparameters
   - batch size
   - 





### Acknowledgements: 
I would like to thank my mentor Aaron for his help, such as giving me suggestions what model to use, explaining CV to me, helping connecting with AWS and giving me suggestions for future work (building machine learning framework, building data pipeline, connecting with available API, etc.). 

I also want to thank the community in Kaggle. The most important part of my learning so far is reading and understing others' code. For example, some of the code are built nicely with structured functions, and some of them have used neat sytax for a perticular line of code. I wouldn't build my python skills as fast if I didn't read those nice posts in Kaggle kernels.

 

---
layout: post
title: Building iceberg classification models with Keras
---

*This is my capstone project #1 in Springboard. And this is a on-going [Kaggle competition](https://www.kaggle.com/c/statoil-iceberg-classifier-challenge) when I worked on this project.*

Here is the link for the full report: [Link](https://github.com/yaqiongz/aws/blob/master/FinalReport/FinalR_AWS.ipynb)


There are so many new images generated every second, from the smart phones by our hand and from the radar in the sky. Letting a computer understand images is useful, yet not easy. It's interesting to recall the moments we teach our children apples and pears from image cards. Human's brains are certainly different, but they also need some teaching efforts. Now, is it the time to teach the computer about the contents in an image?

In this kaggle chanllenge project, the goal is to let the computer tell icebergs and ships from remote sensors' image data. The teaching materials is a training dataset contains about 1600 image data. Those data are marked with labels telling us if a image contains an iceberg or not. After building a predictive model using the training data, we will test the model with new images (about 8000), and the performance of the prediction will be evaluated. 

Let's first take a look at how the training dataset looks like:

Each row in the json file contains a sample. There are two image bands for one sample, band_1 and band2. It's common to see image data have multiple bands. Usually, colored images contains three bands: R, G and B bands. Here in this radar




### Acknowledgements: 
I would like to thank my mentor Aaron for his help, such as giving me suggestions what model to use, explaining CV to me, helping connecting with AWS and giving me suggestions for future work (building machine learning framework, building data pipeline, connecting with available API, etc.). 

I also want to thank the community in Kaggle. The most important part of my learning so far is reading and understing others' code. For example, some of the code are built nicely with structured functions, and some of them have used neat sytax for a perticular line of code. I wouldn't build my python skills as fast if I didn't read those nice posts in Kaggle kernels.

 

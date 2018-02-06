---
layout: post
title: How to setup an AWS EC2 instance
---


In the Kaggle iceberg classification project, I trained several convolutional neural networks in my laptop. In those trainings, each epoch would take serval minutes and training a model with 30 epoches and 10 fold cross validation would take many hours. So I decided to go to amazon web service (AWS) EC2 and lauch a GPU instance.

#### Lauch AWS instance
1. Register in AWS and login into AWS. Students can signup in AWS education for student credit.

2. From the top menu select "Services" and Click "EC2", then click "Launch Instance".

3. In the "Choose an Amazon Machine Image (AMI)", choose AWS Marketplace. Search for an AMI which is good for your application. For me, I chose "Deep learning AMI (Ubuntu)", since it already have the packages I wanted (tensorflow, Keras, jupyter notebook).

4. Follow the steps to launch the AWS instance. At the first step, I choose p2.xlarge instance type, which cost $0.9 per hour of runing. 

5. Once the instance is launched, it will be shown in the list. And in the "Actions" button, i can stop, start and terminate the instance. After each use, remember to stop the instance to avoid charge. But if I terminate the instance, the instance I created won't be found again in the list and all the content in the instance will be deleted.

#### Connect with AWS instance using ssh
I have a Mac, and it's fairly simple to connect with terminal. Not sure how to connect in a windows. But I've heard there is a software can be used to connect.
1. Make sure the instance is runing. 
2. Open a terminal shell and type: ssh -i ~/.ssh/path/to_ssh_key username@ec2 host. Alternatively, I click the "connect" botton and get the ssh command from the instruction: ssh -i "OhioDL.pem" ubuntu@ec2-18-218-218-27.us-east-2.compute.amazonaws.com for connecting purpose.
3. After connected with AMI, run: jupyter notebook --no-browser (setup a password for jupyter notebook when use it at the first time or jupyter notebook will reminder you later).

4. Open up a local terminal shell on your personal.
5. Run : ssh -i ~/.ssh/path/to_ssh_key -NL 8157:localhost:8888 username@ec2 host
6. Open up a browser on your personal local machine.
7. Put http://localhost:8157 in your browser and now you have jupyter notebook running on your local machine pointing to your home directory on your EC2 instance! password

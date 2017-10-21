#**Behavioral Cloning** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./images/image4.png "Bright Image"
[image2]: ./images/imaage7.png "Flipped Image"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
###Files Submitted & Code Quality

####1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md or writeup_report.pdf summarizing the results

####2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

####3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works. 
The model I used was the end to end deep learning model used by Nvidia which is described in the enxt section.

###Model Architecture and Training Strategy

####1. An appropriate model architecture has been employed

My model architecture used is described as follows:

1. Input layer where images of size (160, 320) are used. Here I normalized the images from -0.5 to 0.5 and added a cropping layer which cropped the images to certain extent which made my model to learn faster and in a proper manner.

2. Next I used a convolution layer of filter size (5x5) having 24 neurons and added relu activation as well with subsampling of (2x2).

3. Next I used a convolution layer of filter size (5x5) having 36 neurons and added relu activation as well with subsampling of (2x2). 

4. Next I used a convolution layer of filter size (5x5) having 48 neurons and added relu activation as well with subsampling of (2x2).

5. Next I used a convolution layer of filter size (3x3) having 64 neurons and added relu activation as well.

6. Next I used a convolution layer of filter size (3x3) having 64 neurons and added relu activation as well.

7. Next I added a flatten layer.

8. The flatten layer was connected with a dense layer of 100 neurons.

9. Next a dense layer of size 50 neurons was used.

10. Next a dense layer of size 10 neurons was used and then another dense layer of one neuron predicting the steering angle used for predicting the driving.

####2. Attempts to reduce overfitting in the model

To prevent overfitting I added dropout layers at certain positions in the network to ensure the model learned properly.
Also I ensured I collected enough data from the simulator and through augmentation to enable my model learn properly and perform well on the validation data.

####3. Model parameter tuning

1. I used an adam optimizer so that I didnt have to manually tune the learning rate and used mean square loss function.
2. The number of epochs used were 4.

####4. Appropriate training data

Collecting training data was the crucial part of this project.

I collected around 4 laps of data from the simulator where the first two laps were center lane driving while the rest two were driving along the left and right lane of the road.
 
Apart from that I used image augmentation to gather extra data which helped my model learn better and perform extremely well on validation data as well.

###Model Architecture and Training Strategy

####1. Solution Design Approach

Initially I went with the LeNet architecture however it seemed to overfit the dataset and the car didnt perform well on the curves of the track.
Then I used the well known end to end neural network created by nvidia. This seemed a better idea where the overall architecture gave promising results on the driving.

After deciding the model architecture, I decided to go with the training data consisting of only the lap data I recorded. However after trying 6-7 times which different scenarios like collecting more edge case data along the curves or doing center lane driving for most part of the dataset, it seemed a bad idea to go with as the car didnt perform well on the curves again.

It meant I had to augment the dataset which covered different scenarios like brightness factor, shadow factor, horizontal and vertical shifts etc. which gave promising results. The reason I can come up with this is with augmenting I am making the car cover all cases and learn driving in a more general way instead of specific to that particular track.

After collecting with 4 laps of data aand augmenting the dataset I tested my network to around 60K images which were enough to make the car drive well on the track. The images fed to the network were normalized between -0.5  to 0.5 and cropped so that the car could learn in a better way.

####2. Final Model Architecture

My model architecture used is described as follows:

1. Input layer where images of size (160, 320) are used. Here I normalized the images from -0.5 to 0.5 and added a cropping layer which cropped the images to certain extent which made my model to learn faster and in a proper manner.

2. Next I used a convolution layer of filter size (5x5) having 24 neurons and added relu activation as well with subsampling of (2x2).

3. Next I used a convolution layer of filter size (5x5) having 36 neurons and added relu activation as well with subsampling of (2x2). 

4. Next I used a convolution layer of filter size (5x5) having 48 neurons and added relu activation as well with subsampling of (2x2).

5. Next I used a convolution layer of filter size (3x3) having 64 neurons and added relu activation as well.

6. Next I used a convolution layer of filter size (3x3) having 64 neurons and added relu activation as well.

7. Next I added a flatten layer.

8. The flatten layer was connected with a dense layer of 100 neurons.

9. Next a dense layer of size 50 neurons was used.

10. Next a dense layer of size 10 neurons was used and then another dense layer of one neuron predicting the steering angle used for predicting the driving.


####3. Creation of the Training Set & Training Process

To create a dataset from which the car could learn the driving rules, I performed the following points:

1. Firstly I stored 4 laps of data where the first two laps were center lane driving while the other two were right lane and left lane driving respectively. The reason being I have to train the car for all scnearios, in cases when the car pulls towards the right or left, it has to know where to turn the steer inorder to regain the center position.

2. Next important thing was image augmentation. I tried all possible scenarios where I tested the car on just the lap data but it failed on curves no matter how much laps I recorded. I then understood why image augmentation was important, I didnt cover cases where the brightness factor might come into play or shadow might occur on certain part of image etc. The augmenting process I followed was as follows:
	a. Brightness factor
	b. Shadow factor
	c. Flipping of image
	d. Left and right images from simulator
	e. Horizontal and vertical shifts on image

Example of brightness factor on image: ![Bright Image][image1]
Example of flipped image: ![FLipped Images][image2]

3. I also tried making the car learn through the data of second track but somehow that didnt work. I am trying on how to make it work and perform well on second track.

4. Lastly I ensured the data I passed to fit() function in Keras was shuffled inorder to prevent overfitting or underfitting of the network.

# AI-on-the-edge-quick-train

This is a fork of https://github.com/jomjol/neural-network-autotrain-digital-counter which itself is used to train digital and analog models for https://github.com/jomjol/AI-on-the-edge-device. This fork is essentially a stripped down version focused on getting your very own custom model up and running as fast as possible. This assumes you already have AI-on-the-edge-device up and running, with correct ROIs setup and have used the out-of-the-box AI models at least once.

# Installation

You'll need python, of course, and Jupyter installed.

Download this repository and install all dependancies with

```pip install -r requirements.txt```

# Usage (Not my first AI rodeo)

* Collect and download training images directly from AI-on-the-edge-device
* Drag and drop the ```.jpg``` files into a corresponding labeled subfolder, e.g. ```/raw_digits/1/``` for all the images containing the digit ```1```
* Run ```02 - Digital_Image_Preparation.ipynb``` and then ```03 - Train_CNN_Digital-Readout-Small-v3.ipynb```
* Upload the ```dig-fitted.tflite``` to AI-on-the-edge-device. Tweak as necessary.

# Detailed Usage (I'm new to AI)

## Get ready to train your model

You are probably here because the out-of-the-box AI is underperforming. No problem, we're going to teach it how to recognize your counters specifically. This is especially usefull if your camera or lighting is less than ideal. But to train a computer vision AI, you need to collect some images first! You'll find all the detailed infromation for doing so at https://jomjol.github.io/AI-on-the-edge-device-docs/collect-new-images/. In short:

* On your AI-on-the-edge-device web dashboard, enable "ROI Images Location" etc. for the desired type of AI model under Settings > Configuration > ... ROI Processing
* Under System > File Server ... you'll find the path to the images selected in the previous step. Download images that represent different examples of the types of recognitions.
    * A "digits" model recognizes whole digits. A "1" is a "1", a "2" is a "2", etc. So you'll want at least one copy of each digit to teach the AI what each one of your digits looks like, though a few more examples of pictures will help your AI figure things out when inevitably your picture isn't perfect. About 50 images in total is probably all you need, though more can't hurt.
    * An "analog" model recognizes a dial continously (readings like "7.2..."). This is more advanced AI model that will require more data, I recommend trying to train the digits first, if possible.

## Fix the training data

The downloaded images will need to be relabeled, because the AI didn't do a great job right out of the gate (that's why you are here, no?). File Explorer or some other file manager GUI makes this job simple. Just look at the images and drag and drop the files in the appropriate folder. Ignore the filename, it doesn't matter.

* For a "digits" model, drag all the ```.jpg``` that have the number ```1``` in the picture into ```/raw_digits/1/``` folder, and all those with the number ```2``` in the picture into ```/raw_digits/2/```. Since you are training a custom model, you probably don't have to deal with ```N``` recognitions, since your detections will probably be perfect 99% of the time. Nevertheless, you can attempt to train for N. Place ```.jpg``` files of unrecognizable images into the ```/raw_digits/N/``` folder.
* For an "analog" model, drag all the ```.jpg``` into at most 2 subdir of digits, so for example the image of a dial measuring 7.2 goes into ```/raw_analog/7/2``` folder. You'll need 10 times as many images for simialr accuracy as digits.


## Run the training

* Run ```02 - Digital_Image_Preparation.ipynb``` or ```02 - Analog_Image_Preparation.ipynb```, as the case may be. This will copy the images, appropriatly resized and ready for training.
* Run ```03 - Train_CNN_Digital-Readout-Small-v3.ipynb``` or ```03 - Train_CNN_Analog-Readout_Version-Small2.ipynb```, as the case may be. This will perform the training. Check the results at the bottom. It should look something like this to get good results:

![0b88e420-ad8b-4b51-883b-d2eaf71f943b](https://github.com/user-attachments/assets/768197ca-8af0-4203-897a-219b2b4951e6)

![71c4f356-5e61-4a57-b9b5-eddfe28dec63](https://github.com/user-attachments/assets/9ca9e47e-cf7c-48d7-9d44-8f7b027d8491)


## Use the model

* Upload the model to the ```/config/``` fir in System > File Server. After upload, the model will be selectable on the configuration page.
* Watch the performance for a day or two. Download any images that failed to properly detect, and rerun the [Fix the training data] and [Run the training] steps again

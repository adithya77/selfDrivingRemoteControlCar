**Selfdriving RC Car**

In this project I've used relays to control the car which is not a very good idea but it is easy for me to start with as I don't have great understanding of the controller in that RC car.

Below is the car I've bought in amazon and opened it and connected it to raspberry pi using relay switch.
https://www.amazon.in/AJUDIYA-ENTERPRISE-Rechargeable-Crawler-Monster/dp/B081X18S33

Later built the complete car from scratch controlled by Raspberry PI on the below chasis module
https://www.amazon.in/gp/product/B07D5T2DN3

---

## Prerequisites in the Raspberry PI

1. python3
2. opencv2
3. tensorflow
4. keras

## GPIO Config
Below is the GPIO PINS configuration
| Direction | PIN |
| ----------- | ------|
|FORWARD | 32 |
|BACKWARD | 35|
|LEFT | 31 |
|RIGHT | 33|

These pins will be activated when the direction is predicted. So the raspberry pi should be ready to accept the values and move the car in that direction when the respective PIN is activated. This can be changed as required.

---

## Steps to run

**Step 1** : Capture Data - 
	In this step we generate training data. We control the car manually in the path as shown in the images in repo and parallely record the data.
	What is the data we record -
	For every movement of the car we capture an image of the path infront of the car and record it with Label as the direction the car is moving. The date will be generated in Raspberry Pi with path "/home/pi/car/data/" Copy the data to the <repo>/src/train/data.
	
	Command - python3 train/captureData.py
	
**Step 2**: Flip Data - 
	In this step we are creating more data with the existing data.
	For example there is an image for left turn. In that case we flip the image and record it as a new train data for right. This way we can generate more traiing data.
	
	Command - python3 train/FlipData.py

**Step 3**:  Generate Train Data - 
	We shoud create training data from the images captured. Here we load all the images and labels into a pickle package which would be easy for training. This step is optional, you can modify the training code to directly use the images, but this is recommended.
	
	command - python3 train/GenerateTrainData.py
	
**Step 4**: Training the model - 
	This step will use the generated pickle data from step 3. It will train using mulitple optimizers and multiple learning rates and generates all the models with accuracy in the name of the file. Based on the highest accuracy we can pick the model and proceed to next step.
	
	command - python3 train/keras_train.py
	
**Step 5**: Testing with the RC car - 
	
	command - python3 SelfDrive.py

---

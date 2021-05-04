
# Hand Gesture Recognition
## Danny Lindberg and Nick Littlefield

### Overview
The problem that we face is creating a way for machines to understand certain hand gestures, using computer vision, in order for people to communicate with each other when one, or multiple members, is not able to communicate vocally. This can be handy in hospital emergency rooms, military situations and even, if the dataset was expanded, communicating with someone who only can only communicate through ASL. In this specific instance we focused on the gestures OK, thumbs up, thumbs down and a camera facing palm, to show proof of concept.

### Materials and Methods
#### Datasets
Two datasets were used to build different verisons. 

#### Leap Motion Dataset
Consists of black and white images of different hand gestures taking with an infared leap motion sensor. We were only interested in the images for Okay, Thumb Up, and Palm. We extracted these images from the dataset to use in our project. The dataset didn't have any images for dislike, so these images were generated by flipping the thumbs up images. In total, there were 7,601 images. We split the dataset into a training/validation/test set using a 70/20/10 split. 

The entire dataset can be found here: 
https://www.kaggle.com/gti-upm/leapgestrecog

The images extracted and used in the project can be found here: https://drive.google.com/drive/folders/1wb9q1kvlX7IJt2JLXSBVOUDbX0vTEMNw

#### TinyHGR Dataset
The TinyHGR dataset has images for 7 different hand gestures done by 40 different participants. This dataset is large, so we extracted the four hand gestures we were interested in: Okay, Thumb Up, Thumb Down, and Palm. Once fully extracted we had 134,292 images. This was too many image for us to train a model on in Google Colab, so we took a random subset of the data. This instead gave us 33,574 images to work with. We used a 60/25/15 split to generate the training/validation/test set. 

The entire dataset can be found here: 
https://sites.google.com/view/visionlearning/databases/image-database-for-tiny-hand-gesture-recognition

The images extracted and used in the project can be found here: https://drive.google.com/drive/folders/1wb9q1kvlX7IJt2JLXSBVOUDbX0vTEMNw

#### Libraries 
For this project we used the PyTorch, OpenCV, Pandas, and Numpy libraries. The entire project is built on top of pre-trained models that were downloaded from the PyTorch library. These include:
- ResNet18
- ResNet50
- Inception V3

These models were not fine-tuned to the dataset. Instead, we extracted the features from the models, and added new classification layers to the model. These classification layers were then trained. 

#### Transforms
The following transforms were used on the training dataset:
- Random Crop (Size 224 for Resnet, 299 for InceptionV3)
- Resize (Size 224 for ResNet, 299 for InceptionV3)
- Normalization

Normalization used the same ranges for all models. The means used were [0.485, 0.456, 0.406] and standard deviation [0.229, 0.224, 0.225].

For the validation and testing data only resize and normalization were done.

### Training Information
The Leap Motion models were trained using a learning rate of 0.001. ResNet18 was trained for 10 epoch, ResNet50 was trained for 5, and InceptionV3 was trained for 8. 

The TinyHGR models were also trained using a learning rate of 0.001. ResNet50 and InceptionV3 were both trained for 2 epochs. 

### Results

#### LeapMotion Models
##### ResNet18
The results for ResNet18 are shown below:

```
              precision    recall  f1-score   support

           0      1.000     1.000     1.000       117
           1      1.000     1.000     1.000       113
           2      1.000     0.991     0.996       116
           3      0.991     1.000     0.995       110

    accuracy                          0.998       456
   macro avg      0.998     0.998     0.998       456
weighted avg      0.998     0.998     0.998       456
```

The confusion matrix is:

![plot](/files/resnet18_leapmotion.png)

##### ResNet50
The results for ResNet50 are shown below:

```
               precision    recall  f1-score   support

           0      1.000     0.983     0.991       117
           1      1.000     1.000     1.000       113
           2      1.000     0.922     0.960       116
           3      0.909     1.000     0.952       110

    accuracy                          0.976       456
   macro avg      0.977     0.976     0.976       456
weighted avg      0.978     0.976     0.976       456
```

The confusion matrix is:

![plot](/files/resnet50_leapmotion.png)

##### InceptionV3
The results for InceptionV3 are shown below:

```
              precision    recall  f1-score   support

           0      1.000     0.496     0.663       117
           1      0.990     0.912     0.949       113
           2      0.925     0.741     0.823       116
           3      0.547     1.000     0.707       110

    accuracy                          0.783       456
   macro avg      0.866     0.787     0.786       456
weighted avg      0.869     0.783     0.785       456
```

The confusion matrix is:

![plot](/files/inceptionv3_leapmotion.png)

#### TinyHGR Models
##### ResNet50
The results for ResNet 50 are shown below:
```
              precision    recall  f1-score   support

           0      0.564     0.578     0.571       462
           1      0.625     0.840     0.716       525
           2      0.745     0.478     0.582       477
           3      0.591     0.568     0.579       551

    accuracy                          0.620      2015
   macro avg      0.631     0.616     0.612      2015
weighted avg      0.630     0.620     0.614      2015
```
The confusion matrix is:

![plot](/files/resnet50_tinyhgr_cm.png)

##### InceptionV3
The results for Inception V3 are below:

```
              precision    recall  f1-score   support

           0      0.450     0.647     0.531       462
           1      0.757     0.606     0.673       525
           2      0.526     0.577     0.550       477
           3      0.614     0.454     0.522       551

    accuracy                          0.567      2015
   macro avg      0.587     0.571     0.569      2015
weighted avg      0.593     0.567     0.570      2015
```

The confusion matrix is: 

![plot](/files/inceptionv3_tinyhgr_cm.png)

### Conclusions
The models performed best on the LeapMotion dataset. These images were simple black and white images and only consisted of the arm and hand. Both ResNet models held high accuracy, ResNet18 had 99.8%, while ResNet50 had 97.6%). The Inception network had 78.3% accuracy and struggled with separating Thumb Up, Thumb Down, and Ok from the Palm class. 

The models trained on the TinyHGR dataset performed okay, for only being trained 2 epochs. These images weren't as simple than the LeapMotion dataset, so this was to be expected because the model had more noise and variation than the LeapMotion dataset. Precision and recall were relatively low for the Inception model, and the Resnet50 recall and precision scores were higher. If having to choose between the two models, ResNet50 would be the best model to go with, although it was a close tie. 

### Discussion
The LeapMotion models outperformed the TinyHGR models when it comes to hand recognition, however there were multiple reasons this could have happened. First, the LeapMotion dataset isolated the hand and arm in the image and there was no background or other objects in the image. TinyHGR, on the otherhand, had people, and a background with different objects. This means the models, during training, must learn to discern between the objects in the background and the hand.  Along with this, due to the complexity of the TinyHGR dataset, the models needed to be trained for a lot longer than two epochs to actually learn how to discern between everything. The ResNet50 model had 62% accuracy while the InceptionV3 model had 56.7% accuracy. 

The LeapMotion images were also trained using images taken from a Leap Motion sensor. Therefore, for this model to be used in a real world setting, the images given to the network for classification would need to be taken using a Leap Motion sensor, which may not be ideal. 

There are some limitations to our project. These limitations include:
- Using the same learning rate and optimizers for all the models.  Better results could have been achieved it these were adjusted per model
- ResNet50 and InceptionV3 were limited to being trained for 2 epochs. 
- We aren't using entire datasets. Limiting the gestures the model can classify.
- Trained on balanced dataset. Model would not work well in an environment where the available images/gestures were imbalanced.

### Outlooks
Moving forward we would train the ResNet and Inception models on the TinyHGR dataset further to get better results. After getting better results it could be used to try and capture hand gestures in real time, such as in a hospital setting. Along with this, we could add more gestures to the list to make the application more comprehensive. This would increase the variety of settings the application could be used in. 

### Dataset Citations
P. Bao, A.I. Maqueda, C.R. del Blanco, N. García, “Tiny Hand Gesture Recognition without Localization via a Deep Convolutional Network”, IEEE Trans. Consumer Electronics, vol. 63, no. 3, pp. 251-257, Aug. 2017. (doi: 10.1109/TCE.2017.014971)

T. Mantecón, C.R. del Blanco, F. Jaureguizar, N. García, “Hand Gesture Recognition using Infrared Imagery Provided by Leap Motion Controller”, Int. Conf. on Advanced Concepts for Intelligent Vision Systems, ACIVS 2016, Lecce, Italy, pp. 47-57, 24-27 Oct. 2016. (doi: 10.1007/978-3-319-48680-2_5)



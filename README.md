<<<<<<< HEAD
# Scene Understanding for Autonomous Vehicles 

## Name of the group
¿?¿

## Name and contact email of all the team members

| Name  | Contact | 
| ------------- | ------------- | 
| Yevgeniy Kadranov  | ekadranov@gmail.com| 
| Santiago Barbarisi | santiagobarbarisi@gmail.com | 
| Arnau Vallvé| arnau.vallve94@gmail.com| 

## Project abstract
In this project we will work with an autonomous driving framework using Deep Learning techniques and state-of-the-art models as well as creating our own. We will tackle this using different approaches based on recognition, detection and semantic segmentation.

## Project slides 
link to Google Drive folder: https://drive.google.com/drive/folders/1nyDv1cgMg_tFbe6gXcYfD5-Vn03bLpx3?usp=sharing

## Overleaf article
link to Overleaf article: https://www.overleaf.com/read/rpdkgyndmwnr

## VGG summary:  
The main basis of the VGG model was to focus on smaller filter sizes (3x3) in networks of increasing depth (from 11-19 layers). An equivalent receptive field can be obtained combining several smaller filters, rather than using a single larger one. This way the number of parameters is greatly reduced, decreasing the computational cost and adding more non-linearities, which help the decision function to be more discriminative. VGG won 2nd place on the ImageNet 2014 classification (top-5 error 7.32%) and also performed well on other datasets. Even though it has more layers and parameters than other nets (i.e. AlexNet) it was able to converge in less epochs for the training phase. Several methodologies were also used to improve the results even further such as using image cropping over multi-scale images on train and/or test sets, as well as data augmentation and dense evaluation. Finally they only combined two models and were able to get similar results to other ImageNet submissions which combined more models.

## GoogLeNet summary:
One of the main concerns of GoogLeNet is to provide an efficient network architecture usable for real world applications (e.g. mobile phone) by using stacked Inception modules on the higher layers, for memory efficiency. With it they were able to win the ImageNet 2014 classification with a top-5 error of 6.67%. This module concatenates 1x1, 3x3 and 5x5 filters computed in parallel on a patch, reducing the cost by applying 1x1 filters before computing the larger filters. Due to the efficiency of the modules, they are able to build larger networks without having excessive cost (i.e. GoogLeNet has 22 blocks, 100 layers in total). In order to solve the issue of vanishing gradients for the deeper layers, several auxiliary layers are attached to the network. The final model uses a combination of 7 model versions to generate a final ensemble prediction.

# Object recognition - Week 2
## Abstract
On this part of the project we put the focus on the recognition approach for our Scene Understanding framework for Autonomous Vehicles. To start state-of-the-art models are tested over different datasets. The results with VGG were as expected, with an accuracy of ~95%. A step further was taken when designing our own model, which merges two other state-of-the-art systems: GoogLeNet and ResNet. We took the inception module of the first one and at the end of each module that we include input and output of it are added. As a result our model achieves state-of-the-art measures with a loss of 0.228 and accuracy of 97.07 %.

Link to the <a href="https://drive.google.com/open?id=1-oUoocoUbNQGtc-Pw5gQgt0Lo1bocq5_CWq0KBo2nQ8">Google Slide</a> presentation

Link to a Google Drive with the <a href="https://drive.google.com/open?id=11-X5G42oRKUpxxuu-zUR_sL87CWXa2Fk">weights of the model</a>

## Short explanation of the code in the repository
* *callbacks/*: contains Callback functions
* *config/*: Configure parameters of the training/testing (e.g. data augmentation, learning rates, etc)
* *initializations/*: weight initializer
* *layers/*: Non-native layers (e.g. deconvolution)
* *metrics/*: Model metrics calculation
* *models/*: Models used for project. Our implementation is team7model_2.py
* *tools/*: For large projects handle

## Results of the different experiments

VGG16 with image resize on TT100K dataset| Own Implementation with weight regularizers on convolutional and fully connected layers on TT100K dataset
:-------------------------:|:-------------------------:
![](https://github.com/ArnauVallve94/mcv-m5/blob/master/Images/vgg.jpeg)  |  ![](https://github.com/ArnauVallve94/mcv-m5/blob/master/Images/own.jpeg)

## Table of results
| Method (Dataset) | Validation accuracy| Test accuracy |
| ------------- | ------------- | ------------- |
| VGG16 (TT100K) w/ resize | 91.36 % | 95.89 % |
| VGG16 (TT100K) w/ crop | 92.44 % | 96.15 % |
| VGG16 (BTS)| 94.44 % | 94.44 % |
| VGG16 (BTS) w/ transfer learning | 25.24 % | - |
| VGG16 (KITTI) | 97.40 % | - |
| Own implementation (TT100K) | 86.36 % | 94.80 % |
| Own implementation (TT100K) w/ layer regularizer| 90.54 % | 97.07 % |

## Implemented model Architecture
Our implementation             |  
:-------------------------:|
![](https://github.com/ArnauVallve94/mcv-m5/blob/master/Images/model.png)  |  

## Instructions for using the code
Run VGG16 with TT100K dataset
```
CUDA_VISIBLE_DEVICES=0 python train.py -c config/tt100K_classif.py -e VGG_TT100K
```

Run VGG16 with BTS dataset
```
CUDA_VISIBLE_DEVICES=0 python train.py -c config/BelgiumTSC_classif.py -e VGG_BTS
```

Run OwN model with TT100K dataset
```
CUDA_VISIBLE_DEVICES=0 python train.py -c config/own_tt100K_classif.py -e Team_7_Model_TT100K
```

# Object Detection - Week 3/4

## Abstract
For object detection we used YOLO CNN as initial model to detect objects, applied in traffic signs dataset (TT100K) as well as in a common street elements dataset (e.g. pedestrians, cars, trucks). Several modifications were tested and evaluated to the default YOLO model aiming boost the base performance. Several SSD implementations were checked (i.e https://github.com/rykov8/ssd_keras and https://github.com/pierluigiferrari/ssd_keras), tested and modified in order to apply it in our framework, but due to implementation issues we haven't yet been able to use it without errors. Instead, another network (Tiny YOLO) was trained from scratch and evaluated on the TT100K dataset. We will see how little tweaks on the configuration of the YOLO training can make the system achieve 0.990 AR (on TT100K dataset) and 0.795 AR (on Udacity dataset).

## YOLO (You Only Look Once)
YOLO is a Neural Network that performs object detection. It achieves twice the mean average precision (63.5) than its predecessors and in a faster way (45 fps). The main difference with respect to the others systems is that it is based on a single stream of convolutions, fully connected and regression layers, with no region proposal step. This makes it able to reason about the whole information of the images and generalise better non observed images.

## SSD (Single Shot MultiBox Detector)
The idea behind SSD (Single Shot MultiBox Detector) is to achieve faster object detection without sacrifice on detection accuracy, so that it can be used for real-time applications (59 FPS with mAP 74.3% on VOC2007 test, vs. Faster R-CNN 7 FPS with mAP 73.2% or YOLO 45 FPS with mAP 63.4%). As in YOLO, both the localization and classification stages can be achieved in a single forward pass. To achieve that, they propose to eliminate bounding box proposals and the feature resampling stage. A set of default bounding boxes is used instead, on which different convolutional filters of small size are employed to obtain predictions at multiple scales, using feature maps from different layers from the network at different scales. 

## Instructions for using the code
Run YOLO with TT100K dataset
```
CUDA_VISIBLE_DEVICES=0 python train.py -c config/tt100K_detect.py -e VGG_TT100K
```
## Best YOLO Results 

YOLO (TT100K dataset) + Regul. + Norm. (lr = 0.001, weight_decay = 0.001)| 
:-------------------------:|
![](https://github.com/ArnauVallve94/mcv-m5/blob/master/Images/detection_yolo.jpeg)  | 

## Table of results
| Method (Dataset) | Precision | Recall | F1 | Avg. Recall | Avg. IoU | FPS |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |------------- |
| YOLO (TT100K) | 0.552 | 0.288 | 0.378 | 0.968 | 0.744 | 67.911 |
| Tiny YOLO (TT100K) | 0.575 | 0.311 | 0.404 | 0.957 | 0.735 | 68.314 |
| YOLO (TT100K) + Regul. + Norm.(1) | 0.610 | 0.324 | 0.423 | 0.975 | 0.756 | 59.828 |
| YOLO (TT100K) + Regul. + Norm.(2)| 0.673 | 0.379 | 0.485 | 0.990 | 0.810 | 59.800 |
| YOLO (UDACITY) | 0.156 | 0.153 | 0.155 | 0.523 | 0.507 | 70.723 |
| YOLO (TT100K) + Regul. + Norm.(3) | 0.364 | 0.366 | 0.365 | 0.795 | 0.637 | 59.417 |

(1): lr = 0.0001, weight_decay = 0.0001
(2): lr = 0.001, weight_decay = 0.001
(3): lr = 0.001, weight_decay = 0.001, data augmentation


## Completed Tasks
| Week 1 [100%] | Week 2 [100%] | Week 3 [100%]|
| ------------- | ------------- | ------------- |
| a) Done | a) Done| a) Done|
| b) Done| b) Done| b) Done|
| c) Done| c.2) Done| c) Done|
| d) Done| d) Done| d) Done|
| - | e) Done| e) Done|
=======
# mcv-m5
Master in Computer Vision - M5 Visual recognition

## Fork this project
Fork this project to start your group project. Add [robertbenavente](https://github.com/robertbenavente) and [lluisgomez](https://github.com/lluisgomez/) as contributors.

## Project description
Download the [Overleaf document](https://www.overleaf.com/read/qrjbtzwtjhmx)

## Project slides
- Google slides for [Week 1](https://docs.google.com/presentation/d/1A6hgbNn8N-Iq8MhSa_RPIyf87DBL6PCtoDzy1zqS5Xs/edit?usp=sharing)
- Google slides for [Week 2](https://docs.google.com/presentation/d/1Q69lmzPzgtc4lDw8dr9yyFY_T9JXhjJgL4ShyxFJk3M/edit?usp=sharing)
- Google slides for [Week 3](https://docs.google.com/presentation/d/1WuzVeTwUL65Dnki3vsBJgHXwKffrFanwXbmR_URkLQQ/edit?usp=sharing)
- Google slides for [Week 5](https://docs.google.com/presentation/d/1NB4TfRM1SwddQzBR0adUV7t4p4f7T3GUZDt5Q_psMmk/edit?usp=sharing)
- Google slides for Week 7 (T.B.A.)

## Peer review
- Intra-group evaluation form for Week 2 (T.B.A.)
- Intra-group evaluation form for Weeks 3/4 (T.B.A.)
- Intra-group evaluation form for Weeks 5/6 (T.B.A.)
- Intra-group evaluation form for Week 7 (T.B.A.)

## News
 - Improve your GitHub account following [this document](https://docs.google.com/document/d/14oxSKWBbMajIB5Bn2CM-DNb-vychY1f393qYfsHNJfY/edit?usp=sharing)

## Groups

 - [Group 01](https://github.com/guillemdelgado/mcv-m5)
     - Francisco Roldán Sánchez
     - Guillem Delgado Gonzalo
     - Jordi Gené Mola 

 - [Group 02](https://github.com/rogermm14/mcv-m5)
     - Roger Marí Molas
     - Joan Sintes Marcos
     - Alex Palomo Dominguez
     - Alex Vicente Sola

 - [Group 04](https://github.com/mcvavteam/mcv-m5)
     - Mikel Menta Garde
     - Alex Valles Fernandez
     - Sebastian Camilo Maya Hernández
     - Pedro Luis Trigueros Mondéjar

 - [Group 05](https://github.com/marc-gorriz/mcv-m5)
     - Guillermo Eduardo Torres
     - Marc Górriz Blanch
     - Cristina Maldonado Moncada

 - [Group 06](https://github.com/yixiao1/mcv-m5)
     - Juan Felipe Montesinos Garcia
     - Ferran Carrasquer Mas
     - Yi Xiao

 - [Group 07](https://github.com/ArnauVallve94/mcv-m5)
     - Yevgeniy Kadranov
     - Santiago Barbarisi
     - Arnau Vallvé Gispert

 - [Group 08](https://github.com/jonpoveda/scene-understanding-for-autonomous-vehicles)
     - Ferran Pérez Gamonal
     - Joan Francesc Serracant Lorenzo
     - Jonatan Poveda Pena
     - Martí Cobos Arnalot
 
 - [Group 09](https://github.com/BourbonCreams/mcv-m5)
     - Lorenzo Betto
     - Noa Mor
     - Ana Caballero Cano
     - Ivan Caminal Colell
>>>>>>> a64af9c7ebd1f8987bc574cde76eb5feb36d6c0f

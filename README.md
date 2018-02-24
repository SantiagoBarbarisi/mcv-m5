## Scene Understanding for Autonomous Vehicles (M5 - MCV)

## Name of the group

## Name and contact email of all the team members
Yevgeniy Kadranov   - ekadranov@gmail.com

Santiago Barbarisi  - santiagobarbarisi@gmail.com

Arnau Vallvé        - arnauvallve94@gmail.com

## Short abstract about the project (2 lines max)
In this project we will work with an autonomous driving framework using Deep Learning techniques and state-of-the-art models as well as creating our own. We will tackle this using different approaches based on recognition, detection and semantic segmentation.

## Project Slides 
link to Google Drive folder: https://drive.google.com/drive/folders/1nyDv1cgMg_tFbe6gXcYfD5-Vn03bLpx3?usp=sharing

## Overleaf article
link to Overleaf article: https://www.overleaf.com/read/rpdkgyndmwnr

# VGG summary:  
The main basis of the VGG model was to focus on smaller filter sizes (3x3) in networks of increasing depth (from 11-19 layers). An equivalent receptive field can be obtained combining several smaller filters, rather than using a single larger one. This way the number of parameters is greatly reduced, decreasing the computational cost and adding more non-linearities, which help the decision function to be more discriminative. VGG won 2nd place on the ImageNet 2014 classification (top-5 error 7.32%) and also performed well on other datasets. Even though it has more layers and parameters than other nets (i.e. AlexNet) it was able to converge in less epochs for the training phase. Several methodologies were also used to improve the results even further such as using image cropping over multi-scale images on train and/or test sets, as well as data augmentation and dense evaluation. Finally they only combined two models and were able to get similar results to other ImageNet submissions which combined more models.

# GoogLeNet summary:
One of the main concerns of GoogLeNet is to provide an efficient network architecture usable for real world applications (e.g. mobile phone) by using stacked Inception modules on the higher layers, for memory efficiency. With it they were able to win the ImageNet 2014 classification with a top-5 error of 6.67%. This module concatenates 1x1, 3x3 and 5x5 filters computed in parallel on a patch, reducing the cost by applying 1x1 filters before computing the larger filters. Due to the efficiency of the modules, they are able to build larger networks without having excessive cost (i.e. GoogLeNet has 22 blocks, 100 layers in total). In order to solve the issue of vanishing gradients for the deeper layers, several auxiliary layers are attached to the network. The final model uses a combination of 7 model versions to generate a final ensemble prediction.

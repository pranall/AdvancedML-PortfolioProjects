**YOLO Object Detection: Problem Statement**
The project involved developing an object detection system for self-driving applications using YOLOv5 to identify vehicles such as ambulances, buses, cars, motorcycles, and trucks with the Roboflow OpenImages dataset. The dataset contained 627 images and 1194 annotations, organized into train, validation, and test folders, along with a YAML file defining class names and path configurations. The objective was to train three YOLOv5 variants—YOLOv5s, YOLOv5n, and YOLOv5m—to achieve high-accuracy vehicle detection in real-world conditions.

----------

## YOLO - Object Detection Introduction

YOLO is an object detection framework that performs detection and classification in a single pass using one CNN. It operates in real time, divides the image into a grid, assigns objects to cells, and outputs bounding boxes with class probabilities. It detects multiple objects per image and has evolved through versions such as YOLOv2–YOLOv4 with improvements in speed and accuracy.

YOLOv5 introduced a more efficient architecture, higher accuracy, faster inference, and stronger data augmentation and training procedures. It delivered state-of-the-art performance with sub-50 ms inference on GPUs, making it a more capable and flexible detector than earlier YOLO versions.

YOLOv5 is available in 5 different version such as:

![download](https://github.com/user-attachments/assets/fe155fcb-ec89-41f1-a7a5-39aa993ec45b)

The letter *n* in YOLOv5n denoted the nano variant; other letters referred to small, medium, large, and extra-large versions based on model size and parameter count. As parameters increased, GPU inference time also increased, which required consideration during model selection. 

The detailed YOLOv5 structure and corresponding image sizes were shown below:

<img width="716" height="501" alt="download1" src="https://github.com/user-attachments/assets/26808fad-74cb-49f3-83c4-d50f8a223a3e" />

------

## About Dataset
The Roboflow OpenImages dataset was used, containing the target classes: Ambulance, Bus, Car, Motorcycle, and Truck. The dataset was openly available, though a custom dataset could also be created for training the YOLOv5 model.
The features of this dataset can be observed using the below image:

<img width="1060" height="892" alt="download2" src="https://github.com/user-attachments/assets/3f2f9ad8-d10f-4586-a5b0-fa4ec56d6090" />

- There are 3 sets of data - train, valid and test and one more important file that is the yaml file.
- Each of train,test and valid contains two more sub-folders namely images and labels.
- The images folders contains images and the labels folders contains labels for each image

**Note:**
- The name of image and the name of the label file are absolutely the same.
- The labels folder contain a txt file for each folder as shown below
- Each text file will have 5 columns. The first column correspnds to class label and the next 4 columns denote the coordinates of the center of the box and the width and height of the boxes so these values should be multiplied with width and height of images to obtain the correct values.
- Inorder to visualize a bounding box, we can also obtain the left top coordinates and right-bottom coordinates by using the center coordinates and height and width values.

<img width="838" height="411" alt="download3" src="https://github.com/user-attachments/assets/e9ce76a4-436b-4e57-a088-0a68f19e9ca9" />


**YOLO Object Detection: Problem Statement**
The project involved developing an object detection system for self-driving applications using YOLOv5 to identify vehicles such as ambulances, buses, cars, motorcycles, and trucks with the Roboflow OpenImages dataset. The dataset contained 627 images and 1194 annotations, organized into train, validation, and test folders, along with a YAML file defining class names and path configurations. The objective was to train three YOLOv5 variants—YOLOv5s, YOLOv5n, and YOLOv5m—to achieve high-accuracy vehicle detection in real-world conditions.

----------

Since the .ipynb file was too big, you can access it from [this link](https://drive.google.com/file/d/1ORNKaQVnljTJ1S2QgfKecbRsVRoSM5VN/view?usp=drive_link)

------

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


Two Datasets can be downloaded from here since they're too heavy to upload on GitHub: 
[Inference Data](https://drive.google.com/file/d/1H1ExrfYU_tc1YU2HIIKvIjYqbMNUHYsp/view?usp=drive_link) and [Vehicles Open Images](https://drive.google.com/file/d/1nsrcO3hSF4cdCG60KhrzGpoMNU7R9eg0/view?usp=drive_link)

------

# Functions Used

### OpenCV Functions for Bounding Box Visualization (Summary)

1. **`cv2.rectangle`**: Draws bounding boxes on the image using top-left and bottom-right coordinates, color, and thickness. Thickness `-1` fills the rectangle.

2. **Font Scale Calculation**: `font_scale = min(1, max(0.3, w/1000))` adjusts text size relative to image width for readability.

3. **Font Thickness Calculation**: `thickness = max(1, int(w/500))` scales stroke width with image size to prevent thin text.

4. **`cv2.getTextSize`**: Measures text width and height for proper label placement. Parameters include font type, scale, and thickness.

5. **`cv2.putText`**: Renders class labels above bounding boxes at an offset position using calculated font size and thickness.

**Key Roles:**

* `rectangle`: Draw boxes
* `getTextSize`: Measure label dimensions
* `putText`: Draw labels

--------------------------

## Model Comparison

**1. Performance Metrics Comparison**

Evaluation of different YOLOv5 variants (nano, small, medium) is done based on the the effectiveness and following key metrics:

| Metric                  | YOLOv5n (Nano) | YOLOv5s (Small) | YOLOv5m (Medium) |
| ----------------------- | -------------- | --------------- | ---------------- |
| **mAP\@0.5**            | 0.452          | 0.51            | 0.616            |
| **mAP\@0.5:0.95**       | 0.28           | 0.344           | 0.436            |
| **Parameters**          | 1.7M           | 7.0M            | 20.8M            |
| **GFLOPs**              | 4.1            | 15.8            | 47.9             |
| **Inference Time (ms)** | 30.7ms         | 43.9ms          | 44.5ms           |

* **YOLOv5n (Nano):**
  Fastest among the three, with very low inference time (\~30.7ms) and minimal compute (4.1 GFLOPs). Suitable for edge devices but has the lowest mAP.

* **YOLOv5s (Small):**
  A good balance of speed and accuracy. It achieved 0.51 mAP\@0.5 and still maintains a reasonable speed (\~43.9ms). Suitable for real-time applications where speed and accuracy are both important.

* **YOLOv5m (Medium):**
  Highest accuracy among the three, with a noticeable boost in mAP (0.616). Requires more computation (47.9 GFLOPs) and is slightly slower (\~44.5ms), but it significantly outperforms the others in detecting smaller objects.

**2. Inference and Detection Analysis**

Below is a comparison of object detection across 5 sample images:

| Image    | YOLOv5n Detections | YOLOv5s Detections | YOLOv5m Detections |
| -------- | ------------------ | ------------------ | ------------------ |
| image\_1 | 3 Cars, 3 Trucks   | 3 Cars, 4 Trucks   | 6 Cars, 2 Trucks   |
| image\_2 | 4 Cars             | 1 Car              | 2 Buses, 1 Car     |
| image\_3 | 32 Cars            | 1 Truck            | 24 Cars, 2 Trucks  |
| image\_4 | 3 Cars             | 3 Cars             | 3 Cars             |
| image\_5 | 12 Cars, 1 Truck   | 3 Trucks           | 26 Cars            |

* **YOLOv5n** detected more Cars but was less effective in identifying Trucks and Buses.
* **YOLOv5s** balanced Car and Truck detection but missed several objects that Medium caught.
* **YOLOv5m** was the most comprehensive, detecting nearly all objects with higher precision.

**3. Training Efficiency**

* **YOLOv5n:** Trains very quickly due to its smaller model size (1.7M parameters). However, it may underfit complex datasets due to its limited capacity.
* **YOLOv5s:** Achieves a balance of speed and accuracy, allowing it to generalize well with moderate compute.
* **YOLOv5m:** Takes the longest to train but benefits greatly from layer freezing:

  * Freezing the first 15 layers reduced training time by **30%** while retaining **90% of its accuracy**.
  * Allows for larger batch sizes, improving stability during training.

**4. Use-Case Suitability**

| Model       | Best For                     | Trade-offs                       |
| ----------- | ---------------------------- | -------------------------------- |
| **YOLOv5n** | Edge devices, real-time apps | Lower accuracy on small objects  |
| **YOLOv5s** | Balanced applications        | Moderate resource usage          |
| **YOLOv5m** | High-accuracy requirements   | Higher VRAM and slower inference |

* For **edge computing**, YOLOv5n is optimal.
* For **real-time applications** where speed and accuracy are needed, YOLOv5s fits best.
* For **high-precision tasks**, YOLOv5m is ideal if compute is available.

**5. Comparison with Other Models**

| Model            | mAP\@0.5 | Inference Time | Parameters | Use Case                      |
| ---------------- | -------- | -------------- | ---------- | ----------------------------- |
| **YOLOv5n**      | 0.452    | 30.7ms         | 1.7M       | Real-time edge detection      |
| **YOLOv5s**      | 0.510    | 43.9ms         | 7.0M       | General object detection      |
| **YOLOv5m**      | 0.616    | 44.5ms         | 20.8M      | High-precision tasks          |
| **Faster R-CNN** | 0.58     | 100ms          | 41.0M      | High-accuracy, slower         |
| **SSD (300)**    | 0.50     | 56ms           | 23.0M      | Medium speed, decent accuracy |

* **YOLOv5m** matches **Faster R-CNN** in accuracy but is nearly **twice as fast**.
* **YOLOv5s** is comparable to **SSD** in accuracy but slightly faster with fewer parameters.
* **YOLOv5n** stands out for edge and low-compute environments where neither Faster R-CNN nor SSD are practical.

**6. Key Observations**

* **Frozen vs. Full Training:**

  * Freezing the backbone in YOLOv5m results in **20% faster training** with a **minimal drop in mAP**.
  * YOLOv5n and YOLOv5s do not benefit as much from freezing due to their already small architecture.

* **Batch Size Impact:**

  * Larger batch sizes (16+) improve convergence but require more GPU memory.
  * YOLOv5n and YOLOv5s allow for larger batches with less VRAM, while YOLOv5m needs more optimization for large-scale training.

 ------

 ### **7. Conclusion and Further Scope (10 Marks)**

**Answer:**

**Conclusion:**
In this comparative analysis of YOLOv5 variants—YOLOv5n (Nano), YOLOv5s (Small), and YOLOv5m (Medium)—their performance metrics, training efficiency, inference speed, and suitability for different use cases have been explored. From the assignment's evaluation, it is evident that each model variant caters to distinct application requirements:

* **YOLOv5n (Nano)** excels in edge computing scenarios due to its minimal model size and high inference speed. Its lightweight architecture makes it ideal for mobile and IoT devices where compute power and memory are limited. However, its relatively lower mean Average Precision (mAP) signifies a trade-off in detection accuracy, especially for small and complex objects.

* **YOLOv5s (Small)** strikes a balance between speed and accuracy, making it well-suited for real-time applications such as autonomous driving, drone surveillance, and industrial monitoring. It offers higher precision than YOLOv5n while maintaining reasonable computational efficiency, positioning it as a robust middle-ground solution.

* **YOLOv5m (Medium)** achieves the highest accuracy among the three, proving advantageous for tasks that prioritize detection quality over speed, such as medical image analysis or traffic monitoring systems. Despite its larger model size and slower inference time, it demonstrates strong performance in capturing fine-grained details and smaller objects.

When compared with traditional object detection models like **Faster R-CNN** and **SSD**, YOLOv5 variants outperform in terms of inference speed while offering competitive accuracy. YOLOv5m specifically approaches the accuracy of Faster R-CNN but with almost double the speed, showcasing its optimized design for real-time object detection.

The inclusion of **layer freezing** during training in YOLOv5m further improves its efficiency by accelerating training time by approximately **30%** while maintaining about **90% of its accuracy**. This optimization is particularly useful for transfer learning, where pre-trained weights can be adapted to new datasets with minimal compute overhead.


**Further Scope:**

The current exploration of YOLOv5 variants highlights areas of improvement and expansion for real-world applications. Some promising directions include:

1. **Optimization for Edge Deployment:**

   * Leveraging techniques like **model pruning**, **quantization**, and **knowledge distillation** could further reduce model size and latency, enhancing YOLOv5n’s effectiveness on low-power devices without sacrificing significant accuracy.

2. **Integration with Transformer Architectures:**

   * Recent advancements like **DETR (Detection Transformer)** have shown that self-attention mechanisms improve object detection by capturing long-range dependencies. Merging YOLOv5’s convolution-based architecture with Transformer layers could boost both accuracy and contextual understanding.

3. **Domain Adaptation and Generalization:**

   * Extending YOLOv5 to handle **domain shift**—such as different lighting conditions, weather changes, or diverse camera angles—would enhance its reliability. Techniques like **unsupervised domain adaptation** and **multi-source learning** could be explored.

4. **Multi-Scale Detection and Improved Anchors:**

   * Adapting YOLOv5 for **multi-scale detection** using dynamic anchor boxes and feature pyramid networks (FPN) can improve its ability to detect both small and large objects simultaneously with higher precision.

5. **Integration with Edge AI and 5G Technologies:**

   * With the rise of **Edge AI** and **5G connectivity**, deploying YOLOv5 in real-time for applications like **smart cities**, **traffic monitoring**, and **public safety surveillance** can be made more effective with near-instantaneous data processing.

6. **Real-Time Video Analytics:**

   * While YOLOv5 is highly capable of real-time image detection, expanding its capabilities to efficiently process video streams with low latency could open new avenues for **autonomous vehicles**, **live sports analysis**, and **smart surveillance**.

7. **Augmentation with Data-Efficient Learning:**

   * Incorporating **semi-supervised** or **self-supervised learning** methods to train YOLOv5 on smaller datasets could make it more adaptable for resource-constrained environments, especially in medical or agricultural fields where labeled data is scarce.

**Summary:**

YOLOv5 variants—Nano, Small, and Medium—offer a scalable solution for object detection with varying trade-offs in speed and accuracy. Future improvements in **model optimization**, **domain adaptation**, and **edge deployment** could unlock their full potential, making them not only faster but also more robust in dynamic real-world environments. Through strategic enhancements, YOLOv5 can continue to push the boundaries of **real-time object detection** across diverse sectors.

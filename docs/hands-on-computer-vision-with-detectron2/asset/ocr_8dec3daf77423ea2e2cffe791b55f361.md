# Computer vision tasks

Deep learning achieves state-of-the-art results in many CV tasks. The most common CV task is image classification, in which a deep learning model gives a class label for a given image. However, recent advancements in deep learning allow computers to perform more advanced vision tasks. There are many of these advanced vision tasks.

However, this book focuses on more common and important ones, including object detection, instance segmentation, keypoint detection, semantic segmentation, and panoptic segmentation. It might be challenging for readers to differentiate between these tasks. Figure 1.1 depicts the differences between them. This section outlines what they are and when to use them, and the rest of the book focuses on how to implement these tasks using Detectron2. Let’s get started!

![](bbox=[0.12, 0.35, 0.88, 0.7])

# Object detection

Object detection generally includes object localization and classification. Specifically, deep learning models for this task predict where objects of interest are in an image by applying the bounding boxes around these objects (localization). Furthermore, these models also classify the detected objects into types of interest (classification).
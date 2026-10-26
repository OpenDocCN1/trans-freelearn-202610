# 11

# Fine-Tuning Instance Segmentation Models

The object instance segmentation models utilize results from the object detection models. Therefore, all the techniques introduced in the previous chapters for fine-tuning object detection models work the same for object instance segmentation models. However, object instance segmentation has an important feature to fine-tune: the quality of the boundaries of the detected objects. Therefore, this chapter introduces PointRend, a project inside Detectron2 that helps improve the object boundaries’ sharpness.

By the end of this chapter, you will be able to understand how PointRend works. You will also have hands-on experience developing object instance segmentation applications with better segmentation quality using existing PointRend models. Additionally, you can train an object instance segmentation application using PointRend on a custom dataset. Specifically, this chapter covers the following topics:

+   - Introduction to PointRend
+   - Using existing PointRend models
+   - Training custom PointRend models

# Technical requirements

You must have completed Chapter 1 to have an appropriate development environment for Detectron2. All the code, datasets, and results are available in this book’s GitHub repository at https://github.com/PacktPublishing/Hands-On-Computer-Vision-with-Detectron2.
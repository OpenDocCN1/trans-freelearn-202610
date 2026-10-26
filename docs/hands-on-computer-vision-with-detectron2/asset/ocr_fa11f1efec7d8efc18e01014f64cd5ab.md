This book also covers steps for deploying Detectron2 models into production and developing Detectron2 applications for mobile devices. Specifically, it provides the model formats and platforms that Detectron2 supports, such as TorchScript and ONNX formats. It provides the code to convert Detectron2 into these formats models using tracing and scripting approaches. Additionally, code snippets illustrate how to deploy Detectron2 models into C++ and browser environments. Finally, this book also discusses D2Go, a platform to train, fine-tune, and quantize computer visions so they can be deployable to mobile and edge devices with low-computation resource awareness.

Through this book, you will find that Detectron2 is a valuable framework for anyone looking to build robust computer vision applications.

# Who this book is for

If you are a deep learning application developer, researcher, or software developer with some prior knowledge about deep learning, this book is for you to get started and develop deep learning models for computer vision applications. Even if you are an expert in computer vision and curious about the features of Detectron2, or you would like to learn some cutting-edge deep learning design patterns, you will find this book helpful. Some HTML, Android, and C++ programming skills are advantageous if you want to deploy computer vision applications using these platforms.

# What this book covers

Chapter 1, An Introduction to Detectron2 and Computer Vision Tasks, introduces Detectron2, its architectures, and the computer vision tasks that Detectron2 can perform. Additionally, this chapter provides the steps to set up environments for developing computer vision applications using Detectron2.

Chapter 2, Developing Computer Vision Applications Using Existing Detectron2 Models, guides you through the steps to develop applications for computer vision tasks using state-of-the-art models in the Detectron2 Model Zoo. Thus, you can quickly develop practical computer vision applications without having to train custom models.

Chapter 3, Data Preparation for Object Detection Applications, discusses the steps to prepare data for training models using Detectron2. Additionally, this chapter covers the techniques to convert standard annotation formats to the data format required by Detectron2 in case the existing datasets come in different formats.

Chapter 4, The Architecture of the Object Detection Model in Detectron2, dives deep into the architecture of Detectron2 for the object detection task. This chapter is essential for understanding common terminologies when designing deep neural networks for vision systems.

Chapter 5, Training Custom Object Detection Models, provides steps to prepare data, train an object detection model, select the best model, and perform inferencing object detection tasks. Additionally, it details the development process of a custom trainer by extending the default trainer and incorporating a hook into the training process.
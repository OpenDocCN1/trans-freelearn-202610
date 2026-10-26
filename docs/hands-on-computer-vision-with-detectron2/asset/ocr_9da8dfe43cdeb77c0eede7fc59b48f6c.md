The next important piece of information related to any pre-trained model is the architecture of the pre-trained model. This is often reflected by the model's name in the Model Zoo. For instance, R50 and R101 mean **Microsoft Research Asia (MSRA) ResNet-50** and **ResNet-101**, respectively. Similarly, X101 and X152 mean ResNeXt-101-32x8d and ResNeXt-152-32x8d, respectively. Some model names include FPN. This part of the name indicates that the model uses the **Feature Pyramid Network (FPN)** technique. This technique means that the features are extracted at different layers of the model's architecture (instead of taking the output feature from the last layer and feed to the prediction process). Using the FPN backbone helps to increase the accuracy with the time trade-off. Furthermore, having C4 in its name means that this model uses the conv4 backbone. Similarly, having DC5 in its name indicates that this model uses conv5 with dilations in conv5.

> **Important note**
> Interested readers can refer to the web pages regarding the ResNet architecture at https://github.com/KaimingHe/deep-residual-networks and the ResNeXt architecture at https://github.com/facebookresearch/ResNeXt for further information.

After knowing the trained dataset and the model architecture, the next step is to check for the training and inferencing time, training memory, and model accuracy. This information helps to decide the hardware requirements and the trade-off between speed and accuracy. This information is also listed for every pre-trained model on the Detectron2 Model Zoo.

After deciding which model to work with based on speed versus hardware requirement trade-off, the next step is downloading the model's configuration file. On the Model Zoo, the model's name is linked with a .yaml configuration file. **YAML** (short for **Yet Another Markup Language**, or **YAML ain't markup language**) is a human-readable programming language for serializing data. It is commonly used for writing configuration files and supporting automation processes. Finally, there is also a link to the weights of the pre-trained model for downloading.

By this time, you should be able to read and understand important information about the pre-trained models available on the Detectron2 Model Zoo. The next step is using some pre-trained models to perform important computer vision tasks. Developing computer vision applications using Detectron2 typically includes the following steps:

+   - Selecting a configuration file
+   - Getting a predictor
+   - Performing inferences
+   - Visualizing results

The following sections detail these steps for developing object detection applications.
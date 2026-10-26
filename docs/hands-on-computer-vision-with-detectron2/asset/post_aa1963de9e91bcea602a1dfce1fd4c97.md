
For the next few sections of code for the configuration file, ideally, it would be better to run steps to generate different configurations for learning rates, pixel means and standard deviations, and anchor sizes and ratios. However, for simplicity, we have reused the results from previous executions. Additionally, the PointRend model requires a setting for the number of classes for its Point Head; the following code snippet sets that configuration:

```
cfg.MODEL.POINT_HEAD.NUM_CLASSES = nc
```

Once we have the necessary configuration, we are ready to train the model. The code snippets for creating the trainer class (BrainTumorTrainer), the best model hook (BestModelHook), creating a trainer, and registering the hook remain the same as the source code explained in the previous chapter.

The current PointRend project, at the time of writing, throws an error during training if a training batch does not produce any instances (an empty instance list). This issue does not occur for most of the datasets but would occur for some. For example, this issue happens for the training process of the current model trained on the brain tumor segmentation dataset due to its small batch size. Therefore, the following code snippet modifies the PointRend internal sampling strategy code to handle this exception. The following code snippet imports the packages required for this task:

```
import torch
from detectron2.layers import cat
from detectron2.structures import Boxes
from detectron2.projects.point_rend.point_features import (
    get_point_coords_wrt_image,
    sample_point_labels,
    get_uncertain_point_coords_with_randomness
)
from detectron2.projects.point_rend.mask_head import (
    calculate_uncertainty
)
```

The following code snippet redefines the _sample_train_points method to handle the exception:

```
def _sample_train_points(self, coarse_mask, instances):
    assert self.training
    gt_classes = cat([x.gt_classes for x in instances])
    with torch.no_grad():
        # Step 1: sample point coordinates (point_coords)
        # Step 2: get point_coords with respect to the image
```

![](img/0e7994c850a84b7a53d34c7f943c7edd_260_0.png)

![](img/0e7994c850a84b7a53d34c7f943c7edd_260_1.png)

Ground-truth segment

Predicted segment using PointRend

Figure 11.5: The ground-truth segment versus the predicted one using PointRend

Observably, the predicted segment that uses PointRend (right) has a more precise boundary compared to the ground-truth one.

Congratulations! By now, you should have a profound understanding of the PointRend project and how it helps improve object instance segmentation quality. Additionally, you can develop object instance segmentation applications using existing PointRend models or by training PointRend models on custom datasets.

## Summary

This chapter introduced the techniques to fine-tune object instance segmentation applications trained using Detection2. In general, object instance segmentation applications also use object detection parts. Therefore, all the methods that are utilized to fine-tune object detection models can be used for object instance segmentation models. Additionally, this chapter discussed the PointRend project, which helps improve the object instance segmentation boundaries for the detected objects. Specifically, it described how PointRend works and the steps for developing object instance segmentation applications using the existing models available in the PointRend Model Zoo. Finally, this chapter also provided code snippets to train custom PointRend models on custom datasets.

Congratulations again! By now, you should have profound knowledge regarding Detectron2 and be able to develop computer vision applications by using existing models or training custom models on custom datasets. The next chapter discusses the steps for bringing the built models into deployment.

## Part 4:
Deploying Detectron2 Models
into Production

This last part walks you through the steps in an export process to convert Detectron2 models into deployable artifacts. Specifically, it describes the standard file formats of deep learning models such as TorchScript and corresponding runtimes for these formats, such as PyTorch and C++ runtimes. It then provides the steps to convert Detectron2 models to the standard file formats and deploy them to the corresponding runtimes. Additionally, this part introduces Open Neural Network Exchange (ONNX) framework. This framework helps share deep neural networks across multiple frameworks and platforms. It is extremely helpful when deploying Detectron2 models into browsers or mobile environments is needed. Finally, this part describes D2Go, a framework for training, quantizing, and deploying models with minimal memory storage and computation requirements. Models created using this framework are extremely helpful for deploying into mobile or edge devices.

The fourth part covers the following chapters:

- Chapter 12, Deploying Detectron2 Models into Server Environments
- Chapter 13, Deploying Detectron2 Models into Browsers and Mobile Environments

## 12

## Deploying Detectron2 Models into Server Environments

This chapter walks you through the steps of the export process to convert Detectron2 models into deployable artifacts. Specifically, it describes the standard file formats of deep learning models such as TorchScript and the corresponding runtimes for these formats, such as PyTorch and C++. This chapter then provides the steps to convert Detectron2 models to the standard file formats and deploy them to the corresponding runtimes.

By the end of this chapter, you will understand the standard file formats and runtimes that Detectron2 supports. You can perform steps to export Detectron2 models into TorchScript format using tracing or scripting method. Additionally, you can create a C++ application to load and execute the exported models.

In this chapter, we will cover the following topics:

- Supported file formats and runtimes for PyTorch models
- Deploying custom Detectron2 models

## Technical requirements

You should have completed Chapter 1 to have an appropriate development environment for Detectron2. All the code, datasets, and results are available on the GitHub page of the book at https://github.com/PacktPublishing/Hands-On-Computer-Vision-with-Detectron2.

## Supported file formats and runtimes

This section introduces the development environments, file formats, and runtimes for PyTorch applications in general and Detectron2 applications specifically. It then uses simple examples to explain these concepts.

## Development environments, file formats, and runtimes

There are three main concepts to grasp while developing and deploying deep learning models: the development environment and programming languages, the model file formats, and the runtime environments. Figure 12.1 illustrates these three concepts and examples of each.

![](img/0e7994c850a84b7a53d34c7f943c7edd_265_0.png)

Figure 12.1: Development environments, file formats, and deployment environments

The development environment and programming languages or frameworks such as PyTorch or Python, in general, prioritize ease of use and customizations. However, the Python language and PyTorch framework may have runtime issues at deployment. For instance, they are relatively slow compared to other runtime environments, such as executable files generated by C++ applications. Additionally, they are not deployable directly on the web browser or mobile devices. Therefore, models written in PyTorch often need to go through an export process to become deployable artifacts.

Models written in PyTorch have their own file format and are meant to be executable with PyTorch runtime. However, to support cross-platform and compatibility with other runtime environments, PyTorch supports another intermediate file format called TorchScript. This TorchScript file can be executed in different runtime environments, such as being loaded by C++ applications to generate executable files. PyTorch also allows exporting its models into Open Neural Network Exchange (ONNX) file format to support other runtime environments such as web browsers and mobile devices.

TorchScript has its own interpreter, which functions like a limited version of Python. This format enables us to save the entire model to disk and use it in a different environment, such as a server built with another language other than Python. The TorchScript format allows for compiler optimizations, resulting in more efficient execution. Additionally, TorchScript can interface with multiple backend/device runtimes.

Detectron2 supports exporting its models into TorchScript using the tracing and scripting methods. It also allows users to export their models to an ONNX file format, which can be loaded into different runtime environments such as web browsers and mobile devices. Detectron2 models can be exported into Caffe2 file format using the caffe2_tracing method. However, Caffe2 is about to be deprecated and combined with PyTorch. Therefore, this book does not consider this option. Finally, TensorFlow is considered more popular in production environments compared to PyTorch at the time of writing. Therefore, Detectron2 supports exporting its models to TensorFlow's file format


### Notebook settings

Hardware accelerator
GPU

To get the most out of Colab, avoid using a GPU unless you need one. Learn more

Background execution

Want your notebook to keep running even after you close your browser? Upgrade to Colab Pro+

Omit code cell output when saving this notebook

Cancel Save

Figure 1.3: Select GPU for Hardware accelerator

Detectron2 has a dedicated tutorial on how to install Detectron2 on Google Colab. However, this section discusses each step and gives further details about these. First, Detectron2 is built on top of PyTorch, so we need to have PyTorch installed. By default, Google Colab runtime already installs PyTorch. So, you can use the following snippet to install Detectron2 on Google Colab:

```
!python -m pip install \
'git+https://github.com/facebookresearch/detectron2.git'
```

If you have an error message such as the following one, it is safe to ignore it and proceed:

```
ERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
flask 1.1.4 requires click<8.0,>=5.1, but you have click 8.1.3 which is incompatible.
```

However, if you face problems such as PyTorch versions on Google Colab, they may not be compatible with Detectron2. Then, you can install Detectron2 for specific versions of PyTorch and CUDA. You can use the following snippet to get PyTorch and CUDA versions:

```
import torch
TORCH_VERSION = ".".join(torch.__version__.split(".")[:2])
CUDA_VERSION = torch.__version__.split("+")[-1]
print("torch: ", TORCH_VERSION, "; cuda: ", CUDA_VERSION)
```

After understanding the PyTorch and CUDA versions, you can use the following snippet to install Detectron2. Please remember to replace TORCH_VERSION and CUDA_VERSION with the values found in the previous snippet:

```
!python -m pip install detectron2 -f \
https://dl.fbaipublicfiles.com/detectron2/wheels/{TORCH_VERSION}/{CUDA_VERSION}/index.html
```

Here is an example of such an installation command for CUDA version 11.3 and PyTorch version 1.10:

```
!python -m pip install detectron2 -f \
https://dl.fbaipublicfiles.com/detectron2/wheels/cu113/torch1.10/index.html
```

If you face an error such as the following, it means that there is no matching Detectron2 distribution for the current versions of PyTorch and CUDA:

```
ERROR: Could not find a version that satisfies the requirement detectron2 (from versions: none)
ERROR: No matching distribution found for detectron2
```

In this case, you can visit the Detectron2 installation page to find the distributions compatible with the current PyTorch and CUDA versions. This page is available at https://detectron2.readthedocs.io/en/latest/tutorials/install.html.

Figure 1.4 shows the current Detectron2 distributions with corresponding CUDA/CPU and PyTorch versions:

### Install Pre-Built Detectron2 (Linux only)

Choose from this table to install v0.6 (Oct 2021):

| CUDA | torch 1.10 | torch 1.9 | torch 1.8 |
|------|------------|-----------|-----------|
| 11.3 | ▶ install |  |  |
| 11.1 | ▶ install | ▶ install | ▶ install |
| 10.2 | ▶ install | ▶ install | ▶ install |
| 10.1 |  |  | ▶ install |
| cpu | ▶ install | ▶ install | ▶ install |

Figure 1.4: Current Detectron2 distributions for corresponding CUDA/CPU and PyTorch versions

We will first need to install Jupyter on the local computer. The next step is to enable the `jupyter_http_over_ws` Jupyter extension using the following snippet:

```bash
$ pip install jupyter_http_over_ws
$ jupyter serverextension enable --py jupyter_http_over_ws
```

The next step is to start the Jupyter server on the local machine with an option to trust the WebSocket connections so that the Google Colab notebook can connect to the local runtime, using the following snippet:

```bash
$ jupyter notebook \
--NotebookApp.allow_origin=\
'https://colab.research.google.com' \
--port=8888 \
--NotebookApp.port_retries=0
```

Once the local Jupyter server is running, in the Terminal, there is a backend URL with an authentication token that can be used to access this local runtime from Google Colab. *Figure 1.6* shows the steps to connect the Google Colab notebook to a local runtime: **Connect | Connect to a local runtime:**

![](img/0e7994c850a84b7a53d34c7f943c7edd_34_0.png)

Figure 1.6: Connecting the Google Colab notebook to a local runtime

On the next dialog, enter the backend URL generated in the local Jupyter server and click the **Connect** button. Congratulations! You can now use the Google Colab notebook to code Python applications using a local kernel.

## Summary

This chapter discussed advanced CV tasks, including object detection, instance segmentation, keypoint detection, semantic segmentation, and panoptic segmentation, and when to use them. Detectron2 is a framework that helps implement cutting-edge algorithms for these CV tasks with the advantages of being faster, more accurate, modular, customizable, and built on top of PyTorch. Its architecture has four main parts: input data, backbone, region proposal, and region of interest heads. Each of these components is replaceable with a custom implementation. This chapter also provided the steps to set up a cloud development environment using Google Colab, a local development environment, or to connect Google Colab to a local runtime if needed.

You now understand the leading CV tasks Detectron2 can help develop and have set up a development environment. The next chapter (*Chapter 2*) will guide you through the steps to build CV applications for all the listed CV tasks using the cutting-edge models provided in the Detectron2 Model Zoo.

## Technical requirements

You should have set up the development environment with the instructions provided in Chapter 1. Thus, if you have not done so, please complete setting up the development environment before continuing. All the code, datasets, and respective results are available on the GitHub page of the book (under the folder named Chapter02) at https://github.com/PacktPublishing/Hands-On-Computer-Vision-with-Detectron2. It is highly recommended to download the code and follow along.

## Introduction to Detectron2’s Model Zoo

In deep learning, when developing large models and training models on massive datasets, developers of the deep learning methods often provide pre-trained models. The main reason for this is that the developers of these models usually are the big players in the field (e.g., Facebook, Google, Microsoft, or universities) with access to computation resources (e.g., CPUs and GPUs) to train such models on large datasets. These computation resources are generally not accessible for standard developers elsewhere.

These pre-trained models are often trained on the datasets for use with common tasks that the models are intended for. Therefore, another benefit of such models is for the users to adopt them and use them for specific cases of the tasks at hand if they match what the models were trained for. Furthermore, these models can also be used as the baselines on which the end users can further fine-tune them for their specific cases with smaller datasets that require fewer computation resources to train.

Detectron2 also provides an extensive collection of models trained with Big Basin servers with high computation resources (8 NVIDIA V100 GPUs and NVLink) on its Model Zoo. You can read further information and download all the models from this link: https://github.com/facebookresearch/detectron2/blob/main/MODEL_ZOO.md.

It would be a little overwhelming for new users to grasp information about the available models with different specifications. So, this section details which information users should look for to complete a specific computer vision task at hand using these cutting-edge models from Detectron2 Model Zoo. These items include finding information about the following:

- The dataset on which the model was trained
- The architecture of this model
- Training and inferencing times
- Training memory requirement
- Model accuracy
- The link to the trained model (weights)
- The link to its configuration file (.yaml)

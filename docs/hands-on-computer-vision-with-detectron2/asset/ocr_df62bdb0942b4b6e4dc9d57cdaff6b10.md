# Notebook settings

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
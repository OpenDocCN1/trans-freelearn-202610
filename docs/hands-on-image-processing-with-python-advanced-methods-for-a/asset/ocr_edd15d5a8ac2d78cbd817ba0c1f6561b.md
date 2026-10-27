The total loss is:

$$L_{total} = \lambda_{spa} L_{spa} + \lambda_{exp} L_{exp} + \lambda_{col} L_{col} + \lambda_{tv} L_{tv}$$

The proposed network (DCE-Net) is lightweight and fully convolutional, built with seven convolutional layers, ReLU activations, and skip connections to preserve spatial information.

**Input:** RGB low-light image

**Output:** A stack of 3 × n channels for α_t, for each of the 3 color channels over n iterations.

Final layer uses tanh activation to constrain output α_t ∈ [−1,1].

For the forward pass logic, the network predicts n separate α_t maps, each with 3 channels. These are split along the channel axis to apply the enhancement curve iteratively. Iteratively refine the image x using the enhancement curve. Store intermediate results if needed (e.g., 4th and 8th iterations).

Next python code demonstrates how to use a pretrained model to enhance a few low-light images. It first loads pretrained weights into a DCENet model object instantiated and then moves the model to GPU for acceleration. For enhancing a directory of images, it loads and normalizes each input image, converts them to a tensor and moves to CUDA, then applies the model and saves the enhanced output.

Why does this work?

+   - The enhancement curve is adaptive per-pixel, learned via CNN
+   - Iterative application allows progressive refinement
+   - Loss terms encourage exposure correction, color balancing, and structure preservation without needing supervision
+   - The network is compact (less than 100k parameters) and runs fast on GPUs or even mobile devices

Now, refer to the following code snippet:

```
import torch, torchvision, torch.nn as nn
from pathlib import Path

class DCENet(nn.Module):
    def __init__(self, n=8, return_results=[4, 6, 8]):
        super().__init__()
        self.n = n
```
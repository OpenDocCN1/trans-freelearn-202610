Mathematically, the NAFBlock replaces nonlinearities with:

y = x + Conv₂( σ( LN(Conv₁(x)) ) ⊙ Conv₃(x) )

where ⊙ is elementwise multiplication, and LN is layer normalization. This reduces complexity while retaining representation power, making the network efficient and stable.

Now, let us go through the following Python demonstration that runs NAFNet on a noisy image to denoise it. It uses the official NAFNet repository (https://github.com/megvii-research/NAFNet) and pretrained weights. Here are the steps in detail.

+   1. Clone and install dependencies: Run the following commands in a jupyter cell / on console, which clones the repo, installs requirements, and sets up NAFNet in development mode. The prefix --no_cuda_ext ensures it runs even without GPU-compiled extensions (useful on CPU). Here are the commands:

```
!git clone https://github.com/megvii-research/NAFNet
%cd NAFNet
!pip install -r requirements.txt
!pip install --upgrade --no-cache-dir gdown
!python setup.py develop --no_cuda_ext
```

+   2. Load utilities/libraries: Import the following libraries/modules/functions using the next code block:

+   - basicsr is the restoration framework used by NAFNet.
+   - create_model loads pretrained models.
+   - tensor2img converts tensors → numpy for visualization.

Here is the code:

```
from basicsr.models import create_model
from basicsr.utils import img2tensor as _img2tensor, tensor2img, imwrite
from basicsr.utils.options import parse
```

+   3. Image preprocessing helpers: Define the following helper functions for image i/o, conversion of images to tensors, and providing side-by-side display, as shown in the next code snippet:

```
def imread(img_path):
    img = cv2.imread(img_path)
    img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    return img

def img2tensor(img, bgr2rgb=False, float32=True):
    img = img.astype(np.float32) / 255.
    return _img2tensor(img, bgr2rgb=bgr2rgb, float32=float32)
```
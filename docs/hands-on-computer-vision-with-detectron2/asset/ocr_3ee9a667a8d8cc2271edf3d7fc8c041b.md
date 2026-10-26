The input images have different sizes. Therefore, we transform all the input images into the same size using the albumentations package:

```
import albumentations as A
from albumentations.pytorch import ToTensorV2
image_size = 640
augs = A.Compose([
    A.Resize(height = image_size, width = image_size),
    ToTensorV2()])
```

Notably, we resize all input images to 640 by 640. The following code snippet reads the JSON annotation file and creates a PyTorch data loader:

```
import json
with open(json_file_train) as f:
    ds = json.load(f)['images']

tds = TumorDataset(ds, img_dir=img_dir, transform=augs)
image_loader = DataLoader(tds,
                          batch_size = batch_size,
                          shuffle    = False,
                          num_workers = num_workers,
                          pin_memory  = True)
```

This data loader loads the data in batches because the input dataset might be too large to fit the memory at one time. Therefore, we need to create a class to calculate the means and standard deviations in a rolling/running manner.

# Calculating the running means and standard deviations

The formulas for calculating the mean (x̄) and standard deviation (std) of a set of n values, if you have access to the whole set of values, are straightforward:

sum = Σᵢ₌₁ⁿ xᵢ
x̄ = sum / n
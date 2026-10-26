```
[9, 9, 9, ..., 9, 9, 9],
[9, 9, 9, ..., 9, 9, 9],
[9, 9, 9, ..., 9, 9, 9]], device = 'cuda:0', dtype = torch.int32),
[{'id': 1, 'isthing': True, 'score': 0.9987247586250305, 'category_id': 0, 'instance_id': 0 },
{'id': 2, 'isthing': True, 'score': 0.997495174407959, 'category_id': 0, 'instance_id': 1 },
{'id': 3, 'isthing': True, 'score': 0.9887707233428955, 'category_id': 0, 'instance_id': 2 },
{'id': 4, 'isthing': True, 'score': 0.9777324199676514, 'category_id': 0, 'instance_id': 3 },
{'id': 5, 'isthing': False, 'category_id': 37, 'area': 15173 },
{'id': 6, 'isthing': False, 'category_id': 40, 'area': 163644 },
{'id': 7, 'isthing': False, 'category_id': 45, 'area': 192040 },
{'id': 8, 'isthing': False, 'category_id': 46, 'area': 62182 },
{'id': 9, 'isthing': False, 'category_id': 47, 'area': 164592 }])
```

Specifically, this field is a tuple with two elements. The first element is a tensor of the same size as the input image. This tensor helps to indicate the detected class label number for every pixel from the input image. The second element of this tuple is a list of items that map corresponding label numbers into known classes.

# Visualizing the results

The visualization code snippet for panoptic segmentation is very similar to other vision tasks we have learned, except that we need to extract the two elements of the panoptic_seg field (described previously) from the output. Next, these two fields are passed to the draw_panoptic_seg_predictions method of the Visualizer object to perform the annotations:

```
from google.colab.patches import cv2_imshow
from detectron2.utils.visualizer import Visualizer
from detectron2.data import MetadataCatalog
metadata = MetadataCatalog.get(cfg.DATASETS.TRAIN[0])
v = Visualizer(img[:, :, ::-1], metadata, scale=0.5)
```
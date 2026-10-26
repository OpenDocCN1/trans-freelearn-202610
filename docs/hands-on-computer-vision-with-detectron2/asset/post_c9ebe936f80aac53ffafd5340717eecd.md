
## The Detectron2 data loader

Applying augmentations in Detectron2 can be straightforward and complicated at the same time. It is relatively easy to use the declarative approach and apply existing transformations and augmentations provided by Detectron2, which should meet the most common needs. However, adding custom augmentations that require multiple data samples (e.g., MixUp and Mosaic) is a little complicated. This section describes how Detectron2 loads data and how to incorporate existing and custom data augmentations into training Detectron2 models. Figure 9.1 illustrates the steps and main components of the Detectron2 data loading system.

![](img/0e7994c850a84b7a53d34c7f943c7edd_209_0.png)

There are classes for `Dataset`, `Sampler`, `Mapper`, and `Loader`. The `Dataset` component normally stores a list of data items in JSON format. The `Sampler` component helps to randomly select one data item (`dataset_dict`) from the dataset. The selected data item has the information for the image file name and annotations. Thus, the Mapper loads the actual image. After that, the annotations and loaded image go through augmentations and data conversion into an image tensor and instances for annotations. The Mapper produces one data item, and the Loader helps to pack these in batches and feed them to training. A few augmentation techniques may want to load extra samples (`dataset_dict(s)` and `image(s)`) besides the current input, and these items are expressed in dashed lines in Figure 9.1. Once you understand how Detectron2 loads its data, it is relatively easy to incorporate existing data augmentations into Detectron2 training.

The second augmentation type used is ResizeShortestEdge, and the following code snippet creates and appends that augmentation:

```
min_size = cfg.INPUT.MIN_SIZE_TRAIN
max_size = cfg.INPUT.MAX_SIZE_TRAIN
sample_style = cfg.INPUT.MIN_SIZE_TRAIN_SAMPLING
augs.append(
    T.ResizeShortestEdge(
        min_size,
        max_size,
        sample_style)
)
```

Lastly, the following code snippet adds a RandomFlipping augmentation to the list of augmentations used by this custom trainer:

```
if cfg.INPUT.RANDOM_FLIP != "none":
    augs.append(
        T.RandomFlip(
            horizontal=cfg.INPUT.RANDOM_FLIP == "horizontal",
            vertical=cfg.INPUT.RANDOM_FLIP == "vertical",
        )
    )
```

It is relatively easy to use this declarative approach and incorporate different augmentations/transformations using classes provided by Detectron2, as shown in the MyTrainer class. These existing classes only work with one data item (one image and its related annotations). However, several useful image augmentation techniques may require data from multiple samples. For instance, MixUp loads inputs from two data samples, and Mosaic loads inputs from four. In these cases, it is necessary to rewrite several components of the Detectron2 data loader system to enable combining annotations from different samples into one. The next section illustrates these scenarios.

### Developing custom image augmentation techniques

Suppose the custom augmentation requires annotations loaded from one data sample (`dataset_dict`). In that case, it is relatively simple to implement the custom augmentation or transformation and incorporate it using the declarative approach described in the previous section. This section focuses on more complicated augmentation types that require loading and combining inputs from multiple samples. In these cases, we need to rewrite several parts of the Detectron2 data loader system. Thus, this section describes modifications to the Detectron2 data loader system and develops two custom image augmentations (MixUp and Mosaic) for illustration purposes.

The following is the code snippet for the `_convert_to_annotations` method:

```
annos = []
for i, annotation in enumerate(dataset_dict.pop("annotations")):
    bbox = boxes[i]
    annotation["bbox"] = bbox
    annotation["bbox_mode"] = BoxMode.XYXY_ABS
    annos.append(annotation)
instances = utils.annotations_to_instances(
    annos, image_shape,
    mask_format=self.instance_mask_format
)
if self.recompute_boxes:
    gt_boxes = instances.gt_masks.get_bounding_boxes()
    instances.gt_boxes = gt_boxes
    ft_instances = utils.filter_empty_instances(instances)
    dataset_dict["instances"] = ft_instances
```

This method takes the input as the dataset dictionary (`dataset_dict`) and converts the annotations in `dataset_dict` into instances format using the Detectron2 utility function (`annotations_to_instances`). This method is similar to the `_transform_annotations` method in the `DatasetMapper` class. However, we need to override this so that it takes the transformed bounding boxes instead of the bounding boxes in the original `dataset_dict` before converting annotations to instances.

We need to override the `__call__` method of the `DatasetMapper` class in its extended version because we need to modify the current `dataset_dict` to incorporate more annotations loaded from other sampled inputs. This method has three main steps, which are detailed here:

**Step 1:** This step reads the image and related annotation data into `dataset_dict` using the information specified in the input `dataset_dict`. This step is exactly the same as that in its original version:

```
dataset_dict = copy.deepcopy(dataset_dict)
image = utils.read_image(dataset_dict["file_name"],
                         format=self.image_format)
utils.check_image_size(dataset_dict, image)
if "sem_seg_file_name" in dataset_dict:
    sem_seg_gt = utils.read_image(
        dataset_dict.pop("sem_seg_file_name"), "L"
    ).squeeze(2)
else:
    sem_seg_gt = None

boxes = np.array([
    BoxMode.convert(
        obj["bbox"],
        obj["bbox_mode"], BoxMode.XYXY_ABS)
    for obj in dataset_dict['annotations']])
```

**Step 2:** This step creates an object of `ExtendedAugInput` to include `dataset_dict` as an input parameter because we need to modify this `dataset_dict` to add further bounding boxes from other sampled inputs. Except for the additional modification to `dataset_dict`, all other steps remain the same as in the original version of the `DatasetMapper` class:

```
aug_input = ExtendedAugInput(image,
                            sem_seg      = sem_seg_gt,
                            dataset_dict = dataset_dict,
                            boxes = boxes)

transforms   = self.augmentations(aug_input)
image        = aug_input.image
sem_seg_gt   = aug_input.sem_seg
dataset_dict = aug_input.dataset_dict
boxes        = aug_input.boxes

image_shape = image.shape[:2]
dataset_dict["image"] = torch.as_tensor(
    np.ascontiguousarray(image.transpose(2, 0, 1)))
```

**Step 3:** This last step utilizes the `_convert_to_annotations` method described previously to convert transformed/augmented data listed in `dataset_dict` into instances to be processed by Detectron2:

```
if "annotations" in dataset_dict:
    self._convert_to_annotations(dataset_dict,
                                 boxes,
                                 image_shape)
```

The following code snippet creates a `Sampler` component that helps to sample extra inputs while performing augmentations such as MixUp and Mosaic:

```
from detectron2.data import DatasetCatalog

class DataDictSampler():
    def __init__(self, name_ds):
        ds = DatasetCatalog.get(name_ds)
        self.ds = ds
    def get_items(self, n=3):
        indices = np.random.randint(
            low  = 0,
            high = len(self.ds)-1,
            size = n)
        return [copy.deepcopy(self.ds[_]) for _ in indices]
```

This code snippet takes a dataset name registered with Detectron2 and loads its data (in JSON format). It then builds a method (`get_items`) to randomly sample some inputs for performing augmentations. A deep copy of the data items is necessary because the augmentations may change the original data.

Congratulations! After having these extended classes, we are ready to write custom augmentation classes that combine annotations from different inputs.

### Developing the MixUp image augmentation technique

MixUp is a useful and modern image augmentation technique introduced in the previous chapter. Detectron2 does not provide a built-in class for this image augmentation technique. Therefore, this section provides the steps to develop this image augmentation technique. The following code snippet creates the MixUp augmentation class:

```
class MixUpAug(T.Augmentation):
    def __init__(self, cfg, src_weight=0.5, dst_weight=0.5):
        self.cfg = cfg
        self.sampler = DataDictSampler(cfg.DATASETS.TRAIN[0])
        self.src_weight = src_weight
        self.dst_weight = dst_weight
    def get_transform(self, image, dataset_dict):
        cfg = self.cfg
        # Step 1: get one more random input
        # Step 2: append annotations and get mix-up boxes
```

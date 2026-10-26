# Developing the Mosaic image augmentation technique

The Mosaic image augmentation technique needs to combine three more inputs to the current input and create an augmented version (further detail was provided in the previous chapter). There are also two classes for this technique, one for the Augmentation class and one for the Transformation class. The following code snippet creates the MosaicAugmentation class:

```
class MosaicAug(T.Augmentation):
    def __init__(self, cfg):
        self.cfg = cfg
        self.sampler = DataDictSampler(cfg.DATASETS.TRAIN[0])
    def get_transform(self, image, dataset_dict):
        cfg = self.cfg
        mo_items  = self.sampler.get_items()
        mo_images  = []
        mo_boxes   = []
        # extract mo_images, mo_boxes and update dataset_dict
        mt = MosaicTransform(mo_images, mo_boxes)
        return mt
    def __repr__(self):
        return "MosaicAug(4)"
```

This class utilizes the data sampler built previously and samples for three more inputs. It then extracts the images and bounding boxes from these extra inputs. It also adds the annotations from the extra inputs to the current list of annotations to dataset_dict so the next step can utilize them to generate annotation instances in the format supported by Detectron2. Specifically, the __init__ method creates a data sampler object (sampler) to sample for three more extra inputs for performing the Mosaic task. The __repr__ method is to give a string representation of this object for messaging/logging purposes.

The core method of this class is the get_transform method. It first samples three more extra images using the data sampler. It then loops through the sampled images, extracts their images and bounding boxes, and stores them into mo_images and mo_boxes, respectively. Additionally, it extracts the annotations from these extra images and adds them into the current dataset_dict so they can be converted into Detectron2 annotation instances respectively. Finally, it passes the extracted data as the inputs to generate an object of the MosaicTransform class to perform the transformation:

```
for ds_dict in mo_items:
    # mo_images
    mo_image = utils.read_image(ds_dict["file_name"],
                                format=cfg.INPUT.FORMAT)
```
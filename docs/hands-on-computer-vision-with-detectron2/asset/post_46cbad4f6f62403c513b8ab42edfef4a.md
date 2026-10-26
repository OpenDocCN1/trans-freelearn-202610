
The dataset_path argument is the path to the images and annotations labeled using labelme. Another useful usage pattern of this package is the following:

```
labelme2coco dataset_path --train_split_rate 0.8
```

The second argument (--train_split_rate) allows us to split the data into train and test sets with a specified ratio (0.8 in this case).

The data sources and tools described in this section provide common data formats. However, in reality, some datasets are stored in different storage standards, and we might need to build the dataset in the format supported by Detectron2 from scratch. The next section introduces such a dataset and the steps to prepare this dataset.

## Introduction to the brain tumor segmentation dataset

This chapter uses another brain tumor dataset but for segmentation tasks. This dataset is originally from Jun Cheng's 2017 brain tumor dataset: https://doi.org/10.6084/m9.figshare.1512427.v5. It has 3,064 images with 3 kinds of brain tumors: glioma (1,426 images), meningioma (708 images), and pituitary (930 images) tumors. The following section extracts the data, stores the extracted data in COCO format, and performs a train/test split.

## Data extraction

This dataset is in MATLAB data format (.mat file). That is why it is a good example to demonstrate the steps to extract data from various forms and build a standard dataset accepted by Detectron2 from scratch. Specifically, each .mat file has the following fields:

- label: 1 for meningioma, 2 for glioma, and 3 for pituitary tumor
- PID: the patient identification
- image: the image data
- tumorBorder: the polygon coordinates of the tumor border

The following code snippet downloads the dataset and unzips it:

```
!wget --quiet https://figshare.com/ndownloader/articles/1512427/versions/5 -O segbraintumors.zip
data_folder = "segbraintumors"
!unzip -q segbraintumors.zip -d {data_folder}

sets = ["1-766", "767-1532", "1533-2298", "2299-3064"]
sets = ["brainTumorDataPublic_"+_ for _ in sets]

!unzip -q {data_folder}/{sets[0]}.zip -d {data_folder}
!unzip -q {data_folder}/{sets[1]}.zip -d {data_folder}
!unzip -q {data_folder}/{sets[2]}.zip -d {data_folder}
!unzip -q {data_folder}/{sets[3]}.zip -d {data_folder}
```

Observably, there are four subfolders corresponding with four zip files in this dataset.

As discussed in *Chapter 3*, Detectron2 supports the COCO annotation format. The COCO annotation file has fields for `info`, `licenses`, `categories`, `images`, and `annotations`. The following code snippet creates a helper method that creates a JSON object with the data for the first three fields and empty lists for the last two:

```python
def create_headers():
    data_json = {
        "info": {
            "description": "Desc"
        },
        "licenses": [{
            "id": 1,
            "url": "https://creativecommons.org/licenses/by/4.0/",
            "name": "CC BY 4.0"
        }],
        "categories": [
            {"id": 0, "name": "tumors", "supercategory": "none"},
            {"id": 1, "name": "meningioma", "supercategory": "tumors"},
            {"id": 2, "name": "glioma", "supercategory": "tumors"},
            {"id": 3, "name": "pituitary", "supercategory": "tumors"}
        ],
        "images": [],
        "annotations": []
    }
    return data_json
```

For the annotations, besides the segmentation masks (segmentation), there are bounding boxes (bbox) and areas (area). Therefore, the following code snippets create two helper methods to compute these fields from segmentation coordinates:

```python
def segmentation2bbox(segmentation):
    segmentation = np.array(segmentation)
    segmentation = segmentation.reshape((-1, 2))
    x_min = segmentation[:, 0].min()
    y_min = segmentation[:, 1].min()
    x_max = segmentation[:, 0].max()
    y_max = segmentation[:, 1].max()
    width = x_max - x_min
    height = y_max - y_min
    bbox = [x_min, y_min, width, height]
    return bbox

def bbox2area(bbox):
    return bbox[2]*bbox[3]
```

Additionally, the following code snippet creates a helper method to generate JSON objects for images:

```python
def create_image_obj(id, file_name, height, width):
    return {
        "id": id,
        "license": 1,
        "file_name": file_name,
        "height": height,
        "width": width
    }
```

Similarly, the next one creates a helper method to generate JSON objects for annotations:

```python
def create_annotation_obj(id,
                          image_id,
                          category_id,
                          segmentation,
                          bbox):
    iscrowd = 0
    area = bbox2area(bbox)
    return {
        "id": id,
        "image_id": image_id,
        "category_id": int(category_id),
        "segmentation": [segmentation.tolist()],
        "bbox": bbox,
        "iscrowd": iscrowd,
        "area": area
    }
```

The following code snippet creates a JSON object (data_json) to store annotations, then loops through the .mat files and performs the following tasks on each file:

1. Extracts the tumor image data and writes the image into a file
2. Creates a JSON object for the image and appends it to data_json
3. Extracts planar coordinates (boundaries) and labels and creates JSON objects for the annotations, and appends them to data_json:

```python
data_json = create_headers()
for i in tqdm(range(1, 3064+1)):
    with h5py.File(f'{data_folder}/{i}.mat', 'r') as f:
        obj = f['cjdata']
        # Step 1: extract image and write it to file
        # Step 2: create JSON object for image and append it
        # Step 3: extract boundaries + labels then append them
```

Step 1: This step extracts the image data and writes it into a file. Notably, its data type is float64 to keep the image quality:

```python
image = obj['image'][:, :].astype('float64')
image = (image/image.max())*255.0
file_name = f"{i}.jpg"
cv2.imwrite(os.path.join(output_folder, file_name), image)
```

Step 2: This step creates the image JSON object and appends it into data_json:

```python
height, width = image.shape[:2]
data_json["images"].append(
    create_image_obj(
        id          = i,
        file_name   = file_name,
        height      = height,
        width       = width
    )
)
```

Step 3: This step extracts labels and their corresponding planar coordinates for the brain tumors in the current .mat file. It then creates corresponding annotation objects and appends them to data_json:

```python
label = obj['label'][:, :]
tumorBorder = obj['tumorBorder'][:, :]
for j, lbl in enumerate(label):
    segmentation = tumorBorder[j].reshape((-1, 2))[:, [1, 0]].reshape((-1))
    bbox = segmentation2bbox(segmentation)
    data_json["annotations"].append(
        create_annotation_obj(
            id          = i,
            image_id    = i,
            category_id = lbl[0],
            bbox        = bbox,
            segmentation = segmentation
        )
    )
```

The following code snippet then writes the extracted annotations (data_json) into a JSON COCO annotation file:

```python
af = "_annotations.coco.json"
with open(os.path.join(output_folder, af), "w") as f:
    json.dump(data_json, f)
```

The dataset is now in COCO format. However, it contains all the images and labels in one set. Therefore, the following section splits this dataset into train and test sets.

## Train/test split

The following code snippet creates a helper method to write a JSON file given fields for the COCO annotation format (info, licenses, categories, images, and annotations):

```python
def create_json(info,
                licenses,
                categories,
                images,
                annotations,
                file_name):
    obj = {
        "info"         : info,
        "licenses"     : licenses,
        "categories"   : categories,
        "images"       : images,
        "annotations"  : annotations
    }
    with open(file_name, "w") as f:
        json.dump(obj, f)
    print(f"Saved {file_name}")
```

Next, we extract the COCO annotation file for the whole dataset and get the data for its fields:

```python
name_ds = output_folder
af = "_annotations.coco.json"
with open(os.path.join(name_ds, af), "r") as f:
    annotations_json = json.load(f)

info        = annotations_json["info"]
licenses    = annotations_json["licenses"]
categories  = annotations_json["categories"]
images      = annotations_json["images"]
annotations = annotations_json["annotations"]
```

The first three fields (info, licenses, and categories) are the same for train and test sets. Therefore, we need to only do the splitting for the last two (images and annotations). In this case, one image has one annotation and is stored in the same order. Therefore, it is relatively easy to perform the split:

```python
from sklearn.model_selection import train_test_split
stratify = [i['category_id'] for i in annotations]
test_size = 0.1
images_train, images_test, annotations_train, annotations_test = train_test_split(
    images,
    annotations,
    test_size     = test_size,
    stratify      = stratify,
    random_state  = 42
)
```

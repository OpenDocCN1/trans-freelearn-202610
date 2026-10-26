Step 3: This step extracts labels and their corresponding planar coordinates for the brain tumors in the current .mat file. It then creates corresponding annotation objects and appends them to data_json:

```
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

```
af = "_annotations.coco.json"
with open(os.path.join(output_folder, af), "w") as f:
    json.dump(data_json, f)
```

The dataset is now in COCO format. However, it contains all the images and labels in one set. Therefore, the following section splits this dataset into train and test sets.

# Train/test split

The following code snippet creates a helper method to write a JSON file given fields for the COCO annotation format (info, licenses, categories, images, and annotations):

```
def create_json(info,
                licenses,
                categories,
                images,
                annotations,
                file_name):
    obj = {
```
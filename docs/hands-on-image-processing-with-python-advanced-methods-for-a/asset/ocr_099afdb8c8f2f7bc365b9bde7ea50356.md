```
image = cv2.imread("dogs_cats.jpg")
image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

# INSTANCE SEGMENTATION (Mask R-CNN)
cfg_ins = get_cfg()
cfg_ins.merge_from_file(model_zoo.get_config_file(
    "COCO-InstanceSegmentation/mask_rcnn_R_101_FPN_3x.yaml"))
cfg_ins.MODEL.WEIGHTS = model_zoo.get_checkpoint_url(
    "COCO-InstanceSegmentation/mask_rcnn_R_101_FPN_3x.yaml")
cfg_ins.MODEL.DEVICE = device
predictor_ins = DefaultPredictor(cfg_ins)
outputs_ins = predictor_ins(image)
v_ins = Visualizer(
    image_rgb, MetadataCatalog.get(cfg_ins.DATASETS.TRAIN[0]), scale=1.2)
ins_result = v_ins.draw_instance_predictions(outputs_ins["instances"].to("cpu"))

# PANOPTIC SEGMENTATION (FPN)
cfg_pan = get_cfg()
cfg_pan.merge_from_file(model_zoo.get_config_file(
    "COCO-PanopticSegmentation/panoptic_fpn_R_101_3x.yaml"))
cfg_pan.MODEL.WEIGHTS = model_zoo.get_checkpoint_url(
    "COCO-PanopticSegmentation/panoptic_fpn_R_101_3x.yaml")
cfg_pan.MODEL.DEVICE = device
predictor_pan = DefaultPredictor(cfg_pan)
panoptic_seg, segments_info = predictor_pan(image)["panoptic_seg"]
metadata = MetadataCatalog.get(cfg_pan.DATASETS.TRAIN[0])
v_pan = Visualizer(image_rgb, metadata, scale=1.2)
pan_result = v_pan.draw_panoptic_seg_predictions(
    panoptic_seg.to("cpu"), segments_info)

# SEMANTIC FROM PANOPTIC
panoptic_np = panoptic_seg.to("cpu").numpy()
semantic_map = np.zeros_like(panoptic_np)
for segment in segments_info: semantic_map[panoptic_np == segment["id"]] = segment["category_id"]
v_sem = Visualizer(image_rgb, metadata, scale=1.2, instance_mode=ColorMode.SEGMENTATION)
sem_result = v_sem.draw_sem_seg(torch.tensor(semantic_map))
# Plot image_rgb, sem_result, ins_result, pan_result side-by-side here
```
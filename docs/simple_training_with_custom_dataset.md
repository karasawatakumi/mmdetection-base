# Training with custom dataset

The simplest usage for custom dataset. For more advanced usage, please refer to the original document. ([2: TRAIN WITH CUSTOMIZED DATASETS](https://mmdetection.readthedocs.io/en/v2.25.1/2_new_data_model.html#train-with-customized-datasets))

Two preparations:

- dataset: prepare coco-format annotation and images
- config files: copy basic configs from mmdet, and prepare custom config
  - custom config: dataset path (annotation & image prefix), class names of dataset, and `num_classes` of model

## a. faster rcnn

Config files:

```bash
- configs/
    - _base_/  # copied from mmdet
        - models/faster_rcnn_r50_fpn.py
        - datasets/coco_detection.py
        - schedules/schedule_1x.py
        - default_runtime.py
    - exps/faster_rcnn_r50_fpn_1x_car.py  # custom config
```

Custom config:

```python
_base_ = [
    '../_base_/models/faster_rcnn_r50_fpn.py',
    '../_base_/datasets/coco_detection.py',
    '../_base_/schedules/schedule_1x.py', '../_base_/default_runtime.py'
]

# dataset settings
data_root = '/data/'
img_prefix = data_root + 'images/'
classes = ('car',)
data = dict(
    train=dict(
        classes=classes,
        ann_file=data_root + 'train.json',
        img_prefix=img_prefix),
    val=dict(
        classes=classes,
        ann_file=data_root + 'val.json',
        img_prefix=img_prefix),
    test=dict(
        classes=classes,
        ann_file=data_root + 'val.json',
        img_prefix=img_prefix))

# model settings
model = dict(roi_head=dict(bbox_head=dict(num_classes=1)))
```

## b. yolo

Config files:

```bash
- configs/
    - _base_/default_runtime.py  # copied from mmdet
    - yolo/yolov3_d53_mstrain-608_273e_coco.py  # copied from mmdet
    - exps/yolov3_d53_mstrain-608_273e_car.py  # custom config
```

Custom config:

```python
_base_ = '../yolo/yolov3_d53_mstrain-608_273e_coco.py'

# dataset settings
data_root = '/data/'
img_prefix = data_root + 'images/'
classes = ('car',)
data = dict(
    train=dict(
        classes=classes,
        ann_file=data_root + 'train.json',
        img_prefix=img_prefix),
    val=dict(
        classes=classes,
        ann_file=data_root + 'val.json',
        img_prefix=img_prefix),
    test=dict(
        classes=classes,
        ann_file=data_root + 'val.json',
        img_prefix=img_prefix))

# model settings
model = dict(bbox_head=dict(num_classes=1))
```

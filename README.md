# MMDetection Base

Simplified repository for building [MMDetection](https://github.com/open-mmlab/mmdetection)-based repository

## Files

In advance, the basic tool files have been added from [MMDetection](https://github.com/open-mmlab/mmdetection).

```bash
├── LICENSE
├── README.md
├── configs
├── docker
│   ├── Dockerfile
│   ├── requirements_core.txt
│   └── requirements_dev.txt
├── docker-compose.yml
├── src
├── tests
└── tools
    ├── test.py
    └── train.py
```

## Installation

In this repository, MMDetection is installed by docker. MMDetection files (excluding tools) are not included to build a repository that keeps up with the latest version of MMDetection.

Please specify some versions in `requirements_core.txt` and `docker-compose.yml`.

Current versions:

- python 3.10.6
- pytorch 1.12.0
- torchvision 0.13.0
- mmcv 1.7.0
- mmdetection 2.25.3

Run container:

```bash
export UID=$(id -u)
docker compose build dev
docker compose run --rm -v {data root}:/data dev
```

## Usage

### configs

Please add the mmdet config files to use from [MMDetection](https://github.com/open-mmlab/mmdetection).

### run

train:

```bash
python tools/train.py {config file}
```

test:

```bash
python tools/test.py {config file} {ckpt file}
```

## case 1) Training for custom dataset

The simplest usage for custom dataset. For more advanced usage, please refer to the original document. ([2: TRAIN WITH CUSTOMIZED DATASETS](https://mmdetection.readthedocs.io/en/v2.25.1/2_new_data_model.html#train-with-customized-datasets))

Two preparations:

- dataset: prepare coco-format annotation and images
- config files: copy basic configs from mmdet, and prepare custom config
  - custom config: dataset path (annotation & image prefix), class names of dataset, and `num_classes` of model

### a. faster rcnn

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

### b. yolo

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

## License

This repository itself for building repositories is MIT licensed, but the built repositories are subject to the MMDetection license.

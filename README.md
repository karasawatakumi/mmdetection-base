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

Build & run container:

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

## case 1. Training with custom dataset

The simple usage for custom dataset (faster rcnn, yolo):

[docs/simple_training_with_custom_dataset.md](docs/simple_training_with_custom_dataset.md)

## License

This repository itself for building repositories is MIT licensed, but the built repositories are subject to the MMDetection license.

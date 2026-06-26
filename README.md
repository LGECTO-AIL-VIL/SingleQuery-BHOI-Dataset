# SingleQuery-BHOI Dataset
![Dataset](https://img.shields.io/badge/Dataset-Released-brightgreen)
![License](https://img.shields.io/badge/License-CC_BY--NC_4.0-blue)
![Conference](https://img.shields.io/badge/ECCV-2026-purple)

COCO Panoptic-style annotations, example files, and visualization tools for **Single-Query Person-Centric Bimanual Hand-Object Interaction Detection**.

This repository provides the dataset resources for the paper:

> **Single-Query Person-Centric Bimanual Hand-Object Interaction Detection**
> Jonghyun Kim*, Junho Roh*, Yubin Yoon, Hyotae Lee, Jongkuk Park, Taehwan Hwang, Jaechul Kim†, and Jungho Lee†
> ECCV 2026
> *Equal contribution
> †Corresponding authors

## Overview

The **SingleQuery-BHOI Dataset** is designed for **person-centric bimanual hand-object interaction detection**.

Given an input image, the goal is to detect each person and jointly predict the person's body pose, left and right hands, hand contact states, and the corresponding interaction targets.

Unlike hand-centric datasets that treat each hand independently, our annotations organize both hands and their interaction targets under a shared human instance. This enables models to answer the following structured question:

> Which person is using which hand to interact with which target?

## Task Definition

For each person instance, the dataset provides annotations for:

* Human bounding box
* Body keypoints
* Left hand interaction region
* Right hand interaction region
* Left hand contact state
* Right hand contact state
* Left hand interaction target
* Right hand interaction target
* Target object/person segment ID
* Target object/person bounding box
* Validity flags

The task requires a model to reason about the full person-centric interaction structure rather than detecting hands or objects independently.

## Annotation Format

The annotations follow a **COCO Panoptic-style JSON format**.

Each annotation file contains:

```json
{
  "images": [],
  "annotations": [],
  "categories": []
}
```

The top-level `annotations` list is organized at the image level.
Each image-level annotation contains a `segments_info` list.

```json
{
  "image_id": 731,
  "file_name": "000000000731.png",
  "segments_info": []
}
```

Each item in `segments_info` corresponds to one segment in the image.

A segment can be:

* Person
* Object

Person segments contain additional person-centric bimanual hand-object interaction fields.

For details, please refer to:

```text
annotation_schema.md
```

## Dataset Statistics

| Item                |   Count |
| ------------------- | ------: |
| Images              |  64,111 |
| Person instances    | 262,351 |
| Hand-contact labels | 102,821 |
| No-contact          |  30,403 |
| Self-contact        |  13,657 |
| Person-contact      |   1,286 |
| Object-contact      |  57,475 |

## Contact States

Each hand is annotated with one of the following contact states.

| State            | Description                                 |
| ---------------- | ------------------------------------------- |
| `no_contact`     | The hand is not in contact with any target  |
| `self_contact`   | The hand is in contact with the same person |
| `person_contact` | The hand is in contact with another person  |
| `object_contact` | The hand is in contact with an object       |

## Repository Structure

```text
SingleQuery-BHOI-Dataset/
├── README.md
├── annotation_schema.md
├── LICENSE-DATA.md
├── annotations/
│   ├── README.md
│   ├── singlequery_bhoi_train.zip
│   └── singlequery_bhoi_val.zip
├── examples/
│   ├── 0.jpg
│   ├── 0.json
│   ├── 1.jpg
│   ├── 1.json
│   └── ...
└── tools/
    ├── README.md
    └── SingleQueryBHOIVisualizer.exe
```

## Download

The dataset annotation files are provided as compressed zip files under the `annotations/` directory.

| Resource | File | Description |
|---|---|---|
| Training annotations | [`annotations/singlequery_bhoi_train.zip`](./annotations/singlequery_bhoi_train.zip) | A zip file containing one training JSON file |
| Validation annotations | [`annotations/singlequery_bhoi_val.zip`](./annotations/singlequery_bhoi_val.zip) | A zip file containing per-image validation JSON files |
| Annotation schema | [`annotation_schema.md`](./annotation_schema.md) | Description of the released annotation format |
| Example files | [`examples/`](./examples) | Example COCO images and corresponding per-image JSON files |
| Visualization tool | [`tools/`](./tools) | Executable-based annotation visualization tool |

## Training Annotation Package

The training annotations are distributed as a zip file containing a single JSON file.

```text
annotations/singlequery_bhoi_train.zip
└── singlequery_bhoi_train.json
```

The training JSON follows a COCO Panoptic-style structure and contains the training image entries, category definitions, and image-level annotations.

The released training annotations retain only the labels required for method training and evaluation:

- Object/person detection labels
- Body keypoints
- Hand-object interaction labels

## Validation Annotation Package

The validation annotations are distributed as a zip file containing a folder of per-image JSON files.

```text
annotations/singlequery_bhoi_val.zip
└── singlequery_bhoi_val/
    ├── 000000000139.json
    ├── 000000000285.json
    ├── 000000000632.json
    └── ...
```

Each validation JSON file contains the annotation for the corresponding image only.

This structure is intended to simplify per-image inspection, visualization, and evaluation.

## Examples

The `examples/` directory contains a small number of COCO images and their corresponding per-image JSON annotation files.

Example files are stored directly under the `examples/` directory.

```text
examples/
├── 0.jpg
├── 0.json
├── 1.jpg
├── 1.json
├── 2.jpg
├── 2.json
└── ...
```

Each pair uses the same index.

```text
0.jpg  <->  0.json
1.jpg  <->  1.json
2.jpg  <->  2.json
```

The example JSON file contains the annotation for the corresponding image only.
These examples are intended for quick inspection, visualization, and tool testing.

They are not a replacement for the full training or validation annotation files.

## Original Images

The full dataset is based on COCO images.

We do **not** redistribute the full COCO image dataset in this repository.
Users should download the original images from the official COCO dataset website and follow the corresponding image licenses and terms of use.

Only a small number of example COCO images may be included in the `examples/` directory for demonstration and visualization purposes.

## Expected Dataset Layout for Training and Evaluation

After downloading the original COCO images and extracting the released annotation zip files, the recommended dataset layout is:

```text
datasets/
└── coco/
    ├── train2017/
    │   ├── 000000000009.jpg
    │   ├── 000000000025.jpg
    │   └── ...
    ├── val2017/
    │   ├── 000000000139.jpg
    │   ├── 000000000285.jpg
    │   └── ...
    └── annotations/
        ├── singlequery_bhoi_train.json
        └── singlequery_bhoi_val/
            ├── 000000000139.json
            ├── 000000000285.json
            └── ...
```

In this layout:

- `train2017/` and `val2017/` contain the original COCO images.
- `singlequery_bhoi_train.json` is extracted from `singlequery_bhoi_train.zip`.
- `singlequery_bhoi_val/` is extracted from `singlequery_bhoi_val.zip` and contains per-image validation JSON files.

## Visualization Tool

We provide an executable-based visualization tool for inspecting the annotations.

The visualization tool can load an image and its corresponding JSON annotation file, then render person-centric bimanual interaction annotations, including:

* Human boxes
* Left/right hand boxes
* Hand keypoints
* Hand contact states
* Interaction targets
* Object boxes
* Hand-object interaction regions

The executable will be provided under the `tools/` directory or through GitHub Releases.

Example:

```text
tools/
└── SingleQueryBHOIVisualizer.exe
```

## How to Use the Visualization Tool

For example files:

```text
examples/
├── 0.jpg
└── 0.json
```

Open the visualization tool and select:

```text
Image file:
examples/0.jpg

Annotation file:
examples/0.json
```

The tool will render the annotation over the image.

## Project Page

Project page:

```text
https://LGECTO-AIL-VIL.github.io/SingleQuery-BHOI/
```

## Code

The official training and evaluation code will be released separately.

```text
https://github.com/LGECTO-AIL-VIL/SingleQuery-BHOI-Code
```

## License

### Annotations

The annotation files in this repository are intended for non-commercial research and educational use.

Unless otherwise specified, the annotations are released under the **Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0)**.

### Code and Tools

The visualization and utility scripts in this repository are released under the **Apache License 2.0**, unless otherwise specified.

### Original Images

The original COCO images are not included in the full dataset release of this repository.
Users must download the images from the official COCO dataset and comply with the corresponding image licenses and terms of use.

A small number of COCO images may be included in the `examples/` directory for demonstration and visualization purposes.

### Commercial Use

For commercial use of the annotations, please contact the authors or the organization for permission.

## Citation

If you use this dataset or find it useful for your research, please cite our paper.

```bibtex
@inproceedings{kim2026singlequery,
  title     = {Single-Query Person-Centric Bimanual Hand-Object Interaction Detection},
  author    = {Jonghyun Kim and Junho Roh and Yubin Yoon and Hyotae Lee and Jongkuk Park and Taehwan Hwang and Jaechul Kim and Jungho Lee},
  booktitle = {European Conference on Computer Vision},
  year      = {2026}
}
```

## Contact

For questions, please open an issue in this repository or contact:

```text
jonghyun0.kim@lge.com
```

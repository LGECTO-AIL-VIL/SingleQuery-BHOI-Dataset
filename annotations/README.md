# Annotations

This directory contains annotation files for the **SingleQuery-BHOI Dataset**.

The annotations follow a **COCO Panoptic-style JSON format** and extend person segments with person-centric bimanual hand-object interaction fields.

## Directory Role

This directory is intended for the full annotation files used for training, validation, and evaluation.

Small example images and their corresponding per-image JSON files are provided separately under the `examples/` directory.

```text
SingleQuery-BHOI-Dataset/
├── annotations/
│   ├── README.md
│   ├── singlequery_bhoi_train.json
│   └── singlequery_bhoi_val.json
├── examples/
│   ├── 0.jpg
│   ├── 0.json
│   ├── 1.jpg
│   ├── 1.json
│   └── ...
└── tools/
    └── SingleQueryBHOIVisualizer.exe
```

## Annotation Files

The full annotation files will be released as follows.

| File                          | Description                           | Status      |
| ----------------------------- | ------------------------------------- | ----------- |
| `singlequery_bhoi_train.json` | Training annotations                  | Coming soon |
| `singlequery_bhoi_val.json`   | Validation annotations                | Coming soon |
| `singlequery_bhoi_test.json`  | Test annotations, if released         | Optional    |
| `category_mapping.json`       | Category ID and category name mapping | Optional    |
| `label_mapping.json`          | Contact and hand state label mapping  | Optional    |

Large annotation files may be distributed through **GitHub Releases** instead of being committed directly to this directory.

## Full Annotation Format

The full annotation files follow a COCO Panoptic-style structure.

```json
{
  "info": {},
  "licenses": [],
  "images": [],
  "annotations": [],
  "categories": []
}
```

The top-level `annotations` list is organized at the image level. Each image annotation contains a `segments_info` list.

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
* Stuff

Person segments contain additional fields for bimanual hand-object interaction.

## Person-Centric Fields

For person segments, the annotation may include:

* Human bounding box
* Body keypoints
* Face box and face keypoints
* Left hand box and hand keypoints
* Right hand box and hand keypoints
* Left hand contact state
* Right hand contact state
* Left hand interaction target ID
* Right hand interaction target ID
* Left/right hand-object interaction boxes
* Left/right interacting object boxes
* Validity flags

Please refer to the full schema document for details:

```text
../annotation_schema.md
```

## Example Files

Example files are provided directly under the `examples/` directory.

Each example consists of one COCO image and one corresponding JSON file.

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

The file pair should follow the same index.

```text
0.jpg  <->  0.json
1.jpg  <->  1.json
2.jpg  <->  2.json
```

The example JSON file contains the annotation for the corresponding image only.
These example files are intended for quick inspection, visualization, and tool testing.

They are not a replacement for the full training or validation annotation files.

## Original Images

The full dataset is based on COCO images.

We do **not** redistribute the full COCO image dataset in this repository.
Users should download the original images from the official COCO dataset website and follow the corresponding image licenses and terms of use.

Only a small number of example COCO images may be included in the `examples/` directory for demonstration and visualization purposes.

## Expected Dataset Layout for Training

For actual training or evaluation, the recommended dataset layout is:

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
        └── singlequery_bhoi_val.json
```

In this layout, the image files are the original COCO images, and the annotation files are the released SingleQuery-BHOI annotation JSON files.

## Visualization Tool

We provide an executable-based visualization tool for inspecting the annotations.

The visualization tool allows users to load an image and its corresponding annotation file and display person-centric bimanual interaction information, including:

* Human boxes
* Body keypoints
* Left/right hand boxes
* Hand contact states
* Interaction targets
* Object boxes
* Optional hand-object interaction regions

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

## Recommended Checks

Before using the annotation files, please check the following:

* The `image_id` in the annotation matches the target image.
* The image file name matches the corresponding COCO image.
* Each person segment has a valid `id`.
* Object IDs such as `left_obj_id` and `right_obj_id` refer to valid segment IDs in the same image.
* Segment-level `bbox` uses `[x, y, width, height]`.
* Hand-object interaction boxes such as `left_hand_oih_box` and `left_hand_obj_box` use `[x1, y1, x2, y2]`.
* Invalid hand/contact annotations should be ignored when the corresponding validity flag is `false`.

## Notes

* Full annotation files may be large and can be released as compressed files.
* Example JSON files are provided for readability and debugging.
* Example files use simple indexed names such as `0.jpg` and `0.json`.
* The schema may be updated in future releases.
* Please check the release notes for version-specific changes.

## Citation

If you use these annotations, please cite:

```bibtex
@inproceedings{kim2026singlequery,
  title     = {Single-Query Person-Centric Bimanual Hand-Object Interaction Detection},
  author    = {Jonghyun Kim and Junho Roh and Yubin Yoon and Jaechul Kim and Jungho Lee and Hyotae Lee and Jongkuk Park and Taehwan Hwang},
  booktitle = {European Conference on Computer Vision},
  year      = {2026}
}
```

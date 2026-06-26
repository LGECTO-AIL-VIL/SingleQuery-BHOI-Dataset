# Annotations

This directory contains the released annotation files for the **SingleQuery-BHOI Dataset**.

The annotations follow a **COCO Panoptic-style JSON format** and extend person segments with person-centric bimanual hand-object interaction fields.

## Directory Role

This directory contains the full annotation packages used for training and validation.

```text
SingleQuery-BHOI-Dataset/
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

Small example images and their corresponding per-image JSON files are provided separately under the `examples/` directory.

## Annotation Files

| File | Description | Status |
|---|---|---|
| `singlequery_bhoi_train.zip` | Training annotation package | Available |
| `singlequery_bhoi_val.zip` | Validation annotation package | Available |

## Training Annotation Package

The training annotations are provided as a zip file containing one JSON file.

```text
singlequery_bhoi_train.zip
└── singlequery_bhoi_train.json
```

The extracted training JSON follows a COCO Panoptic-style structure.

```json
{
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

- Person
- Object

Person segments contain additional fields for bimanual hand-object interaction.

## Validation Annotation Package

The validation annotations are provided as a zip file containing a folder of per-image JSON files.

```text
singlequery_bhoi_val.zip
└── singlequery_bhoi_val/
    ├── 000000000139.json
    ├── 000000000285.json
    ├── 000000000632.json
    └── ...
```

Each validation JSON file contains the annotation for the corresponding image only.

This per-image structure is intended to simplify validation-time inspection, visualization, and evaluation.

## Released Annotation Contents

The released annotation files retain the labels required for method training and evaluation:

- Object/person detection labels
- Body keypoints
- Hand-object interaction labels
- Left/right hand contact states
- Left/right interaction target IDs
- Target object/person boxes
- Validity flags for hand-object interaction labels

To reduce file size, fields that are not used for training or evaluation are removed from the released annotations.

Removed fields include:

- Stuff/background segments
- Unused image metadata fields such as `license`, `coco_url`, `flickr_url`, and `date_captured`

## Person-Centric Fields

For person segments, the released annotation may include:

- Human bounding box
- Body keypoints
- Left hand interaction region
- Right hand interaction region
- Left hand contact state
- Right hand contact state
- Left hand interaction target ID
- Right hand interaction target ID
- Left/right interacting object boxes
- Validity flags

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

The file pair follows the same index.

```text
0.jpg  <->  0.json
1.jpg  <->  1.json
2.jpg  <->  2.json
```

The example JSON file contains the annotation for the corresponding image only. These example files are intended for quick inspection, visualization, and tool testing.

They are not a replacement for the full training or validation annotation files.

## Original Images

The full dataset is based on COCO images.

We do **not** redistribute the full COCO image dataset in this repository. Users should download the original images from the official COCO dataset website and follow the corresponding image licenses and terms of use.

Only a small number of example COCO images are included in the `examples/` directory for demonstration and visualization purposes.

## Expected Extraction Result

After extracting both zip files, the annotation files should be arranged as follows:

```text
datasets/
└── coco/
    └── annotations/
        ├── singlequery_bhoi_train.json
        └── singlequery_bhoi_val/
            ├── 000000000139.json
            ├── 000000000285.json
            ├── 000000000632.json
            └── ...
```

## Expected Dataset Layout for Training and Evaluation

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

The visualization tool allows users to load an image and its corresponding JSON annotation file and display person-centric bimanual interaction information, including:

- Human boxes
- Left/right hand interaction regions
- Hand contact states
- Interaction targets
- Object boxes

The executable is provided under the `tools/` directory.

```text
tools/
├── README.md
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

For validation files, select an image from the COCO `val2017/` directory and the corresponding per-image JSON file from the extracted validation annotation folder.

Example:

```text
Image file:
datasets/coco/val2017/000000000139.jpg

Annotation file:
datasets/coco/annotations/singlequery_bhoi_val/000000000139.json
```

## Recommended Checks

Before using the annotation files, please check the following:

- The `image_id` in the annotation matches the target image.
- The image file name matches the corresponding COCO image.
- Each person segment has a valid `id`.
- Object IDs such as `left_obj_id` and `right_obj_id` refer to valid segment IDs in the same image.
- Segment-level `bbox` uses `[x, y, width, height]`.
- Hand-object interaction boxes such as `left_hand_oih_box` and `left_hand_obj_box` use `[x1, y1, x2, y2]`.
- Invalid hand/contact annotations should be ignored when the corresponding validity flag is `false`.

## Notes

- The annotations are generated by rule-based integration of publicly available COCO and Hands23 annotations.
- The released annotation files contain only the labels required for detection, body keypoint, and hand-object interaction training/evaluation.
- The full COCO image dataset is not redistributed in this repository.
- Example JSON files are provided for readability and debugging.
- Example files use simple indexed names such as `0.jpg` and `0.json`.
- Please refer to `../annotation_schema.md` for the detailed annotation format.

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
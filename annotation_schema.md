# Annotation Schema

This document describes the annotation format for the **SingleQuery-BHOI Dataset**.

The dataset follows a **COCO Panoptic-style JSON format** and extends each person segment with additional fields for **person-centric bimanual hand-object interaction detection**.

Unlike COCO instance detection annotations, where each object instance is stored independently in the top-level `annotations` list, this dataset stores annotations in an **image-level panoptic structure**. Each image has one annotation entry, and all person/object/stuff segments in that image are stored under `segments_info`.

## Overview

Each annotation file contains:

* Dataset metadata
* Image metadata
* Category definitions
* Image-level panoptic annotations
* Segment-level information
* Person-centric bimanual hand-object interaction fields

The goal is to represent the following structured information for each person:

> Which person is using which hand to interact with which target?

For each person segment, the annotation may include:

* Human segment box
* Body keypoints
* Face box and face keypoints
* Left hand box and hand keypoints
* Right hand box and hand keypoints
* Left hand contact state
* Right hand contact state
* Left hand interaction target ID
* Right hand interaction target ID
* Left/right hand-object interaction regions
* Left/right interacting object boxes
* Left/right hand state labels
* Validity flags

## Top-Level JSON Structure

Each annotation file follows the structure below.

```json
{
  "info": {},
  "licenses": [],
  "images": [],
  "annotations": [],
  "categories": []
}
```

| Field         | Type   | Description                      |
| ------------- | ------ | -------------------------------- |
| `info`        | object | Dataset metadata                 |
| `licenses`    | array  | License information              |
| `images`      | array  | Image metadata                   |
| `annotations` | array  | Image-level panoptic annotations |
| `categories`  | array  | Category definitions             |

## Image Entry

Each entry in `images` describes one original COCO image.

```json
{
  "license": 3,
  "file_name": "000000000731.jpg",
  "coco_url": "http://images.cocodataset.org/train2017/000000000731.jpg",
  "height": 480,
  "width": 640,
  "date_captured": "2013-11-18 09:33:22",
  "flickr_url": "http://farm9.staticflickr.com/8442/7845580894_6763de0047_z.jpg",
  "id": 731
}
```

| Field           | Type   | Description              |
| --------------- | ------ | ------------------------ |
| `id`            | int    | Unique image ID          |
| `file_name`     | string | Original image file name |
| `width`         | int    | Image width in pixels    |
| `height`        | int    | Image height in pixels   |
| `license`       | int    | License ID               |
| `coco_url`      | string | COCO image URL           |
| `flickr_url`    | string | Flickr image URL         |
| `date_captured` | string | Image capture date       |

## Image-Level Panoptic Annotation Entry

Each entry in the top-level `annotations` list corresponds to one image.

```json
{
  "image_id": 731,
  "file_name": "000000000731.png",
  "segments_info": []
}
```

| Field           | Type   | Description                                       |
| --------------- | ------ | ------------------------------------------------- |
| `image_id`      | int    | ID of the corresponding image                     |
| `file_name`     | string | Panoptic segmentation PNG file name               |
| `segments_info` | array  | List of person/object/stuff segments in the image |

### Important Note

The `file_name` inside `annotations` usually refers to the panoptic segmentation PNG file, while the `file_name` inside `images` refers to the original RGB image.

Example:

```text
images[0].file_name       = 000000000731.jpg
annotations[0].file_name  = 000000000731.png
```

## Category Entry

Each entry in `categories` defines a thing or stuff category.

```json
{
  "supercategory": "person",
  "isthing": 1,
  "id": 1,
  "name": "person"
}
```

| Field           | Type   | Description                                           |
| --------------- | ------ | ----------------------------------------------------- |
| `id`            | int    | Category ID                                           |
| `name`          | string | Category name                                         |
| `supercategory` | string | Supercategory name                                    |
| `isthing`       | int    | `1` for thing categories and `0` for stuff categories |

## Segment Entry

Each item in `segments_info` describes one segment in the image.

A segment can be:

* Person segment
* Object segment
* Stuff segment

All segments share common panoptic fields.

```json
{
  "id": 3879213,
  "category_id": 28,
  "iscrowd": 0,
  "bbox": [39.91, 47.13, 234.07, 126.2],
  "area": 16437
}
```

| Field         | Type         | Description                                      |
| ------------- | ------------ | ------------------------------------------------ |
| `id`          | int          | Unique segment ID within the panoptic annotation |
| `category_id` | int          | Category ID of the segment                       |
| `iscrowd`     | int          | COCO-style crowd flag                            |
| `bbox`        | array[float] | Bounding box in `[x, y, width, height]` format   |
| `area`        | int or float | Segment area in pixels                           |

## Bounding Box Format

All bounding boxes use the COCO-style format:

```text
[x, y, width, height]
```

where:

| Element  | Description                         |
| -------- | ----------------------------------- |
| `x`      | x-coordinate of the top-left corner |
| `y`      | y-coordinate of the top-left corner |
| `width`  | Box width                           |
| `height` | Box height                          |

All coordinates are in image pixel space.

## Person Segment

A person segment is a segment whose `category_id` corresponds to the `person` category.

In addition to the common segment fields, a person segment may include body, face, hand, and interaction annotations.

```json
{
  "id": 2367782,
  "category_id": 1,
  "iscrowd": 0,
  "bbox": [131.8, 89.55, 140.28, 338.38],
  "area": 28332,

  "keypoints": [],
  "face_box": [],
  "lefthand_box": [],
  "righthand_box": [],

  "face_kpts": [],
  "lefthand_kpts": [],
  "righthand_kpts": [],

  "face_valid": true,
  "lefthand_valid": true,
  "righthand_valid": true,
  "foot_valid": false,
  "foot_kpts": [],

  "left_hand_contact": "object_contact",
  "right_hand_contact": "object_contact",

  "left_hand_oih_box": [],
  "right_hand_oih_box": [],

  "left_hand_obj_box": [],
  "right_hand_obj_box": [],

  "left_hand_state": "neither_,_held",
  "right_hand_state": "neither_,_held",

  "left_hand_contact_valid": true,
  "right_hand_contact_valid": true,

  "left_obj_id": 3879213,
  "right_obj_id": 3879213
}
```

## Common Person Fields

| Field             | Type         | Description                                |
| ----------------- | ------------ | ------------------------------------------ |
| `keypoints`       | array[float] | COCO-style body keypoints                  |
| `face_box`        | array[float] | Face bounding box                          |
| `lefthand_box`    | array[float] | Left hand bounding box                     |
| `righthand_box`   | array[float] | Right hand bounding box                    |
| `face_kpts`       | array[float] | Face keypoints                             |
| `lefthand_kpts`   | array[float] | Left hand keypoints                        |
| `righthand_kpts`  | array[float] | Right hand keypoints                       |
| `foot_kpts`       | array[float] | Foot keypoints                             |
| `face_valid`      | bool         | Whether the face annotation is valid       |
| `lefthand_valid`  | bool         | Whether the left hand annotation is valid  |
| `righthand_valid` | bool         | Whether the right hand annotation is valid |
| `foot_valid`      | bool         | Whether the foot annotation is valid       |

## Body Keypoint Format

Body keypoints follow the COCO-style keypoint format.

```text
[x1, y1, v1, x2, y2, v2, ..., xK, yK, vK]
```

where each keypoint is represented by:

| Element | Description     |
| ------- | --------------- |
| `x`     | x-coordinate    |
| `y`     | y-coordinate    |
| `v`     | visibility flag |

The visibility flag follows the COCO convention:

| Value | Description             |
| ----- | ----------------------- |
| `0`   | Not labeled             |
| `1`   | Labeled but not visible |
| `2`   | Labeled and visible     |

## Face and Hand Keypoint Format

Face and hand keypoints are stored as flattened arrays.

```text
[x1, y1, s1, x2, y2, s2, ..., xK, yK, sK]
```

where:

| Element | Description                  |
| ------- | ---------------------------- |
| `x`     | x-coordinate                 |
| `y`     | y-coordinate                 |
| `s`     | confidence or validity score |

For hand keypoints, each hand typically contains 21 keypoints.

## Bimanual Hand-Object Interaction Fields

The following fields are added to person segments.

| Field                      | Type         | Description                                               |
| -------------------------- | ------------ | --------------------------------------------------------- |
| `left_hand_contact`        | string       | Contact state of the left hand                            |
| `right_hand_contact`       | string       | Contact state of the right hand                           |
| `left_hand_oih_box`        | array[float] | Left hand-object interaction region box                   |
| `right_hand_oih_box`       | array[float] | Right hand-object interaction region box                  |
| `left_hand_obj_box`        | array[float] | Bounding box of the object associated with the left hand  |
| `right_hand_obj_box`       | array[float] | Bounding box of the object associated with the right hand |
| `left_hand_state`          | string       | Left hand state label                                     |
| `right_hand_state`         | string       | Right hand state label                                    |
| `left_hand_contact_valid`  | bool         | Whether the left hand contact annotation is valid         |
| `right_hand_contact_valid` | bool         | Whether the right hand contact annotation is valid        |
| `left_obj_id`              | int          | Segment ID of the object associated with the left hand    |
| `right_obj_id`             | int          | Segment ID of the object associated with the right hand   |

## Contact States

Each hand may have one of the following contact states.

| Contact State    | Description                                 |
| ---------------- | ------------------------------------------- |
| `no_contact`     | The hand is not in contact with any target  |
| `self_contact`   | The hand is in contact with the same person |
| `person_contact` | The hand is in contact with another person  |
| `object_contact` | The hand is in contact with an object       |

Additional states may appear depending on annotation version. Please refer to the release notes for version-specific contact labels.

## Object ID Rules

The `left_obj_id` and `right_obj_id` fields indicate the target segment associated with each hand.

| Case           |          Object ID Field | Object Box Field          | Description                               |
| -------------- | -----------------------: | ------------------------- | ----------------------------------------- |
| No contact     |          `-1` or missing | empty or missing          | The hand has no interaction target        |
| Object contact |        Target segment ID | Target object box         | The hand interacts with an object segment |
| Person contact | Target person segment ID | Target person box         | The hand interacts with another person    |
| Self contact   |   Same person segment ID | Person or body-region box | The hand interacts with the same person   |

For object contact, `left_obj_id` or `right_obj_id` should match the `id` of another segment in the same image-level `segments_info` list.

Example:

```json
{
  "left_hand_contact": "object_contact",
  "left_obj_id": 3879213,
  "left_hand_obj_box": [38.345632, 47.351373, 280.041017, 200.105358]
}
```

The segment with ID `3879213` should exist in the same `segments_info`.

```json
{
  "id": 3879213,
  "category_id": 28,
  "iscrowd": 0,
  "bbox": [39.91, 47.13, 234.07, 126.2],
  "area": 16437
}
```

## Hand-Object Interaction Region

The fields `left_hand_oih_box` and `right_hand_oih_box` represent the hand-object interaction region.

These boxes may correspond to the local region around the hand and contacted object area.

```json
{
  "left_hand_oih_box": [180.666148, 210.753166, 203.360411, 227.535526],
  "right_hand_oih_box": [166.414555, 199.719417, 178.50479, 221.248193]
}
```

The box format is:

```text
[x1, y1, x2, y2]
```

where:

| Element | Description       |
| ------- | ----------------- |
| `x1`    | Left coordinate   |
| `y1`    | Top coordinate    |
| `x2`    | Right coordinate  |
| `y2`    | Bottom coordinate |

Unlike `bbox`, which uses `[x, y, width, height]`, the `*_oih_box` fields are represented as corner coordinates `[x1, y1, x2, y2]`.

## Hand Object Box

The fields `left_hand_obj_box` and `right_hand_obj_box` represent the bounding box of the object associated with each hand.

```json
{
  "left_hand_obj_box": [38.345632, 47.351373, 280.041017, 200.105358],
  "right_hand_obj_box": [38.908587, 45.320197, 275.543547, 199.84595]
}
```

The box format is:

```text
[x1, y1, x2, y2]
```

where:

| Element | Description       |
| ------- | ----------------- |
| `x1`    | Left coordinate   |
| `y1`    | Top coordinate    |
| `x2`    | Right coordinate  |
| `y2`    | Bottom coordinate |

Note that this may differ from the segment-level `bbox`, which uses `[x, y, width, height]`.

## Hand State

The fields `left_hand_state` and `right_hand_state` describe the state of each hand.

Example:

```json
{
  "left_hand_state": "neither_,_held",
  "right_hand_state": "neither_,_held"
}
```

The exact hand state vocabulary may depend on the dataset release version. Please refer to the released label mapping file if provided.

## Object and Stuff Segments

Non-person segments follow the standard panoptic segment format.

```json
{
  "id": 3879213,
  "category_id": 28,
  "iscrowd": 0,
  "bbox": [39.91, 47.13, 234.07, 126.2],
  "area": 16437
}
```

Stuff segments also use the same format.

```json
{
  "id": 8949649,
  "category_id": 194,
  "iscrowd": 0,
  "bbox": [0, 191, 640, 289],
  "area": 157436
}
```

The difference between thing and stuff categories is defined by the `isthing` field in `categories`.

```json
{
  "supercategory": "ground",
  "isthing": 0,
  "id": 194,
  "name": "dirt-merged"
}
```

## Full Image-Level Example

```json
{
  "image_id": 731,
  "file_name": "000000000731.png",
  "segments_info": [
    {
      "id": 2367782,
      "category_id": 1,
      "iscrowd": 0,
      "bbox": [131.8, 89.55, 140.28, 338.38],
      "area": 28332,
      "keypoints": [],
      "lefthand_box": [173.61, 207.92, 32.57, 22.87],
      "righthand_box": [163.91, 200.29, 22.52, 24.26],
      "lefthand_valid": true,
      "righthand_valid": true,
      "left_hand_contact": "object_contact",
      "right_hand_contact": "object_contact",
      "left_hand_contact_valid": true,
      "right_hand_contact_valid": true,
      "left_obj_id": 3879213,
      "right_obj_id": 3879213
    },
    {
      "id": 3879213,
      "category_id": 28,
      "iscrowd": 0,
      "bbox": [39.91, 47.13, 234.07, 126.2],
      "area": 16437
    }
  ]
}
```

## Validation Rules

Before using the annotations, we recommend checking the following rules.

### Image-Level Consistency

* Every `image_id` in `annotations` must exist in `images`.
* Each image-level annotation should contain `segments_info`.
* The `file_name` in image-level annotations should refer to the corresponding panoptic segmentation PNG.

### Segment Consistency

* Every segment in `segments_info` must have an `id`.
* Segment IDs should be unique within each image.
* Every `category_id` in `segments_info` must exist in `categories`.
* Every segment should have a valid `bbox` and `area`.

### Bounding Box Validity

* Segment-level `bbox` should follow `[x, y, width, height]`.
* `width` and `height` should be positive.
* `lefthand_box`, `righthand_box`, and `face_box` should follow `[x, y, width, height]`.
* `left_hand_oih_box`, `right_hand_oih_box`, `left_hand_obj_box`, and `right_hand_obj_box` should follow `[x1, y1, x2, y2]`.

### Contact-Target Consistency

* If `left_hand_contact` is `object_contact`, `left_obj_id` should refer to a valid segment ID in the same image.
* If `right_hand_contact` is `object_contact`, `right_obj_id` should refer to a valid segment ID in the same image.
* If a contact annotation is invalid, the corresponding `*_hand_contact_valid` field should be `false`.
* If a hand is not valid, the corresponding hand contact annotation should not be used for training or evaluation.

### Left/Right Hand Consistency

* Left-hand fields should describe the person's left hand.
* Right-hand fields should describe the person's right hand.
* Left and right hands are defined from the annotated person's perspective, not from the image viewer's perspective.

## Coordinate Convention

All coordinates are expressed in image pixel coordinates.

* Origin: top-left corner of the image
* x-axis: left to right
* y-axis: top to bottom

The dataset uses two box conventions:

| Field Type           | Format                  |
| -------------------- | ----------------------- |
| Segment `bbox`       | `[x, y, width, height]` |
| `face_box`           | `[x, y, width, height]` |
| `lefthand_box`       | `[x, y, width, height]` |
| `righthand_box`      | `[x, y, width, height]` |
| `left_hand_oih_box`  | `[x1, y1, x2, y2]`      |
| `right_hand_oih_box` | `[x1, y1, x2, y2]`      |
| `left_hand_obj_box`  | `[x1, y1, x2, y2]`      |
| `right_hand_obj_box` | `[x1, y1, x2, y2]`      |

## Notes

* Original COCO images are not redistributed in this repository.
* Users should download the original images from the official COCO dataset website.
* Panoptic segmentation PNG files may be required depending on the use case.
* This annotation format is designed to remain compatible with COCO Panoptic-style loading while adding person-centric bimanual hand-object interaction fields.
* Field names may be updated in future releases. Please refer to the release notes for version-specific schema changes.

## Version

Current schema version:

```text
v1.0
```

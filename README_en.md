# Clash Royale Image Dataset
> This dataset does not contain any video data. Video data is sourced from YouTube or self-recorded videos.

[中文](README.md) | English

## Overview
### Sliced Dataset
This dataset collects a total of 154 categories (see [`label_list.py`](https://github.com/wty-yy/KataCR/blob/master/katacr/constants/label_list.py) for all sliced names), totaling 4654 slices, used to create generative datasets.
![Slice size distribution](./asserts/segment_size_en.png)

<div align="center">
    <img src="images/segment/archer/archer_1_0000007.png" width="10%"/>
    <img src="images/segment/archer/archer_1_0000009.png" width="10%"/>
    <img src="images/segment/archer/archer_1_0000010.png" width="10%"/>
    <img src="images/segment/archer/archer_1_0000028.png" width="10%"/>
    <img src="images/segment/archer/archer_1_0000057.png" width="10%"/>
    <img src="images/segment/archer/archer_1_0000060.png" width="10%"/>
    <img src="images/segment/archer/archer_1_0000094.png" width="10%"/>
    <img src="images/segment/archer/archer_1_0000168.png" width="10%"/>
    <img src="images/segment/archer/archer_1_0000176.png" width="10%"/>
</div>
<div align="center">
    <img src="images/segment/hog-rider/hog-rider_0_0000004.png" width="10%"/>
    <img src="images/segment/hog-rider/hog-rider_0_0000027.png" width="10%"/>
    <img src="images/segment/hog-rider/hog-rider_0_0000053.png" width="10%"/>
    <img src="images/segment/hog-rider/hog-rider_0_0000059.png" width="10%"/>
    <img src="images/segment/hog-rider/hog-rider_0_0000062.png" width="10%"/>
    <img src="images/segment/hog-rider/hog-rider_0_0000054.png" width="10%"/>
    <img src="images/segment/hog-rider/hog-rider_1_0000493.png" width="10%"/>
    <img src="images/segment/hog-rider/hog-rider_1_0000557.png" width="10%"/>
    <img src="images/segment/hog-rider/hog-rider_1_0000496.png" width="10%"/>
</div>
<div align="center">
    <img src="images/segment/queen-tower/queen-tower_0_0000000.png" width="10%"/>
    <img src="images/segment/queen-tower/queen-tower_0_0000007.png" width="10%"/>
    <img src="images/segment/queen-tower/queen-tower_0_0006331.png" width="10%"/>
    <img src="images/segment/queen-tower/queen-tower_0_0006335.png" width="10%"/>
    <img src="images/segment/queen-tower/queen-tower_0_0006380.png" width="10%"/>
    <img src="images/segment/queen-tower/queen-tower_1_attack_929.png" width="10%"/>
    <img src="images/segment/queen-tower/queen-tower_1_006330.png" width="10%"/>
    <img src="images/segment/queen-tower/queen-tower_1_0009264.png" width="10%"/>
    <img src="images/segment/queen-tower/queen-tower_1_0007320.png" width="10%"/>
</div>

### Generative Dataset
Generated using [`generator.py`](https://github.com/wty-yy/KataCR/blob/master/katacr/build_dataset/generator.py), for training with [YOLOv8](https://github.com/ultralytics/ultralytics).
<div style="display: flex; flex-wrap: nowrap; justify-content: space-between;">
    <img src="asserts/generation1.jpg" alt="Generation 1" width="49%" />
    <img src="asserts/generation2.jpg" alt="Generation 2" width="49%" />
</div>

### Card Dataset
This dataset only collects images of the 2.6 Hog Cycle deck.
<div align="center">
    <img src="images/card_classification/cannon/00030_2.jpg" width="9%"/>
    <img src="images/card_classification/fireball/00285_1.jpg" width="9%"/>
    <img src="images/card_classification/hog-rider/00045_3.jpg" width="9%"/>
    <img src="images/card_classification/ice-golem/00450_1.jpg" width="9%"/>
    <img src="images/card_classification/ice-spirit-evolution/04425_4.jpg" width="9%"/>
    <img src="images/card_classification/ice-spirit/00105_4.jpg" width="9%"/>
    <img src="images/card_classification/musketeer/00480_3.jpg" width="9%"/>
    <img src="images/card_classification/skeletons/00780_1.jpg" width="9%"/>
    <img src="images/card_classification/skeletons-evolution/04875_2.jpg" width="9%"/>
    <img src="images/card_classification/the-log/00210_3.jpg" width="9%"/>
</div>

### Elixir Dataset
This dataset only collects images of 5 different elixir numbers, used for further classification of target recognition results.
<div align="center">
    <img src="images/elixir_classification/-1/101.jpg" width="19%"/>
    <img src="images/elixir_classification/-2/11.jpg" width="19%"/>
    <img src="images/elixir_classification/-3/302.jpg" width="19%"/>
    <img src="images/elixir_classification/-4/10.jpg" width="19%"/>
    <img src="images/elixir_classification/1/105.jpg" width="19%"/>
</div>


## Introduction and Usage
All codes related to this dataset are located in [`KataCR`](https://github.com/wty-yy/KataCR),
The following codes are under the `KataCR` project,
which includes **label conversion for dataset** and **generation code for generative dataset**.
This dataset contains four sub-datasets:
1. [Manually labeled target recognition image dataset](/images/part2/):
  File path format is `part2/match_video_name/round number`,
  Open the subfolder using [Labelme](https://github.com/labelmeai/labelme) to edit the bounding box. Each subfolder contains the following files (with `frame` being the number of frames in the video):

- `frame.jpg`: Target recognition image.
- `frame.json`: Bounding box information recorded by Labelme.
- `frame.txt`: Convert the `json` file to a `txt` file for YOLO model training, generated using [`label_builder.py`](https://github.com/wty-yy/KataCR/blob/master/katacr/build_dataset/label_builder.py).
  - The format of each line is `category relative coordinates of the bounding box center (x, y) relative width and height (w, h) affiliated category 6 zeros`.

2. [Generative dataset](/images/segment/):
  - File path format `segment/cls_name/{cls_name}_{bel}_id.png`:
    - `cls_name` is the sliced category name (see [`label_list.py`](https://github.com/wty-yy/KataCR/blob/master/katacr/constants/label_list.py) for all sliced names)
    - `bel` is the category's affiliated faction
    - `id` is the image number
  - Usage of generative dataset:
    1. Configure the `path_dataset` path in [`constant.py`](https://github.com/wty-yy/KataCR/blob/master/katacr/build_dataset/constant.py) to the `Clash-Royale-Dataset` folder.
    2. Run [`generator.py`](https://github.com/wty-yy/KataCR/blob/master/katacr/build_dataset/generator.py) to generate target recognition images and labeled target recognition images in the `KataCR/logs/generation` folder. (Configuration files are at the bottom of `generator.py` and in [`generator_config.py`](https://github.com/wty-yy/KataCR/blob/master/katacr/build_dataset/generation_config.py))
  - YOLOv8 training method: see...

3. [Card Classification](/images/card_classification/):
  - File path format `card_classification/card_name/id.jpg`:
    - `card_name` is the name of the card
    - `id` is the image number

4. [Elixir Classification](/images/card_classification/):
  - File path format `elixir_classification/cls_name/id.jpg`:
    - `cls_name` is the name of the elixir number
    - `id` is the image number
# Dataset: CARLA Traffic-Light Images

## Description

Simulated traffic-light images generated from the **CARLA** driving simulator, cropped to **32×32 pixels** in three classes:

| Class | Label | Count (approx.) |
|-------|-------|-----------------|
| 🟢 Green | `green/` | ~660 |
| 🔴 Red | `red/` | ~540 |
| 🟡 Yellow | `yellow/` | ~710 |

**Split:** 80% train / 10% validation / 10% test (split by source image to avoid leakage).  
**Test set:** 191 images (66 green, 54 red, 71 yellow).

## Download / Reproduce

The full dataset is not included in this repository due to size constraints.

**Option 1:** Generate your own using CARLA simulator  
**Option 2:** Contact the authors for a copy of the processed dataset

## Sample Images

See `sample_images/` for a few representative examples from each class.

## Directory Structure (when downloaded)

```
dataset/
├── train/
│   ├── green/
│   ├── red/
│   └── yellow/
├── val/
│   ├── green/
│   ├── red/
│   └── yellow/
├── test/
│   ├── green/
│   ├── red/
│   └── yellow/
└── sample_images/
    ├── green_sample.png
    ├── red_sample.png
    └── yellow_sample.png
```

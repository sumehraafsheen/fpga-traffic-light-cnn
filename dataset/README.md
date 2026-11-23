# Dataset: CARLA Traffic-Light Images

## Description

Simulated traffic-light images generated from the **CARLA** driving simulator, cropped to **32×32 pixels** in four classes:

| Class | Folder | Train | Val | Total |
|-------|--------|-------|-----|-------|
| 🟢 Green | `green/` | — | — | ~659 |
| 🔴 Red | `red/` | — | — | ~664 |
| 🟡 Yellow | `yellow/` | — | — | ~586 |
| ⬛ Background | `back/` | — | — | ~617 |

**Split:** 80% train / 20% validation (split by source image to avoid leakage).
- **Train:** 2,143 images
- **Val:** 383 images
- **Test set (separate):** 191 images (66 green, 54 red, 71 yellow) — stored in `../model/evaluation/`

## Directory Structure

```
dataset/
├── train/
│   ├── back/       # Background / no traffic light
│   ├── green/
│   ├── red/
│   └── yellow/
├── val/
│   ├── back/
│   ├── green/
│   ├── red/
│   └── yellow/
├── sample_images/  # Representative demo images
│   ├── green_0.png ... green_5.png
│   ├── red_0.png ... red_5.png
│   └── yellow_0.png ... yellow_5.png
└── README.md
```

## Source

Images were generated using the CARLA simulator and sourced from Kaggle.

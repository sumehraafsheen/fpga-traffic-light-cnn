# FPGA-Based Traffic-Light Classification on PYNQ-Z2 Using a Tensil CNN Accelerator

> Reconfigurable Computing Course Project — November 2026

A complete end-to-end pipeline that trains a small CNN to classify traffic-light images as **red**, **yellow**, or **green**, compiles it to a Tensil accelerator bitstream, and runs inference on a **PYNQ-Z2** FPGA board at **7.56 ms per image** and **1.44 W**.

## Team

| # | Name | ID |
|---|------|----|
| 1 | Yash Raj | 2024A8PS0474H |
| 2 | Sumehra Afsheen | 2024A8PS0505H |

---

## Overview

Recognising traffic lights is a core function of driver-assistance systems. Inspired by [Al Amin et al. (IEEE Access, 2024)](https://doi.org/10.1109/ACCESS.2024.XXXXXXX) who deployed YOLOv3-Tiny on a Kria KV260, we built a scaled-down version using the open-source **[Tensil](https://github.com/tensil-ai/tensil)** framework on the more accessible PYNQ-Z2 board.

### Pipeline

```
CARLA Dataset → PyTorch Training → ONNX Export → Tensil Compilation → Vivado Bitstream → PYNQ-Z2 Inference
```

## Architecture

**CNN Model (~6.1k parameters)**
- 3 × Conv2D (3×3, 8→16→32 filters) + ReLU
- 2 × MaxPool2D (2×2) after first two conv layers  
- 8×8 Average Pool → Dense (3-class softmax)
- Input: 32×32 RGB cropped traffic-light images

**FPGA Accelerator**
- Tensil 8×8 systolic array, 16-bit fixed-point (8 fractional bits)
- 4,762 compiled instructions, ~26,977 cycles/image
- Connected via AXI DMA to Zynq PS @ 50 MHz

## Results

| Resource / Metric | Value |
|---|---|
| LUTs | 14,347 |
| Flip-flops | 9,996 |
| BRAM / DSP | 41 / 73 |
| Clock | 50 MHz |
| Power (total) | 1.439 W |
| Float accuracy (PyTorch) | 97.9% |
| On-board accuracy | 83.25% (159/191) |
| Inference time | 7.56 ms/image |

**Per-class on-board accuracy:** Green 98.5% · Red 83.3% · Yellow 69.0%

## Repository Structure

```
├── model/
│   ├── training/           # Training notebooks & scripts
│   │   └── train.ipynb     # PyTorch model training notebook
│   └── export/             # Exported model files
│       └── model.onnx      # ONNX export of trained model
├── tensil/
│   ├── architecture/       # Tensil architecture config (.tarch)
│   └── compiled/           # Compiler output (.tmodel, .tprog, .tdata)
├── hardware/
│   ├── vivado/             # Vivado project TCL scripts & block design
│   │   └── block_design.tcl
│   └── bitstream/          # Generated bitstream & hardware handoff
│       ├── design.bit
│       └── design.hwh
├── deployment/
│   └── inference.ipynb     # PYNQ-Z2 inference notebook (Python driver)
├── dataset/
│   ├── README.md           # Dataset description & download instructions
│   └── sample_images/      # A few sample images per class
├── results/
│   └── board_demo.png
├── docs/
│   ├── figures/            # Block diagram, board photos, etc.
│   └── proposal.pdf        # Project proposal document
├── .gitignore
├── README.md               # ← You are here
├── LICENSE
└── requirements.txt
```

## Getting Started

### Prerequisites

- Python 3.8+ with PyTorch, ONNX
- [Tensil](https://github.com/tensil-ai/tensil) (via Docker)
- Xilinx Vivado 2022.1+
- PYNQ-Z2 board with PYNQ v2.7+ image

### 1. Train the Model

```bash
cd model/training
jupyter notebook train.ipynb
```

### 2. Compile with Tensil

```bash
# Using the Tensil Docker container
docker run -v $(pwd):/work tensil/tensil \
  compile -a tensil/architecture/arch.tarch \
  -m model/export/model.onnx \
  -o tensil/compiled/ -t onnx
```

### 3. Build the Bitstream

Open Vivado, source the TCL script to recreate the block design, then generate the bitstream:

```bash
vivado -mode batch -source hardware/vivado/block_design.tcl
```

### 4. Run Inference on PYNQ-Z2

Upload `deployment/inference.ipynb`, the bitstream (`.bit` + `.hwh`), and compiled model files to the board, then run the notebook.

## Key Findings

- The 14.7-point accuracy drop (97.9% → 83.25%) from float to on-board is primarily due to **16-bit fixed-point quantization** — the Tensil emulator already showed reduced scores vs. float.
- **Red misclassified as green** occurred 5/54 times — a safety-critical failure mode to address.
- Using **simulated CARLA data** limits real-world generalization assessment.

## Future Work

- [ ] Fix max-pool numerical precision issue
- [ ] Add confidence threshold with temporal voting
- [ ] Test with real-world photographs
- [ ] Integrate live camera feed
- [ ] Implement wireless alert system

## References

1. R. Al Amin et al., "FPGA-based real-time object detection and classification system using YOLO for edge computing," *IEEE Access*, vol. 12, 2024.
2. Tensil AI, [Tensil open-source ML accelerator framework](https://github.com/tensil-ai/tensil).

## License

This project is for academic purposes. See [LICENSE](LICENSE) for details.

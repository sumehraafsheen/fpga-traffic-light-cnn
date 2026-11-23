# Hardware Design

## `vivado/`
- `block_design.tcl` — TCL script to recreate the Vivado block design
- The design connects the Tensil Compute Unit (TCU) to the Zynq PS:
  - AXI DMA streams instructions from DDR → TCU
  - TCU reads weights/images and writes results via 2 HP AXI ports
  - ARM controls DMA over GP AXI port
  - Clock: 50 MHz (100 MHz failed timing with worst slack −7.843 ns)

## `bitstream/`
- `design.bit` — FPGA bitstream
- `design.hwh` — Hardware handoff file (required by PYNQ Python driver)

> **Note:** Since the model is loaded at runtime by the Python driver, a new model does **not** require a new bitstream.

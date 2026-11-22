# Heterogeneous SoC CNN Accelerator

This EECS 298 project explores FPGA acceleration on the AMD Kria KR260. It implements two CNN convolution layers in programmable logic and uses a PYNQ notebook to compare two FPGA matrix multiplication kernels against NumPy on the processor.

## Repository contents

| Path | Contents |
| --- | --- |
| `HLS_src/` | C++ HLS implementations of two 5×5 convolution layers and two 16×16 integer matrix multiplication kernels. |
| `bit_hwh_files/` | KR260 bitstreams and matching hardware handoff files generated with Vivado 2025.1. The `fasterConv` overlay exposes `conv1_accel_0` and `conv3_accel_0`. |
| `JupyterNotebooks/matMul.ipynb` | Correctness checks and timing comparisons for the baseline and optimized matrix multiplication overlays. |

## Run the matrix multiplication benchmark

1. Copy or clone this repository onto a KR260 with PYNQ, Jupyter, and NumPy installed.
2. Open `JupyterNotebooks/matMul.ipynb` in Jupyter and select the PYNQ kernel. Keep the notebook and `bit_hwh_files/` in their repository locations so the notebook can find the overlays.
3. Run the cells in order. The notebook programs the FPGA with each overlay, compares its results with NumPy, and reports average latency.

The convolution overlay can be loaded separately with PYNQ. The repository includes the two hardware kernels and their overlay. End-to-end CNN inference requires additional host code, model parameters, and input data.

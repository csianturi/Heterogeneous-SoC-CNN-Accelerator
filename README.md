# Heterogeneous SoC CNN Accelerator

This EECS 298 repository contains two independent FPGA accelerator efforts for the AMD Kria KR260.

- **CNN convolution.** The `fasterConv` overlay implements two 5×5 convolution layers in programmable logic. PYNQ can load the overlay directly. End-to-end CNN inference requires additional host code, model parameters, and input data.
- **Matrix multiplication.** Two standalone overlays implement baseline and optimized 16×16 integer matrix multiplication. A PYNQ notebook checks both implementations against NumPy and measures their latency.

## Repository contents

| Path | Contents |
| --- | --- |
| `HLS_src/` | C++ HLS implementations of two 5×5 convolution layers and two 16×16 integer matrix multiplication kernels. |
| `bit_hwh_files/` | KR260 bitstreams and matching hardware handoff files generated with Vivado 2025.1. The `fasterConv` overlay exposes `conv1_accel_0` and `conv3_accel_0`. |
| `JupyterNotebooks/matMul.ipynb` | Correctness checks and timing comparisons for the baseline and optimized matrix multiplication overlays. |
| `JupyterNotebooks/optimizedImplementation.ipynb` | CNN host-side prototype with image padding, ReLU, and max pooling in NumPy. |

## CNN notebook

The notebook expects a local `images.bin` containing a 16-byte header followed by 10,000 grayscale images of 28×28 bytes each. It currently covers preprocessing and NumPy helpers. Running the CNN on the FPGA requires adding PYNQ overlay calls and computing the intermediate tensors referenced by the pooling checks.

## Run the matrix multiplication benchmark

1. Copy or clone this repository onto a KR260 with PYNQ, Jupyter, and NumPy installed.
2. Open `JupyterNotebooks/matMul.ipynb` in Jupyter and select the PYNQ kernel. Keep the notebook and `bit_hwh_files/` in their repository locations so the notebook can find the overlays.
3. Run the cells in order. The notebook programs the FPGA with each overlay, compares its results with NumPy, and reports average latency.

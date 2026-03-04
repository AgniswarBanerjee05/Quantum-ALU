![zenodo 18866275](https://github.com/user-attachments/assets/0bf38250-a68c-43c0-8f5f-7414e579a71d) Quantum ALU (QALU)


A fully quantum Arithmetic Logic Unit (ALU) built with [Qiskit](https://qiskit.org/). The QALU implements 16 classical ALU operations as reversible quantum circuits, verifies correctness through exhaustive simulation, and measures quantum resource metrics (T-gate count, CX-gate count, circuit depth).

---

## Table of Contents

- [Overview](#overview)
- [Supported Operations](#supported-operations)
- [Requirements](#requirements)
- [Repository Structure](#repository-structure)
- [Usage](#usage)
- [Quantum Metrics](#quantum-metrics)
- [License](#license)

---

## Overview

Classical ALUs are the computational heart of every processor. This project recreates that functionality inside a quantum circuit, mapping each arithmetic and logical operation to a reversible gate network built from standard quantum gates (Toffoli/CCX, MCX, CX, X, etc.).

For every operation the notebook:

1. **Builds** the corresponding quantum circuit using `build_qalu_circuit()`.
2. **Simulates** it exhaustively with Qiskit Aer across all possible input combinations.
3. **Verifies** each result against a classical reference implementation (`classical_alu()`).
4. **Records** quantum resource metrics (T-count, CX-count, depth) and plots them.

Status flags — Zero (Z), Carry (C), Overflow (V), and Negative (N) — are computed and returned alongside the result register.

---

## Supported Operations

| Opcode | Operation          | Description                          |
|--------|--------------------|--------------------------------------|
| ADD    | Addition           | A + B with carry/overflow flags      |
| SUB    | Subtraction        | A − B with borrow/overflow flags     |
| AND    | Bitwise AND        | A AND B                              |
| OR     | Bitwise OR         | A OR B                               |
| XOR    | Bitwise XOR        | A XOR B                              |
| NOTA   | Bitwise NOT        | NOT A                                |
| NAND   | Bitwise NAND       | NOT (A AND B)                        |
| NOR    | Bitwise NOR        | NOT (A OR B)                         |
| XNOR   | Bitwise XNOR       | NOT (A XOR B)                        |
| SHL    | Shift Left         | A << 1                               |
| SHR    | Shift Right        | A >> 1                               |
| ROTL   | Rotate Left        | Circular left rotation of A          |
| ROTR   | Rotate Right       | Circular right rotation of A         |
| PASSA  | Pass A             | Output = A (identity / wire through) |
| PASSB  | Pass B             | Output = B (identity / wire through) |
| CMP    | Compare            | Compare A and B, set flags           |

---

## Requirements

| Package        | Version   |
|----------------|-----------|
| Python         | ≥ 3.9     |
| qiskit         | ≥ 2.0     |
| qiskit-aer     | ≥ 0.14    |
| numpy          | ≥ 1.24    |
| pandas         | ≥ 2.0     |
| matplotlib     | ≥ 3.7     |
| pylatexenc     | ≥ 2.10    |

Install all dependencies with:

```bash
pip install qiskit qiskit-aer numpy pandas matplotlib pylatexenc
```

---

## Repository Structure

```
Quantum-ALU/
│
├── QALU_Final.ipynb          # Main Jupyter notebook — circuit construction,
│                             #   simulation, verification, and plotting
│
├── Circuit Diagram.png       # High-level diagram of the full QALU circuit
│
├── T-Count Scaling Plot.png  # Plot showing how T-gate count scales with
│                             #   operand bit-width (n) for each operation
│
├── QALU Tables/              # Summary tables of quantum resource metrics
│   ├── Table 1.png           #   Gate counts and circuit depth per operation
│   └── Table 2.png           #   T-count breakdown and flag logic costs
│
├── alu_figures_n4/           # Per-operation simulation results for n = 4 bits
│   ├── n4_ADD_R.png          #   Result register heatmap — ADD
│   ├── n4_ADD_FLAGS.png      #   Flag register heatmap — ADD
│   ├── n4_SUB_R.png          #   Result register heatmap — SUB
│   ├── n4_SUB_FLAGS.png      #   Flag register heatmap — SUB
│   ├── n4_AND_R.png          #   … (same pattern for all 16 operations)
│   ├── n4_AND_FLAGS.png
│   ├── n4_OR_R.png
│   ├── n4_OR_FLAGS.png
│   ├── n4_XOR_R.png
│   ├── n4_XOR_FLAGS.png
│   ├── n4_NOTA_R.png
│   ├── n4_NOTA_FLAGS.png
│   ├── n4_NAND_R.png
│   ├── n4_NAND_FLAGS.png
│   ├── n4_NOR_R.png
│   ├── n4_NOR_FLAGS.png
│   ├── n4_XNOR_R.png
│   ├── n4_XNOR_FLAGS.png
│   ├── n4_SHL_R.png
│   ├── n4_SHL_FLAGS.png
│   ├── n4_SHR_R.png
│   ├── n4_SHR_FLAGS.png
│   ├── n4_ROTL_R.png
│   ├── n4_ROTL_FLAGS.png
│   ├── n4_ROTR_R.png
│   ├── n4_ROTR_FLAGS.png
│   ├── n4_PASSA_R.png
│   ├── n4_PASSA_FLAGS.png
│   ├── n4_PASSB_R.png
│   ├── n4_PASSB_FLAGS.png
│   ├── n4_CMP_R.png
│   ├── n4_CMP_FLAGS.png
│   └── n4_CMP_CMP.png
│
└── LICENSE                   # MIT License
```

### Key file descriptions

| File / Folder | Purpose |
|---|---|
| `QALU_Final.ipynb` | End-to-end implementation. Open and run top-to-bottom to reproduce all results. |
| `Circuit Diagram.png` | Visual overview of the qubit registers and gate structure of the QALU. |
| `T-Count Scaling Plot.png` | Shows how the T-gate cost of each operation grows with the number of qubits n. |
| `QALU Tables/` | Static images of the resource-metric tables generated by the notebook. |
| `alu_figures_n4/` | Heatmap images produced by the exhaustive simulation for 4-bit operands. `*_R` files show the result register; `*_FLAGS` files show the Z/C/V/N flag register. |

---

## Usage

1. **Clone the repository**

   ```bash
   git clone https://github.com/AgniswarBanerjee05/Quantum-ALU.git
   cd Quantum-ALU
   ```

2. **Install dependencies** (see [Requirements](#requirements))

3. **Open the notebook**

   ```bash
   jupyter notebook QALU_Final.ipynb
   ```

4. **Run all cells** — the notebook will:
   - Define all helper functions and gate primitives
   - Build the QALU circuit for the desired bit-width `n`
   - Run exhaustive simulation with Qiskit Aer
   - Verify all outputs against the classical reference
   - Save figures to `alu_figures_n4/` and print metric tables

---

## Quantum Metrics

The notebook tracks the following resource metrics for each operation:

| Metric | Description |
|---|---|
| **T-count** | Number of T (π/8) gates — the dominant cost in fault-tolerant quantum computing |
| **CX-count** | Number of CNOT (CX) gates |
| **Depth** | Longest path through the circuit (critical path length) |

These metrics are collected via `t_count_metrics()` after transpiling each circuit to a basis gate set, and are summarised in `QALU Tables/`.

---

## License

This project is licensed under the [MIT License](LICENSE).

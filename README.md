# VQE Hardware Benchmark: IBM Heron Processors (r1 vs r2)

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Qiskit](https://img.shields.io/badge/Qiskit-1.0%2B-purple)
![License](https://img.shields.io/badge/License-MIT-green)

**A comparative analysis of Variational Quantum Eigensolver (VQE) performance across IBM Quantum's "Heron" processor generations.**

## 📖 Overview
This project benchmarks the fidelity of hybrid quantum-classical algorithms on utility-scale hardware. Specifically, it implements the **Variational Quantum Eigensolver (VQE)** to simulate the ground state energy of the **Lithium Hydride (LiH)** molecule.

The study compares the performance of the **Heron r1 (`ibm_torino`)** architecture against the newer **Heron r2 (`ibm_fez`, `ibm_marrakesh`)** revision, analyzing the impact of hardware noise versus algorithmic expressibility.

## 🚀 Key Features
* **Hybrid Optimization Loop:** Implements VQE using Qiskit Runtime V2 Primitives (`EstimatorV2`) and the `COBYLA` classical optimizer.
* **Hardware Verification:** Validates the optimized circuit parameters on three physical QPUs: `ibm_fez`, `ibm_marrakesh`, and `ibm_torino`.
* **Benchmarking Analysis:** Decouples device noise from model error by comparing:
    1.  **Exact Classical Solution** (Ground Truth)
    2.  **Noiseless Simulator** (Algorithmic Baseline)
    3.  **Physical Hardware Execution** (Experimental Result)

## 🔬 Experimental Results

Hardware verification was conducted on three IBM Quantum 'Heron' processors. The experiment demonstrated high consistency between the noiseless simulation and physical execution, validating the stability of the latest QPU generation.

| Backend | Processor Type | Energy (Hartree) | Deviation from Sim | Job Duration (Total) |
| :--- | :--- | :--- | :--- | :--- |
| **Simulator** | *Ideal Statevector* | -7.0047 | 0.0000 | *N/A* |
| **ibm_marrakesh** | Heron r2 (156q) | -6.9144 | **0.0903** | 3m 28s |
| **ibm_fez** | Heron r2 (156q) | -6.8223 | 0.1824 | 3m 22s |
| **ibm_torino** | Heron r1 (133q) | -6.7596 | 0.2451 | 3m 18s |

> **Note:** *Job Duration includes queue time, runtime initialization, and network latency. Actual QPU execution time is estimated at <1 second per job.*

### Visual Analysis
![VQE Benchmark Graph](vqe_benchmark_3_backends.png)
*The plot above demonstrates that the hardware results (colored markers) closely align with the simulator baseline (blue dots). The gap between the simulator and the exact classical line (gray) indicates that the primary limitation was the expressibility of the `EfficientSU2` ansatz, not hardware noise.*

## 🧠 Algorithmic Approach
As a Computer Science initiative, this implementation treats chemical simulation as a **hybrid quantum-classical optimization problem**:

1.  **Mapping:** The molecular Hamiltonian is mapped to a qubit operator using the Jordan-Wigner transformation.
2.  **Ansatz:** A parameterized quantum circuit (`EfficientSU2`) prepares a trial wavefunction $|\psi(\theta)\rangle$.
3.  **Optimization:** A classical optimizer (`COBYLA`) iteratively tunes the circuit parameters $\theta$ to minimize the expectation value $\langle \psi(\theta) | H | \psi(\theta) \rangle$.

## 🛠️ Installation

Clone the repository and install the required quantum computing libraries:

```bash
qiskit>=1.0.0
qiskit-ibm-runtime
qiskit-nature
qiskit-algorithms
pyscf
matplotlib
numpy

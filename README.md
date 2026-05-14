# VQE for the Transverse Field Ising Model

Jupyter notebooks implementing the Variational Quantum Eigensolver (VQE) for the 1D Transverse Field Ising Model (TFIM) with Qiskit and the IBM Quantum Runtime. Ground state energies and observables produced by VQE are benchmarked against exact diagonalization via [QuSpin](https://quspin.github.io/QuSpin/).

The TFIM Hamiltonian used throughout is

$$H = -J \sum_{\langle i,j \rangle} Z_i Z_j - g \sum_i X_i,$$

with periodic (PBC) or open (OBC) boundary conditions.

## Notebooks

- **`Ising_model_VQE_IBM_runtime_08_25.ipynb`** — VQE on either `FakeBrisbane` (noise model based on the IBM Brisbane device) or a real IBM backend selected via `QiskitRuntimeService.least_busy`. Uses a symmetry-preserving PBC ansatz (`Rxx`, `Ryy`, `Rzz` brick layers + transverse `Rx`), `optimization_level=3` transpilation with `sabre` routing, and dynamical decoupling (`XGate, XGate`) when running on hardware. COBYLA minimizes the energy via `EstimatorV2` over a sweep of transverse field strengths $g$.

- **`Ising_model_VQE_Oct_25.ipynb`** — Noiseless `AerSimulator` study comparing PBC and OBC symmetry-preserving ansätze across a sweep $g \in [-2.5, 2.5]$. Records per-iteration cost history through a SciPy callback, then evaluates ground-state energy error and overlap fidelity against QuSpin.

- **`Ising_sb_VQE_v3.ipynb`** — Symmetry-broken VQE with a small $Z_{N-1}$ pinning field $h$ to lift the $\mathbb{Z}_2$ degeneracy. Computes the per-site magnetization $\langle M_z \rangle / N$ for $N \in \{4, 6, 8, 10, 12\}$, then performs a $1/N$ finite-size extrapolation to estimate the $N \to \infty$ value and compares against exact diagonalization.

## Requirements

- `qiskit`
- `qiskit-ibm-runtime`
- `qiskit-aer`
- `numpy`, `scipy`, `matplotlib`
- `quspin` (for the exact-diagonalization comparison cells)

Running on real IBM hardware additionally requires a configured `QiskitRuntimeService` account. The IBM Open Plan does not support `Session()`-based execution; switch the `USE_SIMULATOR` flag to `True` (or use a paid plan) accordingly.

## Usage

Open any notebook in Jupyter and run cells top to bottom. Key knobs:

- `N` — number of qubits / spins
- `gs` — array of transverse field strengths to sweep
- `num_layers` — depth of the ansatz (defaults to `N // 2`)
- `PBC` — boundary condition toggle (in the Oct '25 notebook)
- `USE_SIMULATOR` — backend toggle (in the Aug '25 notebook)

# GQAT-Energy-Aware-Quantum-Benchmarking
# Defining the Green Quantum Advantage Threshold (GQAT)
### An Empirical Model for Energy-Aware Quantum Benchmarking

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://python.org)
[![Qiskit](https://img.shields.io/badge/Qiskit-2.3.1-purple)](https://qiskit.org)
[![Qiskit Aer](https://img.shields.io/badge/Qiskit%20Aer-0.17.2-blueviolet)](https://github.com/Qiskit/qiskit-aer)
[![CodeCarbon](https://img.shields.io/badge/CodeCarbon-3.2.6-green)](https://codecarbon.io)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Google%20Colab-orange)](https://colab.research.google.com)

📄 Overview

This repository contains the complete experimental code, results, and figures
for the IEEE conference paper:

> Defining the Green Quantum Advantage Threshold: An Empirical Model for Energy-Aware Quantum Benchmarking
> Nwoke Victor Kelechukwu
> Department of Computer Engineering, University of Uyo
> Uyo, Akwa Ibom State, Nigeria

The paper introduces the **Green Quantum Advantage Threshold (GQAT)** — a
novel dimensionless metric that benchmarks quantum computers on both
**performance and energy efficiency** simultaneously, filling a critical
gap in existing quantum benchmarking frameworks.


🔑 Key Concept

Current quantum advantage claims focus purely on speed. GQAT redefines
advantage to require energy efficiency as well:
GQAT = (Q_perf / C_perf) × (C_energy / Q_energy)
| GQAT Value | Interpretation |
|---|---|
| GQAT > 1 | True green quantum advantage |
| GQAT = 1 | Threshold boundary |
| GQAT < 1 | Quantum is energy-inefficient vs classical |

---

## 🧪 Experimental Setup

| Component | Details |
|---|---|
| **Platform** | Google Colaboratory (free tier) |
| **Runtime** | NVIDIA Tesla T4 GPU |
| **Host Machine** | HP Folio 9470m (Intel Core i5-3437U, 8GB RAM, Windows 11 Pro) |
| **Location** | Uyo, Akwa Ibom State, Nigeria |
| **Quantum Framework** | Qiskit 2.3.1 + Qiskit Aer 0.17.2 |
| **Energy Tracker** | CodeCarbon 3.2.6 |
| **Benchmark Task** | VQE on H₂ Hamiltonian (STO-3G, 4 qubits) |
| **Classical Baseline** | SciPy exact diagonalisation (eigsh) |

---

📊 Key Results

| Shots | Q_perf (s) | C_perf (s) | Q_energy (J) | C_energy (J) | GQAT |
|---|---|---|---|---|---|
| 100 | 2.8074 | 9.85×10⁻⁴ | 64,746.84 | 0.3139 | ≈ 0 |
| 500 | 2.3952 | 9.85×10⁻⁴ | 55,240.26 | 0.3139 | ≈ 0 |
| 1000 | 2.3919 | 9.85×10⁻⁴ | 55,162.52 | 0.3139 | ≈ 0 |
| 5000 | 2.6353 | 9.85×10⁻⁴ | 60,777.25 | 0.3139 | ≈ 0 |

**Quantum-to-classical energy ratio: ~1.76 × 10⁵ : 1**

The GQAT ≈ 0 result confirms that current superconducting quantum systems
operate far below the green advantage threshold, driven primarily by
cryogenic overhead (~23 kW continuous power draw).

---

📁 Repository Structure
GQAT-Energy-Aware-Quantum-Benchmarking/
│
├── 📓 GQAT_Research.ipynb        ← Full experiment notebook (Google Colab)
│
├── 📊 results/
│   └── gqat_results.csv          ← Raw experimental results
│
├── 🖼️ figures/
│   └── gqat_figures.png          ← Publication figures (3 plots)
│
├── 📄 paper/
│   └── GQAT_IEEE_Paper.pdf       ← Final published paper (add after publication)
│
└── 📋 README.md                  ← This file
---

🚀 How to Run

Option 1 — Google Colab (Recommended)

1. Click the badge below to open the notebook directly in Colab:

https://colab.research.google.com/drive/1hKzAsH03aRMOR_8QJdz309ac-9vyZNou#scrollTo=-IYhHv5_LYy3


2. Run all cells in order from top to bottom
3. Results will be printed and saved automatically

Option 2 — Local (Advanced)

```bash
# Clone the repository
git clone https://github.com/YourUsername/GQAT-Energy-Aware-Quantum-Benchmarking.git
cd GQAT-Energy-Aware-Quantum-Benchmarking

# Install dependencies
pip install qiskit==2.3.1 qiskit-aer==0.17.2 codecarbon==3.2.6
pip install numpy scipy matplotlib pandas jupyter

# Launch notebook
jupyter notebook GQAT_Research.ipynb
```

> ⚠️ **Note:** Running locally requires Linux for full CodeCarbon RAPL
> support. On Windows/Mac, CodeCarbon will use software-based energy
> estimation (same as Colab).

---

## 📦 Dependencies
qiskit==2.3.1
qiskit-aer==0.17.2
codecarbon==3.2.6
scipy==1.16.3
numpy
matplotlib==3.10.0
pandas
jupyter
Install all at once:
```bash
pip install qiskit qiskit-aer codecarbon numpy scipy matplotlib pandas jupyter -q
```

---

## 🏗️ Three-Layer Energy Model

A core contribution of this work is a structured energy accounting
model for superconducting quantum hardware:

| Layer | Components | Power Draw | Method |
|---|---|---|---|
| L1: Cryogenic | Dilution fridge, pulse tubes | 15–25 kW continuous | Amortised over job duration |
| L2: Control Electronics | Microwave generators, ADC/DAC | 2–5 kW per rack | Proportional to qubit count |
| L3: Classical Co-processing | Error mitigation, COBYLA optimiser | 100–500 W | Measured via CodeCarbon |

---

## 📈 Figures

<img width="1490" height="494" alt="GQAT_FIGURES" src="https://github.com/user-attachments/assets/d9530d60-6dac-4cb0-8bfd-59cc46d0671d" />


*Left: GQAT score vs shot count. Centre: Total quantum energy vs shot count.
Right: VQE fidelity error vs shot count.*

---

🔬 Methodology

1. **Classical baseline:** Exact diagonalisation of H₂ Hamiltonian using
   SciPy `eigsh` with CodeCarbon energy tracking
2. **Quantum simulation:** VQE with `efficient_su2` ansatz (2 reps) on
   Qiskit Aer with custom depolarising + thermal relaxation noise model
3. **Energy accounting:** Layer 3 measured via CodeCarbon; Layers 1 & 2
   modelled theoretically from IBM published hardware specifications
4. **GQAT computation:** Applied across shot counts {100, 500, 1000, 5000}

---

## 📚 Citation

If you use this code or methodology in your research, please cite:

```bibtex
@inproceedings{nwoke2026gqat,
  author    = {Nwoke, Victor Kelechukwu},
  title     = {Defining the Green Quantum Advantage Threshold:
               An Empirical Model for Energy-Aware Quantum Benchmarking},
  booktitle = {Proceedings of the IEEE International Conference},
  year      = {2026},
  address   = {Uyo, Akwa Ibom State, Nigeria},
  note      = {Code: https://github.com/Bastilista/GQAT-Energy-Aware-Quantum-Benchmarking}
}
```


📖 References

Key papers this work builds on:

- Arute et al. (2019) — Quantum supremacy, *Nature*
- Cross et al. (2019) — Quantum Volume, *Physical Review A*
- Peruzzo et al. (2014) — VQE, *Nature Communications*
- Jaschke & Montangero (2023) — Green quantum advantage, *Quantum Science and Technology*
- Fellous-Asiani et al. (2021) — Resource constraints, *PRX Quantum*
- Courty et al. (2024) — CodeCarbon, *Zenodo*


👤 Author

**Nwoke Victor Kelechukwu**
Department of Computer Engineering
University of Uyo, Nigeria
📧 victorkelech6@gmail.com


🌍 Reproducibility Statement

This research was conducted entirely using free, open-source tools on
Google Colaboratory's free tier, accessed remotely from Uyo, Nigeria.
No paid cloud services, real quantum hardware, or institutional HPC
resources were required. The full experiment can be reproduced by any
researcher worldwide with a Google account.

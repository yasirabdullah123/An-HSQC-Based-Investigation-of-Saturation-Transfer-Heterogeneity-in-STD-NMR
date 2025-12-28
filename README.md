# STD-NMR Saturation Transfer Heterogeneity Analysis

**A computational framework for investigating atomic-resolution saturation transfer kinetics in STD-NMR spectroscopy**

[![Julia](https://img.shields.io/badge/Julia-1.9+-9558B2?style=flat&logo=julia)](https://julialang.org/)
[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat&logo=python)](https://python.org/)

## Overview

This repository contains the complete computational infrastructure for a groundbreaking MSc dissertation that challenges fundamental assumptions in STD-NMR spectroscopy used for fragment-based drug discovery. The research reveals **systematic artifacts in standard protocols** that create false-negative rates exceeding 10% in pharmaceutical screening, with particularly severe bias against aromatic binding sites.

### Key Findings

- **8-fold kinetic heterogeneity** across protein atomic sites (0.563-4.635 s⁻¹)
- **60% of aromatic sites** classified as blind spots under standard conditions
- **Novel RBH algorithm** with >95% assignment reliability

### Research Impact

This work transforms STD-NMR from assumption-based to evidence-based methodology, with direct implications for:
- Pharmaceutical screening protocols (millions in avoided missed hits)
- Fragment-based drug discovery optimization
- Biophysical method validation standards

---

## Repository Structure

```
├── src/                                    # Source code
│   ├── assignment/
│   │   ├── peak_assignment_rbh.jl         # Reciprocal Best Hit peak assignment algorithm
│   │   └── utils/
│   │       └── nmr_analysis_utils.jl      # Core NMR processing utilities
│   ├── analysis/
│   │   ├── saturation_time_analysis.jl    # Temporal kinetics optimization (0.1-5.0 s)
│   │   ├── pulse_shape_analysis.jl        # Gaussian/E-BURP1/I-BURP2 comparison
│   │   └── frequency_offset_analysis.jl   # Blind spot mapping (-0.5 to 8.5 ppm)
│   └── visualization/
│       ├── generate_publication_figures.jl # Master figure generation pipeline
│       ├── generate_frequency_heatmaps.jl  # Frequency-dependent saturation maps
│       ├── generate_pulse_heatmaps.jl      # Pulse shape comparison visualizations
│       └── generate_time_heatmaps.jl       # Temporal saturation kinetics maps
├── data/
│   ├── raw_experiments/                    # 301 Bruker NMR experiments
│   ├── example/                            # Example dataset (experiment 31)
│   └── *.tsv                               # Peak assignments and master lists
├── output/
│   ├── figures/                            # Publication-ready figures
│   └── pymol_scripts/
│       └── generate_std_nmr_optimizer.py   # 3D molecular visualization automation
└── docs/                                   # Documentation and manuscripts
```

---

## Quick Start

### Prerequisites

- **Julia** ≥ 1.9 with packages: `NMRTools`, `Plots`, `StatsPlots`, `LsqFit`, `Measurements`
- **Python** ≥ 3.8 with `PyMOL` for 3D visualization
- **ImageMagick** for figure processing

### Installation

```bash
git clone https://github.com/yasirabdullah123/An-HSQC-Based-Investigation-of-Saturation-Transfer-Heterogeneity-in-STD-NMR.git
cd An-HSQC-Based-Investigation-of-Saturation-Transfer-Heterogeneity-in-STD-NMR

# Install Julia dependencies
julia --project=. -e 'using Pkg; Pkg.instantiate()'
```

### Running Analyses

```bash
# Parameter optimization studies
cd src/analysis
julia saturation_time_analysis.jl      # Temporal kinetics
julia pulse_shape_analysis.jl          # Pulse uniformity
julia frequency_offset_analysis.jl     # Blind spot identification

# Generate publication figures
cd ../visualization
julia generate_publication_figures.jl

# 3D molecular visualizations
cd ../../output/pymol_scripts
python generate_std_nmr_optimizer.py
```

---

## Key Algorithms

### 1. Reciprocal Best Hit (RBH) Peak Assignment

**Location:** `src/assignment/peak_assignment_rbh.jl`

Novel bidirectional verification algorithm for robust HSQC peak assignment:
- Bidirectional verification ensuring >95% reliability
- Normalized Euclidean distance: `d = sqrt((Δδ_H/0.1)² + (Δδ_C/1.0)²)`
- Systematic chemical shift corrections for temperature/experimental conditions

### 2. Δ-Σ Diagnostic Framework

**Implemented in:** `src/analysis/frequency_offset_analysis.jl`

Quantitative blind spot identification:
- **Σ** = Total saturation accessibility (aliphatic + aromatic)
- **Δ** = Frequency-dependent bias (aliphatic - aromatic)
- **Critical threshold:** Σ ≥ 0.8 AND Δ < -0.15

### 3. Multi-Exponential Kinetic Fitting

**Core implementation:** `src/utils/nmr_analysis_utils.jl`

Statistical model selection via AICc:
- Mono-exponential: `I(t) = I₀(1 - e^(-kt))`
- Stretched exponential: `I(t) = I₀(1 - e^(-(kt)^β))`
- Quality filters: R² > 0.85, uncertainty < 25%

---

## Data

### Raw NMR Experiments (`data/raw_experiments/`)

**301 systematically designed Bruker experiments:**
- **Temporal series:** 10 time points (0.1-5.0 s) → kinetic heterogeneity quantification
- **Frequency sweep:** 19 offsets (-0.5 to 8.5 ppm) → blind spot mapping
- **Pulse shapes:** Gaussian/E-BURP1/I-BURP2 → uniformity optimization
- **Temperature study:** 283K, 293K, 303K → thermal effects on spin diffusion

### Example Dataset

A complete analysis example is provided in `data/example/experiment_31/`:
- Full frequency sweep (19 saturation offsets)
- HSQC spectra (real/imaginary components)
- PyMOL visualization scripts
- Carbon functional group analysis

---

## Publication Figures

All figures generated by `src/visualization/generate_publication_figures.jl`:

- **Figure 1:** Global Kinetic Portrait (3-panel composite)
- **Figure 2:** Δ-Σ Diagnostic Plot (6 critical blind spots identified)
- **Figure 3:** Frequency Response Profile (average saturation behavior)
- **Figure 4:** Pulse Shape Performance (split raincloud visualization)

**Output:** High-resolution SVG (300 DPI equivalent), standardized I/I₀ color mapping

---

## Citation

If you use this code or methodology in your research, please cite:

```bibtex
@mastersthesis{yasir2025stdnmr,
  author  = {Yasir, Abdullah},
  title   = {An HSQC-Based Investigation of Saturation Transfer Heterogeneity in STD-NMR},
  school  = {University College London},
  year    = {2025},
  type    = {MSc Dissertation},
  supervisor = {Waudby, Christopher A.}
}
```

---

## Author

**Abdullah Yasir**
MSc Drug Discovery and Development, University College London
Supervisor: Dr. Christopher A. Waudby

---

## Acknowledgments

- **Dr. Christopher Waudby** (UCL) - Supervision and NMR expertise
- **BMRB Entry 4562** - Reference chemical shift database
- **NMR community** - Open-source tools and methodologies

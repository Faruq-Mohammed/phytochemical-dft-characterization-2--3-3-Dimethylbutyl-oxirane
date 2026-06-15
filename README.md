# DFT-Based Quantum Chemical Characterization of 2-(3,3-Dimethylbutyl)oxirane

## Electronic Structure, Topological Surface Analysis, and Global Reactivity Descriptors

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Method](https://img.shields.io/badge/Method-B3LYP%2F6--311%2BG(d%2Cp)-blue.svg)]()
[![Software](https://img.shields.io/badge/Software-Gaussian%2009W-green.svg)](https://gaussian.com)
[![Compound](https://img.shields.io/badge/PubChem-CID%20545295-orange.svg)](https://pubchem.ncbi.nlm.nih.gov/compound/545295)

---

## Table of Contents

- [Overview](#overview)
- [Workflow Summary](#workflow-summary)
- [Repository Structure](#repository-structure)
- [Detailed Methodology](#detailed-methodology)
- [Key Mathematical Formalisms](#key-mathematical-formalisms)
- [Key Computational Results](#key-computational-results)
- [Figures](#figures)
- [Tools & Software](#tools--software)
- [Prerequisites](#prerequisites)
- [How to Reproduce This Analysis](#how-to-reproduce-this-analysis)
- [References](#references)
- [Author](#author)
- [License](#license)

---

## Overview

This repository contains the complete computational workflow for the quantum chemical characterization of **2-(3,3-Dimethylbutyl)oxirane**, a bioactive phytochemical isolated from *Drimycarpus racemosus*. Utilizing **Density Functional Theory (DFT)**, this study provides deep insights into the molecular geometry, electronic properties, frontier molecular orbitals (FMO), and molecular electrostatic potential (MEP) surfaces of the compound to evaluate its chemical stability, reactivity profile, and potential as a pharmaceutical lead.

---

## Workflow Summary

```
         PubChem Database
                ↓  (Download 3D SDF)
            GaussView
                ↓  (Structure cleaning, Symmetrization → .mol format)
             Gabedit
                ↓  (Conformational Search via MD: 298.15 K, Berendsen)
         Gaussian (Opt + Freq)
                ↓  (DFT / B3LYP, Basis Set: 6-311+G(d,p))
     ┌──────────┴──────────┐
     ↓                     ↓
 FMO Analysis          MEP Mapping
 (HOMO, LUMO)     (Electrophilic/Nucleophilic Sites)
     │                     │
     └──────────┬──────────┘
                ↓
   Global Reactivity Descriptors
 (Energy Gap, Hardness, Softness, etc.)
```

---

## Repository Structure

```
phytochemical-dft-characterization-2-(3,3-Dimethylbutyl)oxirane/
│
├── README.md
│
├── 01_raw_structure/
│   ├── 545295_raw.sdf                        # Raw 3D SDF downloaded from PubChem
│   └── 545295_clean.mol                      # Symmetrized and cleaned structure
│
├── 02_conformational_search/
│   └── 545295_minimized.mol                  # Low-energy conformer output
│
├── 03_gaussian_calculations/
│   ├── opt_freq_calculation.gjf              # Gaussian input file (Link0, Route card)
│   └── opt_freq_calculation.log              # Comprehensive calculation output log
│
├── 04_fmo_analysis/
│   ├── calculations_descriptors.xlsx         # Spreadsheet for reactivity parameters
│   └── figures/
│       ├── homo_orbital.tif                  # HOMO distribution plot
│       ├── lumo_orbital.tif                  # LUMO distribution plot
│       └── fmo_figure.jpg                    # Combined FMO figure for manuscript
│
├── 05_mep_analysis/
│   └── mep_mapping.jpg                       # Color-mapped MEP surface
│
└── results/
    └── dft_summary_report.docx               # Detailed analysis writeup
```

---

## Detailed Methodology

### Step 1 — Structural Retrieval & Pre-processing

- **Source:** [PubChem](https://pubchem.ncbi.nlm.nih.gov/compound/545295) (CID: 545295)
- **Pre-processing:** The 3D structure was imported into **GaussView**, cleaned for valence errors, and molecular symmetry adjustments were applied. The standardized coordinate structure was exported in `.mol` format.

### Step 2 — Conformational Search & Energy Minimization

- **Tool:** **Gabedit**
- **Method:** Molecular Dynamics (MD) Conformational Search
- **Parameters:**
  - Simulation Temperature: $298.15\ \text{K}$
  - Thermostat Algorithm: Berendsen
- **Objective:** Sampling the conformational space to locate the global minimum-energy conformation, bypassing local minima traps prior to high-level quantum mechanical calculations.

### Step 3 — Quantum Chemical Calculations (DFT)

- **Software:** **Gaussian 09W** (job setup performed via GaussView)
- **Job Type:** Geometry Optimization + Frequency Analysis (`opt+freq`)
- **Computational Specifications:**
  - **Method:** DFT / B3LYP
  - **Basis Set:** `6-311+G(d,p)` — triple-zeta quality with diffuse and polarization functions for high accuracy
  - **Link0 Directives:** `%mem=1GB`, `%nprocshared=4`
- **Validation:** Harmonic frequency analysis confirmed the absence of imaginary frequencies (NImag = 0), establishing the optimized geometry as a true minimum on the potential energy surface.

### Step 4 — Frontier Molecular Orbital (FMO) Analysis

- **Calculations:** $E_{HOMO}$ and $E_{LUMO}$ were extracted directly from the Gaussian output log.
- Orbital isosurface visualizations were generated for the Highest Occupied Molecular Orbital (HOMO) and Lowest Unoccupied Molecular Orbital (LUMO) to assess kinetic stability and charge-transfer characteristics.

### Step 5 — Molecular Electrostatic Potential (MEP) Mapping

- MEP was mapped over the total electron density isosurface to identify reactive sites: **red/orange regions** indicate electron-rich (nucleophilic) zones, while **blue regions** indicate electron-deficient (electrophilic) zones.

---

## Key Mathematical Formalisms

Global reactivity descriptors were derived under **Koopmans' Theorem**, using the frontier molecular orbital energies ($E_{HOMO}$ and $E_{LUMO}$):

| Descriptor | Formula |
|---|---|
| **Energy Gap** | $\Delta E = E_{LUMO} - E_{HOMO}$ |
| **Ionization Potential** | $I = -E_{HOMO}$ |
| **Electron Affinity** | $A = -E_{LUMO}$ |
| **Electronegativity** | $\chi = \dfrac{I + A}{2} = -\dfrac{E_{HOMO} + E_{LUMO}}{2}$ |
| **Chemical Hardness** | $\eta = \dfrac{I - A}{2} = \dfrac{E_{LUMO} - E_{HOMO}}{2}$ |
| **Chemical Softness** | $S = \dfrac{1}{2\eta}$ |
| **Electrophilicity Index** | $\omega = \dfrac{\chi^2}{2\eta}$ |

---

## Key Computational Results

> **Note:** Values in the *Location* column are descriptive annotations and should be updated to reflect the actual orbital distribution after visual inspection in GaussView.

| Parameter | Value (Hartree) | Value (eV) | Interpretation |
|---|:---:|:---:|---|
| **$E_{HOMO}$** | −0.2741 | −7.4586 | Primary electron-donating site: *(update after FMO visualization)* |
| **$E_{LUMO}$** | −0.0012 | −0.0332 | Primary electron-accepting site: *(update after FMO visualization)* |
| **Energy Gap ($\Delta E$)** | — | 7.4254 | Large gap indicates high kinetic stability and low reactivity |
| **Chemical Hardness ($\eta$)** | — | 3.7127 | High hardness reflects strong resistance to electronic polarization |
| **Chemical Softness ($S$)** | — | 0.1347 | Low softness implies limited polarizability and intermolecular interaction capacity |
| **Electrophilicity Index ($\omega$)** | — | 1.8897 | Moderate electrophilic character; indicates tendency to accept electron density |

---

## Figures

| Asset | Description |
|---|---|
| `04_fmo_analysis/figures/homo_orbital.jpg` | 3D isosurface of the HOMO showing the spatial distribution of electron-donating density |
| `04_fmo_analysis/figures/lumo_orbital.jpg` | 3D isosurface of the LUMO showing the spatial distribution of electron-accepting density |
| `04_fmo_analysis/figures/fmo_figure.jpg` | Combined HOMO–LUMO panel prepared for manuscript inclusion |
| `05_mep_analysis/mep_mapping.jpg` | MEP surface mapped over electron density: red = nucleophilic, blue = electrophilic zones |

---

## Tools & Software

| Tool / Software | Version | Primary Use |
|---|---|---|
| PubChem Database | — | 3D coordinate structure retrieval |
| GaussView | 6.0.16 | Structure visualization, job setup, and FMO mapping |
| Gabedit | — | Conformational space sampling via Molecular Dynamics |
| Gaussian 09W | Rev. D.01 | Core quantum chemistry engine — geometry optimization and frequency analysis |

---

## Prerequisites

Before attempting to reproduce this analysis, ensure the following software is installed and licensed:

- **GaussView 6** — for structure visualization and Gaussian input preparation
- **Gaussian 09W** — licensed quantum chemistry package (requires institutional or personal license)
- **Gabedit** — freely available at [gabedit.sourceforge.net](http://gabedit.sourceforge.net/)
- A system with at least **1 GB RAM** and **4 CPU cores** available for the Gaussian job (as specified in Link0 directives)

---

## How to Reproduce This Analysis

1. **Verify Structure:** Open `01_raw_structure/545295_clean.mol` in GaussView to confirm valence correctness and symmetry elements.

2. **Conformational Search:** Load the cleaned structure in Gabedit and execute a Molecular Dynamics conformational search using the Berendsen thermostat at $298.15\ \text{K}$. Export the lowest-energy conformer.

3. **Run Gaussian Calculation:** Submit the pre-configured input file `03_gaussian_calculations/opt_freq_calculation.gjf` to Gaussian 09W for geometry optimization and frequency analysis.

4. **Verify Convergence:** Inspect `opt_freq_calculation.log` and confirm `NImag = 0` to validate that the optimized structure is a true energy minimum.

5. **Extract FMO Energies:** Search the log file for the keywords `Alpha occ.` / `Alpha virt.` or use GaussView's MO viewer to retrieve $E_{HOMO}$ and $E_{LUMO}$ values.

6. **Generate MEP Surface:** Using GaussView or Gabedit, compute the electrostatic potential on a cube grid and render the color-mapped MEP isosurface.

7. **Calculate Descriptors:** Enter the extracted FMO energies into `04_fmo_analysis/calculations_descriptors.xlsx` to auto-calculate all global reactivity parameters.

---

## References

1. Becke, A. D. (1993). Density-functional thermochemistry. III. The role of exact exchange. *Journal of Chemical Physics*, 98(7), 5648–5652.
2. Lee, C., Yang, W., & Parr, R. G. (1988). Development of the Colle-Salvetti correlation-energy formula into a functional of the electron density. *Physical Review B*, 37(2), 785–789.
3. Koopmans, T. (1934). Über die Zuordnung von Wellenfunktionen und Eigenwerten zu den Einzelnen Elektronen Eines Atoms. *Physica*, 1(1–6), 104–113.
4. Parr, R. G., & Yang, W. (1989). *Density-Functional Theory of Atoms and Molecules*. Oxford University Press.
5. Frisch, M. J., et al. (2009). *Gaussian 09, Revision D.01*. Gaussian, Inc., Wallingford CT.

---

## Author

**Faruq Mohammed** — B.Pharm, M.Pharm (ongoing)

Research Interests: Computer-Aided Drug Design (CADD) · Bioinformatics · Pharmacology · Phytochemical Research · Alternative Medicine · Pharmaceutical Technology

- 📧 Email: [faruqmdtashriq@gmail.com](mailto:faruqmdtashriq@gmail.com)
- 🔗 LinkedIn: [Faruq Mohammed Tashriq](https://www.linkedin.com/in/faruq-mohammed-tashriq/)
- 🐙 GitHub: [Faruq-Mohammed](https://github.com/Faruq-Mohammed/)

---

## License

This project is licensed under the [MIT License](LICENSE).

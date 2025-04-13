# methods.md

## Simulation Framework

This file provides a detailed breakdown of the simulation methodology used in the preprint:
**"Preclinical Investigation of Resveratrol and CAR-Macrophage Synergy for IDH1-Mutant Glioblastoma"**

---

## 🧠 Multi-Scale Model Architecture

### 1. Agent-Based Model (ABM)
- **Platform:** CompuCell3D (v4.2.5)
- **Cell Types:**
  - GBM cells (IDH1-MUT, WT, Resistant)
  - CAR-Macrophages
  - Resident Macrophages
- **Behaviours Implemented:**
  - Proliferation: Based on local oxygen/nutrient levels
  - Migration: Random walk with chemotaxis toward GBM antigens
  - Phagocytosis: Conditional on antigen level + CAR-M state
  - Polarisation: M0→M1/M2 transition based on cytokine field
  - Exhaustion: Energy pool model linked to ODE states

### 2. Ordinary Differential Equations (ODE)
- **Purpose:** Intracellular metabolism and immunomodulation
- **IDH1-Mutant GBM Cells:**
  - 2-HG accumulation pathway
  - NAD+/NADH dynamics
  - SIRT1 activity
  - Antigen expression modulation (IL13Rα2, EGFR)
- **CAR-Macrophages:**
  - NAD+ restoration via Resveratrol
  - Energy depletion and exhaustion logic

### 3. Reaction-Diffusion Model
- **Species Simulated:** O2, Glucose, Lactate, 2-HG, Resveratrol (free & exosomal), Cytokines
- **Equations:** Finite difference discretization of:
  $$ \frac{\partial C}{\partial t} = D \nabla^2 C + R $$
- **Diffusion:** Coefficients adjusted for ECM tortuosity
- **Release:** pH-dependent Resveratrol exosome release
- **Decay:** Based on molecular half-lives

---

## 🔁 Simulation Procedure

1. Initialise spatial domain (2D lattice, 100x100 px)
2. Seed tumour cells and macrophages
3. Load initial concentration fields
4. For each timestep:
   - Update diffusion fields
   - Query microenvironment values (each agent)
   - Solve ODEs (per agent)
   - Apply ABM actions (grow, migrate, phagocytose)
   - Update output metrics
5. Record tumour size, antigen levels, 2-HG, M1/M2 ratio, CAR-M activity

---

## 📊 Monte Carlo Sensitivity Analysis
- **Runs:** 250
- **Method:** Parameter sampling via Latin Hypercube
- **Inputs Varied:** Resveratrol dose, CAR-M persistence, 2-HG inhibition, kill capacity
- **Outputs:** Tumour reduction %, synergy index, 95% CI

---

## 📁 Key Files (see `/models/` directory)
- `abm_logic_summary.txt` – Core ABM rules
- `ode_equations_summary.txt` – Metabolic reaction network
- `reaction_diffusion_overview.txt` – Diffusion equations and parameters
- `supplementary/S1_simulation_parameters.csv` – Parameter values and ranges

---

## 🧾 Notes
- Key simulation logic, parameter files, and structured model documentation are provided here. A reproducible codebase is under reconstruction and will be shared post-publication in phases.
- For custom adaptations or early access, contact: contact@themackinstitute.org

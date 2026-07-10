---
title: "5. Ab Initio Multiple Spawning: Electronic Structure and Nonadiabatic Dynamics"
questions:
  - "What electronic-structure methods describe excited states and conical intersections?"
  - "How does Ab Initio Multiple Spawning (AIMS) propagate nuclear wavepackets through them?"
  - "How do we run these calculations in practice with OpenMolcas and PySpawn?"
objectives:
  - "Understand the multiconfigurational methods (CASSCF, CASPT2, RASSI) used for excited states."
  - "Run the full set of OpenMolcas excited-state examples hands on."
  - "Understand the AIMS/spawning algorithm and its spawning criterion."
  - "Run and analyze a full PySpawn AIMS simulation, from initial conditions to populations."
keypoints:
  - "Conical intersections are the funnels of nonadiabatic dynamics; describing them needs multireference methods."
  - "AIMS represents the nuclear wavepacket with traveling Gaussians that spawn near strong coupling."
  - "OpenMolcas supplies the on-the-fly electronic structure; PySpawn drives the spawning dynamics."
---

# Ab Initio Multiple Spawning: Electronic Structure and Nonadiabatic Dynamics

> ## Overview
>
> **Teaching:** 120 min (two 60 min lectures)
> **Exercises:** 300 min (two hands-on sessions)
>
> **Questions**
> - What electronic-structure methods describe excited states and conical intersections?
> - How does Ab Initio Multiple Spawning propagate nuclear wavepackets through them?
> - How do we run these calculations in practice?
>
> **Objectives**
> - Understand the multiconfigurational methods behind excited-state calculations.
> - Run the OpenMolcas excited-state examples hands on.
> - Understand the AIMS spawning algorithm.
> - Run and analyze a full PySpawn AIMS simulation.

This session pairs the **electronic structure** of excited states with the **nonadiabatic dynamics**
that unfolds on them. We first cover the multiconfigurational methods used to compute excited-state
energies, couplings, and spectra, and practice them in OpenMolcas. We then turn to Ab Initio
Multiple Spawning (AIMS), the method that propagates nuclear wavepackets through conical
intersections, and run it end to end in PySpawn.

Two repositories are used throughout. **Clone both** into your working directory before starting:

```bash
git clone https://github.com/compchem-cybertraining/Tutorials_OpenMolcas.git
git clone https://github.com/compchem-cybertraining/Tutorials_PySpawn.git
```

Everything referenced below (inputs, reference outputs, analysis scripts, and per-example READMEs)
lives in those two repositories.

---

# Table of Contents

1. [Lecture: Theory of Multireference Electronic Structure Calculations (60 min)](#1)
2. [Hands On: OpenMolcas Excited-State Tutorials](#2)
3. [Lecture: Ab Initio Multiple Spawning (60 min)](#3)
4. [Hands On: PySpawn AIMS Dynamics](#4)

---

## 1. Lecture: Theory of Multireference Electronic Structure Calculations (60 min) <a name="1"></a>

[Back to TOC](#table-of-contents)

A 60 minute lecture on the multiconfigurational electronic-structure methods used throughout the
OpenMolcas tutorial. The goal is to understand what each method computes, when it is needed, and how
to read its output, before running them in the hands-on session.

Topics covered:

- **Why single-reference methods fail** near excited states and conical intersections, and the idea
  of a multiconfigurational wavefunction.
- **CASSCF** (Complete Active Space SCF): active-space selection, state averaging, and why the
  choice of active orbitals dominates the quality of the result.
- **CASPT2 / XMS-CASPT2**: adding dynamic correlation on top of CASSCF for quantitative excitation
  energies, and the role of the IPEA and imaginary shifts.

These methods map directly onto the hands-on examples in the next section.

### Reference reading

- Per-example READMEs in the [Tutorials_OpenMolcas](https://github.com/compchem-cybertraining/Tutorials_OpenMolcas) repository.
- PySpawn-OpenMolcas interface: Ibele, Mehmood, Levine, Avagliano, *J. Chem. Theory Comput.* (2024), [doi:10.1021/acs.jctc.4c00855](https://doi.org/10.1021/acs.jctc.4c00855).

---

## 2. Hands On: OpenMolcas Excited-State Tutorials <a name="2"></a>

[Back to TOC](#table-of-contents)

Work through the numbered examples in the
[**Tutorials_OpenMolcas**](https://github.com/compchem-cybertraining/Tutorials_OpenMolcas) repository.
Each example is a self-contained folder with an input file, a `*.job`, a reference output, and a
README explaining what the calculation does and how to read the result. Run each with:

```bash
cd <example_folder>
sbatch *.job
```

The examples build from ground-state structure to excited-state spectroscopy and couplings:

| # | Example | What you compute |
|---|---------|------------------|
| 1 | Geometry + Hessian (DFT) | Optimized geometry and Hessian, the input to Wigner sampling in PySpawn. |
| 2 | S0 optimization (CASSCF) | Ground-state minimum at the multiconfigurational level. |
| 3 | S1 optimization (XMS-CASPT2) | Excited-state minimum and transition moments. |
| 4 | Oscillator strengths + NTOs | Which states are bright, and the hole/particle orbitals of each transition. |
| 5 | Spin-orbit coupling | Singlet-triplet coupling in thioformaldehyde (RASSI-SO), reproducing a benchmark. |
| 6 | Photoelectron spectrum | Valence Dyson-orbital spectrum of uracil, ground and excited state. |
| 7 | Conical intersection (MECI) | The S1/S0 minimum-energy crossing, optimized at CASPT2. |
| 8 | Nonadiabatic coupling | The derivative-coupling vector, the AIMS spawning trigger. |
| 9 | Core-hole XPS | O 1s core-level spectrum of uracil (RAS + HEXS + Dyson). |

> Examples 1, 7, and 8 connect directly to the dynamics: the Hessian seeds the initial conditions,
> the MECI is the funnel the trajectories spawn around, and the derivative coupling is the quantity
> the spawning criterion monitors.

### 2.1. Demonstrations

Follow each folder's README in [Tutorials_OpenMolcas](https://github.com/compchem-cybertraining/Tutorials_OpenMolcas).
Reference outputs are provided so you can follow the analysis even if a long job has not finished.

### 2.2. Homeworks

- Run Examples 1 to 3 and confirm your optimized geometries and Hessian.
- Run Example 5 and compare your singlet-triplet gap and spin-orbit coupling with the benchmark
  values in the README.
- Run Example 7 and confirm the energy gap collapses to zero at the converged conical intersection.

---

## 3. Lecture: Ab Initio Multiple Spawning (60 min) <a name="3"></a>

[Back to TOC](#table-of-contents)

A 60 minute lecture on the theory of Ab Initio Multiple Spawning and its implementation in PySpawn.
The aim is to understand the algorithm well enough to run, restart, and analyze a simulation, and to
interpret what the trajectories and populations mean.

Topics covered:

- **The nuclear wavepacket as a basis of traveling Gaussians** (trajectory basis functions, TBFs),
  each guided by a classical trajectory on an electronic surface.
- **The spawning idea**: when the nonadiabatic coupling between states becomes large (near a conical
  intersection), a new TBF is spawned on the coupled state to capture population transfer, without
  preselecting a reaction coordinate.
- **The spawning criterion**: the time-derivative coupling and the threshold that triggers a spawn,
  connecting directly to the derivative coupling from OpenMolcas Example 8.
- **Initial conditions**: Wigner sampling around the Franck-Condon point using the Hessian from
  OpenMolcas Example 1.
- **On-the-fly electronic structure**: how PySpawn calls OpenMolcas at each step for energies,
  forces, and wavefunctions, via the interface paper above.

### Reference reading

- Ben-Nun, Quenneville & Martinez, *J. Phys. Chem. A* **104**, 5161 (2000), [doi:10.1021/jp994174i](https://doi.org/10.1021/jp994174i).
- Ben-Nun & Martinez, *J. Chem. Phys.* **108**, 7244 (1998), [doi:10.1063/1.476142](https://doi.org/10.1063/1.476142).
- Curchod & Martinez, *Chem. Rev.* **118**, 3305 (2018), [doi:10.1021/acs.chemrev.7b00423](https://doi.org/10.1021/acs.chemrev.7b00423).
- Fedorov, Seritan, Fales, Martinez & Levine, *J. Chem. Theory Comput.* **16**, 5485 (2020), [doi:10.1021/acs.jctc.0c00575](https://doi.org/10.1021/acs.jctc.0c00575).

---

## 4. Hands On: PySpawn AIMS Dynamics <a name="4"></a>

[Back to TOC](#table-of-contents)

Work through the [**Tutorials_PySpawn**](https://github.com/compchem-cybertraining/Tutorials_PySpawn)
repository, which runs a complete AIMS simulation of the photodynamics of ethylene, launched from the
S1 state, using OpenMolcas for on-the-fly electronic structure.

**Install PySpawn** (Python 2.7 environment plus the code):

```bash
bash setup_pyspawn.sh
conda activate $HOME/pyspawn
```

The workflow, each step documented in the repository README:

1. **Prepare the Hessian.** Convert a Hessian from OpenMolcas, ORCA, or Gaussian into PySpawn's
   `hessian.hdf5` format (this uses the geometry and Hessian from OpenMolcas Example 1).
2. **Generate initial conditions.** `Generate_ICs.sh` clones the template folder `1/`, assigns each
   IC a unique random seed for Wigner sampling, and submits the jobs.
3. **Run and restart.** Each IC is an independent AIMS trajectory; `restart.py` resumes a run from
   its checkpoint after any interruption.
4. **Analyze.** `analysis.py` turns `sim.hdf5` into electronic populations, energies, nuclear basis
   populations, and geometric observables, and can be run while a simulation is still in progress.
5. **Ensemble result.** The provided post-processing scripts collect and plot the ensemble S1
   population decay; reference AIMS data is included in case your trajectories have not finished.

> **Start with the model potential.** Before the full ethylene run, the `model_potential` example provides an
> analytic two-state conical intersection with zero electronic-structure cost. It is the fastest way
> to see spawning and population transfer, and every number can be checked against the closed-form
> surfaces. See its README for the derivation.

### 4.1. Demonstrations

Follow the repository README and each folder's instructions in
[Tutorials_PySpawn](https://github.com/compchem-cybertraining/Tutorials_PySpawn).

### 4.2. Homeworks

- Run the `model_potential` example and plot the electronic populations; confirm population transfers
  from the upper to the lower state as the trajectory passes through the cone.
- Generate a small set of ethylene initial conditions and launch them.
- Run `analysis.py` on a trajectory and plot the total electronic populations and an energy
  conservation check.
- Extract and plot the ensemble S1 population decay (use the provided reference data if needed).


## 5. Presentations and Videorecordings

### 5.1. Presentations


### 5.2. Classroom recording

<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%">
  <iframe src="https://ub.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=e11093a3-7d2f-41e4-bfca-b48100db8a07
  &autoplay=false&offerviewer=true&showtitle=true&showbrand=true&captions=false&interactivity=all" 
  style="border: 1px solid #464646; position: absolute; top: 0; left: 0; width: 100%; height: 100%; box-sizing: border-box;" 
  allowfullscreen allow="autoplay" aria-label="Panopto Embedded Video Player" aria-description="Cyber Training 2026, Wednesday, July 8 (morning)"></iframe>
</div>


<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%">
  <iframe src="https://ub.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=edaabd86-9607-49b2-8e13-b48100dc0014
  &autoplay=false&offerviewer=true&showtitle=true&showbrand=true&captions=false&interactivity=all" 
  style="border: 1px solid #464646; position: absolute; top: 0; left: 0; width: 100%; height: 100%; box-sizing: border-box;" 
  allowfullscreen allow="autoplay" aria-label="Panopto Embedded Video Player" aria-description="CyberTraining 2026, Wednesday, July 8 (afternoon)"></iframe>
</div>


### 5.3. Zoom recordings

<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%">
  <iframe src="https://ub.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=666c4248-0bae-4172-bcb9-b4810131d922
  &autoplay=false&offerviewer=true&showtitle=true&showbrand=true&captions=false&interactivity=all" 
  style="border: 1px solid #464646; position: absolute; top: 0; left: 0; width: 100%; height: 100%; box-sizing: border-box;" 
  allowfullscreen allow="autoplay" aria-label="Panopto Embedded Video Player" aria-description="Zoom: CyberTraining 2026, Wednesday morning"></iframe>
</div>

<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%">
  <iframe src="https://ub.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=1aaf5e8f-1787-4a89-b48a-b48101824854
  &autoplay=false&offerviewer=true&showtitle=true&showbrand=true&captions=false&interactivity=all" 
  style="border: 1px solid #464646; position: absolute; top: 0; left: 0; width: 100%; height: 100%; box-sizing: border-box;" 
  allowfullscreen allow="autoplay" aria-label="Panopto Embedded Video Player" aria-description="Zoom: CyberTraining 2026, Wednesday afternoon"></iframe>
</div>


---

> ## Key Points
>
> - Conical intersections are the funnels of nonadiabatic dynamics; describing them needs multireference methods (CASSCF, CASPT2, RASSI).
> - AIMS represents the nuclear wavepacket with traveling Gaussians that **spawn** new basis functions near strong nonadiabatic coupling.
> - The spawning criterion is set by the time-derivative coupling, the same quantity computed as the derivative coupling in OpenMolcas.
> - OpenMolcas supplies the on-the-fly electronic structure; PySpawn drives the spawning dynamics.
> - Both repositories, [Tutorials_OpenMolcas](https://github.com/compchem-cybertraining/Tutorials_OpenMolcas) and [Tutorials_PySpawn](https://github.com/compchem-cybertraining/Tutorials_PySpawn), contain all inputs, outputs, and analysis scripts.



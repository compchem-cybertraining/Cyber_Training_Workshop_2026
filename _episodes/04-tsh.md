---
title: "4. Nonadiabatic Dynamics and Trajectory Surface Hopping with Libra"
questions:
- "How do I activate and run libra environment?"
- "How do I set up Jupyter on the OOD knowing libra environment?"
- "How do I conduct TSH calculations with analytic (model) Hamiltonians using Libra code?"
- "How do I conduct exact quantum calculations on the grid using Lbira code?"
- "How do I conduct atomistic NBRA calculations using Libra and CP2K/MOPAC/DFTB+ codes?"
- "How do I conduct atomistic on-the-fly calculations using Libra/DFTB+ interface?"
- "How do I compute properties of interest that characterize NA-MD"
objectives:
- "Activate libra Conda environment"
- "Install Jupyter kernel for libra environment"
- "Learn theory and machniery behind TSH calculations with Libra"
- "Conduct TSH calculations for spin-boson and other model Hamiltonians using a variety of TSH schemes"
- "Conduct a 4-steps workflow for NBRA calculations with Libra/CP2K"
- "Compute time-overlaps, NACs for NBRA calculations using MOPAC or DFTB+"
- "Execute on-the-fly TSH calculations for small molecules using Libra/DFTB+ interface"
- "Compute descriptive properties such as population dynamics, influence spectra, NAC distributions, etc."
keypoints:
- "TBD"

---

# 1. Setting up individual Jupyter kernel for using Libra on the OOD (Open On Demand)

## 1.1. Add this in your `.bashrc`:

```bash
module use /projects/academic/cyberwksp21/MODULES
module load libra_ava/devel
```

Restart your terminal or reload your `.bashrc`:

```bash
source ~/.bashrc
```

## 1.2. Activate libra environment and install jupyter kernel in user location:
```bash
conda activate libra 
python -m ipykernel install     --user     --name libra     --display-name "Python (libra)"
```

## 1.3. Update the `kernel.json` file in `/user/<your username>/.local/share/jupyter/kernels/libra` to be like this:


```bash
{
 "argv": [
  "/user/<your username>/.local/share/jupyter/kernels/libra/launcher.sh",
  "-f",
  "{connection_file}"
 ],
 "display_name": "Python (libra)",
 "language": "python",
 "metadata": {
  "debugger": true
 }
}
```

> Note: Replace `<your username>` with your actual user name e.g. `alexeyak`


## 1.4. Create the file `launcher.sh` in `/user/<your username>/.local/share/jupyter/kernels/libra` to be like this:

```bash
#!/bin/bash
# ======================================================
# HARD CLEAN (CRITICAL on CCR)
# ======================================================
unset PYTHONPATH
unset PYTHONHOME
unset EBPYTHONPREFIXES
# Prevent user site leakage
export PYTHONNOUSERSITE=1
# ======================================================
# Load module environment (ONLY ONE layer)
# ======================================================
module use /projects/academic/cyberwksp21/MODULES
module load libra_ava/devel
# ======================================================
# Activate Conda environment (must match module!)
# ======================================================
source /projects/academic/cyberwksp21/SOFTWARE/Conda/etc/profile.d/conda.sh
conda activate libra
# ======================================================
# Libra runtime libraries
# ======================================================
export LD_LIBRARY_PATH=/projects/academic/cyberwksp21/SOFTWARE/libra/_build/src:$LD_LIBRARY_PATH
# ======================================================
# Launch kernel
# ======================================================
exec /projects/academic/cyberwksp21/SOFTWARE/Conda/envs/libra/bin/python \
     -m ipykernel_launcher "$@"
```

And make it executable:

```bash
chmod +x .local/share/jupyter/kernels/libra/launcher.sh
```

## 1.5. Launch Jupyter on the OOD without any additional modules load needed

## 1.6. In the started Jupyter select "Python (libra)" kernel


# 2. Starting tutorials

The Libra tutorials are available at [https://github.com/compchem-cybertraining/Tutorials_Libra](https://github.com/compchem-cybertraining/Tutorials_Libra)

it is advisable that you just clone this repository to your local working directory and go from there. 

## 2.1. Go to your working directory, e.g.:
```bash
cd /projects/academic/cyberwksp21/Students/alexeyak/libra_examples`
```

or go to your home directory: 
```
cd
```

## 2.2. Clone the Tutorials_Libra repository:

```bash
git clone https://github.com/compchem-cybertraining/Tutorials_Libra.git
```

## 2.3. Start your Jupyter app on the OOD and open the desired tutorial/example

## 2.4. Keep in mind that Jupyter app run on the OOD can "see" only your home directory. 
If you keep your examples elsewhere, e.g. on the `/projects/academic/cyberwksp21/Students/alexeyak`,

you need to create a symlink (symbolic link) to that directory in your home directory, e.g.:

```bash
cd
ln -s /projects/academic/cyberwksp21/Students/<my working folder> workshop
```

> Note: replace `<my working folder>` with the actual name

This will create a link (that would appear as a folder) in your home directory called `workshop`. It will point to the actual folder
located at `/projects/academic/cyberwksp21/Students/<my working folder>`

> WARNING: Link behaves the same way as the actual folder, so if you try to delete the link like this `rm -r workshop`, it will delete your actual tutorials folder.
  If you no longer need the link, use `rm workshop` (no `-r` option!)


# 3. Lesson plan

## 3.1. Abstract model Hamiltonians (morning session)

### 3.1.1. Abstract (model Hamiltonian) NA-MD: 

 - **General NAMD:** 6_dynamics/1_trajectory_based/10_model_many_methods
 - **FMO example:** 6_dynamics/1_trajectory_based/12_model_spin_boson_fmo

### 3.1.2. Exact dynamics with PyTorch: 

 - **1D, 1 state:** 6_dynamics/4_wavepackets/6_soft_with_pytorch/1_single_state
 - **1D, multiple states:** /6_dynamics/4_wavepackets/6_soft_with_pytorch/2_multiple_states


## 3.2. Atomistic Hamiltonians (afternoon session)

### 3.2.1. Maing course: NBRA workflow with CP2K

 - **Step 1: adiabatic MD**  11_program_specific_methods/3_cp2k_methods/6_hpc_namd_workflow/1_step1
 - **Step 2: single-particle time-overlaps**  11_program_specific_methods/3_cp2k_methods/6_hpc_namd_workflow/2_step2
 - **Step 3: TD-DFT time-overlaps**  11_program_specific_methods/3_cp2k_methods/6_hpc_namd_workflow/3_step3
 - **Step 4: NBRA NA-MD** 6_dynamics/2_nbra_workflows/9_step4

### 3.2.2. Additional modules 

#### A. Computing time-overlaps

 - **Advanced Step 3 with CP2K:** 11_program_specific_methods/3_cp2k_methods/5_namd_workflow
 - **Steps 2 and 3 for DFTB+:** 11_program_specific_methods/4_dftbplus_methods/3_workflow
 - **Steps 2 and 3 for MOPAC:** 

    - 11_program_specific_methods/5_mopac_methods/1_initial_tutorial
    - 11_program_specific_methods/5_mopac_methods/2_using_active_spaces
     
#### B. Pre-NAMD analysis

 - **Time-resolved energies and influence spectra/spectral densities:** 11_program_specific_methods/3_cp2k_methods/3_time_resolved_energies
 - **Composition of excited states in therms of determinants:** 11_program_specific_methods/3_cp2k_methods/4_excitation_analysis

#### C. Running NA-MD
 
 - **Additional example of NBRA run:** 6_dynamics/2_nbra_workflows/10_generic_step3_4/1_Example1
 - **Non-NBRA example with DFTB+:** 11_program_specific_methods/4_dftbplus_methods/4_non_nbra_workflow


#### D. Post-NAMD analysis 

 - **Plotting TRPES:** 6_dynamics/2_nbra_workflows/18_plotting_trpes


# 4. Presentations and Videorecordings

## 4.1. Presentations

[Libra Overview, simplified introduction into TSH methods](../files/Akimov/2026_July9-parts-1-3.pdf)

[Algorithms, methods and options for Libra](../files/Akimov/2026_July9-part-4.pdf)


## 4.2. Classroom recording

<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%">
  <iframe src="https://ub.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=8cb37794-ccc3-407d-82d5-b44401019d22
  &autoplay=false&offerviewer=true&showtitle=true&showbrand=true&captions=false&interactivity=all" 
  style="border: 1px solid #464646; position: absolute; top: 0; left: 0; width: 100%; height: 100%; box-sizing: border-box;" 
  allowfullscreen allow="autoplay" aria-label="Panopto Embedded Video Player" aria-description="CyberTraining 2026, Thursday, July 9 (morning)"></iframe>
</div>

<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%">
  <iframe src="https://ub.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=4f746e66-96d9-4077-b754-b48100ddfae3
  &autoplay=false&offerviewer=true&showtitle=true&showbrand=true&captions=false&interactivity=all" 
  style="border: 1px solid #464646; position: absolute; top: 0; left: 0; width: 100%; height: 100%; box-sizing: border-box;" 
  allowfullscreen allow="autoplay" aria-label="Panopto Embedded Video Player" aria-description="CyberTraining 2026, Thursday, July 9 (afternoon)"></iframe>
</div>


## 4.3. Zoom recordings

<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%">
  <iframe src="https://ub.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=da6c95bc-f1a9-428c-b8dd-b48201259501
  &autoplay=false&offerviewer=true&showtitle=true&showbrand=true&captions=false&interactivity=all" 
  style="border: 1px solid #464646; position: absolute; top: 0; left: 0; width: 100%; height: 100%; box-sizing: border-box;" 
  allowfullscreen allow="autoplay" aria-label="Panopto Embedded Video Player" aria-description="Zoom: CyberTraining 2026, Thursday morning"></iframe>
</div>


<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%">
  <iframe src="https://ub.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=e4846b3e-f590-4a11-b6ee-b48201756fa9
  &autoplay=false&offerviewer=true&showtitle=true&showbrand=true&captions=false&interactivity=all" 
  style="border: 1px solid #464646; position: absolute; top: 0; left: 0; width: 100%; height: 100%; box-sizing: border-box;" 
  allowfullscreen allow="autoplay" aria-label="Panopto Embedded Video Player" aria-description="Zoom: CyberTraining 2026, Thursday afternoon"></iframe>
</div>





---
title: "3. Excited-State Electronic Structure: PySCF and Prism"
teaching: 30
exercises: 180
questions:
- "How do I use the PySCF/Prism environment on CCR?"
- "How can I run the PySCF and Prism tutorial examples interactively or through batch jobs?"
- "How can I install PySCF and Prism locally if I do not have CCR access?"
- "What examples are included in the PySCF and Prism tutorials?"
- "How can we compare computed excited-state energies with QUEST reference data?"
objectives:
- "Activate and test the PySCF/Prism environment on CCR."
- "Clone or access the PySCF and Prism tutorial examples."
- "Run Python input files from a terminal using the activated environment."
- "Understand the main scientific goal of each tutorial example."
- "Compile computed energies and compare them with QUEST reference values."
keypoints:
- "Run the PySCF/Prism examples from a terminal where the correct environment is activated."
- "Most examples are available directly on CCR in the shared examples directory."
- "PySCF can also be installed locally with pip or conda, while Prism is installed by cloning the repository and adding it to PYTHONPATH."
- "The tutorials form a method ladder from CIS/TDA and TD-DFT to ADC, EOM-CCSD, CASSCF, NEVPT2, spin-orbit NEVPT2, and MR-ADC."
---

> ## Overview
>
> Teaching: 30 min  
> Exercises: 180 min
>
> This episode introduces the PySCF and Prism tutorial examples for the workshop.
> The goal is to learn how to run short excited-state and spectroscopic calculations,
> analyze the resulting electronic states, and compare different levels of theory
> against reference data from the QUEST database.
>
> The examples should normally be run from a terminal session on CCR where the
> PySCF/Prism environment has already been activated. 
{: .overview}

# 1. Using the PySCF/Prism environment on CCR

The workshop PySCF/Prism environment is already installed on CCR. Open a terminal on the workshop
system and activate the environment:

```bash
source /projects/academic/cyberwksp21/SOFTWARE_2026/pyscf_prism_environment.sh
```

After activation, check which Python executable is being used:

```bash
which python
python --version
```

Then test the most important imports:

```bash
python -c "import pyscf; print('PySCF version:', pyscf.__version__)"
python -c "import prism; print('Prism import: OK')"
```

If these commands work, the terminal is ready for running the tutorial input files.

# 2. Accessing the tutorial examples on CCR

The examples are available in the shared workshop directory:

```bash
/projects/academic/cyberwksp21/SOFTWARE_2026/pyscf_prism_examples
```

Go to this directory:

```bash
cd /projects/academic/cyberwksp21/SOFTWARE_2026/pyscf_prism_examples
ls
```

If you want a copy in your own working directory, use:

```bash
mkdir -p ~/cybertraining_examples
cd ~/cybertraining_examples
cp -r /projects/academic/cyberwksp21/SOFTWARE_2026/pyscf_prism_examples .
cd pyscf_prism_examples
```

# 3. Cloning the GitHub tutorial repositories

You can also clone the public tutorial repositories.

For the PySCF tutorial examples:

```bash
git clone https://github.com/compchem-cybertraining/Tutorials_PySCF.git
cd Tutorials_PySCF
```

For the Prism/SQA tutorial examples:

```bash
git clone https://github.com/compchem-cybertraining/Tutorials_Prism_SQA.git
cd Tutorials_Prism_SQA
```

If you are working on CCR, first activate the workshop environment as described above.
If you are working on your own computer, follow the local installation instructions below.

# 4. Running calculations on CCR

## 4.1 Running a calculation interactively

For short tutorial examples, first request or open an interactive compute session following the
CCR workshop instructions:

```bash
salloc \
   --partition=general-compute \
   --qos=general-compute \
   --mem=50G \
   --nodes=1 \
   --time=4:00:00 \
   --ntasks-per-node=1 \
   --cpus-per-task=12 \
   --no-shell
# above command will output the jobid
srun --jobid=JOBID_HERE --export=HOME,TERM,SHELL --pty /bin/bash --login
```

Once you are on the appropriate node, activate the environment:

```bash
source /projects/academic/cyberwksp21/SOFTWARE_2026/pyscf_prism_environment.sh
```

Go to an example directory and run the input file:

```bash
cd ~/cybertraining_examples/pyscf_prism_examples/02_cis_tdhf
python 02_cis_tdhf.py > 02_cis_tdhf.dat &
```

Monitor the output:

```bash
tail -f 02_cis_tdhf.dat
```

To stop monitoring the file, press `Ctrl+C`. This stops `tail`, not the calculation itself.

Check background jobs:

```bash
jobs
```

## 4.2 Batch-job template

For examples that take longer, use a batch script. Adjust the account, partition, wall time, memory,
and number of cores according to the workshop instructions.

Create a file such as `run_pyscf_example.slurm`:

```bash
#!/bin/bash
#SBATCH --job-name=pyscf_example
#SBATCH --output=slurm-%j.out
#SBATCH --time=00:30:00
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G

source /projects/academic/cyberwksp21/SOFTWARE_2026/pyscf_prism_environment.sh

python 04_sr_adc.py > 04_sr_adc.dat 2>&1
```

Submit it:

```bash
sbatch run_pyscf_example.slurm
```

Check the queue:

```bash
squeue -u $USER
```

# 5. Local installation without CCR access

If you do not have CCR access, you can install PySCF and Prism locally. A conda environment is
recommended to keep the workshop software separate from your system Python.

## 5.1 Create a conda environment

```bash
conda create -n pyscf-prism python=3.12 -y
conda activate pyscf-prism
python -m pip install --upgrade pip
```

## 5.2 Install PySCF with pip

The PySCF documentation recommends pip installation for non-developers:

```bash
python -m pip install --prefer-binary pyscf
```

Test the installation:

```bash
python -c "import pyscf; print(pyscf.__version__)"
```

Optional packages for plotting and notebooks:

```bash
python -m pip install matplotlib pandas openpyxl jupyterlab ipykernel
```

## 5.3 Install Prism

Clone Prism:

```bash
git clone https://github.com/sokolov-group/prism.git
```

Add Prism to your `PYTHONPATH`. For the current terminal session:

```bash
export PYTHONPATH=$PWD/prism:$PYTHONPATH
```

To make this permanent, add the corresponding line to your shell startup file, for example
`~/.bashrc` or `~/.zshrc`.

Install dependencies:

```bash
python -m pip install numpy scipy h5py psutil matplotlib sympy opt_einsum
```

Test the installation:

```bash
python -c "import prism; print('Prism import: OK')"
```

> ## Note
>
> Prism uses PySCF to generate molecular integrals, molecular orbitals, and reference
> wavefunctions. In typical Prism calculations, a PySCF Hartree--Fock, DFT, CASCI,
> or CASSCF object is passed to the Prism interface.
{: .callout}

# 6. Tutorial examples: PySCF method ladder

The PySCF tutorial examples introduce a sequence of increasingly accurate and increasingly
expensive excited-state methods. Most examples use ethylene as a compact test system and compare
results with QUEST reference data.

## 6.1 `01_test_environment`

Purpose: check that the environment can import the required modules and run a minimal calculation.

What to inspect:

- Does the Python environment import the required modules?
- Does the reference calculation converge?
- Are there any missing-library or missing-module errors?

## 6.2 `02_cis_tdhf`

Purpose: compute excited states with HF-based TDA/CIS and full TDHF/RPA.

Scientific questions:

- Which states are bright and which are dark?
- How different are TDA/CIS and full TDHF/RPA excitation energies?
- Do the NTOs identify the expected valence excitation?
- How large is the CIS error relative to QUEST?

## 6.3 `03_tda_tddft`

Purpose: compute excited states with B3LYP-based TDA-DFT and full TDDFT.

Scientific questions:

- How do TD-DFT/TDA and full TDDFT differ?
- How does TD-DFT compare with CIS/TDA and QUEST?
- Are the errors similar for all states or dependent on state character?

## 6.4 `04_sr_adc`

Purpose: compute correlated single-reference excited states with ADC(2) and ADC(3).

Scientific questions:

- How do ADC(2) and ADC(3) compare with each other?
- Which ADC method is closer to QUEST for the lowest states?
- Does increasing the number of roots reveal additional bright or Rydberg-like states?
- Do NTOs and density differences give consistent state assignments?

## 6.5 `05_eom_ccsd`

Purpose: compute EOM-EE-CCSD singlet excitation energies as a higher-level single-reference
comparison.

Scientific questions:

- How close is EOM-CCSD to QUEST?
- Which lower-cost method is closest to EOM-CCSD for each state?
- Does EOM-CCSD change the state ordering relative to TD-DFT or ADC?

## 6.6 `06_sa_casscf`

Purpose: introduce state-averaged CASSCF and active-space analysis.

Scientific questions:

- How sensitive are CASSCF results to the initial orbital guess?
- Do MP2 natural orbitals or CIS natural orbitals produce a clearer active space?
- Which natural occupations suggest multiconfigurational character?
- What does CASSCF capture that single-reference methods may miss?

# 7. Tutorial examples: Prism multireference spectroscopy

The Prism examples extend the PySCF workflow to multireference perturbation theory, spin-orbit
coupling, magnetic properties, and core-level spectroscopy.

## 7.1 `07_nevpt2`

Purpose: compute state-specific NEVPT2 and quasidegenerate NEVPT2 on top of a state-averaged
CASSCF reference.

Scientific questions:

- How much do NEVPT2 corrections shift the CASSCF excitation energies?
- Do state-specific NEVPT2 and QD-NEVPT2 give the same state ordering?
- Which states are bright or dark?
- Which NTOs correspond to valence π→π* excitations?
- How do the results compare with QUEST?

## 7.2 `08_si_soc_nevpt2`

Purpose: compute spin-free and spin-orbit-coupled QD-NEVPT2 states and magnetic properties.

Scientific questions:

- How does spin-orbit coupling split or mix spin-free states?
- Which states are most affected by SOC?
- How anisotropic is the computed g-tensor?
- What information is missing if only spin-free excitation energies are computed?

## 7.3 `09_cvs_ip_mr_adc`

Purpose: compute CVS-IP-MR-ADC spectra for oxygen K-edge X-ray photoelectron spectroscopy.

Scientific questions:

- Which core-ionized states produce the strongest XPS peaks?
- Are there weak satellite-like features?
- How does the XPS spectrum change between electronic-state references?
- What does MR-ADC add beyond a simple orbital-energy interpretation?

# 8. Geometry and reference-data files

The `geometries/` directory contains small molecules useful for method comparisons:

```text
acrolein.xyz
butadiene.xyz
ethylene.xyz
formaldehyde.xyz
glyoxal.xyz
hexatriene.xyz
nitrosomethane.xyz
nitroxyl.xyz
tetrazine.xyz
```

The `QUEST/` directory contains a spreadsheet of QUEST reference data:

```text
QUEST/QUEST-All.xlsx
```


# 9. Presentations and Videorecordings

## 9.1. Presentations 

[Presentation 1](../files/Sokolov/presentation.pdf)


## 9.2. Classroom recording

<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%">
  <iframe src="https://ub.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=d700e681-d48b-4c5c-831f-b44401019deb
  &autoplay=false&offerviewer=true&showtitle=true&showbrand=true&captions=false&interactivity=all" 
  style="border: 1px solid #464646; position: absolute; top: 0; left: 0; width: 100%; height: 100%; box-sizing: border-box;" 
  allowfullscreen allow="autoplay" aria-label="Panopto Embedded Video Player" aria-description="CyberTraining 2026, Tuesday, July 7"></iframe>
</div>

## 9.3. Zoom recordings

Unfortunately, only morning session was recorded this way

<div style="position: relative; width: 100%; height: 0; padding-bottom: 56.25%">
  <iframe src="https://ub.hosted.panopto.com/Panopto/Pages/Embed.aspx?id=0622e097-3982-41f1-8e4b-b480011f5c04
  &autoplay=false&offerviewer=true&showtitle=true&showbrand=true&captions=false&interactivity=all" 
  style="border: 1px solid #464646; position: absolute; top: 0; left: 0; width: 100%; height: 100%; box-sizing: border-box;" 
  allowfullscreen allow="autoplay" aria-label="Panopto Embedded Video Player" aria-description="CyberTraining 2026, Tuesday morning"></iframe>
</div>




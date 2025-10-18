+++
title = "Getting Started with FAIRChem"
date = 2025-10-14
description = "A comprehensive tutorial on using FAIRChem for materials science calculations, from online demos to local API implementation on macOS"
slug = "fairchem-materials-calculation-guide"
[taxonomies]
tags = ["FAIRChem", "Machine Learning", "Materials Science", "Computational Chemistry", "Python", "ASE", "DFT"]
categories = ["Scientific Software", "Tutorial", "Computational Methods"]
+++


FAIRChem is a powerful machine learning framework developed by Meta AI for accelerating materials discovery and catalysis research. This comprehensive guide walks you through using FAIRChem, from exploring the web-based demo to performing local energy calculations on macOS.

<!-- more -->

## What is FAIRChem?

FAIRChem (formerly known as Open Catalyst Project) is an open-source framework that uses machine learning to predict atomic interactions and materials properties at speeds far exceeding traditional density functional theory (DFT) calculations. The project has demonstrated impressive performance on benchmark datasets, with models like eSEN-30m-oam ranking highly on the [Matbench Discovery leaderboard](https://matbench-discovery.materialsproject.org/models/eSEN-30m-oam).

**Key Resources:**
- [Official website](https://fair-chem.github.io/)
- [GitHub repository](https://github.com/facebookresearch/fairchem)
- [Matbench Discovery](https://matbench-discovery.materialsproject.org/models/eSEN-30m-oam)

## Getting Started: Web-Based Demo

Before diving into local installation, you can explore FAIRChem's capabilities through the [educational demo](https://facebook-fairchem-uma-demo.hf.space/). This interactive interface allows you to:

- Visualize molecular and crystal structures
- Run quick energy predictions
- Experiment with different model configurations

**First-time setup requirements:**
- Personal information registration
- Institutional email address
- Organization name
- Affiliation details

The web demo provides immediate access without installation, making it ideal for initial exploration and educational purposes.

## Understanding FAIRChem Tasks

FAIRChem supports multiple specialized tasks, each optimized for different types of materials and applications:

| Task Name | Application Domain |
|-----------|-------------------|
| `oc20`    | Catalyst structures and surface reactions |
| `omat`    | Inorganic crystalline materials |
| `omol`    | Molecular structures and conformations |
| `odac`    | Metal-organic frameworks (MOFs) |
| `omc`     | Molecular crystals |

Selecting the appropriate task name is crucial for obtaining accurate predictions tailored to your specific material system.

## Local Installation on macOS

### Step 1: Environment Setup

FAIRChem requires a clean Python environment to avoid dependency conflicts. I strongly recommend creating a dedicated environment, as mixing with other computational chemistry packages can lead to version incompatibility issues.

**Option A: Using Python venv**
```bash
python3 -m venv fairchem
source fairchem/bin/activate
pip install fairchem-core
```

**Option B: Using Conda (Recommended)**
```bash
conda create -n fairchem python=3.10
conda activate fairchem
pip install fairchem-core
```

**Important Note on Dependencies:**
FAIRChem requires numpy >= 2.0.0, which conflicts with some older packages like abipy 0.9.8 (requires numpy < 2.0.0). This is why a separate environment is essential. I learned this the hard way when my DFT environment broke after installing FAIRChem alongside abipy.

### Step 2: Configuring HuggingFace Access

FAIRChem's UMA models are hosted on HuggingFace and require authentication:

1. **Register with your institutional email** at [HuggingFace](https://huggingface.co/)

2. **Request access** to the UMA model at https://huggingface.co/facebook/UMA

3. **Create an access token** at https://huggingface.co/settings/tokens with the permission: "Read access to contents of all public gated repos you can access"

   **Common Error:** If you don't select any permissions, you'll encounter:
   ```
   403 Forbidden: Please enable access to public gated repositories in your 
   fine-grained token settings to view this repository.
   Cannot access content at: https://huggingface.co/facebook/UMA/resolve/main/checkpoints/uma-s-1p1.pt
   ```

4. **Authenticate** using one of these methods:
   ```bash
   # Method 1: Interactive login
   huggingface-cli login
   # Then paste your token when prompted
   
   # Method 2: Environment variable
   export HF_TOKEN=your_token_here
   ```
   
   **Note:** If you see a deprecation warning about `huggingface-cli login`, use the newer command instead:
   ```bash
   hf auth login
   ```

## Basic Usage: Energy Calculations with ASE

### Example 1: Simple Bulk Structure

Here's a minimal example calculating the energy of bulk iron:

```python
from ase.build import bulk
from fairchem.core import pretrained_mlip, FAIRChemCalculator

# Load the UMA model (CPU for macOS)
predictor = pretrained_mlip.get_predict_unit("uma-s-1p1", device="cpu")
calc = FAIRChemCalculator(predictor, task_name="omat")

# Create structure and attach calculator
atoms = bulk("Fe")
atoms.calc = calc

# Calculate total energy
total_energy = atoms.get_potential_energy()
print(f"Energy: {total_energy:.6f} eV")
```

**Key Points:**
- Use `device="cpu"` on macOS (MPS support is limited)
- Choose `task_name` based on your material type
- FAIRChem integrates seamlessly with ASE's Atoms object
- For molecules, use `task_name="omol"` with `ase.build.molecule()`

### Example 2: Real-World Case - ZrAl₃ Structure Optimization

Let's walk through a complete workflow: downloading a structure from Materials Project and performing geometry optimization.

**Step 1: Obtain the structure**
Download the CIF file for ZrAl₃ (mp-395) from [Materials Project](https://materialsproject.org/):

```cif
# generated using pymatgen
data_ZrAl3
_symmetry_space_group_name_H-M   'P 1'
_cell_length_a   3.99506449
_cell_length_b   3.99506449
_cell_length_c   17.24547215
_cell_angle_alpha   90.00000000
_cell_angle_beta   90.00000000
_cell_angle_gamma   90.00000000
_symmetry_Int_Tables_number   1
_chemical_formula_structural   ZrAl3
_chemical_formula_sum   'Zr4 Al12'
_cell_volume   275.24705319
_cell_formula_units_Z   4
loop_
 _symmetry_equiv_pos_site_id
 _symmetry_equiv_pos_as_xyz
  1  'x, y, z'
loop_
 _atom_site_type_symbol
 _atom_site_label
 _atom_site_symmetry_multiplicity
 _atom_site_fract_x
 _atom_site_fract_y
 _atom_site_fract_z
 _atom_site_occupancy
  Zr  Zr0  1  0.00000000  0.00000000  0.11879205  1
  Zr  Zr1  1  0.50000000  0.50000000  0.38120795  1
  Zr  Zr2  1  0.50000000  0.50000000  0.61879205  1
  Zr  Zr3  1  0.00000000  0.00000000  0.88120795  1
  Al  Al4  1  0.50000000  0.00000000  0.00000000  1
  Al  Al5  1  0.00000000  0.50000000  0.00000000  1
  Al  Al6  1  0.50000000  0.00000000  0.25000000  1
  Al  Al7  1  0.00000000  0.50000000  0.25000000  1
  Al  Al8  1  0.00000000  0.00000000  0.37471447  1
  Al  Al9  1  0.50000000  0.50000000  0.12528553  1
  Al  Al10  1  0.00000000  0.50000000  0.50000000  1
  Al  Al11  1  0.50000000  0.00000000  0.50000000  1
  Al  Al12  1  0.00000000  0.50000000  0.75000000  1
  Al  Al13  1  0.50000000  0.00000000  0.75000000  1
  Al  Al14  1  0.50000000  0.50000000  0.87471448  1
  Al  Al15  1  0.00000000  0.00000000  0.62528553  1
```

**Step 2: Create the calculation script**
Save the following as `calculate_ZrAl3.py`:

```python
from ase.io import read, write
from ase.optimize import BFGS
from fairchem.core import pretrained_mlip, FAIRChemCalculator

# Read the CIF structure file
atoms = read("ZrAl3.cif")
print("Original structure information:")
print(f"Chemical formula: {atoms.get_chemical_formula()}")
print(f"Number of atoms: {len(atoms)}")
print(f"Cell parameters: {atoms.cell.cellpar()}")

# Load FAIRChem pretrained model
print("\nLoading FAIRChem model...")
predictor = pretrained_mlip.get_predict_unit("uma-s-1p1", device="cpu")
calc = FAIRChemCalculator(predictor, task_name="omat")

# Attach calculator to atoms
atoms.calc = calc

# Calculate energy before optimization
print("\n=== Before Optimization ===")
initial_energy = atoms.get_potential_energy()
print(f"Initial energy: {initial_energy:.6f} eV")

# Perform structure optimization
print("\nStarting structure optimization...")
optimizer = BFGS(atoms, trajectory="ZrAl3_optimization.traj")
optimizer.run(fmax=0.05)  # Force convergence criterion: 0.05 eV/Å

# Energy after optimization
print("\n=== After Optimization ===")
final_energy = atoms.get_potential_energy()
print(f"Optimized energy: {final_energy:.6f} eV")
print(f"Energy difference: {final_energy - initial_energy:.6f} eV")

# Save optimized structure
write("ZrAl3_optimized.cif", atoms)
print("\nOptimized structure saved to: ZrAl3_optimized.cif")

# Print final cell parameters
print(f"\nOptimized cell parameters: {atoms.cell.cellpar()}")
print(f"Total energy: {final_energy:.6f} eV")
print(f"Energy per atom: {final_energy/len(atoms):.6f} eV/atom")
```

**Step 3: Run the calculation**
```bash
python calculate_ZrAl3.py
```

**Troubleshooting Note:**
If you encounter "ModuleNotFoundError: No module named 'ase'" when using `python3`, try using `python` instead. This typically occurs when the conda/venv environment hasn't properly updated the Python executable path.

## Best Practices and Tips

1. **Environment Isolation**: Always use a dedicated environment for FAIRChem to prevent dependency conflicts

2. **GPU vs CPU**: While FAIRChem supports CUDA for NVIDIA GPUs, macOS users should stick with `device="cpu"` as MPS (Metal) support is experimental

3. **Model Selection**: The `uma-s-1p1` model offers a good balance between speed and accuracy for most applications

4. **Convergence Criteria**: Adjust `fmax` in optimization based on your accuracy requirements (0.05 eV/Å is reasonable for most cases)

5. **Trajectory Analysis**: The `.traj` file generated during optimization can be visualized with ASE's GUI or analyzed programmatically

## Conclusion

FAIRChem provides a powerful, accessible framework for materials energy calculations that significantly reduces computational costs compared to traditional DFT. While it requires initial setup effort, particularly for HuggingFace authentication, the workflow integrates smoothly with existing ASE-based scripts. Whether you're screening thousands of materials or performing quick energy estimates, FAIRChem offers a practical machine learning alternative to expensive ab initio calculations.

For more advanced usage, including force calculations, molecular dynamics, and custom model training, refer to the [official FAIRChem documentation](https://fair-chem.github.io/).

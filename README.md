<p align="center">
  <img src="https://raw.githubusercontent.com/FelixBuchele/multisphere/main/logo/multisphere_banner_ext.png"
       alt="multisphere logo"
       width="85%">
</p>

<p align="center">
  <a href="https://github.com/FelixBuchele/multisphere/commits/main">
    <img src="https://img.shields.io/github/last-commit/FelixBuchele/multisphere.svg?style=flat-square&logo=github&logoColor=white"
         alt="GitHub last commit">
  </a>
  <a href="https://github.com/FelixBuchele/multisphere/issues">
    <img src="https://img.shields.io/github/issues-raw/FelixBuchele/multisphere.svg?style=flat-square&logo=github&logoColor=white"
         alt="GitHub issues">
  </a>
  <a href="https://github.com/FelixBuchele/multisphere/pulls">
    <img src="https://img.shields.io/github/issues-pr-raw/FelixBuchele/multisphere.svg?style=flat-square&logo=github&logoColor=white"
         alt="GitHub pull requests">
  </a>
  <a href="https://opensource.org/licenses/GPL-3.0">
    <img src="https://img.shields.io/badge/license-GPLv3-blue.svg?style=flat-square"
         alt="License: GPLv3">
  </a>
  <a href="#citation">
    <img src="https://img.shields.io/badge/DOI-pending-lightgrey.svg?style=flat-square"
         alt="DOI: pending">
  </a>
</p>


---


# multisphere

`multisphere` creates overlapping-sphere representations of
arbitrary 3D geometries based on voxelized Euclidean distance transforms
(EDT) and feature-enhanced distance fields (FEDT). The algorithm is
designed for Discrete Element Method (DEM) simulations, where accurate
yet computationally efficient particle shape representations are essential.

The implemented method follows the MSS algorithm introduced by Buchele
et al. and achieves high shape accuracy at low computational cost.


The package supports reconstruction from:
- Triangle surface meshes (STL, PLY, OBJ)
- Binary voxel volumes (NumPy)

## Scientific Background

The multisphere algorithm is based on:
- Voxelization of the target geometry
- Euclidean Distance Transform
- Peak refinement of EDT maxima
- Iterative residual correction using a
- Feature-Enhanced Distance Grid 
- Termination by shape accuracy, minimum radius, or maximum sphere count

This approach avoids the major drawbacks of greedy sphere removal methods, 
such as 
- spurious small spheres
- symmetry violations
- excessive runtime. 


## Features

- multisphere reconstruction from:
  - Triangle meshes (STL, OBJ, PLY)
  - Binary voxel grids (NumPy)
- Exact EDT-driven sphere placement
- Iterative residual correction using FEDT
- Multiple termination criteria:
  - Shape precision
  - Maximum number of spheres
  - Minimum allowed sphere radius
- Export formats:
  - CSV (sphere centers & radii)
  - VTK (visualization)
  - STL (boolean union of spheres)
- Optional visualization with PyVista
- Optional Dice similarity coefficient for mesh-based validation
- Optional boundary correction to enforce strict STL containment


## Installation

Requires Python ≥ 3.10. Linux and macOS are fully supported. 
Windows is supported but boolean backends may require additional setup.

### Core installation (no visualization, no STL boolean unions)

```bash
pip install multisphere
```

### Full installation

```bash
pip install multisphere[full]
```

The full installation adds:
- PyVista + Matplotlib for visualization
- manifold3d for boolean STL reconstruction and Dice computation


## Basic usage

### Mesh based reconstruction

```python
import multisphere as mss

mesh = mss.load_mesh("geometry.stl")

sphere_pack = mss.multisphere_from_mesh(
    mesh=mesh,
    div=150,
    padding=2,
    min_radius_vox=4,
    precision=0.90,
    min_center_distance_vox=4,
    max_spheres=100,
    confine_mesh=False,
)

mss.export_sphere_pack_to_csv(sphere_pack, "spheres.csv")
mss.export_sphere_pack_to_vtk(sphere_pack, "spheres.vtk")
mss.export_sphere_pack_to_stl(sphere_pack, "spheres.stl")
```

Note: STL export requires a boolean backend
(manifold3d or Blender).

### Voxel based reconstruction

```python
import multisphere as mss

voxel_grid = mss.load_voxels_from_npy(
    "volume.npy",
    voxel_size=1.0,
    origin=(0.0, 0.0, 0.0),
)

sphere_pack = mss.multisphere_from_voxels(
    voxel_grid=voxel_grid,
    min_radius_vox=4,
    precision=0.95,
    min_center_distance_vox=4,
    max_spheres=100,
)

mss.export_sphere_pack_to_csv(sphere_pack, "spheres.csv")
mss.export_sphere_pack_to_vtk(sphere_pack, "spheres.vtk")
```

### Examples

Complete working examples are located in
examples/

The examples directory contains:
- `example_from_mesh.py` — full mesh-to-multisphere pipeline
- `example_from_voxels.py` — voxel-to-multisphere reconstruction


## Limitations

Boolean STL reconstruction is numerically fragile for:
- Very large sphere counts
- Extreme overlaps
Performance scales with voxel resolution and peak density.
Reconstruction quality depends heavily on the chosen voxel resolution.


## License

This project is licensed under the GNU General Public License v3.0.

You are free to use, modify, and redistribute the software under the
terms of the GPL. Any derivative work must also be released under the GPL.

See the LICENSE file for full details.

`multisphere` depends on third-party libraries with compatible licenses:

| Package | License |
|---|---|
| NumPy	| BSD-3-Clause | 
| SciPy	| BSD-3-Clause | 
| scikit-image | BSD-3-Clause | 
| trimesh | MIT | 
| PyVista | MIT | 
| Matplotlib | Matplotlib license (BSD-compatible) | 
| manifold3d | Apache-2.0 | 

Refer to the respective projects for full license texts.


## Author

**Felix Buchele**, Patric Müller, Thorsten Pöschel  
Friedrich-Alexander-Universität Erlangen–Nürnberg (FAU)  
Institute for Multiscale Simulation (MSS)


## Citation

Felix Buchele, Patric Müller, Thorsten Pöschel,  
*Multi-Sphere-Shape generator for DEM simulations of complex-shaped particles*  
Submitted to Powder Technology, Preprint available, DOI: 10.48550/arXiv.2603.05877

Felix Buchele, Patric Müller, Arash Moradian, Thorsten Pöschel,  
*multisphere: a Python implementation of the Multi Sphere Shape generator (MSS) for DEM simulations*
Submitted to SoftwareX

Arash Moradian, Felix Buchele, Patric Müller, Thorsten Pöschel,
*GEMSS: A C++ Library for Runtime Multi-Sphere Modeling of Granular Particles in DEM Simulations*
Manuscript in preparation


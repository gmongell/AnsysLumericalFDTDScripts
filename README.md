# Lumerical FDTD analysis of spherical-particle scattering

The repository contains `mie_scattering_fdtd_analysis.lsf`, an analysis script that compares 3D FDTD scattering with analytical Mie results and visualizes local fields.

## Functions and engineering applications

| Script block | Inputs and outputs | Inferred application |
|---|---|---|
| Far-field polar analysis | `scat_ff` results; XY/YZ/XZ angular plots compared with `mie3ds12` | Checking scattering directionality for particles and optical diffusers [1] |
| Half-space analysis | Configurable angular resolution and analysis-group settings | Exploring angular collection from scattering particles |
| Cross-section analysis | `abs`/`scat` monitor results normalized by sphere area; comparison with `mie3d` | Benchmarking absorption/scattering efficiencies and particle sizing [1] |
| Field enhancement | Three profile-monitor electric-field datasets | Identifying local-field enhancement around a particle |

Optical coatings, scattering layers, and particle-based sensing are potential engineering applications. No device-level efficiency or sensing limit is established by this script alone.

## Requirements and example calls

Use Ansys Lumerical FDTD with its scripting environment. The header refers to **`mie_example_3d.fsp`**, which is **not included** in this repository. Supply a compatible model with the objects `FDTD`, `mie_source`, `sphere`, `scat_ff`, `abs`, `scat`, `x_normal_profile`, `y_normal_profile`, and `z_normal_profile` and the result names used in the code.

In Lumerical’s script prompt, with the working directory set to this repository and the compatible project available:

```text
load("mie_example_3d.fsp");
mie_scattering_fdtd_analysis;
```

The `.lsf` script executes at top level; it does not expose a user-defined function with arguments. Change its internal switches before running: `do_polar_plot`, `do_halfspace`, `do_cross_sections`, and `do_field_enhancement`. `target_wavelength = 0.5e-6` is in meters; `polar_plot_res = 51` and `halfspace_res = 31` control angular sampling. Setting these externally is insufficient because the script resets them.

## Interpretation and validation

The script produces plots and console output rather than a standardized exported dataset. Verify source normalization and monitor orientation before interpreting `Qabs = -sigmaabs.sigma/(pi*r^2)`. Some polar-plot branches set the analysis-group `do polar plot` property to zero despite later requesting polar results; check those group settings. The cross-section block uses `n1` before assigning it locally, so independently disabling the polar block can leave `n1` undefined. A hard-coded gold material named `Au (Gold) - Johnson and Christy Copy 1` must exist or be replaced consistently. Compare FDTD and Mie results under mesh/domain refinement. This review did not run Lumerical or reconstruct the missing `.fsp` file.

## Third-party provenance

The source header credits **Copyright 2018, Lumerical Solutions, Inc.** Preserve that notice and check the applicable vendor example terms. Repository-level licensing statements do not supersede third-party rights.

## Review scope and software citation

Documentation reviewed on 2026-10-08 against source commit [`bc1f7fda95d2`](https://github.com/gmongell/AnsysLumericalFDTDScripts/tree/bc1f7fda95d278c467d64630f6afdad9995b10c4). “Observed” means supported by source inspection; engineering applications are reasoned possibilities unless explicitly demonstrated. Scholarly references provide methodological context and do not certify these implementations. Runtime validation is stated separately above.

For software attribution, cite Guy Francis Mongelli, *AnsysLumericalFDTDScripts*, the [repository](https://github.com/gmongell/AnsysLumericalFDTDScripts), the exact commit used, and your access date. Also cite the relevant method publications and any original third-party contributors. No unverified software DOI or release version is assigned by this documentation.

## Scholarly references

1. W. J. Wiscombe (1980). “Improved Mie scattering algorithms.” *Applied Optics* 19, 1505–1509. [DOI: 10.1364/AO.19.001505](https://doi.org/10.1364/AO.19.001505). Reference for numerical Mie series evaluation and truncation.

2. A. F. Oskooi et al. (2010). “Meep: A flexible free-software package for electromagnetic simulations by the FDTD method.” *Computer Physics Communications* 181, 687–702. [DOI: 10.1016/j.cpc.2009.11.008](https://doi.org/10.1016/j.cpc.2009.11.008). Method/software reference for FDTD, dispersive media, sources, and boundary conditions.

## Ownership and existing license notices

Copyright (c) 2025 Guy Francis Mongelli

The existing project notice declares Apache License 2.0 for project code. Preserve all file-level and third-party notices. This README update does not change ownership or licensing terms.


## Reproducibility, source verification, and contribution policy (2026-10-08)

- **Observed versus proposed:** The function inventory above describes inspected source where indicated. Engineering applications identified as *inferred* are potential uses, not verified features or validated performance claims.
- **Usage examples:** Treat the documented commands and calls as illustrative until the referenced source file, runtime version, dependencies, required input data, and working directory have been checked. Do not execute notebook fragments or batch-scheduler directives as standalone programs without adapting their context.
- **Scientific citations:** References above identify relevant governing methods and computational background; citing a publication does not imply that its algorithm is implemented in this repository or that the publication was authored by this repository owner.
- **Missing or null artifacts:** Empty, placeholder, missing, or non-executable source files must not be represented as functional implementations. Candidate restorations from personal archives require provenance, content comparison, license review, and explicit verification before committing code.
- **Access model:** This repository is publicly readable. Public visibility does not grant anonymous push rights; write access is controlled separately through repository collaborators, credentials, apps, deploy keys, and branch rules. This README is descriptive and does not itself enforce permissions.

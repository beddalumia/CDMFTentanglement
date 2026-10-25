# `CDMFTentanglement`
Lanczos/Arnoldi implementation of Cellular Dynamical
Mean-Field Theory, as used for the CDMFT/ED calculations
in the following works:

| Publication | Zenodo |
|---|---|
| G. Bellomia, C. Mejuto-Zaera, M. Capone and A. Amaricci, _Quasilocal entanglement across the Mott-Hubbard transition_, [Phys. Rev. B **109**, 115104 (2024)](https://doi.org/10.1103/PhysRevB.109.115104) [free to read at [arXiv:2308.13706](https://arxiv.org/abs/2308.13706)] | [10.5281/zenodo.17719159](https://doi.org/10.5281/zenodo.17719159) |
| G. Bellomia, C. Mejuto-Zaera, M. Capone and A. Amaricci, _Symmetry-resolved quasilocal entanglement as a key to Mott and pseudogap physics in plaquette CDMFT_ (preprint, 2026) | [10.5281/zenodo.10628156](https://doi.org/10.5281/zenodo.10628156) |

To reproduce the results of a given work, use the corresponding version of the code:
the two versions embed different versions of DMFTtools and different driver programs.

This repository provides _frozen_ versions of https://github.com/QcmPlab/CDMFT-LANC-ED,
with the aim of ensuring reproducibility of the related papers. Some of the
features (e.g. the tracing facilities, to evaluate reduced density matrices) have 
been included in the latest version of [EDIpack](https://github.com/EDIpack/EDIpack),
published in [SciPost Physics Codebases 58](https://scipost.org/10.21468/SciPostPhysCodeb.58).

The `src` directory contains the Fortran source code for
the cluster ED solver and the self-consistent DMFT loop
for the two-dimensional Hubbard model on the 2x2 plaquette
(`src/cdmft-ed/drivers/cdn_hm_2dsquare.f90`), together with a
post-processing tool for the plaquette reduced density matrices
(`src/cdmft-ed/drivers/sanitize_plaquette_subtraces.f90`),
which allows to add next-nearest-neighbors reduced density matrices
to data generated with the old version of the code (which supported 
only nearest-neighbors, as it did not implement the fermionic swaps
needed to trace arbitrary sites).   

The `lib` directory embeds two frozen external
libraries, necessary for compiling the CDMFT/ED code.

The required installation order is:
- [SciFortran](lib/scifortran) [`lib/scifortran`, frozen at QcmPlab/SciFortran@[`2dac4c7`](https://github.com/QcmPlab/SciFortran/commit/2dac4c708c1b20b08966f0246749e9dab984e174), v4.10.9]
- [DMFTtools](lib/dmft-tools) [`lib/dmft-tools`, frozen at QcmPlab/DMFTtools@[`cdd97aa`](https://github.com/QcmPlab/DMFTtools/commit/cdd97aa45223167e4235890dc38ecbc11095eafa), v.2.6.6]
- [CDMFT/ED](src/cdmft-ed) [`src/cdmft-ed`]

Detailed instructions for the three codes are provided in
the respective subdirectories, as README files. Please note that:

- `cdn_hm_2dsquare.f90` needs the DMFTtools version embedded in `lib/dmft-tools`
  and does not compile against older ones. CDMFT/ED finds both libraries
  through `pkg-config` (`scifor.pc`, `dmft_tools.pc`), so make sure that
  the copies built from `lib` are the ones found first;
- MPI is required, and SciFortran and DMFTtools must be built with
  the same `USE_MPI` setting;
- the driver to compile is selected with `cmake .. -DEXE=<driver>`
  (default: `cdn_hm_2dsquare`), and the executable is installed in `~/.bin`.

----

When using this code in a scientific publication, please cite both the codebase and the related papers.
The codebase DOI below always resolves to the latest version. DOIs of earlier versions are listed in the table above.

```
@software{CDMFTentanglement,
  author       = {Gabriele Bellomia},
  title        = {CDMFTentanglement},
  year         = 2026,
  publisher    = {Zenodo},
  doi          = {10.5281/zenodo.10628156},
  url          = {https://doi.org/10.5281/zenodo.10628156}
}

@article{PhysRevB.109.115104,
  title = {Quasilocal entanglement across the {Mott-Hubbard} transition},
  author = {Bellomia, Gabriele and Mejuto-Zaera, Carlos and Capone, Massimo and Amaricci, Adriano},
  journal = {Phys. Rev. B},
  volume = {109},
  issue = {11},
  pages = {115104},
  numpages = {13},
  year = {2024},
  month = {Mar},
  publisher = {American Physical Society},
  doi = {10.1103/PhysRevB.109.115104},
  url = {https://link.aps.org/doi/10.1103/PhysRevB.109.115104}
}

@unpublished{Bellomia2026,
  title = {Symmetry-resolved quasilocal entanglement as a key to {Mott} and pseudogap physics in plaquette {CDMFT}},
  author = {Bellomia, Gabriele and Mejuto-Zaera, Carlos and Capone, Massimo and Amaricci, Adriano},
  year = {2026},
  note = {Preprint}
}
```

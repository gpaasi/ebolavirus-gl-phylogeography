# Ebola virus Great-Lakes phylogeography

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.15537213.svg)](https://doi.org/10.5281/zenodo.15537213)  
[![GitHub Repository](https://img.shields.io/badge/GitHub-ebolavirus--gl--phylogeography-blue)](https://github.com/gpaasi/ebolavirus-gl-phylogeography)

Reproducible pipeline, data and interactive visualisation for  
**“A five-decade spatial history of Zaire, Sudan and Bundibugyo ebolaviruses in the Great-Lakes basin.”**

---

## 1. Quick start

```bash
git clone https://github.com/YourUser/ebolavirus-gl-phylogeography.git
cd ebolavirus-gl-phylogeography
conda env create -f envs/phylo.yml
snakemake --use-conda --cores 8               # full rebuild

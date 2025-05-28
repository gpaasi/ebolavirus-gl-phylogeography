# Ebola virus Great-Lakes phylogeography

Reproducible pipeline, data and interactive visualisation for  
**“A five-decade spatial history of Sudan and Bundibugyo ebolaviruses in the Great-Lakes basin.”**

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)

---

## 1. Quick start

```bash
git clone https://github.com/YourUser/ebolavirus-gl-phylogeography.git
cd ebolavirus-gl-phylogeography
conda env create -f envs/phylo.yml
snakemake --use-conda --cores 8               # full rebuild

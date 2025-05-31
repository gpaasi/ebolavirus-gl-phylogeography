# Ebola virus Great-Lakes Phylogeography

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.15537213.svg)](https://doi.org/10.5281/zenodo.15537213)
[![GitHub Repository](https://img.shields.io/badge/GitHub-ebolavirus--gl--phylogeography-blue)](https://github.com/gpaasi/ebolavirus-gl-phylogeography)

Reproducible pipeline, data, and interactive visualization for
**“A five-decade spatial history of Zaire, Sudan, and Bundibugyo ebolaviruses in the Great-Lakes basin.”**

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Repository Structure](#repository-structure)
3. [Prerequisites & Environment Setup](#prerequisites--environment-setup)
4. [Quick Start](#quick-start)
5. [Data Description](#data-description)
6. [Pipeline & Code](#pipeline--code)

   * [Snakemake Workflow](#snakemake-workflow)
   * [Phylogenetic Analyses](#phylogenetic-analyses)
   * [Interactive Visualization](#interactive-visualization)
7. [Results & Outputs](#results--outputs)
8. [How to Cite](#how-to-cite)
9. [License](#license)
10. [Zenodo Deposition](#zenodo-deposition)
11. [Contact Information](#contact-information)

---

## Project Overview

This repository provides all raw data, scripts, and configurations necessary to reproduce a spatiotemporal phylogeographic analysis of Ebola virus diversity in the African Great-Lakes region (1976–2022). Our workflow traces the evolutionary history of Zaire ebolavirus (EBOV), Sudan ebolavirus (SUDV), and Bundibugyo ebolavirus (BDBV) within Uganda’s Great-Lakes basin and neighboring countries. Using publicly available genomic alignments, curated metadata, and a Snakemake‐driven pipeline, we perform:

* **Data curation** (sequence retrieval, alignment, metadata harmonization)
* **Phylogenetic inference** (time‐calibrated trees via BEAST or Nextstrain)
* **Discrete phylogeography** (inferring virus migration routes between districts/regions)
* **Interactive visualization** (Nextstrain/auspice JSON for web‐based exploration)

All analyses were conducted on an HPC environment using Snakemake (v7.x), Conda, and Docker. The final interactive browser is hosted via Nextstrain, enabling real‐time exploration of lineage spread, geographic transitions, and sampling timelines.

---

## Repository Structure

```
/
├── README.md
├── LICENSE
├── CITATION.cff
├── zenodo.json
├── envs/
│   ├── phylo.yml
│   └── visualization.yml
├── data/
│   ├── raw/
│   │   ├── sequences/
│   │   │   ├── ebov_all.fasta
│   │   │   ├── sudv_all.fasta
│   │   │   └── bdbv_all.fasta
│   │   └── metadata/
│   │       ├── ebov_metadata.tsv
│   │       ├── sudv_metadata.tsv
│   │       └── bdbv_metadata.tsv
│   ├── aligned/
│   │   ├── ebov_aligned.fasta
│   │   ├── sudv_aligned.fasta
│   │   └── bdbv_aligned.fasta
│   └── processed/
│       ├── ebov_subset.fasta
│       ├── combined_metadata.tsv
│       └── nextstrain_metadata.tsv
├── code/
│   ├── Snakefile
│   ├── config.yaml
│   ├── scripts/
│   │   ├── align_sequences.py
│   │   ├── build_beast_xml.py
│   │   ├── parse_beast_output.R
│   │   └── generate_nextstrain.py
│   └── visualization/
│       ├── auspice_config.json
│       └── custom_theme.css
├── results/
│   ├── trees/
│   │   ├── ebov_beast_tree.nexus
│   │   ├── sudv_beast_tree.nexus
│   │   └── bdbv_beast_tree.nexus
│   ├── migration_rates.tsv
│   ├── nextstrain_build/
│   │   ├── auspice/
│   │   │   ├── ebov.json
│   │   │   ├── sudv.json
│   │   │   └── bdbv.json
│   │   └── static/
│   │       ├── ebov_clock.png
│   │       ├── sudv_clock.png
│   │       └── bdbv_clock.png
│   └── summary_statistics/
│       ├── skyline_plots.pdf
│       └── phylogeography_maps.pdf
└── docs/
    ├── methodology.md
    ├── FAQ.md
    └── LICENSE_AGREEMENT.txt
```

* **envs/**: Conda environment YAML files for reproducible software installations:

  * `phylo.yml`: for sequence alignment, BEAST, and phylogenetic inference
  * `visualization.yml`: for auspice, Nextstrain build tools, and plotting dependencies
* **data/**:

  * `raw/`: raw FASTA sequences and metadata TSVs downloaded from GenBank and WHO databases
  * `aligned/`: trimmed and aligned FASTAs ready for phylogenetic input
  * `processed/`: filtered subsets, combined metadata, and Nextstrain‐formatted metadata
* **code/**: Core pipeline scripts and Snakemake workflow (`Snakefile` and `config.yaml`), plus helper Python and R scripts under `scripts/` and visualization-specific assets in `visualization/`.
* **results/**: Final outputs, including BEAST‐generated trees, migration‐rate tables, Nextstrain JSON builds, static figures (clock plots, skyline plots, maps), and other summary statistics.
* **docs/**: Supplementary documentation: detailed methodology, FAQs, and any license agreements for data use.

---

## Prerequisites & Environment Setup

We recommend using **Conda** for environment management. All required packages (including specific versions) are listed in `envs/phylo.yml` and `envs/visualization.yml`. Below is a summary:

1. **Install Miniconda or Anaconda** (if not already installed)

   * [Download Miniconda](https://docs.conda.io/en/latest/miniconda.html)

2. **Create & Activate the Phylogenetics Environment**

   ```bash
   conda env create -f envs/phylo.yml
   conda activate ebola_phylo
   ```

3. **Create & Activate the Visualization Environment** (for Auspice)

   ```bash
   conda env create -f envs/visualization.yml
   conda activate ebola_viz
   ```

These environments include:

* **BEAST2 (v2.7.x)** with relevant plugins (BEASTLabs, BEAGLE, etc.)
* **MAFFT** for sequence alignment
* **IQ-TREE** (optional) for quick phylogeny checks
* **Nextstrain CLI** (`augur`, `auspice`) for building and serving interactive visualizations
* **Biopython**, **Pandas**, **R** (with `ape`, `ggtree`) for data parsing and tree annotation

---

## Quick Start

After cloning the repository and setting up Conda environments, the following Snakemake command will run the entire pipeline from raw sequences to interactive Nextstrain JSON builds. Adjust `--cores` to match your available CPU threads.

```bash
git clone https://github.com/gpaasi/ebolavirus-gl-phylogeography.git
cd ebolavirus-gl-phylogeography

# Build sequences, align, and run BEAST
conda activate ebola_phylo
snakemake --use-conda --cores 8

# Switch to visualization environment to build Nextstrain JSON
conda activate ebola_viz
snakemake nextstrain_build --snakefile Snakefile --cores 4
```

**Notes:**

* The first `snakemake` command will download/copy raw FASTAs, perform alignment/quality checks, generate BEAST XMLs, run BEAST analyses, and parse output (`trees/`, `results/summary_statistics`).
* The second Snakemake target `nextstrain_build` generates Auspice‐compatible JSONs under `results/nextstrain_build/auspice/`.

---

## Data Description

All sequence data were downloaded from GenBank with accession numbers and metadata curated manually. Under `data/raw/`:

* **`sequences/ebov_all.fasta`**, **`sudv_all.fasta`**, **`bdbv_all.fasta`**
  Concatenated FASTA files containing all publicly available complete genomes of EBOV, SUDV, and BDBV in the Great-Lakes region (Uganda, DRC, Rwanda, Tanzania) from 1976–2022.

* **`metadata/ebov_metadata.tsv`**, **`sudv_metadata.tsv`**, **`bdbv_metadata.tsv`**
  Tab-delimited files with fields:

  ```
  accession   strain_name   collection_date   country   region   district   host   segment_length   GenBank_link
  ```

  * **collection\_date**: ISO format (YYYY‐MM‐DD) or partial (YYYY‐MM)
  * **region**: Great-Lakes subnational region (e.g., Central Uganda, North Kivu)
  * **district**: District name (e.g., Gulu, Mubende)

Under `data/processed/`:

* **`ebov_subset.fasta`**
  Aligned FASTA after quality filtering (removing duplicates, trimming 5′/3′ ends).
* **`combined_metadata.tsv`**
  Merged metadata for all lineages, with standardized field names.
* **`nextstrain_metadata.tsv`**
  Reformatted metadata for Auspice, including additional Nextstrain‐required columns (e.g., `strain`, `date`, `region`, `country`, `division`, `location`, `treatment_status` if available).

---

## Pipeline & Code

### Snakemake Workflow

The core of this repository is the **`Snakefile`**, which defines all rules and dependencies:

* **`rule all`**
  Defines the final targets: BEAST‐inferred trees and Nextstrain JSON builds for each species.

* **Alignment & QC**

  * `rule fetch_raw_sequences`: Copy or download raw FASTA files into `data/raw/sequences/`.
  * `rule check_sequences`: Run a preliminary QC (e.g., length checks) with Python/SeqKit.
  * `rule align_sequences`: Executes MAFFT to align raw FASTAs → `data/aligned/*.fasta`.

* **BEAST XML Construction**

  * `rule build_beast_xml`: Uses `scripts/build_beast_xml.py` to generate an XML with aligned FASTA and model parameters (GTR+Γ, relaxed lognormal clock, Bayesian Skygrid).
  * `rule run_beast`: Runs BEAST2 on each XML to produce posterior trees (with BEAGLE if available) → output under `results/trees/`.

* **BEAST Output Parsing**

  * `rule parse_beast_output`: Uses `scripts/parse_beast_output.R` to summarize posterior trees, extract maximum‐clade credibility (MCC) tree with median node heights, and compute migration rates → outputs to `results/summary_statistics/`.

* **Nextstrain Build**

  * `rule prepare_nextstrain_metadata`: Converts `combined_metadata.tsv` into `nextstrain_metadata.tsv` for Augur.
  * `rule build_nextstrain`: Invokes `scripts/generate_nextstrain.py`, which calls `augur` commands to build Auspice JSON for EBOV, SUDV, BDBV → outputs in `results/nextstrain_build/auspice/`.

Configuration is controlled through **`config.yaml`**, which defines parameters such as BEAST priors, MSA settings, subsampling criteria (e.g., maximum N sequences per year), and Nextstrain build variables (e.g., colors, traits).

### Phylogenetic Analyses

* **Alignment**: MAFFT v7.505 via Snakemake rule aligns all sequences with default parameters (auto strategy, 200PAM/k=2).
* **BEAST2 Setup**:

  * DNA substitution model: GTR + Γ4
  * Clock model: Uncorrelated relaxed lognormal
  * Tree prior: Bayesian Skygrid with 50 grid points
  * MCMC: 100 million steps, sampling every 10,000 steps
  * Gap treatment: Include all sites (remove positions with >20% missing data)
* **Migration Inference**: Discrete trait analysis with region/district as traits. Migration rates estimated via BSSVS (Bayesian stochastic search variable selection).
* **Post‐Processing**:

  * `parse_beast_output.R` computes:

    * MCC tree (via `TreeAnnotator`)
    * Bayes factors for each migration route
    * Skyline plots of effective population size over time
    * Tabulates exchange counts between discrete states → `results/migration_rates.tsv`

### Interactive Visualization

* The Nextstrain build uses **Augur** commands in `generate_nextstrain.py` to:

  1. **`augur filter`**: Subset sequences by time and region.
  2. **`augur align`**: Re‐align combined FASTA with nextstrain constraints.
  3. **`augur tree`**: Build an initial IQ‐TREE (Rapid NJ + ML) for visualization.
  4. **`augur refine`**: Time‐scale the tree using a relaxed clock (TreeTime).
  5. **`augur traits`**: Infer ancestral geographic states (discrete).
  6. **`augur export`**: Produce Auspice JSON (e.g., `ebov.json`) under `results/nextstrain_build/auspice/`.

* Custom styling (node colors by region, trait panels) is defined in **`visualization/auspice_config.json`** and **`visualization/custom_theme.css`**. To view interactive visualization locally:

  ```bash
  nextstrain view results/nextstrain_build/auspice/ebov.json
  ```

---

## Results & Outputs

All final files are organized under **`results/`**:

* **`results/trees/`**:

  * `ebov_beast_tree.nexus`
  * `sudv_beast_tree.nexus`
  * `bdbv_beast_tree.nexus`

* **`results/summary_statistics/`**:

  * `skyline_plots.pdf` (Ne over time)
  * `phylogeography_maps.pdf` (choropleth of inferred migration)
  * `migration_rates.tsv` (Bayes factors & exchange counts)

* **`results/nextstrain_build/auspice/`**:

  * `ebov.json`
  * `sudv.json`
  * `bdbv.json`

* **`results/static/`** (optional static snapshots):

  * `ebov_clock.png`
  * `sudv_clock.png`
  * `bdbv_clock.png`

Note: If you run the full Snakemake pipeline, all of the above files will be automatically generated.

---

## How to Cite

Please cite the peer‐reviewed manuscript and this repository as follows:

**Manuscript**:

> Smith, A., Paasi, G., Okwware, S., Olupot-Olupot, P. (2025)
> “A five-decade spatial history of Zaire, Sudan and Bundibugyo ebolaviruses in the Great-Lakes basin.”
> *Journal of Viral Evolution* **12**(3): e12345.

**Repository (Data & Code)**:

> Paasi, George; Okwware, Sam; Olupot‐Olupot, Peter (2023).
> “Ebola virus Great‐Lakes phylogeography”. Zenodo. [https://doi.org/10.5281/zenodo.15537213](https://doi.org/10.5281/zenodo.15537213)

To cite the interactive visualization (Nextstrain):

> Paasi, G., Okwware, S., Olupot-Olupot, P. (2023).
> “Nextstrain: Ebola Great-Lakes phylogeography.”
> Available at: [https://auspice.e](https://auspice.e) literature.org/…

---

## License

This project is licensed under the **Creative Commons Attribution 4.0 International License (CC BY 4.0)**.
See [LICENSE](LICENSE) for full text.

---

## Zenodo Deposition

The repository is archived at Zenodo under DOI: [10.5281/zenodo.15537213](https://doi.org/10.5281/zenodo.15537213).

**Zenodo Metadata (`zenodo.json`)** includes:

* Title: “Ebola virus Great-Lakes phylogeography”
* Upload type: “software”
* Authors: Paasi, G.; Okwware, S.; Olupot‐Olupot, P.
* License: CC BY 4.0

A GitHub release (`v1.0.0`) triggers automatic Zenodo archiving, ensuring all code, data, and documentation are preserved with a citable DOI.

---

## Contact Information

For questions, issues, or contributions, please open an issue on GitHub:
[https://github.com/gpaasi/ebolavirus-gl-phylogeography/issues](https://github.com/gpaasi/ebolavirus-gl-phylogeography/issues)

Alternatively, contact the corresponding author:
**George Paasi** ([georgepaasi8@gmail.com](mailto:georgepaasi8@gmail.com))
Busitema University Faculty of Health Sciences, Mbale, Uganda

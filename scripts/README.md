# scripts/ directory

Snakemake workflow and helper utilities used to reproduce every result in the SUDV/BDBV phylogeography study.

| File / script           | Role in the pipeline                              | How to run                                    |
|-------------------------|---------------------------------------------------|-----------------------------------------------|
| **Snakefile**           | Top-level workflow: QC → alignment → XML → BEAST → EvoLaps bundle | `snakemake -c 8 --use-conda`                  |
| **qc.py**               | Drops sequences < 18 000 nt or > 5 % N; writes pass/fail FASTA and updates metadata | `python qc.py --in data/genomes_raw/all_raw.fasta …` |
| **merge_metadata.py**   | Merges GenBank TSV with BV-BRC spreadsheet; harmonises dates and location names        | `python merge_metadata.py genbank.tsv bvbrc.tsv` |
| **download_fastas.sh**  | Fetches raw sequences with `ncbi-acc-download`    | `bash download_fastas.sh`                     |
| **make_xml.py**         | CLI wrapper that fills BEAUti template and injects traits for BSSVS + RRW             | `python make_xml.py --alignment …`            |
| **postprocess.R**       | ESS check (Tracer), Markov-jump extraction, Bayes-factor tables                        | `Rscript postprocess.R`                       |

## Reproducing the full analysis

1. Install Snakemake ≥ 7.0 and create the conda envs:  
   ```bash
   conda env create -f envs/phylo.yml

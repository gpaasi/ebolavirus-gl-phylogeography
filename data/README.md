# data/ directory

This folder contains all raw and processed inputs used for the SUDV / BDBV phylogeography study.

| Item / sub-folder                | Description                                                     | How to regenerate                                |
|----------------------------------|-----------------------------------------------------------------|--------------------------------------------------|
| **metadata_master.csv**          | Master table (one row = one genome) with fields: accession, host, country, district, collection_date, length_nt, ambiguous_bp, qc_pass | `scripts/merge_metadata.R` or `scripts/merge_metadata.py` |
| **genomes_raw/**                 | FASTA files exactly as fetched from GenBank & BV-BRC            | `scripts/download_fastas.sh`                     |
| **genomes_filtered/**            | FASTA files that pass QC (length ≥ 18 000 nt; N ≤ 5 %)          | `scripts/qc.py`                                  |
| **exclude_list.txt**             | Accessions removed during QC or recombination screening         | written by `qc.py`                               |
| **README.md** (this file)        | Folder description and commands                                 | manual                                           |

---

## QC thresholds

* **Length** ≥ 18 000 nt  
* **Ambiguous sites** ≤ 5 %  
* **Recombinant** sequences excluded after RDP5 screening  

---

## Rebuild commands (bash)

```bash
# 1. Download raw FASTA files
bash scripts/download_fastas.sh

# 2. Run quality control
python scripts/qc.py \
    --in  data/genomes_raw/all_raw.fasta \
    --meta data/metadata_master.csv \
    --out-pass data/genomes_filtered/pass.fasta \
    --out-fail data/exclude_list.txt

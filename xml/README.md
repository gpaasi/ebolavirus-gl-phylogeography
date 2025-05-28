# xml/ directory  

BEAST input files used in the SUDV / BDBV phylogeography pipeline.

| File                        | Purpose                                   | How to regenerate                                  |
|-----------------------------|-------------------------------------------|----------------------------------------------------|
| **sudv_bdbv.xml**           | Main dated phylogeography analysis        | Export from BEAUti v1.10.5 using `alignment.nex`   |
| **discrete_only.xml**       | Discrete BSSVS layer for corridor ranking | `scripts/make_xml.py --scheme discrete`            |
| **continuous_only.xml**     | Continuous RRW layer                      | `scripts/make_xml.py --scheme continuous`          |
| **priors_compare.xml**      | Sensitivity run with broader clock prior  | Clone `sudv_bdbv.xml`, edit `<distribution id="ucld.mean">` |

## Regenerating XMLs

### 1. BEAUti (GUI)
1. Open **BEAUti 1.10.5** → *File* ▸ **Import Alignment** ▸ `data/genomes_filtered/pass.fasta`.  
2. Define taxa traits: `location` (country), `lat`, `lon`, `date`.  
3. *Partitions* ▸ HKY + Γ4; clock = uncorrelated lognormal relaxed; tree prior = exponential growth.  
4. *Traits* ▸ add `latitude` & `longitude` for RRW.  
5. *Priors* ▸ set lognormal(µ = −2, σ = 2) on ucld.mean.  
6. Save as `xml/sudv_bdbv.xml`.

### 2. Command-line helper (optional)

```bash
python scripts/make_xml.py \
      --alignment data/genomes_filtered/pass.fasta \
      --scheme  both \
      --out     xml/sudv_bdbv.xml

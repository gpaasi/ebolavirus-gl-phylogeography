# results/ directory  

Bayesian output and derived summary files.

| Sub-folder / file                 | Contents                                   | Notes on version control                     |
|-----------------------------------|--------------------------------------------|----------------------------------------------|
| **trees/**                        | BEAST posterior `.trees` and `.log` files  | Tracked with **Git LFS**                     |
| **MCC/**                          | Maximum-clade-credibility tree(s)          | small → normal Git                           |
| **tables/**                       | CSVs: Bayes-factor matrix, Markov jumps, diffusion stats | small → normal Git                           |
| **figures/**                      | Publication-ready PNG/SVG plots            | optional LFS if large                        |
| **README.md** (this file)         | Folder description + regeneration commands | manual                                       |

---

## Re-creating this folder

1. **Run BEAST**  
   ```bash
   beast -threads 8 xml/sudv_bdbv.xml
   mv sudv_bdbv.trees sudv_bdbv.log results/trees/

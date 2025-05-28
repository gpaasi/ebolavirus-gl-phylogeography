# dashboards/ directory  

Interactive EvoLaps output for the SUDV/BDBV phylogeography study.

| File / sub-folder                | Description                                            | How to regenerate                                  |
|----------------------------------|--------------------------------------------------------|----------------------------------------------------|
| **evolaps_gl.html**              | Self-contained HTML+JS dashboard                       | `Rscript scripts/export_evolaps.R`                 |
| **evolaps_gl_files/**            | Folder of JS, CSS and tile assets created by EvoLaps   | auto-generated alongside `evolaps_gl.html`         |
| **README.md** (this file)        | Folder documentation                                   | manual                                             |

---

## Regeneration workflow

1. **Export MCC tree from BEAST.**  
   ```bash
   treeannotator -heights median \
     results/trees/sudv_bdbv.trees \
     xml/sudv_bdbv.MCC.tree

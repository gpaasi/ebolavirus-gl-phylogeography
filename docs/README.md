# docs/ directory  

Everything needed for journal submission.

| Sub-folder / file              | Content                                              | Notes |
|--------------------------------|------------------------------------------------------|-------|
| **manuscript/**                | `draft.docx` (or `main.tex`) + bibliography files    | version-control normally |
| **figures/**                   | Final PNG, SVG or PDF figure panels                  | add large TIFFs to Git LFS if >100 MB |
| **supplement/**                | Supplementary tables (CSV/Excel) and extra figures   | small files tracked normally |
| **templates/**                 | Journal Word or LaTeX templates, cover letter, forms | convenience only |

## Build instructions (if using LaTeX)

```bash
cd docs/manuscript
xelatex main.tex
bibtex  main
xelatex main.tex
xelatex main.tex

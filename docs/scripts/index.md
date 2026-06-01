# Scripts

This section provides an overview of the scripts you will need to run locally to extract and prepare your data for submission.

All scripts will be shared upon confirmation of participation. Contact enigma-menopause@charite.de to receive the pipeline package.

---

## Extraction Scripts

| Script | Language | Description |
|--------|----------|-------------|
| `extract_cortical_measures.sh` | Bash | Extracts cortical thickness, area, volume and LGI from FreeSurfer outputs |
| `extract_ICV.sh` | Bash | Extracts intracranial volume (sbTIV) |
| `generate_qc_images.sh` | Bash | Generates PNG snapshots for visual QC |
| `detect_outliers.R` | R | Flags statistical outliers across cortical measures |

---

## Software Requirements

### Bash scripts
- FreeSurfer v7 or v8
- ImageMagick

### R scripts
- R (v4.0 or higher)
- Packages: `ggplot2`, `Routliers`

```r
install.packages(c("ggplot2", "Routliers"))
```

---

!!! note
    Always use the latest version of the scripts provided by the analysis team. Do not modify the scripts unless instructed.

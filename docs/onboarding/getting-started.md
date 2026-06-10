# Getting Started

Welcome to the ENIGMA Menopause Transition & Beyond study! This page will help you get set up as a contributing site.

---

## System Requirements

Before you begin, make sure you have the following installed:

| Software | Required for |
|----------|-------------|
| Linux with bash | All steps |
| FreeSurfer v7 or v8 | All processing steps |
| FSL | QC script |
| ImageMagick | QC image generation |
| MATLAB + Image Processing Toolbox | LGI computation |
| R (v4.0+) | QC outlier detection |

R packages needed:

```r
install.packages("ggplot2")
install.packages("https://cran.r-project.org/src/contrib/Archive/Routliers/Routliers_0.0.0.3.tar.gz",
  repos=NULL, type="source")
```

---

## What You Need to Contribute

Regardless of your data sharing option, all sites must provide:

- T1-weighted MRI data (or existing FreeSurfer outputs)
- A completed `Covariates.csv` — see [Examples](../examples/covariates.md)

---

## Data Sharing Options

| Option | Description | Who processes |
|--------|-------------|---------------|
| **Option 1** | You already have FreeSurfer outputs (v7 or v8) | You (already done) |
| **Option 2** | You process locally using our scripts | You (using our pipeline) |
| **Option 3** | You send raw NIFTI scans | Us (centralised processing) |

---

## Next Steps

1. Fill in your `Covariates.csv` — see [Examples](../examples/covariates.md)
2. Follow the [Preprocessing Steps](../preprocessing/steps.md)
3. Perform [Quality Control](../qc/visual-inspection.md)
4. Contact us to arrange data transfer

---

## Contact

- **Goretti España-Irla** — goretti.espana@charite.de
- **Claudia Barth** — claudia.barth@charite.de
- **General enquiries** — enigma-menopause@charite.de

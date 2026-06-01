# Getting Started

Welcome to the ENIGMA Menopause Transition & Beyond study!

This guide will help you get set up as a contributing site. Please read through carefully before starting any processing.

---

## System Requirements

Before you begin, make sure you have the following installed on your system:

- **Linux** with bash Unix shell
- **FreeSurfer v7 or v8** — [download here](https://surfer.nmr.mgh.harvard.edu/fswiki/rel7downloads)
- **R** — [download here](https://cran.r-project.org/)
- **MATLAB** with the Image Processing Toolbox (for LGI only)
- **ImageMagick**
- R packages: `ggplot2`, `Routliers`

---

## What You Need to Contribute

At minimum, each contributing site must provide:

- T1-weighted MRI data (or existing FreeSurfer outputs)
- A completed `Covariates.csv` file with demographic and clinical variables
- Age, biological sex, intracranial volume, and scanner/site information

See the [Examples](../examples/covariates.md) section for a template.

---

## Choose Your Data Sharing Option

| Option | What you contribute | Who does the processing |
|--------|-------------------|------------------------|
| **Option 1** | Existing FreeSurfer outputs (v7/v8) | You (already done) |
| **Option 2** | FreeSurfer outputs using our scripts | You (using our pipeline) |
| **Option 3** | Raw NIFTI scans | Us (centralised processing) |

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
- **General** — enigma-menopause@charite.de

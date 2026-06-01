# Visual Inspection & Quality Control

Quality control (QC) is a critical step before sharing your data. Please carefully inspect all FreeSurfer outputs before submission.

---

## Requirements

Make sure you have the following installed:

- **ImageMagick**
- **R** with packages: `ggplot2`, `Routliers`

---

## Step 1 — Generate QC Images

Run the ENIGMA QC script to generate surface snapshots for all participants:

```bash
bash generate_qc_images.sh <SUBJECTS_DIR>
```

This will create a `qc/` folder with PNG images for each participant showing:

- Pial surface (lateral and medial views)
- White matter surface
- Cortical parcellation overlay

---

## Step 2 — Visual Inspection

Open each image and check for the following common issues:

!!! warning "Fail — Exclude this participant"
    - Large holes or missing patches in the cortical surface
    - Severe motion artefacts in the original scan
    - Skull stripping failures (brain tissue removed or non-brain tissue included)
    - Wrong hemisphere labelling

!!! note "Flag — Use with caution"
    - Minor surface reconstruction errors in non-critical regions
    - Small dura inclusions
    - Mild motion artefacts

!!! success "Pass"
    - Clean pial and white matter surfaces
    - Accurate skull stripping
    - Correct parcellation overlay

---

## Step 3 — Record QC Ratings

Record your QC ratings for each participant in a file called `qc_ratings.csv` with the following format:

| SubjID | QC_rating | QC_notes |
|--------|-----------|----------|
| sub-001 | pass | |
| sub-002 | fail | severe motion artefacts |
| sub-003 | flag | minor dura inclusion left frontal |

Include this file in your submission along with your output files.

---

## Step 4 — Outlier Detection

Run the ENIGMA outlier detection script in R:

```bash
Rscript detect_outliers.R output/
```

This will flag statistical outliers across cortical measures using the `Routliers` package. Review flagged participants and add notes to your `qc_ratings.csv`.

---

!!! tip
    When in doubt, flag rather than fail. We will review borderline cases together before exclusion.

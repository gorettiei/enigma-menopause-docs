# Quality Control

In this step, distributions of each measure are assessed for normality and subjects are flagged as outliers based on their global and regional surface measures and sbTIV. Flagged subjects should be inspected visually to ensure acceptable data quality.

---

## Dependencies

Make sure the following are installed before running the QC scripts:

- FSL
- ImageMagick
- R with packages `ggplot2` and `Routliers`

To install the R packages:

```r
cat > /home/maes11/enigma-menopause-docs/docs/qc/visual-inspection.md << 'EOF'
# Quality Control

In this step, distributions of each measure are assessed for normality and subjects are flagged as outliers based on their global and regional surface measures and sbTIV. Flagged subjects should be inspected visually to ensure acceptable data quality.

---

## Dependencies

Make sure the following are installed before running the QC scripts:

- FSL
- ImageMagick
- R with packages `ggplot2` and `Routliers`

To install the R packages:

```r


cat > /home/maes11/enigma-menopause-docs/docs/qc/visual-inspection.md << 'EOF'
# Quality Control

In this step, distributions of each measure are assessed for normality and subjects are flagged as outliers based on their global and regional surface measures and sbTIV. Flagged subjects should be inspected visually to ensure acceptable data quality.

---

## Dependencies

Make sure the following are installed before running the QC scripts:

- FSL
- ImageMagick
- R with packages `ggplot2` and `Routliers`

To install the R packages:

```r
install.packages("ggplot2")
install.packages("https://cran.r-project.org/src/contrib/Archive/Routliers/Routliers_0.0.0.3.tar.gz",
  repos=NULL, type="source")
```

The following scripts must all be present in your `scripts/` folder:

- `run_quality_control.sh`
- `create_samseg_images.sh`
- `create_histograms.R`
- `identify_outliers.R`
- `inspect_subject.sh`

---

## Step 1 — Run QC Script

From the ENIGMA scripts directory, run:

```bash
bash scripts/run_quality_control.sh
```

This generates:

- `output/images/global_histograms/` and `output/images/regional_histograms/` — distribution plots
- `output/images/sbTIV/` — SAMSEG segmentation images
- `docs/Outliers.csv` — list of flagged subjects

---

## Step 2 — Inspect Histograms

For each measure and hemisphere, inspect the histograms in `output/images/global_histograms/` and `output/images/regional_histograms/` and check that distributions are approximately normal. Since some datasets may be small, use your discretion. Note that flagged outliers may appear in the histograms — these will be inspected in detail in Step 4.

---

## Step 3 — Inspect sbTIV Images

Inspect each image in `output/images/sbTIV/`. The left column shows the complete SAMSEG parcellation and the right column shows only the cortex segmentation. Check that the cortex has been adequately captured and that there are no major segmentation errors.

Examples of acceptable segmentations and major errors can be found in `SAMSEG_QC_examples.pdf` in the `examples/` directory.

---

## Step 4 — Inspect Flagged Subjects

For each subject listed in `docs/Outliers.csv`, inspect the data visually in FreeView:

```bash
bash scripts/inspect_subject.sh <subjectID> <QC-type>
```

Where `<QC-type>` is:

- `internal` — checks voxel-wise segmentation (recommended for thickness and volume outliers)
- `external` — checks surface-based parcellation (recommended for area and LGI outliers)

For an overview of error types, see `ENIGMA_Cortical_QC_2.0.pdf` in the `examples/` directory (pages 4 and 6 onwards).

---

## Step 5 — Record QC Decisions

After visual inspection, fill in the `QC_code` column in `docs/Outliers.csv` for each flagged subject:

| QC_code | When to use |
|---------|-------------|
| `OK` | No major error found — subject included despite being flagged |
| `Exclude` | Major segmentation or parcellation error found |

!!! note
    Being flagged as an outlier does **not** automatically mean exclusion. Only exclude subjects with clear segmentation or parcellation errors. When in doubt, contact Goretti or Claudia.

---

## Questions?

Do not hesitate to contact us if you have problems running the QC scripts or are unsure whether a subject should be excluded.

- **Goretti España-Irla** — goretti.espana@charite.de
- **Claudia Barth** — claudia.barth@charite.de

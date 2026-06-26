# Scripts

All scripts required for the ENIGMA Menopause preprocessing and QC pipeline are publicly available and can be downloaded from our GitHub repository:

**[Download Scripts](https://github.com/gorettiei/enigma-menopause-scripts)**

---

## Required Scripts

The following scripts must be present in your `scripts/` folder:

| Script | Description |
|--------|-------------|
| `extract_data.sh` | Extracts all surface measures and sbTIV into CSV files |
| `run_quality_control.sh` | Runs outlier detection and generates QC images |
| `create_samseg_images.sh` | Generates sbTIV segmentation images (called by QC script) |
| `create_histograms.R` | Generates distribution histograms (called by QC script) |
| `identify_outliers.R` | Flags statistical outliers (called by QC script) |
| `inspect_subject.sh` | Opens FreeView for visual inspection of flagged subjects |

---

## Software Requirements

| Software | Required for |
|----------|-------------|
| FreeSurfer v7 or v8 | All processing steps |
| FSL | QC script |
| ImageMagick | QC image generation |
| R (v4.0+) with `ggplot2` and `Routliers` | QC outlier detection |

---

!!! note
    Always use the latest version of the scripts provided by the analysis team. Do not modify the scripts unless instructed.

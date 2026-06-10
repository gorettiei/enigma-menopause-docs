# Preprocessing Overview

This section describes the neuroimaging preprocessing pipeline for the ENIGMA Menopause Transition & Beyond study.

---

## Goal

The goal of preprocessing is to extract **surface-based morphometric measures** from T1-weighted MRI scans using FreeSurfer. These measures include:

- Cortical thickness
- Cortical surface area
- Cortical volume
- Local Gyrification Index (LGI)
- Intracranial Volume (sbTIV)

All cortical measures are parcellated using the **Desikan-Killiany atlas**.

---

## Pipeline Summary

| Step | Description |
|------|-------------|
| **Step 1** | Run FreeSurfer `recon-all` — full cortical reconstruction |
| **Step 2** | Run SAMSEG — whole-brain segmentation and sbTIV extraction |
| **Step 3** | Compute LGI — local gyrification index |
| **Step 4** | Extract FreeSurfer output — generate CSV files for submission |
| **Step 5** | Quality control — outlier detection and visual inspection |
| **Step 6** | Data transfer — compress and send output files |

See [Preprocessing Steps](steps.md) for detailed instructions.

---

## Output Files

After completing the pipeline, the following files will be generated in your `output/` directory:

| File | Description |
|------|-------------|
| `lh.thickness.aparc.csv` | Left hemisphere cortical thickness |
| `rh.thickness.aparc.csv` | Right hemisphere cortical thickness |
| `lh.area.aparc.csv` | Left hemisphere surface area |
| `rh.area.aparc.csv` | Right hemisphere surface area |
| `lh.volume.aparc.csv` | Left hemisphere cortical volume |
| `rh.volume.aparc.csv` | Right hemisphere cortical volume |
| `lh.lgi.aparc.csv` | Left hemisphere LGI |
| `rh.lgi.aparc.csv` | Right hemisphere LGI |
| `sbTIV.csv` | Intracranial volume |

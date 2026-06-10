# Preprocessing Overview

This section describes the neuroimaging preprocessing pipeline for the ENIGMA Menopause Transition & Beyond study.

---

## Goal

The goal of preprocessing is to extract **surface-based morphometric measures** from T1-weighted MRI scans using FreeSurfer. These measures include:

- Cortical thickness
- Cortical surface area
- Cortical volume
- Local Gyrification Index (LGI)
- Subcortical volumes
- Intracranial Volume (ICV / sbTIV)

All measures are extracted using the **Desikan-Killiany atlas**.

---

## Output Files

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

---

## Pipeline Steps

1. **Step 1** — Run FreeSurfer `recon-all`
2. **Step 2** — Extract surface measures
3. **Step 3** — Extract ICV (sbTIV)
4. **Step 4** — Quality Control

See [Preprocessing Steps](steps.md) for detailed instruct
cat > docs/preprocessing/steps.md << 'EOF'
# Preprocessing Steps

Follow these steps carefully. If you already have FreeSurfer outputs (Option 1), skip to [Step 2](#step-2-extract-surface-measures).

---

## Step 1 — Run FreeSurfer recon-all

Run `recon-all` for each participant:

```bash
recon-all -s <subject_id> -i <path_to_T1.nii> -all
```

**For 3T scanners**, add the `-3T` flag:

```bash
recon-all -s <subject_id> -i <path_to_T1.nii> -all -3T
```

!!! note
    Use FreeSurfer v7 or v8. Note which version you used — you will need to report this in your `Covariates.csv`.

!!! warning
    Do not mix FreeSurfer v7 and v8 outputs within the same dataset.

---

## Step 2 — Extract Surface Measures

Once `recon-all` has finished, extract cortical measures using the ENIGMA extraction scripts.

Download the ENIGMA cortical extraction scripts:

```bash
# Contact enigma-menopause@charite.de to receive the scripts
```

Run the extraction:

```bash
bash extract_cortical_measures.sh <SUBJECTS_DIR>
```

This will generate the following files in your `output/` directory:

- `lh.thickness.aparc.csv`
- `rh.thickness.aparc.csv`
- `lh.area.aparc.csv`
- `rh.area.aparc.csv`
- `lh.volume.aparc.csv`
- `rh.volume.aparc.csv`
- `lh.lgi.aparc.csv`
- `rh.lgi.aparc.csv`

---

## Step 3 — Extract ICV (sbTIV)

Run the ICV extraction script:

```bash
bash extract_ICV.sh <SUBJECTS_DIR>
```

This will generate `sbTIV.csv` in your `output/` directory.

---

## Step 4 — Prepare Covariates

Fill in the `Covariates.csv` file with demographic and clinical variables for all participants.

See the [Covariates Example](../examples/covariates.md) for the required format and column names.

**Required columns:**

| Column | Description |
|--------|-------------|
| `SubjID` | Unique participant identifier |
| `Age` | Age in years |
| `Sex` | Biological sex (1=Female, 2=Male) |
| `Site` | Scanner/site identifier |
| `Dx` | Diagnosis (1=Control) |
| `MenoStatus` | Menopausal status |

---

## Step 5 — Quality Control

Before sending your data, perform visual QC on all FreeSurfer outputs.

See [Visual Inspection](../qc/visual-inspection.md) for instructions.

---

## Data Transfer

Once preprocessing and QC are complete, compress your output files:

```bash
tar -czf <site_name>_enigma_menopause.tar.gz docs/ output/
```

Then contact us to arrange secure data transfer:

- **Goretti España-Irla** — goretti.espana@charite.de
- **Claudia Barth** — claudia.barth@charite.de

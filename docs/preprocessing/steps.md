# Preprocessing Steps

Follow these steps in order. If you already have FreeSurfer outputs (Option 1), skip to [Step 3](#step-3-lgi).

---

## Before You Start

Set your FreeSurfer subjects directory:

```bash
export SUBJECTS_DIR=/path/to/your/freesurfer/output
```

---

## Step 1 — recon-all

Run the full FreeSurfer pipeline for each subject:

```bash
recon-all -s <subjectID> -i <path_to_T1.nii.gz> -all -sd $SUBJECTS_DIR
```

!!! note
    - Use FreeSurfer v7 or v8. Do not mix versions within the same dataset.
    - This takes 6–10 hours per subject. Submit as a batch job on a cluster to avoid session timeouts.

---

## Step 2 — SAMSEG

Run SAMSEG for each subject to obtain whole-brain segmentation and intracranial volume (sbTIV):

```bash
run_samseg --input <path_to_T1.nii.gz> --output $SUBJECTS_DIR/<subjectID>/samseg/
```

!!! note "Why SAMSEG?"
    SAMSEG performs whole-brain segmentation directly from the T1w image and is robust to multi-site acquisition variability, making it well-suited for the ENIGMA multi-site framework.

!!! warning
    The output must be saved to `$SUBJECTS_DIR/<subjectID>/samseg/` exactly — this path is required by the extraction script in Step 4.

---

## Step 3 — LGI

!!! note
    This step requires MATLAB with the Image Processing Toolbox. If unavailable, contact Goretti or Claudia.

**3.1** Compute LGI maps for each subject:

```bash
recon-all -s <subjectID> -localGI -sd $SUBJECTS_DIR
```

!!! warning
    Takes ~1 hour per subject. Submit as a batch job on a cluster.

**3.2** Verify both hemispheres completed:

```bash
ls $SUBJECTS_DIR/<subjectID>/surf/ | grep lgi
```

You should see both `lh.pial_lgi` and `rh.pial_lgi`. If only one is present, rerun step 3.1.

**3.3** Extract LGI stats for each hemisphere:

```bash
mris_anatomical_stats -a $SUBJECTS_DIR/<subjectID>/label/lh.aparc.annot \
  -t $SUBJECTS_DIR/<subjectID>/surf/lh.pial_lgi \
  -f $SUBJECTS_DIR/<subjectID>/stats/lh.aparc_lgi.stats <subjectID> lh

mris_anatomical_stats -a $SUBJECTS_DIR/<subjectID>/label/rh.aparc.annot \
  -t $SUBJECTS_DIR/<subjectID>/surf/rh.pial_lgi \
  -f $SUBJECTS_DIR/<subjectID>/stats/rh.aparc_lgi.stats <subjectID> rh
```

---

## Step 4 — Extract FreeSurfer Output

This step extracts all surface measures (thickness, area, volume, LGI) and sbTIV into CSV files for submission.

**4.1** Navigate to the ENIGMA scripts directory:

```bash
cd /path/to/ENIGMA_menopause
export SUBJECTS_DIR=/path/to/your/freesurfer/output
```

**4.2** Create `docs/Subjects.txt` with one subject ID per line:

```bash
echo "<subjectID>" >> docs/Subjects.txt
```

!!! warning
    Subject IDs must match exactly the folder names inside `$SUBJECTS_DIR`.

**4.3** Run the extraction script:

```bash
bash scripts/extract_data.sh
```

Confirm that `SUBJECTS_DIR` and the SAMSEG directory are correct when prompted. Output files will be written to the `output/` directory.

---

## Step 5 — Quality Control

See [Quality Control](../qc/visual-inspection.md) for full instructions.

---

## Step 6 — Data Transfer

Compress your output files and contact us to arrange data transfer:

```bash
tar -czf <site_name>_ENIGMA_menopause_output.tar.gz docs/ output/
```

- **Goretti España-Irla** — goretti.espana@charite.de
- **Claudia Barth** — claudia.barth@charite.de

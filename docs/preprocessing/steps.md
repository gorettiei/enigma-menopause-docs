# Preprocessing Steps

Follow these steps in order. If you already have FreeSurfer outputs (Option 1), skip to [Step 3](#step-3-extract-freesurfer-output).

---

## Before You Start

Set your FreeSurfer subjects directory:

```bash
export SUBJECTS_DIR=/path/to/your/freesurfer/output
```

---

## Step 1 — recon-all

**1.1** Check your FreeSurfer version:

```bash
recon-all --version
```

The output should look like `freesurfer-linux-...-8.1.0-...` where the version number should be either v7 or v8. As an extra precaution, check that all paths point to the correct FreeSurfer installation:

```bash
printenv | grep -i freesurfer
```

If you have multiple FreeSurfer versions installed, switch to the correct one with:

```bash
export FREESURFER_HOME=<path_to_FreeSurfer_v7_or_v8>
source $FREESURFER_HOME/SetUpFreeSurfer.sh
```

**1.2** Set your subjects directory:

```bash
export SUBJECTS_DIR=/path/to/your/freesurfer/output
```

**1.3** Run `recon-all` for each subject:

```bash
recon-all -s <subjectID> -i <path_to_T1.nii.gz> -all -sd $SUBJECTS_DIR
```

!!! note
    - Use FreeSurfer v7 or v8. Do not mix versions within the same dataset.
    - This takes ~10 hours per subject.

**1.4** Given the long runtime, we recommend processing subjects in parallel rather than sequentially. There are two options:

**Sequential** (one subject at a time — simplest but slowest):

```bash
recon-all -s sub-001 -i /path/to/nifti/sub-001_T1w.nii.gz -all -sd $SUBJECTS_DIR
recon-all -s sub-002 -i /path/to/nifti/sub-002_T1w.nii.gz -all -sd $SUBJECTS_DIR
recon-all -s sub-003 -i /path/to/nifti/sub-003_T1w.nii.gz -all -sd $SUBJECTS_DIR
```

**Parallel** (all subjects simultaneously — recommended for large datasets):

Submit a separate job for each subject using your cluster's job scheduler. Below is an example using SLURM job arrays, where `docs/Subjects.txt` contains one subject ID per line:

```bash
#!/bin/bash
#SBATCH --job-name=recon-all
#SBATCH --array=1-N        # replace N with your number of subjects
#SBATCH --time=12:00:00
#SBATCH --mem=8G
#SBATCH --cpus-per-task=4

module load freesurfer

export SUBJECTS_DIR=/path/to/your/freesurfer/output

subjectID=$(sed -n "${SLURM_ARRAY_TASK_ID}p" docs/Subjects.txt)

recon-all -s $subjectID -i /path/to/nifti/${subjectID}_T1w.nii.gz -all -sd $SUBJECTS_DIR
```

!!! note
    The above example uses SLURM. If your cluster uses a different job scheduler (e.g. SGE, PBS), the syntax will differ. Contact your local HPC support team for guidance.

**1.5** After `recon-all` completes, verify it ran correctly for each subject:

```bash
cat $SUBJECTS_DIR/<subjectID>/scripts/recon-all-status.log
```

The last line should read `recon-all finished without error`.

For more information see the [FreeSurfer Beginner's Guide](https://surfer.nmr.mgh.harvard.edu/fswiki/FreeSurferBeginnersGuide).

---

## Step 2 — SAMSEG

Run SAMSEG for each subject to obtain whole-brain segmentation and intracranial volume (sbTIV):

```bash
run_samseg --input <path_to_T1.nii.gz> --output $SUBJECTS_DIR/<subjectID>/samseg/
```

!!! note "Why SAMSEG?"
    SAMSEG performs whole-brain segmentation directly from the T1w image and is robust to multi-site acquisition variability, making it well-suited for the ENIGMA multi-site framework.

!!! warning
    The output must be saved to `$SUBJECTS_DIR/<subjectID>/samseg/` exactly — this path is required by the extraction script in Step 3.

---

## Step 3 — Extract FreeSurfer Output

This step extracts all surface measures (thickness, area, volume) and sbTIV into CSV files for submission.

**3.1** Navigate to the ENIGMA scripts directory:

```bash
cd /path/to/ENIGMA_menopause
export SUBJECTS_DIR=/path/to/your/freesurfer/output
```

**3.2** Create `docs/Subjects.txt` with one subject ID per line:

```bash
echo "<subjectID>" >> docs/Subjects.txt
```

!!! warning
    Subject IDs must match exactly the folder names inside `$SUBJECTS_DIR`.

**3.3** Run the extraction script:

```bash
bash scripts/extract_data.sh
```

Confirm that `SUBJECTS_DIR` and the SAMSEG directory are correct when prompted. Output files will be written to the `output/` directory.

---

## Step 4 — Quality Control

See [Quality Control](../qc/visual-inspection.md) for full instructions.

---

## Step 5 — Data Transfer

Compress your output files and contact us to arrange data transfer:

```bash
tar -czf <site_name>_ENIGMA_menopause_output.tar.gz docs/ output/
```

- **Goretti España-Irla** — goretti.espana@charite.de
- **Claudia Barth** — claudia.barth@charite.de

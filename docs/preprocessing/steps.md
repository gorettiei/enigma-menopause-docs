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

This step reconstructs cortical and subcortical surfaces from each participant's T1-weighted scan using FreeSurfer's `recon-all` pipeline. The output includes cortical thickness, surface area, and volume measures parcellated according to the Desikan-Killiany atlas — the core structural measures used throughout the rest of the analysis.

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
    Use FreeSurfer v7 or v8. Do not mix versions within the same dataset.

**1.4** `recon-all` takes approximately **10 hours per subject**. If you have multiple subjects, we strongly recommend running them in parallel rather than one at a time. Choose one of the two options below:

---

**Option A — Sequential** (simplest, but slow — subjects run one after another):

```bash
recon-all -s sub-001 -i /path/to/nifti/sub-001_T1w.nii.gz -all -sd $SUBJECTS_DIR
recon-all -s sub-002 -i /path/to/nifti/sub-002_T1w.nii.gz -all -sd $SUBJECTS_DIR
recon-all -s sub-003 -i /path/to/nifti/sub-003_T1w.nii.gz -all -sd $SUBJECTS_DIR
```

This is only practical for very small datasets (1–3 subjects).

---

**Option B — Parallel** (recommended — all subjects run simultaneously on the cluster):

Create a file called `docs/Subjects.txt` with one subject ID per line, then submit a job array using your cluster's scheduler. Below is an example using SLURM:

```bash
#!/bin/bash
#SBATCH --job-name=recon-all
#SBATCH --array=1-N        # replace N with your total number of subjects
#SBATCH --time=12:00:00    # 12 hours per subject
#SBATCH --mem=8G
#SBATCH --cpus-per-task=4

module load freesurfer

export SUBJECTS_DIR=/path/to/your/freesurfer/output

subjectID=$(sed -n "${SLURM_ARRAY_TASK_ID}p" docs/Subjects.txt)

recon-all -s $subjectID -i /path/to/nifti/${subjectID}_T1w.nii.gz -all -sd $SUBJECTS_DIR
```

Save this as `run_recon_all.sh` and submit with:

```bash
sbatch run_recon_all.sh
```

!!! note
    The above example uses SLURM. If your cluster uses a different job scheduler (e.g. SGE, PBS), the syntax will differ. Contact your local HPC support team for guidance on submitting parallel jobs on your system.

---

**1.5** After `recon-all` completes, verify it ran correctly for each subject:

```bash
cat $SUBJECTS_DIR/<subjectID>/scripts/recon-all-status.log
```

The last line should read `recon-all finished without error`.

For more information see the [FreeSurfer Beginner's Guide](https://surfer.nmr.mgh.harvard.edu/fswiki/FreeSurferBeginnersGuide).

---

## Step 2 — SAMSEG

This step runs SAMSEG (Sequential Adaptive Multimodal SEGmentation), a FreeSurfer tool that performs whole-brain segmentation directly from the T1-weighted image. It is used here specifically to obtain the SAMSEG-derived estimate of intracranial volume (sbTIV), which is used as a covariate in the mega-analysis to account for individual differences in head size.

```bash
run_samseg --input <path_to_T1.nii.gz> --output $SUBJECTS_DIR/<subjectID>/samseg/
```

!!! note "Why SAMSEG?"
    SAMSEG is robust to multi-site acquisition variability, making it well-suited for the ENIGMA multi-site framework, where scans come from different scanners and protocols.

!!! warning
    The output must be saved to `$SUBJECTS_DIR/<subjectID>/samseg/` exactly — this path is required by the extraction script in Step 3.

---

## Step 3 — Extract FreeSurfer Output

This step compiles all preprocessed neuroimaging measures into CSV files ready for the mega-analysis. Specifically, it extracts regional cortical surface measures (area, thickness, and volume) parcellated according to the Desikan-Killiany atlas, as well as the SAMSEG-derived intracranial volume estimate (sbTIV). The output files from this step are what will be shared with the ENIGMA consortium.

To run the `extract_data.sh` script, your working directory should be the base directory of the ENIGMA Menopause Transition & Beyond Mega-Analysis study scripts package. This step assumes that you have a list in your `docs` folder called `Subjects.txt` containing all your subject IDs, one ID per line — these must match exactly the folder names inside `$SUBJECTS_DIR`.

!!! note
    This step requires the ENIGMA processing scripts. If you haven't downloaded them yet, see the [Scripts](../scripts/index.md) page.

**3.1** Check that the active FreeSurfer version is v7 or v8 and that `SUBJECTS_DIR` is set correctly:

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

**3.3** Run the extraction and QC script:

```bash
bash scripts/extract_data.sh
```

The script will double-check your environment variables and perform some basic checks on the data. It generates the `output/` directory where the surface measures will be written to files named `<hemi>.<measure>.aparc.csv`.

!!! note
    The `--skip` flag is used internally for the `aparcstats2table` command, so subjects with missing data will be skipped automatically. The script will throw an error if abnormally many subjects (>90%) have missing data.

!!! note
    The script will ask about LGI even though this study does not require it. When prompted, answer `y` to continue, then `n` when asked if you still wish to include LGI.

---

## Step 4 — Quality Control

This step checks the quality of your processed data before submission. It flags statistical outliers across all measures and provides diagnostic images so you can visually verify that segmentation and parcellation were performed correctly for each subject.

See [Quality Control](../qc/visual-inspection.md) for full instructions.

---

## Step 5 — Data Transfer

This final step packages all your output files for secure submission to the ENIGMA consortium.

After completing Steps 1 through 4, you should have the following files ready:

- `output/<hemi>.<measure>.aparc.csv` — surface measures (thickness, area, volume)
- `output/sbTIV.csv` — SAMSEG-derived intracranial volume
- `output/images/` — diagnostic plots and sbTIV images
- `docs/Outliers.csv` — list of flagged subjects with QC codes filled in

Compress the `docs/` and `output/` directories:

```bash
tar -czf <site_name>_ENIGMA_menopause_output.tar.gz docs/ output/
```

Replace `<site_name>` with your site or institution name (e.g. `Oslo`, `Amsterdam`).

Then contact us for secure data transfer instructions:

- **Goretti España-Irla** — goretti.espana@charite.de
- **Claudia Barth** — claudia.barth@charite.de

Do not hesitate to contact us if you experience any problems or have questions regarding any steps in this procedure. Thank you for your participation!

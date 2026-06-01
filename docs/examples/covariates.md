# Covariates CSV — Format & Example

The `Covariates.csv` file contains demographic and clinical variables for all participants in your dataset. This file is **required** for all contributing sites regardless of data sharing option.

---

## Required Columns

These columns must be present for all participants:

| Column | Type | Description |
|--------|------|-------------|
| `SubjID` | string | Unique participant identifier |
| `Age` | numeric | Age in years |
| `Sex` | numeric | Biological sex (1=Female, 2=Male) |
| `Site` | string | Scanner/site identifier |
| `Dx` | numeric | Diagnosis (1=Healthy control) |
| `MenoStatus` | string | Menopausal status (see below) |
| `FreeSurfer_version` | string | FreeSurfer version used (e.g. 7.4.1) |

---

## Menopausal Status Values

Use the following values for the `MenoStatus` column:

| Value | Description |
|-------|-------------|
| `pre` | Premenopausal |
| `peri_early` | Early perimenopausal |
| `peri_late` | Late perimenopausal |
| `post_early` | Early postmenopausal |
| `post_late` | Late postmenopausal |
| `unknown` | Status unknown |

---

## Optional Columns

Include as many of the following as available:

| Column | Type | Description |
|--------|------|-------------|
| `MenoType` | string | Type of menopause (natural, surgical, chemical) |
| `AgeMenopause` | numeric | Age at menopause (if postmenopausal) |
| `HRT_ever` | numeric | Ever used HRT (0=No, 1=Yes) |
| `HRT_current` | numeric | Currently using HRT (0=No, 1=Yes) |
| `HC_ever` | numeric | Ever used hormonal contraceptives (0=No, 1=Yes) |
| `BMI` | numeric | Body mass index (kg/m²) |
| `Handedness` | string | Handedness (right, left, ambidextrous) |
| `Education` | numeric | Years of education |
| `MRS_total` | numeric | Menopause Rating Scale total score |
| `Greene_total` | numeric | Greene Climacteric Scale total score |
| `MENQOL_total` | numeric | MENQOL total score |

---

## Example
---

## Notes

!!! warning
    - Do not include any identifying information (names, dates of birth, full dates)
    - Use the exact column names as shown above
    - Missing values should be left empty (not filled with 0 or NA)
    - One row per participant

!!! tip
    Example files can be found in the `examples/` directory of the pipeline package. Use these to validate your formatting before submission.

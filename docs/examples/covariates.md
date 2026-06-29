# Covariates CSV — Format & Example

The `Covariates.csv` file contains demographic and clinical variables for all participants in your dataset. This file is **required** for all contributing sites regardless of data sharing option.

---

## Required Columns

These columns must be present for all participants:

| Column | Type | Description |
|--------|------|-------------|
| `SubjID` | string | Unique participant identifier |
| `Age` | numeric | Age in years |
| `Sex` | numeric | Biological sex (0=Female, 1=Male) |
| `Site` | string | Scanner/site identifier |
| `FreeSurfer_version` | string | FreeSurfer version used (e.g. 7.4.1, 8.1.0) |

---

## Menopausal Status

At least one of the following columns is required to stage menopausal status. Provide as many as available:

| Column | Type | Description |
|--------|------|-------------|
| `MenoStatus` | string | Menopausal status — see values below |
| `HadMenopause` | numeric | Has the participant reached menopause? (0=No, 1=Yes) |
| `AgeMenopause` | numeric | Age at menopause (if `HadMenopause=1`, otherwise NA) |
| `MenstrualStatus` | string | Current menstrual status (`regular`, `irregular`, `absent`) |
| `TimeSinceFinalPeriod_years` | numeric | Years since final menstrual period (if applicable, otherwise NA) |

**`MenoStatus` values:**

If detailed staging is available (preferred):

| Value | Description |
|-------|-------------|
| `pre` | Premenopausal |
| `peri_early` | Early perimenopausal |
| `peri_late` | Late perimenopausal |
| `post_early` | Early postmenopausal (< 5 years since FMP) |
| `post_late` | Late postmenopausal (≥ 5 years since FMP) |
| `unknown` | Status unknown |

If only basic staging is available:

| Value | Description |
|-------|-------------|
| `pre` | Premenopausal |
| `peri` | Perimenopausal |
| `post` | Postmenopausal |
| `unknown` | Status unknown |

---

## Optional Columns

Include as many of the following as available. All optional columns should use `NA` for missing values.

### Anthropometric
- `Height_cm` — height in centimetres
- `Weight_kg` — weight in kilograms
- `BMI` — body mass index (kg/m²); can be left as NA if height and weight are provided

### Sociodemographic
- `Handedness` — `right`, `left`, `ambidextrous`
- `Education_years` — years of education
- `Ethnicity` — ancestral background (`European`, `African`, `East Asian`, `South Asian`, `Latin American`, `Middle Eastern`, `Mixed`, `Other`, `Unknown`)
- `Gender` — self-identified gender (`woman`, `man`, `non-binary`, `other`, `prefer not to say`)

### Hormonal
- `HRT_ever` — ever used hormone replacement therapy (0=No, 1=Yes)
- `HRT_current` — currently using HRT (0=No, 1=Yes)
- `HC_ever` — ever used hormonal contraceptives (0=No, 1=Yes)

### Menopausal Symptoms
- `MRS_total` — Menopause Rating Scale total score
- `Greene_total` — Greene Climacteric Scale total score
- `MENQOL_total` — Menopause Specific Quality of Life total score

### Additional Variables

Sites are encouraged to include any additional variables available. These may include:

- **Cognitive performance** — e.g. MoCA, MMSE, memory, processing speed, executive function
- **Mood and anxiety** — e.g. PHQ-9, BDI, GAD-7, STAI
- **Hormonal blood data** — e.g. estradiol (E2), FSH, LH, progesterone, testosterone
- **Metabolic markers** — e.g. fasting glucose, HbA1c, lipid panel
- **Lifestyle factors** — e.g. smoking, alcohol, physical activity, sleep (PSQI)
- **Reproductive history** — e.g. number of live births, age at first birth
- **Comorbidities** — e.g. diabetes, hypertension, cardiovascular disease
- **Medications** — e.g. antidepressants, antihypertensives

For additional variables, use self-explanatory column names and include units where relevant. Contact us if you are unsure how to format any variables.

---

## Notes

!!! warning
    - Do not include any identifying information (names, dates of birth, full dates)
    - Use the exact column names as shown above for required and recommended variables
    - Missing values should be coded as `NA`
    - One row per participant

!!! tip
    Download the template and example files to get started:

    - [Covariates_template.csv](../assets/Covariates_template.csv) — blank template with column headers only
    - [Covariates_example.csv](../assets/Covariates_example.csv) — filled example with 15 fictional participants

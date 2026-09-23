# Codebook — `memclinic.csv`

A fictional memory-clinic cohort: 180 people referred for cognitive assessment,
60 in each diagnostic group. Cross-sectional — one measurement occasion per
person. Used in sessions 03 to 06.

These data are **simulated**, so the true values behind them are known and are
listed at the bottom. Comparing your estimates against them is the point.

| Variable | Type | Units / values | Meaning |
|---|---|---|---|
| `id` | character | `P001`–`P180` | Participant identifier |
| `group` | character | `HC`, `MCI`, `AD` | Clinical diagnosis: healthy control, mild cognitive impairment, early Alzheimer's disease |
| `age` | integer | years, 50–91 | Age at assessment |
| `sex` | character | `F`, `M` | Sex as recorded in the clinical file |
| `educ_years` | integer | years, 5–20 | Years of formal education |
| `moca` | integer | 0–30 | Montreal Cognitive Assessment total. **Higher is better.** Below ~26 is the usual screening cut-off |
| `hippo_vol_mm3` | integer | mm³, ~2600–4600 | Total hippocampal volume from structural MRI. **Higher is better** (less atrophy) |
| `stroop_cost_ms` | integer | ms | Stroop interference cost: mean RT on incongruent trials minus mean RT on congruent trials. **Higher is worse** (more interference, poorer inhibitory control) |
| `rt_mean` | integer | ms | Mean simple reaction time. **Higher is worse** (slower) |
| `csf_ptau` | numeric | pg/mL | Phosphorylated tau in cerebrospinal fluid. **Higher is worse** (more tau pathology). Right-skewed, as in real assays |

## Things worth knowing before you analyse these

- **`group` reads in as character, not a factor.** `read.csv()` has not converted
  strings to factors since R 4.0. Convert it yourself when you need it as a
  factor — session 04 does this.
- **`stroop_cost_ms` can be negative.** A cost near or below zero means that
  person showed no interference on the task. This is a real pattern, not an
  error, and the values have deliberately not been truncated at zero: chopping
  off part of a distribution biases a regression fitted to it.
- **The file is not sorted by group.** Rows are in random order, as a real export
  would be.
- **Do not open this file in Excel.** On an Italian locale Excel will rewrite the
  decimal separators and silently corrupt `csf_ptau`.

## True values used to generate the data

Set in `../data-raw/make-memclinic.R`, with `set.seed(2026)`.

Hippocampal volume:

- 3800 mm³ for a 70-year-old control
- −12 mm³ per year of age
- group offsets: HC 0, MCI −350, AD −700
- residual SD 230

**Stroop cost — this is the relationship session 03 fits:**

- intercept 327 ms
- **slope −0.06 ms per mm³** of hippocampal volume, i.e. −6 ms per 100 mm³
- residual SD 25

MoCA: 30 minus group effect (HC 1.2, MCI 4.8, AD 10.5), residual SD 1.8,
clipped to 0–30.

Simple RT: 380 ms at age 70, +3.2 ms per year, group offsets HC 0, MCI +25,
AD +70, residual SD 45.

CSF p-tau: lognormal, log-means HC 2.9 / MCI 3.3 / AD 3.7, log-SD 0.30.

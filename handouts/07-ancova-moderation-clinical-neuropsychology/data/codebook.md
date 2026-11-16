# Codebook — session 07 datasets

All four files are simulated.

## `sdmt-age.csv`

A normative sample of 120 people aged 10 to 85.

| Variable | Type | Units | Meaning |
|---|---|---|---|
| `id` | integer | — | Person identifier |
| `age` | integer | years | Age |
| `sdmt` | integer | correct in 90 s | Symbol Digit Modalities Test, oral version. Higher means faster processing |

## `tbi-memory.csv`

40 patients with traumatic brain injury (TBI) and 40 healthy controls.

| Variable | Type | Units | Meaning |
|---|---|---|---|
| `id` | integer | — | Person identifier |
| `group` | character | — | `Control` or `TBI` |
| `premorbid_iq` | integer | IQ points | Premorbid IQ estimated from a word-reading test (e.g. TOPF) |
| `recall` | numeric | words (0–15) | RAVLT delayed recall. Higher is better |

## `reserve-moca.csv`

100 older adults with small-vessel disease, half with low and half with high
education (a common proxy for cognitive reserve).

| Variable | Type | Units | Meaning |
|---|---|---|---|
| `id` | integer | — | Person identifier |
| `education` | character | — | `Low` (≤ 8 years) or `High` (≥ 13 years) |
| `lesion_ml` | numeric | mL | White-matter hyperintensity volume on MRI |
| `moca` | integer | points (0–30) | Montreal Cognitive Assessment. Higher is better |

## `stroke-training.csv`

50 stroke patients, randomised to computerised working-memory training or usual
care, tested at three visits. This is **long** format: one row per patient per
visit, 150 in total.

| Variable | Type | Units | Meaning |
|---|---|---|---|
| `patient` | integer | — | Patient identifier |
| `group` | character | — | `UsualCare` or `Training` |
| `visit` | character | — | `Baseline`, `Post` (after training) or `FollowUp` |
| `digit_span` | integer | points | Digit span total score. Higher is better |

## Things worth knowing

- **Text columns sort alphabetically when they become factors.** Set the levels
  yourself, or `High` becomes the reference group and the visits are out of order.
- **Do not open these files in Excel.**

## True values used to generate the data

Set in `../data-raw/make-reg3.R`, with `set.seed(42)`.

- `sdmt-age.csv`: `sdmt = 30 + 1.4·age − 0.018·age²`, error SD 6 (peak near age 39).
- `tbi-memory.csv`: `recall = −1 + 0.11·premorbid_iq − 2.5·TBI`, error SD 1.8.
- `reserve-moca.csv`: `moca = 28.5 + 0.5·High − 0.55·lesion + 0.35·lesion·High`,
  error SD 1.6, capped at 30.
- `stroke-training.csv`: true means of 20 / 21 / 21 (`UsualCare`) and 20 / 25 / 24
  (`Training`) at Baseline / Post / FollowUp, between-patient SD 3, residual SD 2.

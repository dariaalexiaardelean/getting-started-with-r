# Codebook — `tmt-groups.csv` and `tmt-rehab.csv`

Simulated scores on the Trail Making Test part B (TMT-B). The patient connects
numbers and letters in alternating order as quickly as possible. The score is
the completion time, so **a higher score means slower, worse performance**.

## `tmt-groups.csv`

One row per person: 40 in each diagnostic group, 120 in total.

| Variable | Type | Units | Meaning |
|---|---|---|---|
| `id` | integer | — | Person identifier |
| `diagnosis` | character | — | `Control`, `MCI` (Mild Cognitive Impairment) or `AD` (Alzheimer's disease) |
| `tmt_b` | integer | seconds | TMT-B completion time |

## `tmt-rehab.csv`

40 stroke patients, each tested at three visits. This is **long** format: one
row per patient per visit, 120 in total.

| Variable | Type | Units | Meaning |
|---|---|---|---|
| `patient` | integer | — | Patient identifier |
| `visit` | character | — | `Baseline`, `Post` (after rehabilitation) or `FollowUp` |
| `tmt_b` | integer | seconds | TMT-B completion time |

## Things worth knowing

- **Text columns sort alphabetically when they become factors.** Set the levels
  yourself, or `AD` becomes the reference group and the visits are out of order.
- **Do not open these files in Excel.**

## True values used to generate the data

Set in `../data-raw/make-tmt.R`, with `set.seed(42)`.

- `tmt-groups.csv`: true means of 100 (`Control`), 130 (`MCI`) and 160 (`AD`)
  seconds, with an error SD of 25.
- `tmt-rehab.csv`: true means of 180 (`Baseline`), 155 (`Post`) and 130
  (`FollowUp`) seconds, with a between-patient SD of 30 and a residual SD of 15.

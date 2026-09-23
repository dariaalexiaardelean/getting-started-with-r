# Codebook — `stroop-rm.csv`

Thirty-six people from the memory-clinic cohort — 12 from each diagnostic group —
who completed the full Stroop task. One row per person, with their mean reaction
time in each of the three trial conditions.

This is **wide** format: one row per person, several columns per person. Session
02 converts it to long format, which is the shape session 04 needs.

| Variable | Type | Units | Meaning |
|---|---|---|---|
| `id` | character | — | Participant identifier. **Joins to `memclinic.csv`** on the same column |
| `rt_congruent` | integer | ms | Mean RT on congruent trials (the word and the ink colour agree) |
| `rt_neutral` | integer | ms | Mean RT on neutral trials |
| `rt_incongruent` | integer | ms | Mean RT on incongruent trials (the word and the ink colour conflict) |

## Things worth knowing

- **There is no `group` column, on purpose.** To compare diagnostic groups you
  have to `merge()` this file with `memclinic.csv` by `id`. That is the exercise.
- **The three conditions are ordered** in difficulty: congruent is fastest,
  incongruent slowest, neutral in between.
- `rt_incongruent - rt_congruent` is the Stroop interference cost, so it should
  reproduce `stroop_cost_ms` in `memclinic.csv` for the same person — up to the
  measurement noise added to each condition separately. The two files describe
  the same people.
- Rows are sorted by `id`, unlike `memclinic.csv`, which is deliberately shuffled.
- **Do not open this file in Excel.**

## True values used to generate the data

Set in `../data-raw/make-stroop-rm.R`, with `set.seed(2027)`.

- `rt_congruent` = that person's `rt_mean` from `memclinic.csv`, plus noise (SD 12 ms).
- `rt_neutral` = `rt_congruent` + 0.25 × their `stroop_cost_ms`, plus noise (SD 10 ms).
- `rt_incongruent` = `rt_congruent` + their full `stroop_cost_ms`, plus noise (SD 10 ms).

Because the noise is added to each condition independently, the recovered cost
and the stated cost correlate about .97 rather than matching exactly — which is
what measurement noise does in a real study.

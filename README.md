LinkedIn: https://www.linkedin.com/in/jacob-reid-541935334/
Email: jacobreid108@gmail.com

# Wearable Device Health Data Analysis

An end-to-end data cleaning and exploratory analysis of a simulated multi-user
wearable dataset, demonstrating a full data science workflow from raw CSV to
statistical insights.

**👉 [View the full analysis notebook →](analysis.ipynb)**

---

## Overview

Wearable devices generate complex messy health data, but raw
sensor output is rarely analysis-ready. This project takes a realistic wearable
dataset (5 users × 60 days of activity, sleep, heart rate, and SpO2 data), cleans
it through a documented pipeline, engineers useful features, and extracts patterns
about sleep, activity, and health.

The goal is to demonstrate the full analyst workflow: 
- Data quality assessment,
- Cleaning, 
- Feature engineering, 
- Statistical analysis, 
- Interpretation 

All applied to a realistic health tech use case.

---

## Key Findings

- **Strong mechanical correlations:** Steps, calories burned, and active minutes
  are near-perfectly correlated (Spearman r > 0.96 for all pairs). This confirms
  the three metrics capture the same underlying activity dimension; a single
  activity measure would suffice for most analyses.

- **No significant weekend effect:** Weekend steps averaged ~3.3% higher than
  weekday steps, but the difference was not statistically significant
  (Welch's t-test, p = 0.314). We found no evidence that activity patterns shift
  meaningfully across the week.

- **Sleep quality was not associated with activity:** Average steps did not
  differ significantly across sleep quality categories (one-way ANOVA, p = 0.719).
  Mean sleep values were nearly identical across groups (Poor: 6.09h, Fair: 6.40h,
  Good: 6.27h, Excellent: 6.32h), suggesting sleep *quality* as recorded here
  does not predict next-day activity.

- **Sleep duration and activity were essentially uncorrelated** (r ≈ 0.0),
  reinforcing the absence of a sleep-activity relationship in this dataset.

**Overall conclusion:** only mechanical correlations held up under statistical
testing. The behavioural patterns expected in real-world wearable data (weekend
effects, sleep-activity links) were not detected here — a finding that could
reflect limitations of the simulated dataset, or a genuine absence of these
effects in this population. Distinguishing these explanations would require real
wearable data with a larger and more diverse sample.

---

## Dataset

- **Size:** 304 records (5 users × ~60 days each, after cleaning)
- **Source:** Simulated — designed to mimic real wearable output (see
  `generate_data.ipynb`)
- **Columns:**
  - `user_id` — anonymous subject identifier
  - `timestamp` — measurement time (mixed formats in raw data; parsed to datetime)
  - `steps`, `active_minutes`, `calories_burned` — activity metrics
  - `heart_rate_bpm`, `spo2_percent` — vitals
  - `sleep_hours`, `sleep_quality` — sleep metrics
  - `exercise_type` — activity category (Running / Cycling / Strength / Yoga / Walking)

The dataset intentionally contains realistic issues: missing values, impossible
readings (HR = 0, SpO2 = 150%), duplicate rows, mixed-casing categories, and
inconsistent date formats. All are addressed in the cleaning pipeline — see the
notebook's cleaning section for documented reasoning on each decision.

---

## Approach

The analysis is structured in five phases:

1. **Data inspection** — shape, dtypes, missingness, duplicates, `describe()`
2. **Cleaning** — duplicates removed; timestamps parsed from 3 mixed formats
   (ISO, UK slash-format, textual); physiologically impossible values nulled;
   categorical labels standardised to canonical form
3. **Feature engineering** — `hour`, `day_of_week`, `is_weekend`, `did_exercise`,
   `step_intensity`, `activity_level`, `sleep_category`
4. **Exploratory analysis** — distributions, per-user comparisons, temporal
   patterns, correlation matrix
5. **Statistical analysis** — Welch's t-tests (weekend vs weekday), one-way
   ANOVA (sleep quality → activity), Spearman correlations

Full reasoning and decisions are documented in markdown cells throughout the
notebook — including a bug I found and fixed in the datetime parsing (mixed-format
ambiguity that silently flipped day/month on 20 rows).

---

## How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/JacobReid108/Project.git
   cd Project
# heart-disease — Risk Factors & Exercise Signals (UCI Cleveland, N=303)

This project investigates which patient factors are most associated with a heart disease diagnosis (**presence** vs **absence**) in a clinical evaluation dataset (Cleveland Clinic Foundation; UCI Heart Disease — Cleveland data).

Repo includes:
- Clean, commented analysis notebook
- Presentation slides
- Poster for LinkedIn-style sharing

---

## Dataset
- **Source:** UCI Machine Learning Repository (Heart Disease – Cleveland)
- **Rows:** 303 patients
- **Target:** `heart_disease` (presence / absence)
- Key variables used:
  - `cp` (chest pain type)
  - `exang` (exercise-induced angina; 0/1)
  - `thalach` (max heart rate during exercise test)
  - `age`, `trestbps` (resting blood pressure)
  - `chol` (cholesterol), `fbs` (fasting blood sugar >120; 0/1)

**Data location in this repo:** `data/heart_disease.csv`  
> Note: This is a **clinical evaluation sample**, not a random population sample.

---

## Methods (hypothesis testing)
- **Numeric vs diagnosis (2 groups):** Welch two-sample t-test  
  (e.g., `thalach`, `age`, `trestbps`, `chol`)
- **Categorical vs diagnosis:** Chi-square test of independence  
  (e.g., `cp`, `exang`, `sex`)
- **Thalach across multiple cp groups:** One-way ANOVA + Tukey HSD (FWER = 0.05)
- **Benchmark comparison (FBS rate vs 8%):** Binomial test (one-sided)

Significance threshold: **α = 0.05**.

---

## Key findings
### Strongest signals (symptoms + exercise test)
- **Chest pain type (`cp`) is strongly associated with diagnosis** (χ² p = 1.25e−17).  
  Asymptomatic patients have the highest heart disease proportion.
- **Exercise-induced angina (`exang`) is a strong indicator:**  
  heart disease rate **76.8%** (exang=1) vs **30.9%** (exang=0), χ² p = 1.41e−13.
- **Max exercise heart rate (`thalach`) is much lower in heart disease:**  
  ~**19 bpm** lower on average, p = 3.46e−14.
- **Across chest pain types, mean thalach differs** (ANOVA p = 1.91e−10).  
  Tukey HSD shows the **asymptomatic** group has significantly lower thalach than each other cp type.

### Supporting predictors (demographics/vitals)
- **Age:** heart disease group is older (mean +4.0 years; p ≈ 7.06e−05).
- **Resting BP (`trestbps`):** slightly higher in heart disease (mean +5.3 mmHg; p = 0.00855).
- **Sex:** diagnosis is more frequent among males in this sample  
  (55.3% vs 25.8%; χ² p = 2.67e−06).

### Benchmarks
- **Cholesterol threshold (240 mg/dl):** heart disease group mean **251.47** (>240; p(one-sided)=0.0035).  
  However, cholesterol is **not a strong between-group discriminator** here (two-sample p ≈ 0.137).
- **High fasting blood sugar (`fbs` >120):** **14.85%** observed vs **8%** benchmark  
  (45 observed vs ~24 expected; p = 4.69e−05), consistent with a clinical (non-population) sample.

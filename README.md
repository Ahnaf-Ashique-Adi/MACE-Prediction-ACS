# Machine Learning Prediction of Major Adverse Cardiac Events (MACE) in Acute Coronary Syndrome

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23884663.svg)](https://doi.org/10.5281/zenodo.23884663)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)


---

## Repository Contents

| File | Description |
| :--- | :--- |
| [`MACE_PREDICTION_MODEL.ipynb`](./MACE_PREDICTION_MODEL.ipynb) | Complete Jupyter/Colab notebook executing exploratory data analysis, leak-free stratified 5-fold cross-validation, collinearity pruning via centered VIF, Spearman correlation screening, SMOTE-Tomek rebalancing, and inner 3-fold CV decision threshold optimization for Logistic Regression, Random Forest, XGBoost, and SVM. |
| [`hypothesis_imputation_formula.pdf`](./hypothesis_imputation_formula.pdf) | Mathematical derivation and cohort calibration document for the closed-form hemodynamic LVEF estimator. |


---

## Hemodynamic LVEF Imputation Formulation

**Methodology & Derivation by:** Ahnaf Ashique Adi & Taufiq Mustafizur Rahman

To prevent cross-patient data leakage during cross-validation, missing Left Ventricular Ejection Fraction (LVEF) is estimated using a closed-form physiological formula rather than multi-patient statistical imputation (e.g., MICE or KNN).

### Theoretical Basis
From basic fluid dynamics, pressure approximates forward flow multiplied by peripheral vascular resistance ($R$):

$$\text{CO} = \text{HR} \times \text{SV} = \text{HR} \times \text{EDV} \times \text{LVEF}$$

$$\text{SBP} \approx \text{CO} \times R = (\text{HR} \times \text{EDV} \times \text{LVEF}) \times R$$

Rearranging for myocardial contractility establishes that LVEF varies directly with systolic blood pressure and inversely with heart rate:

$$\text{LVEF} \propto \frac{\text{SBP}}{\text{HR}} \implies \text{LVEF} \propto \frac{1}{\text{Shock Index}}$$

### Calibration & Applied Estimator
Accounting for pulmonary congestion escalation via Killip classification ($K \in \{1, 2, 3, 4\}$) and transmural myocardial necrosis penalties ($\mathbb{I}_{\text{STEMI}} \in \{0, 1\}$) calibrated on complete cohort records ($n = 1,054$):

$$\hat{\text{LVEF}} = 52.0 - 3.5(K - 1) - 4.0\left(\frac{\text{HR}}{\text{SBP}}\right) - 5.1\,\mathbb{I}_{\text{STEMI}}$$

To prevent extreme artifacts from acute hemodynamic instability, estimates are clamped to physiological limits:

$$\hat{\text{LVEF}}_{\text{final}} = \min(70.0, \max(15.0, \hat{\text{LVEF}}))$$

*For the complete first-principles proof and step-by-step derivation, refer to [`hypothesis_imputation_formula.pdf`](./hypothesis_imputation_formula.pdf).*

---

## Authors & Contributions

* **Khondoker Ashab Zaman** — *Conceptualization, Methodology, Data curation, Formal analysis, Visualization, Investigation, Writing – original draft, Project administration*
* **Taufiq Mustafizur Rahman** — *Software, Methodology, Formal analysis, Writing – review & editing*
* **Ahnaf Ashique Adi** — *Software, Imputation Methodology (Hemodynamic Formulation), Investigation, Writing – review & editing*
* **Khondoker Qumruzzaman** — *Supervision, Validation, Resources, Writing – review & editing*
* **Sayeed Ahmed** — *Investigation, Validation, Writing – review & editing*
* **Mohammod Zafor Iqbal Jamali** — *Investigation, Validation, Writing – review & editing*
* **Sohel Mahmud** — *Supervision, Validation, Writing – review & editing*
* **Harun Or Rashid Mojumder** — *Investigation, Validation, Writing – review & editing*
* **Umme Kulsum** — *Conceptualization, Supervision, Validation, Writing – review & editing*
* **Ahmed Saiful Bari** — *Supervision, Validation, Writing – review & editing*
* **Md. Abdus Salam** — *Supervision, Validation, Writing – review & editing*
* **Kibriya Shamim** — *Supervision, Validation, Writing – review & editing*

---

## Citation

If you use this codebase, pipeline, or the hemodynamic LVEF formula in your research, please cite the permanent archive:

```bibtex
@software{zaman_mace_2026,
  author       = {Zaman, Khondoker Ashab and
                  Rahman, Taufiq Mustafizur and
                  Adi, Ahnaf Ashique and
                  Qumruzzaman, Khondoker and
                  Ahmed, Sayeed and
                  Jamali, Mohammod Zafor Iqbal and
                  Mahmud, Sohel and
                  Mojumder, Harun Or Rashid and
                  Kulsum, Umme and
                  Bari, Ahmed Saiful and
                  Salam, Md. Abdus and
                  Shamim, Kibriya},
  title        = {{MACE-Prediction-ACS: Leak-Free MACE Prediction Pipeline \& Hemodynamic LVEF Derivation}},
  month        = nov,
  year         = 2026,
  publisher    = {Zenodo},
  version      = {v1.0.0},
  doi          = {10.5281/zenodo.23884663},
  url          = {[https://doi.org/10.5281/zenodo.23884663](https://doi.org/10.5281/zenodo.23884663)}
}

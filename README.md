# Machine Learning Prediction of Major Adverse Cardiac Events (MACE) in Acute Coronary Syndrome

Codebase, leak-free validation pipeline, and analytical derivation for the study:  
**"Machine learning prediction of major adverse cardiac events in acute coronary syndrome using routine admission data: a comparison of four models"**

---

## Repository Contents

* [`MACE_PREDICTION_MODEL.ipynb`](./MACE_PREDICTION_MODEL.ipynb): Full reproducible pipeline implementing stratified 5-fold cross-validation with training-contained imputation, VIF collinearity pruning, feature screening, SMOTE-Tomek balancing, and inner 3-fold CV decision threshold optimization across Logistic Regression, Random Forest, XGBoost, and SVM.
* [`hypothesis_imputation_formula.pdf`](./hypothesis_imputation_formula.pdf): Analytical derivation and cohort parameter calibration of the closed-form hemodynamic LVEF estimator.

---

##  Hemodynamic LVEF Imputation Formulation

**Formulated by:** Ahnaf Ashique Adi & Taufiq Mustafizur Rahman

To prevent cross-patient data leakage during validation, missing Left Ventricular Ejection Fraction (LVEF) was estimated using a physiology-grounded closed-form formula rather than data-wide statistical filling:

$$\text{Cardiac Output (CO)} = \text{HR} \times \text{SV} = \text{HR} \times \text{EDV} \times \text{LVEF}$$

$$\text{SBP} \approx \text{CO} \times R \implies \text{LVEF} \propto \frac{\text{SBP}}{\text{HR}} \implies \text{LVEF} \propto \frac{1}{\text{Shock Index}}$$

Accounting for pulmonary fluid escalation (Killip class) and transmural myocardial necrosis penalties (STEMI presentation) empirically calibrated on complete cohort records ($n = 1,054$):

$$\hat{\text{LVEF}} = 52.0 - 3.5(K - 1) - 4.0\left(\frac{\text{HR}}{\text{SBP}}\right) - 5.1\,\mathbb{I}_{\text{STEMI}}$$

$$\hat{\text{LVEF}}_{\text{final}} = \min(70, \max(15, \hat{\text{LVEF}}))$$

*The formal proof and fluid dynamic relationships are detailed in [`hypothesis_imputation_formula.pdf`](./hypothesis_imputation_formula.pdf).*

---

## Authors & Roles

* **Khondoker Ashab Zaman** — *Conceptualization, Methodology, Data curation, Formal analysis, Visualization, Investigation, Writing – original draft, Project administration*
* **Taufiq Mustafizur Rahman** — *Software, Methodology, Formal analysis, Writing – review & editing*
* **Ahnaf Ashique Adi** — *Software, Imputation Methodology , Investigation, Writing – review & editing*
* **Khondoker Qumruzzaman** — *Supervision, Validation, Writing – review & editing*
* **Umme Kulsum** — *Conceptualization, Supervision, Validation, Writing – review & editing*

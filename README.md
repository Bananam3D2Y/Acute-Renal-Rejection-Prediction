# Acute Renal Rejection Prediction

> An interpretable machine-learning pipeline that predicts acute kidney transplant rejection from OPTN STAR registry data, trained on about 4.86 million transplant follow-up records, with SHAP used to explain what drives each prediction.

![Python](https://img.shields.io/badge/Python-3.12-blue) ![scikit-learn](https://img.shields.io/badge/scikit--learn-models-orange) ![SHAP](https://img.shields.io/badge/SHAP-explainability-green)

A walkthrough of the project is on YouTube: https://youtu.be/A-tvZGjENvw

## Overview

Acute rejection is one of the main causes of early kidney graft loss. This project asks how well rejection can be predicted from information recorded around the time of transplant, and which donor and recipient factors carry the signal.

- **Data:** OPTN STAR kidney files: follow-up, transplant, deceased-donor and living-donor tables, merged on transplant and donor IDs.
- **Size:** about 4.86 million follow-up records (56% no rejection, 44% rejection), split 80/20 into train and test.
- **Target:** `Rejection = 1` if the record reports at least one acute rejection episode.

## Results

Held-out test set of 971,196 records, from the notebook in this repository:

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
| --- | --- | --- | --- | --- | --- |
| Logistic Regression | 0.654 | 0.579 | 0.747 | 0.652 | 0.714 |
| Decision Tree | 0.709 | 0.666 | 0.664 | 0.665 | 0.789 |
| **Random Forest** (GridSearchCV) | **0.710** | **0.669** | **0.656** | **0.663** | **0.788** |

The written report ([`ARRP_Report.pdf`](ARRP_Report.pdf)) also discusses an XGBoost model (100 trees) that reached 0.743 ROC-AUC. That run is described in the report; its code is not part of this notebook.

**What drives the predictions.** In both tree models the strongest predictors were panel reactive antibody (PRA), cold ischemia time, recipient age, donor age, and donor history of hypertension and diabetes, followed by HLA mismatch count and donor type. These are the factors transplant medicine would expect to matter: recipient sensitisation, organ preservation time, age, and donor health history.

## Approach

1. **Load and merge** the four SAS tables into one modelling table.
2. **Engineer features** chosen to be clinically interpretable: PRA (calculated PRA, back-filled with peak PRA), HLA mismatch count, previous-transplant flag, donor type (living, DCD), donor diabetes and hypertension history, donor and recipient age and sex, cold ischemia hours, and one-hot recipient ethnicity.
3. **Clean** by imputing missing numeric values and encoding categoricals.
4. **Train** Logistic Regression, a Decision Tree, and a Random Forest tuned with 3-fold `GridSearchCV` on ROC-AUC, all with balanced class weights.
5. **Evaluate** with accuracy, precision, recall, F1, ROC-AUC, confusion matrices and ROC curves.
6. **Explain** with `shap.TreeExplainer` and a SHAP summary plot, alongside model coefficients and feature importances.

## Limitations

- **Records are follow-ups, not patients.** A transplant can have several follow-up records, and the notebook does not reduce them to one per transplant. With a random split, records from the same transplant can land in both train and test, so the tree-model scores are likely optimistic. The fix is to keep one follow-up per transplant (for example the one-year record) or to split by transplant ID.
- **Moderate discrimination.** Even taken at face value, the best model misses roughly a third of rejections at the default threshold. This is an exploratory analysis, not a clinical tool.
- **Registry data only.** No immunosuppression regimen, lab trends or biopsy data are used.

## Repository contents

```
.
├── SAT(Final_Project).ipynb       # the full pipeline, with outputs
├── ARRP_Report.pdf                # written report: method, results, discussion
├── SAT5114 Final Project.pptx     # presentation slides
├── Youtube Presentation           # link to the recorded walkthrough
├── requirements.txt
└── README.md
```

## Running it

The data are not included. OPTN STAR files are released under a data use agreement and cannot be redistributed.

1. Request the Kidney STAR files from the [OPTN data request portal](https://optn.transplant.hrsa.gov/data/request-data/).
2. Place these four files next to the notebook:
   - `KIDNEY_FOLLOWUP_DATA.sas7bdat`
   - `KIDPAN_DATA.sas7bdat`
   - `DECEASED_DONOR_DATA.sas7bdat`
   - `LIVING_DONOR_DATA.sas7bdat`
3. Install the dependencies and open the notebook:

```bash
git clone https://github.com/Bananam3D2Y/Acute-Renal-Rejection-Prediction.git
cd Acute-Renal-Rejection-Prediction
python3 -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook "SAT(Final_Project).ipynb"
```

The full dataset needs a machine with plenty of memory; the saved notebook outputs show every result without re-running.

## Context

Final project for SAT 5114 at Michigan Technological University, Spring 2025.

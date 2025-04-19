# Acute-Renal-Rejection-Prediction
Interpretable ML pipeline to predict 1‑year acute kidney transplant rejection using OPTN STAR data. Includes data loading, preprocessing, and feature engineering (PRA, HLA mismatches, donor type); trains Logistic Regression, Decision Tree, and tuned Random Forest; evaluates via ROC and confusion matrix; uses SHAP for explainability.
Acute Renal Transplant Rejection Prediction

Project Overview

This project implements an interpretable machine learning pipeline to predict one-year acute renal transplant rejection using the OPTN STAR registry data. It demonstrates:

Data Loading & Merging: Combining follow‑up, transplant, and donor datasets.

Feature Engineering: Creating clinically relevant features (PRA levels, HLA mismatches, donor type, etc.).

Data Cleaning & Preprocessing: Imputing missing values and encoding categorical variables.

Model Training: Building and evaluating three models—Logistic Regression, Decision Tree, and Random Forest (with GridSearchCV tuning).

Evaluation & Visualization: Computing accuracy, precision, recall, F1-score, ROC-AUC; plotting confusion matrices and ROC curves.

Explainability: Using SHAP values to explain individual predictions and global feature importance.

Dataset Access

The data used in this project are proprietary and maintained by the Organ Procurement and Transplantation Network (OPTN). To access the required SAS STAR files, please:

Visit the OPTN data request portal: https://optn.transplant.hrsa.gov/data/request-data/

Submit a data request for the Kidney Transplant STAR files.

Once approved, download the following files and place them in a data/ directory:

KIDNEY_FOLLOWUP_DATA.sas7bdat

KIDPAN_DATA.sas7bdat

DECEASED_DONOR_DATA.sas7bdat

LIVING_DONOR_DATA.sas7bdat

Note: Without proper authorization, the data cannot be shared publicly. Researchers should follow OPTN’s data use agreement and privacy policies.

Setup & Installation

Clone this repository:

git clone https://github.com/YourUsername/renal-rejection-prediction.git
cd renal-rejection-prediction

Create a Python virtual environment and install dependencies:

python3 -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt

Ensure the data/ directory contains the required SAS files (see Dataset Access above).

Usage

Run the pipeline script:

python final_pipeline.py

This script will:

Load and merge data

Engineer features and preprocess

Train and evaluate models

Generate evaluation plots (histograms, confusion matrices, ROC curves)

Produce SHAP explainability visualizations

Inspect Results:

Model performance metrics will be printed to the console.

Plots will be saved in the output/ directory.

Code Structure

final_pipeline.py — Main Python script implementing the full pipeline.

requirements.txt — List of Python packages needed (pandas, scikit-learn, xgboost, shap, matplotlib, seaborn).

output/ — Directory to store generated plots and logs.

README.md — This file.


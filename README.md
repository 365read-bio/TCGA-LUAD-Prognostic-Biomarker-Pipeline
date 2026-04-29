# TCGA-LUAD-Prognostic-Biomarker-Pipeline
A data wrangling and downstream analysis pipeline for TCGA-LUAD. The project aims to build a prognostic model for patient survival. Workflow: DGE analysis(tumor vs normal), Univariate Cox, Elastic Net regression, model training(Multivariate Cox Regression / Random Forest Survival), evaluation(C-index / Time-dependent ROC-AUC).

## 🧬 Workflow Steps
Step 1: Data Acquisition & Setup
data: Fetch TCGA-LUAD clinical profiles and pre-normalized mRNA expression data (RSEM z-scores) via cBioPortal.

Step 2: Data Wrangling & Train/Test Splitting
prep: Align clinical patient IDs with mRNA data. Handling missing values (NAs).
split: Split the dataset into Training Set (70%) and Test Set (30%).

Step 3: 1st Filter - DGE Analysis (Tumor vs Normal)
filter-1: Perform Differential Gene Expression (DGE) analysis between tumor and normal tissues strictly within the Training Set to identify biologically relevant candidates.

Step 4: 2nd Filter - Survival Analysis (Univariate Cox Regression)
filter-2: Apply Univariate Cox Proportional Hazards regression to the DGE-filtered genes to filter those signiticantly associated with OS(overall survival).

Step 5: Final Feature Selection (Machine Learning)
ml-prep: Apply Elastic Net Regression to the survival-associated genes to eliminate redundancy and handle multicollinearity.
signature: Extract the final core gene signature (expected ~5-20 genes) that forms the prognostic biomarker panel.

Step 6: Model Training
model: Train a multivariate survival model (e.g., Multivariate Cox, Random Forest Survival) using the final gene signature on the Training Set.

Step 7: Model Evaluation
eval: Validate the model's predictive accuracy on the unseen Test Set (30%) using C-index and Time-dependent ROC-AUC.

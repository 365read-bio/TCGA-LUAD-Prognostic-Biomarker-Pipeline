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

### Step 0: cBioPortal interface
<img width="1440" height="2036" alt="Image" src="https://github.com/user-attachments/assets/c32d7cb0-e99b-43e1-99f6-ff6338a2a859" />

1) Download Data

2) Gene Query

3) Data Types vs Case Lists: genomic data(RSEM)와 clinical data(Samples with mRNA data) 수가 510으로 일치 => Data Integrity

4) Tumor Type: 'Lung Adenocarcinoma (NOS)', 'Lung Adenocarcinoma, Mixed Subtype', 'Lung Adenocarcinoma', 'Lung Papillary Adenocarcinoma', etc

5) Genetic Ancestry Label: 'EUR'(86.7%), 'AFR'(7.2%), 'AFR_ADMIX'(3.2%), 'EAS'(9%), 'AMR'(0.2%), 'NA'(1.1%)

6) Mutated Genes

7) [Gene Query] - [Comparison/Survival] - [Survival]: DSS vs OS

8) Hazard ratio

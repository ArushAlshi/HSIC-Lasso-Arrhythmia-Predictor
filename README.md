# ECG Arrhythmia Classification with HSIC-Lasso Feature Selection

## Problem Statement
Automated ECG-based arrhythmia classification typically relies on high-dimensional 
feature sets extracted from raw signal data. This project investigates whether 
HSIC-Lasso, a kernel-based nonlinear feature selection method, can identify the 
most diagnostically relevant features from a 12-feature ECG set, and whether 
classifiers trained on this reduced set match full-feature performance.

## Data
- MIT-BIH Arrhythmia Database (PhysioNet), records 100, 200, 208, 210
- 4 beat classes: N (normal), V (ventricular), F (fusion), A (atrial premature)
- 10,445 total beats after filtering non-beat annotation markers (+, ~, |)

## Method
1. Beat windowing (±180 samples around each R-peak)
2. Feature extraction (12 total):
   - Time-domain (5): mean, std, skewness, kurtosis, zero-crossing rate
   - Frequency-domain (3): dominant frequency, spectral energy, spectral centroid
   - Wavelet-domain (4): energy per decomposition level (db4, level 3)
3. HSIC-Lasso feature selection → top 6 features
4. Classification: SVM and Random Forest, full (12) vs. selected (6) features

## Results
Full Dataset
<img width="797" height="304" alt="image" src="https://github.com/user-attachments/assets/6312aeb6-c7f4-4ca9-9622-fcaa41a07ac0" />
<img width="894" height="315" alt="image" src="https://github.com/user-attachments/assets/ab838142-53e8-4192-8c50-d2cb0c46ce3a" />

Selected Dataset
<img width="762" height="302" alt="image" src="https://github.com/user-attachments/assets/1f046be1-e48b-4abd-83bd-0af8e9148cee" />
<img width="727" height="307" alt="image" src="https://github.com/user-attachments/assets/496b27d1-9a97-46ca-a996-684174f1e963" />

Confusion matrix for Random Forest (Selected Features)
<img width="770" height="675" alt="image" src="https://github.com/user-attachments/assets/b6efee68-8506-4356-9726-f8f2700f2d28" />

Confusion matrix for SVM (Selected Features)
<img width="770" height="673" alt="image" src="https://github.com/user-attachments/assets/91a6798b-5c91-4f5f-8c8b-14b3f3998ebb" />


## Key Findings
- HSIC-Lasso-selected features matched full feature-set performance for both classifiers
- Spectral energy and skewness were the most informative features
- Random Forest handled class imbalance better than SVM
- Class A (63 examples) remained difficult to classify due to data scarcity

## How to Run
Open `notebook.ipynb` in Google Colab and run all cells sequentially.

## License
MIT

# ECG Arrhythmia Classification with HSIC-Lasso Feature Selection

## Problem Statement
Automated ECG-based arrhythmia classification typically relies on high-dimensional 
feature sets extracted from raw signal data. This project investigates whether 
HSIC-Lasso, a kernel-based nonlinear feature selection method, can identify the 
most diagnostically relevant features from a 12-feature ECG set, and how it 
compares against other common feature selection techniques in terms of both 
classification performance and computational cost.

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
3. Feature selection — four methods compared, each selecting the top 6 of 12 features:
   - HSIC-Lasso (kernel-based, nonlinear)
   - Pearson Correlation (ANOVA F-test)
   - Mutual Information
   - Lasso (L1-regularized, scaled features)
4. Classification: Random Forest, trained separately on each method's selected features

## Results

### Feature Selection Comparison

| Method | Selected Features | Selection Time (s) | Train Time (s) | Accuracy | Macro F1 |
|---|---|---|---|---|---|
| HSIC-Lasso | spectral_energy, skew, spectral_centroid, wavelet_cA3, dominant_freq, mean | 8.31 | 3.91 | 0.97 | 0.67 |
| Correlation | std, skew, kurtosis, zero-crossing, spectral_energy, wavelet_cA3 | 0.0066 | 3.15 | 0.96 | 0.66 |
| Mutual Information | spectral_centroid, wavelet_cA3, skew, kurtosis, std, spectral_energy | 1.01 | 4.20 | 0.96 | 0.67 |
| Lasso | dominant_freq, kurtosis, mean, spectral_centroid, spectral_energy, skew | 0.0072 | 4.10 | 0.97 | 0.67 |

### Confusion Matrices (HSIC-Lasso)


<img width="727" height="307" alt="image" src="https://github.com/user-attachments/assets/04772421-d75c-4cad-8009-022ae494abcd" />





<img width="762" height="302" alt="image" src="https://github.com/user-attachments/assets/2ba76f70-fa82-40b1-b56e-e259f2595a98" />




## Key Findings
- All four feature selection methods achieved comparable classification performance 
  (96-97% accuracy, 0.66-0.67 macro F1) — no method dominated on accuracy alone
- **Spectral energy and skewness** were selected by every method, providing strong 
  cross-method validation of their importance
- **HSIC-Lasso's selection time was substantially higher** (8.31s) than Correlation 
  or Lasso (~0.007s) due to its kernel-based computation — a clear speed-vs-rigor 
  tradeoff rather than a straightforward performance win
- HSIC-Lasso's advantage lies in its ability to rigorously capture **nonlinear** 
  feature-label dependencies with statistical guarantees, unlike Correlation 
  (linear only) or Lasso (linear model assumption)
- Class A (63 examples) remained difficult to classify across all methods due to 
  data scarcity
- `class_weight='balanced'` improved Random Forest's minority-class recall but 
  degraded SVM's overall accuracy, tested separately from the main comparison

## How to Run
Open the notebook in Google Colab and run all cells sequentially.

## License
MIT

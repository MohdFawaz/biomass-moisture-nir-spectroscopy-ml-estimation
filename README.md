# Moisture Content Estimation in Biomass Using NIR Spectroscopy and Machine Learning

This project aims to predict the moisture content of biomass samples using near-infrared (NIR) spectroscopy data. Accurate moisture estimation is essential for optimizing biomass processing and energy production. The project applies multiple preprocessing techniques and machine learning models to relate high-dimensional spectral data to laboratory-measured moisture values.

---

## 📊 Dataset Overview

- **Samples:** 773 biomass samples  
- **Features:** 1037 spectral features (wavelengths from ~11988.51 to ~3996.17)  
- **Target:** Moisture content (%), ranging from 18 to 73 (mean ≈ 46)

No missing values were found, and both spectral and moisture data were statistically analyzed. Correlation analysis identified specific wavelength regions strongly linked to moisture (e.g., 5114.791, 5107.076).

---

## 🧹 Preprocessing Techniques

1. **Min–Max Normalization:** Scales each spectrum to [0, 1]
2. **Savitzky–Golay Smoothing:** Reduces high-frequency noise while preserving key spectral features (window = 31, polyorder = 3)
3. **Standard Normal Variate (SNV):** Standardizes each spectrum to zero mean and unit variance

All transformations were visually validated to ensure key spectral patterns (peaks/valleys) were preserved and noise was reduced.

---

## 🔍 Outlier Detection

- **Spectral Outlier Detected:** Sample ID `3134RanBioMixMalarenergi.2` (based on PCA + z-score on Euclidean distance)
- **No Moisture Outliers:** Verified using IQR method

---

## 🧠 Modeling Approaches

### ✅ Cross-Validation:
- 5-fold cross-validation used throughout
- Grid search employed for hyperparameter tuning

### 1. **Partial Least Squares (PLS) Regression**
- Dimensionality reduction via latent variables
- Tuned `n_components` ∈ {5, 10, 15, 20}

### 2. **Support Vector Regression (SVR)**
- RBF kernel used
- Tuned:
  - `C`: [1, 10, 100]
  - `gamma`: [0.001, 0.01, 0.1]

### 3. **Artificial Neural Network (ANN)**
- Implemented with `MLPRegressor`
- Tuned:
  - `hidden_layer_sizes`: (50,), (100,), (50, 50), (100, 50)
  - `alpha` (L2 regularization): 0.0001, 0.001, 0.01
- Activation: ReLU  
- Optimizer: Adam  
- Epochs: max 1000

---

## 📈 Model Performance

### 🚀 Best Results **(Before Tuning)**

| Preprocessing | Model | R² Score | RMSE |
|---------------|--------|----------|------|
| SNV           | SVR    | **0.976** | **2.232** |
| SNV           | PLS    | 0.970    | 2.515 |
| SNV           | ANN    | 0.931    | 3.781 |

### 🔧 Results **After Grid Search Tuning**

| Preprocessing | Model | R² Score | RMSE | Best Params |
|---------------|--------|----------|------|-------------|
| Smoothed      | PLS    | 0.973    | 2.385 | `n_components=20` |
| Smoothed      | SVR    | 0.970    | 2.511 | `C=100`, `gamma=0.001` |
| Smoothed      | ANN    | 0.950    | 3.234 | `(50, 50)`, `alpha=0.01` |
| Normalized    | PLS    | 0.972    | 2.416 | `n_components=20` |
| Normalized    | SVR    | 0.970    | 2.518 | `C=100`, `gamma=0.001` |
| Normalized    | ANN    | 0.948    | 3.276 | `(50, 50)`, `alpha=0.001` |
| Raw           | PLS    | 0.965    | 2.695 | `n_components=20` |
| Raw           | SVR    | 0.951    | 3.199 | `C=100`, `gamma=0.01` |
| Raw           | ANN    | 0.539    | 9.800 | `(100, 50)`, `alpha=0.0001` |

---

## 🥇 Ranked Performance Summary

1. **SVR on SNV (before tuning):** R² = 0.976, RMSE = 2.232  
2. **PLS on Smoothed (after tuning):** R² = 0.973, RMSE = 2.385  
3. **PLS on Normalized (after tuning):** R² = 0.972, RMSE = 2.416  
4. **SVR on Normalized (after tuning):** R² = 0.970, RMSE = 2.518  
5. **SVR on Smoothed (after tuning):** R² = 0.970, RMSE = 2.511  
6. **ANN on Smoothed (after tuning):** R² = 0.950, RMSE = 3.234  
7. **ANN on Normalized (after tuning):** R² = 0.948, RMSE = 3.276  

---

## ⚠️ Bottlenecks & Challenges

- **High dimensionality** of spectral features (1037 per sample) complicates modeling
- **Model sensitivity**: ANN performance fluctuated based on architecture & preprocessing
- **Limited dataset size** (773 samples) posed generalizability risks

---

## 🔮 Future Improvements

- Apply **dimensionality reduction** (e.g. PCA, variable selection)
- Explore **ensemble models** and **deep architectures**
- Refine preprocessing techniques (e.g., baseline correction)
- Incorporate **domain knowledge** (focus on highly informative spectral bands)
- Expand dataset to improve robustness

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

---

## 👨‍💻 Author

**Moh’d Abu Quttain**  

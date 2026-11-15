
# IN718 LPBF Relative Density Prediction Using Physics-Informed Machine Learning

## Overview

This project investigates the prediction of relative density in Inconel 718 (IN718) manufactured through Laser Powder Bed Fusion (LPBF) using conventional and physics-informed machine learning approaches.

The study incorporates physics-derived features based on process parameters, including energy density metrics and geometric ratios, to improve predictive performance and investigate the relationship between LPBF process conditions and relative density.

## Research Objectives

- Develop machine learning models to predict relative density in IN718 LPBF.
- Investigate the contribution of physics-derived features to predictive performance.
- Compare conventional machine learning models with physics-informed approaches.
- Identify influential process parameters and derived physical descriptors.
- Evaluate the predictive performance of different regression algorithms.

## Dataset

The dataset contains 203 observations with five LPBF process parameters and one target variable.

| Variable | Description | Unit |
|---|---|---|
| `laser_power_w` | Laser power | W |
| `scan_speed_mm_s` | Scanning speed | mm/s |
| `hatch_space_mm` | Hatch spacing | mm |
| `layer_thickness_mm` | Layer thickness | mm |
| `spot_size_mm` | Laser spot size | mm |
| `rd_mean` | Mean relative density | % |

**Target variable:** Mean relative density (`rd_mean`).

## Methodology

### 1. Data Preprocessing and Exploratory Analysis

- Dataset inspection and statistical analysis.
- Missing-value and duplicate-value assessment.
- Distribution analysis of input parameters and target variable.
- Correlation analysis to investigate relationships between process parameters and relative density.

### 2. Physics-Informed Feature Engineering

Five additional features were derived from the original LPBF process parameters.

| Feature | Formula | Unit |
|---|---|---|
| Volumetric Energy Density (VED) | `P / (v × h × t)` | J/mm³ |
| Linear Energy Density (LED) | `P / v` | J/mm |
| Areal Energy Density (AED) | `P / (v × h)` | J/mm² |
| Beam-to-Hatch Ratio (BHR) | `d / h` | Dimensionless |
| Beam-to-Layer Ratio (BLR) | `d / t` | Dimensionless |

Where:

- `P` = Laser power
- `v` = Scanning speed
- `h` = Hatch spacing
- `t` = Layer thickness
- `d` = Laser spot size

Two feature configurations were investigated:

- **Conventional ML:** Five original process parameters.
- **Physics-Informed ML:** Five original parameters plus five derived physics-based features.

### 3. Machine Learning Models

The following regression algorithms were evaluated:

- Random Forest (RF)
- Extra Trees (ET)
- Gradient Boosting (GB)
- HistGradientBoosting (HistGB)
- XGBoost
- Support Vector Regression (SVR)
- K-Nearest Neighbors (KNN)
- Artificial Neural Network (ANN)
- Gaussian Process Regression (GPR)

### 4. Model Evaluation and Optimization

- Stratified train-test splitting to preserve relative-density distribution across subsets.
- Five-fold stratified cross-validation.
- Hyperparameter optimization using RandomizedSearchCV.
- Model performance evaluation using R², Root Mean Squared Error (RMSE), and Mean Absolute Error (MAE).

### 5. Feature Importance Analysis

Permutation importance was applied to the best-performing physics-informed Random Forest model to investigate the contribution of individual input features to predictive performance.

## Results

### Best Tuned Model Performance

The physics-informed Random Forest model achieved the best overall test-set performance.

| Feature Set | Model | Train R² | Test R² | Test RMSE | Test MAE |
|---|---|---:|---:|---:|---:|
| Physics-Informed ML | RF | 0.9625 | **0.8825** | **0.8025** | 0.5261 |
| Physics-Informed ML | ET | 0.9826 | 0.8545 | 0.8929 | **0.4994** |
| Conventional ML | ET | 0.9583 | 0.8283 | 0.9701 | 0.5809 |
| Conventional ML | RF | 0.9376 | 0.7917 | 1.0683 | 0.5705 |
| Physics-Informed ML | HistGB | 0.9455 | 0.6843 | 1.3153 | 0.6803 |
| Conventional ML | HistGB | 0.9775 | 0.6027 | 1.4756 | 0.8270 |

### Key Findings

- Physics-informed feature engineering improved the test-set predictive performance of RF, ET, and HistGB compared with their conventional counterparts.
- Physics-informed RF achieved the highest test R² of **0.8825** and the lowest test RMSE of **0.8025**.
- Physics-informed ET achieved the lowest test MAE of **0.4994**.
- Physics-informed RF reduced test RMSE by approximately **24.9%** compared with conventional RF.
- The results suggest that incorporating energy-density descriptors and geometric ratios provides useful information for predicting relative density.

### Permutation Feature Importance

The permutation importance analysis identified the following influential features.

| Rank | Feature | Importance Mean |
|---:|---|---:|
| 1 | AED | 1.1892 |
| 2 | VED | 0.5253 |
| 3 | LED | 0.1357 |
| 4 | BLR | 0.1242 |
| 5 | Scan Speed | 0.0563 |
| 6 | BHR | 0.0508 |
| 7 | Laser Power | 0.0371 |
| 8 | Hatch Spacing | 0.0350 |
| 9 | Spot Size | 0.0264 |
| 10 | Layer Thickness | 0.0157 |

### Mechanical Interpretation

- AED and VED emerged as the two dominant predictive features, highlighting the importance of combined energy-input characteristics in LPBF.
- Energy input influences melt-pool development and powder consolidation. Insufficient energy can contribute to lack-of-fusion defects, whereas excessive energy input can promote keyhole formation and melt-pool instability.
- BLR and BHR provide additional geometric information about the relationship between laser spot size, layer thickness, and hatch spacing.
- Scan speed influences laser-material interaction time and melt-pool development.
- The relative importance of derived features suggests that combined process descriptors can provide useful information beyond individual process parameters.

These interpretations are physically plausible but are not direct experimental evidence of defect formation or melt-pool behavior. AED, VED, and LED are also mathematically related, so their importance scores should not be interpreted as independent physical effects.

## Technologies and Libraries

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- XGBoost
- SHAP
- Jupyter Notebook / Kaggle

## Reproducibility

The analysis was conducted using Python in a Kaggle notebook environment.

To reproduce the analysis:

1. Download or access the dataset.
2. Install the required Python libraries.
3. Run the notebook cells in sequence.
4. Perform preprocessing and physics-informed feature engineering.
5. Train and evaluate the machine learning models.
6. Compare conventional and physics-informed model performance.

## Limitations and Future Work

- The dataset contains 203 observations, which limits the amount of available training data.
- Energy-density features are mathematically correlated, which can affect feature importance interpretation.
- Independent experimental validation or source-wise validation would strengthen the generalizability assessment.
- Future work can investigate explainable machine learning and additional physics-based descriptors.

## Citation

If you use or refer to this project, please cite the repository and acknowledge the original dataset source.

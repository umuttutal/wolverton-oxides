# Predicting Thermodynamic Stability of Perovskite Oxides

A machine learning approach to classify the thermodynamic stability of perovskite oxide structures (ABO₃) using DFT-calculated properties and elemental features.

## Project Overview

Perovskite oxides are versatile materials with applications in catalysis, fuel cells, photovoltaics, and electronics. This project predicts their thermodynamic stability using energy above convex hull (e_hull) values as the classification target, with a threshold of 0.04 eV distinguishing stable from unstable compositions.

### Key Features
- Classification model achieving high precision and recall
- Feature engineering from elemental properties
- Dimensionality reduction using PCA
- XGBoost with early stopping to prevent overfitting
- SHAP analysis for model interpretability

## Dataset

The dataset combines:
- **Base data**: DFT calculations from Wolverton and Emery (2017) for ABO₃ perovskite oxides
- **Elemental properties**: Extracted from PubChem Database

### Features (16 total)
- Formation energy (eV)
- Volume per atom (Å³/atom)
- Bandgap from PBE calculations (eV)
- Lattice parameters (a, b, c in Å)
- Oxygen vacancy formation energy (eV)
- Weighted atomic mass
- Electronegativity difference and ratio (A-B sites)
- Ionization energy difference (A-B sites)
- Mean lattice parameter
- Lowest distortion symmetry (4 binary categorical variables)

### Target Variable
**Energy above convex hull (e_hull)**: Binary classification
- Stable: e_hull < 0.04 eV
- Unstable: e_hull ≥ 0.04 eV

## Methodology

### Data Preprocessing
1. **Missing value imputation**: KNN imputation with standard scaling (~17% missing in electronegativity features)
2. **Outlier detection**: Local Outlier Factor (LOF) identified 1.61% outliers, retained due to potential information value
3. **Dimensionality reduction**: PCA reduced 16 features to 7 principal components (90% variance explained)

### Model Selection
Baseline evaluation of 4 algorithms:
- Logistic Regression
- Random Forest
- Support Vector Classifier
- **XGBoost** (selected as final model)

### Hyperparameter Optimization
- Grid search with 5-fold cross-validation
- Early stopping using validation set (70:15:15 train-test-val split)
- Stratified splitting to maintain class balance

### Evaluation Metrics
- **F1-Score**: Primary metric (balances precision and recall)
- ROC-AUC
- Precision-Recall curves
- Confusion matrix

## Results

The final XGBoost model demonstrates strong performance with balanced precision and recall, successfully identifying stable perovskite compositions while minimizing both false positives and false negatives.

Key insights from analysis:
- Formation energy shows moderate positive correlation with e_hull
- Oxygen vacancy formation energy inversely correlates with stability
- Natural clustering by crystal symmetry (cubic, orthorhombic, rhombohedral, tetragonal)
- Semiconductor perovskites form distinct cluster

## Repository Structure

```
├── wolverton.ipynb           # Main analysis notebook
├── wolverton_oxides.csv      # Processed dataset
├── PubChemElements_all.csv   # Elemental properties
├── shannon-radii.json        # Shannon radii reference data
└── README.md                 # This file
```

## Requirements

```python
numpy
pandas
scikit-learn
xgboost
matplotlib
seaborn
shap
umap-learn
```

## Usage

Open and run `wolverton.ipynb` to:
1. Load and preprocess the data
2. Perform exploratory data analysis
3. Train and evaluate classification models
4. Analyze feature importance with SHAP

## Future Work

- Incorporate ionic radii using Shannon Radii Table with Pymatgen for coordination number and oxidation state calculations
- Create stability maps highlighting application-specific properties (bandgap for photovoltaics, oxygen vacancy formation for catalysis)
- Explore ensemble methods combining multiple model architectures
- Extend to other perovskite structure types beyond ABO₃

## References

1. Emery, A., Wolverton, C. High-throughput DFT calculations of formation energy, stability and oxygen vacancy formation energy of ABO₃ perovskites. *Sci Data* **4**, 170153 (2017). https://doi.org/10.1038/sdata.2017.153

2. Kim, S., et al. PubChem 2025 update. *Nucleic Acids Res.*, **53**(D1), D1516–D1525 (2025). https://doi.org/10.1093/nar/gkae1059


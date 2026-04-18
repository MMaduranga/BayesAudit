# Bayesian Audit Grade Prediction under Label Scarcity

This repository contains the implementation and experiments for a **probabilistic audit grading system** that predicts retail branch audit outcomes using **Bayesian machine learning**.

The project focuses on leveraging **daily KPI-based risk indicators** and **temporal patterns** to generate **audit grade predictions with uncertainty estimates**, enabling proactive and risk-aware auditing.

---

## 📌 Problem Statement

Retail audit processes are:

- **Manual and time-consuming** (2–3 days per branch)
- Conducted **infrequently**, leading to delayed issue detection
- Dependent on **limited labeled audit data**
- Surrounded by **large volumes of unlabeled KPI data**

This creates challenges such as:

- **Label scarcity** (only ~116 audited branches available)
- **Class imbalance** across audit grades
- Inability of traditional models to provide **reliable confidence measures**

---

## 🎯 Objectives

- Develop a **Bayesian classification model** for audit grade prediction  
- Capture **temporal KPI behavior prior to audits**  
- Provide **probabilistic predictions with uncertainty estimates**  
- Evaluate model robustness through **kernel experiments**  
- Improve performance using **semi-supervised learning**  

---

## 🧠 Methodology Overview

### Data Sources
- **Audit Dataset**: Historical audit grades with audit dates (~116 records after filtering)
- **KPI Dataset**: Daily branch-level risk indicators (~58,000+ records)

### Data Processing
- Convert date fields into datetime format
- Filter audit data to **recent 60-day window**
- Remove KPI features with **>25% missing values**
- Handle missing values using **imputation (filled with 0 after filtering)**

---

## 🔧 Feature Engineering

- Construct **sliding time windows** for each branch
- Extract **last N days (7–45 days tested)** before audit date
- Each observation includes:
  - `day` (temporal index)
  - Multiple KPI risk scores

- Final feature vector:
X = [time_index, KPI_features...]


- Separate datasets:
- **Graded branches** (labeled)
- **Ungraded branches** (unlabeled)

---

## 🔍 Model

### Sparse Variational Gaussian Process (SVGP)

Implemented using **GPflow**, the model:

- Handles **multi-class classification**
- Uses **inducing points (KMeans)** for scalability
- Optimizes **Evidence Lower Bound (ELBO)**

### Why SVGP?

- Works well with **small labeled datasets**
- Provides **probabilistic outputs**
- Captures **nonlinear relationships**
- Enables **uncertainty quantification**

---

## 🧪 Experiments

### 1️⃣ Baseline Model Performance

- Accuracy: **0.84**
- Macro F1-score: **0.63**

The model shows strong performance despite:
- Limited labeled data
- Imbalanced classes

---

### 2️⃣ Kernel Sensitivity Analysis

| Kernel | Accuracy | Macro-F1 |
|--------|----------|----------|
| **RBF + Linear** | **0.862** | **0.805** |
| RBF | 0.853 | 0.637 |
| Matern32 × Linear | 0.853 | 0.637 |
| RBF × Linear | 0.845 | 0.633 |
| Matern32 | 0.845 | 0.505 |
| Linear | 0.612 | 0.445 |

**Insight:**
- Additive kernel (**RBF + Linear**) performs best
- Captures both **global trends + nonlinear variations**

---

### 3️⃣ Bayesian Uncertainty Analysis

We evaluate prediction confidence using **predictive entropy**:

- Mean entropy (Correct): **0.168**
- Mean entropy (Incorrect): **0.356**

➡️ Incorrect predictions show **significantly higher uncertainty**

---

### 4️⃣ Accuracy–Coverage Trade-off

Selective prediction using entropy threshold:

| Coverage | Accuracy |
|----------|----------|
| 43% | 88% |
| **61%** | **90%** |
| 70% | 89% |

➡️ Enables **human-in-the-loop auditing**
- High-confidence → automated
- Low-confidence → manual review

---

### 5️⃣ Dynamic Time Window Experiment

| Window (Days) | Accuracy |
|--------------|----------|
| 7 | **0.8707** |
| 14 | 0.8534 |
| 21 | **0.8707** |
| 30 | 0.8448 |
| 45 | 0.8276 |

**Insight:**
- Shorter windows (7–21 days) perform better
- Long windows introduce **noise**

---

### 6️⃣ Semi-Supervised Learning (Pseudo-Labeling)

Using unlabeled branches:

| Method | Accuracy |
|--------|----------|
| Baseline | 0.8448 |
| Confidence-based | 0.8448 |
| **Entropy-based** | **0.8534** |

**Insight:**
- Entropy filtering improves performance
- Confidence-only filtering is less reliable

---

## 📊 Key Findings

- Bayesian models perform well under **label scarcity**
- **Kernel selection** significantly impacts performance
- **Uncertainty estimation** improves decision reliability
- **Short-term temporal patterns** are most informative
- **Semi-supervised learning** provides measurable gains

---

## 🛠️ Technologies Used

- Python  
- Pandas, NumPy  
- Scikit-learn  
- GPflow (TensorFlow backend)  
- Matplotlib, Seaborn  

---

## 📁 Project Structure
/Experiments
├── kernel_analysis
├── uncertainty_analysis
├── time_window_experiment
├── semi_supervised
/Notebooks
README.md


---

## 🚀 Future Improvements

- Advanced Bayesian models (Deep GP, BNN)
- Real-time deployment pipeline
- Improved class imbalance handling
- Explainability (SHAP, feature attribution)
- Calibration metrics (ECE, Brier Score)

---

## 📬 Contact

For questions, discussions, or collaboration:
- Open an issue in this repository
# Bayesian Audit Grade Prediction under Label Scarcity

This repository contains the code and experiments for an **MCS research project** focused on **audit grade prediction** using **Bayesian machine learning** under **extreme label scarcity**.

The work investigates whether **temporal KPI-based risk indicators** can be used to probabilistically predict audit grades when only a small number of labeled audits are available.

---

## 📌 Problem Statement

Retail audit outcomes (grades) are expensive and infrequent, resulting in:

- **Very limited labeled data** (63 audited branches)
- **Large amounts of unlabeled operational data** (300+ branches)
- High uncertainty and class imbalance

Traditional machine learning models tend to **overfit** and produce **overconfident predictions** in such settings.

---

## 🎯 Research Objectives

1. Develop a **Bayesian classification framework** suitable for small labeled datasets  
2. Model **temporal KPI behavior prior to audits**  
3. Quantify **predictive uncertainty** to support risk-aware decision-making  
4. Evaluate how **kernel assumptions** affect performance under label scarcity  

---

## 🧠 Methodology Overview

### Data Processing
- KPI time-series aligned relative to audit dates
- Fixed **30-day pre-audit window** per branch
- Missing-value analysis and feature pruning (>25% nulls removed)
- Branch-level temporal aggregation

### Feature Representation
Each observation consists of:
- Time index (`day`)
- Operational risk KPIs (inventory, cash, credit, arrears, etc.)

---

## 🔍 Model

We use a **Sparse Variational Gaussian Process (SVGP)** classifier implemented with **GPflow**.

### Why Bayesian Gaussian Processes?
- Sample-efficient learning
- Explicit modeling of **epistemic uncertainty**
- Well-suited for **low-label regimes**
- Probabilistic outputs (not just hard predictions)

---

## 🧪 Experiments

### 1️⃣ Kernel Sensitivity Analysis

We compare multiple kernels to evaluate modeling assumptions about KPI behavior:

| Kernel | Accuracy | Macro-F1 |
|------|----------|----------|
| **RBF + Linear** | **0.862** | **0.805** |
| RBF | 0.853 | 0.637 |
| RBF × Linear | 0.845 | 0.633 |
| Matern32 × Linear | 0.853 | 0.637 |
| Matern32 | 0.845 | 0.505 |
| Linear | 0.612 | 0.445 |

**Key insight:**  
Additive kernels (RBF + Linear) outperform multiplicative and non-smooth kernels by balancing expressiveness and regularization under limited labeled data.

---

### 2️⃣ Bayesian Uncertainty Analysis (Core Contribution)

We evaluate whether **predictive entropy** correlates with model error.

**Results:**
- Mean entropy (correct predictions): **0.168**
- Mean entropy (incorrect predictions): **0.356**

Incorrect predictions exhibit **more than double the uncertainty**, demonstrating that the model is aware of its own limitations.

---

### 3️⃣ Accuracy–Coverage Trade-off (Selective Prediction)

By rejecting high-uncertainty predictions:

| Coverage | Accuracy |
|--------|----------|
| 43% | 88% |
| **61%** | **90%** |
| 70% | 89% |

This enables a **human-in-the-loop audit workflow**, where uncertain cases are deferred for manual review.

---

## 📊 Key Findings

- Bayesian uncertainty is **strongly correlated with misclassification**
- Additive kernels are more robust under label scarcity
- Selective prediction improves decision reliability
- Gaussian Processes are well-suited for audit risk modeling

---

## 🛠️ Technologies Used

- Python
- Pandas, NumPy
- Scikit-learn
- GPflow (TensorFlow)
- Matplotlib, Seaborn


# 🧬 MLB S25 Hackathon - ML-Guided Protein Engineering (Group 3)

This repository contains the solution for **MLB S25 Hackathon** submitted by **Group 3**, aiming to predict protein variant fitness using active learning and ESM-based deep learning models.

## 📌 Problem Overview

- **Goal**: Predict the functional fitness (DMS_score) of protein point mutations.
- **Data**: Only 10% of the full mutation dataset is labeled; the rest can be explored via active learning (up to 300 queries).
- **Task**:
  - Train a model to predict fitness for all test mutations.
  - Select 3 × 100 mutations to query.
  - Recommend top 10 beneficial mutants.

## 📁 Dataset

- `sequence.fasta`: Wild-type protein sequence
- `train.csv`: Labeled data with `mutant`, `DMS_score`
- `test.csv`: All possible single-point mutants for prediction and querying

Mutation format: `M0A` → WT(M) at position 0 mutated to A.

## 🔬 Approach Overview

We propose a **deep learning + active learning** pipeline:

### 🔧 Model Design

- **Backbone**: [ESM-2 (12-layer, 35M)](https://github.com/facebookresearch/esm) pre-trained protein language model
- **Encoder**: Extracts residue-level features from wild-type and mutant sequences.
- **Regression Head**: 
  - Takes concatenated vectors of WT & mutant at the mutation site
  - Supports optional physicochemical features (gravy, pI, molecular weight, aromaticity) from Biopython
- **Loss**: MSELoss
- **Optimizer**: AdamW + OneCycleLR

### ⚙️ Active Learning Strategy

We designed 6 query strategies:
- `greedy`: select top-scoring mutants
- `uncertainty`: score closest to 0.5
- `random`: random sampling
- `mcd_uncertainty`: MC Dropout std
- `ucb`: mean + std
- `ei`: expected improvement (Gaussian)

Each round:
1. Train model on current labeled data.
2. Predict scores (with uncertainty) on candidates.
3. Query 100 new labels via selected strategy.
4. Update training set.

We used `mcd_uncertainty` in our final submission due to better exploration capability.

## 🧪 Results

- Spearman Correlation on validation set: **up to 0.73**
- Top-10 predicted beneficial mutations submitted from final round

## 🛠️ How to Run

### 1. Clone the repo
```bash
git clone https://github.com/yourusername/MLB-Hackathon-Group3.git
cd MLB-Hackathon-Group3
```
### 2. Run the code
Use `ActiveLearning_Hackathon_Group_3.ipynb` on **Google Colab** (recommended for GPU) or Jupyter to train and evaluate.


## 🙌 Contributors

- Xinyue Huang  
- Yiling Wu 
- Sizhe Fang  
- Xuanzhang Liu

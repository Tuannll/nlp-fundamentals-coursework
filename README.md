<div align="center">

# 🧠 NLP Fundamentals Coursework

**Comprehensive Natural Language Processing coursework: from text preprocessing and feature extraction to classical ML, Vietnamese Word2Vec Skip-gram, and Deep Learning BiLSTM.**

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![Gensim](https://img.shields.io/badge/Gensim-Word2Vec-forestgreen)](https://radimrehurek.com/gensim/)
[![Underthesea](https://img.shields.io/badge/Underthesea-Vietnamese_NLP-orange)](https://github.com/undertheseanlp/underthesea)
[![CI](https://img.shields.io/github/actions/workflow/status/Tuannll/nlp-fundamentals-coursework/ci.yml?branch=main&logo=githubactions&logoColor=white&label=CI)](https://github.com/Tuannll/nlp-fundamentals-coursework/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**English** | [Tiếng Việt](README.vi.md)

**[📌 Overview](#overview) • [🔬 Lab Modules](#labs) • [📦 Assets & Models](#assets) • [🚀 Quick Start](#quick-start) • [📄 License](#license)**

</div>

---

<a id="overview"></a>
## 📌 Repository Overview

This repository packages 4 progressive hands-on lab modules:

| Directory | Subject | Techniques & Architecture | Dataset | Benchmark Highlights |
| :--- | :--- | :--- | :--- | :---: |
| [`01-text-preprocessing-vectorization/`](./01-text-preprocessing-vectorization/) | Preprocessing & Vectorization | Underthesea, BoW, TF-IDF Unigram/Bigram | Vietnamese news corpus | Term distribution, WordCloud, TF-IDF Heatmap |
| [`02-sentiment-classification-ml/`](./02-sentiment-classification-ml/) | Classical Sentiment Classification | Multiclass Logistic Regression, Linear SVM, Balanced Weights | 3-class user reviews | 76.5% Accuracy, 0.76 Macro F1 |
| [`03-vietnamese-word2vec-skipgram/`](./03-vietnamese-word2vec-skipgram/) | Word2Vec Skip-gram Training | LineSentence streaming, Negative Sampling ($k=5$), t-SNE | Vietnamese Wikipedia (7M sentences) | Semantic Word Analogy, 2D t-SNE cluster |
| [`04-deep-learning-bilstm-sentiment/`](./04-deep-learning-bilstm-sentiment/) | Deep Learning Sentiment Analysis | Bidirectional LSTM, Pretrained Embeddings, Early Stopping | 50,000 IMDB Movie Reviews | 87.2% Val Accuracy, negation-aware |

---

<a id="labs"></a>
## 🔬 Detailed Lab Modules

### 1. Lab 1: Vietnamese Text Preprocessing & Vectorization (`01-text-preprocessing-vectorization/`)
* **Notebook**: `text_preprocessing_vectorization.ipynb`
* **Pipeline**: Lowercasing, punctuation stripping, compound word segmentation via `underthesea` (`_`), and stopword pruning.
* **Feature Extraction**: Vocabulary building, Bag-of-Words (BoW) matrices, and TF-IDF representations (Unigram + Bigram).
* **Visualizations**: Word frequency distributions (`word_frequency.png`), word cloud graphics (`wordcloud.png`), and TF-IDF weight heatmaps (`tfidf_heatmap.png`).
* **Theory**: `theory_discussion.md` compares TF-IDF properties versus raw frequency matrices.

### 2. Lab 2: Sentiment Classification with Classical ML (`02-sentiment-classification-ml/`)
* **Notebook**: `sentiment_classification_ml.ipynb`
* **Task**: 3-class sentiment prediction (Negative = 0, Neutral = 1, Positive = 2).
* **Algorithms**: Multiclass Logistic Regression and Linear Support Vector Machines (Linear SVM).
* **Class Imbalance Mitigation**: Dynamic cost-sensitive weighting via `class_weight='balanced'`.
* **Evaluation**: Confusion matrices, multiclass ROC-AUC curves, and top-10 indicative n-grams (`top10_words_per_class.png`).
* **Theory**: `theory_discussion.md` details imbalance loss penalization and optimization mechanics.

### 3. Lab 3: Vietnamese Word2Vec Skip-gram (`03-vietnamese-word2vec-skipgram/`)
* **Notebook**: `vietnamese_word2vec_skipgram.ipynb`
* **Data Ingestion**: 7-million sentence Wikipedia corpus streamed via `LineSentence` into multi-threaded C backend, preventing memory overhead.
* **Architecture**: Skip-gram, embedding dimension 100, window size 5, Negative Sampling ($k=5$).
* **Verification & Inference**:
  - Nearest neighbor retrieval (`most_similar`) across semantic categories.
  - Word vector arithmetic (Word Analogy): $\vec{w}_{\text{Hanoi}} - \vec{w}_{\text{Vietnam}} + \vec{w}_{\text{France}} \approx \vec{w}_{\text{Paris}}$.
  - 2D semantic space visualization using t-SNE dimensionality reduction.
* **Theory**: `theory_discussion.md` contrasts Skip-gram vs. CBOW, Hierarchical Softmax vs. Negative Sampling, and Out-of-Vocabulary (OOV) strategies.

### 4. Lab 4: BiLSTM Sentiment Analysis on IMDB (`04-deep-learning-bilstm-sentiment/`)
* **Notebook**: `bilstm_sentiment_analysis.ipynb`
* **Architecture**: PyTorch Bidirectional LSTM (BiLSTM) with pretrained embedding weights.
* **Negation Retention**: Preserves critical negation markers (`not`, `no`, `never`, `nor`) during token filtering, accurately predicting polarity shifts ("Not bad", "Never disappoints").
* **Regularization**: 0.5 Dropout, L2 Weight Decay ($10^{-4}$) in Adam Optimizer, and Early Stopping with patience = 2.

---

<a id="assets"></a>
## 📦 Large Datasets & Model Weights (Releases)

Large binaries and raw corpora are distributed via [GitHub Releases](https://github.com/Tuannll/nlp-fundamentals-coursework/releases):

| File | Size | Destination Path | Description | Download |
| :--- | :---: | :--- | :--- | :---: |
| **`datatrain.txt`** | 888 MB | `03-vietnamese-word2vec-skipgram/datatrain.txt` | 7M-sentence tokenized Vietnamese Wikipedia corpus | [Download v1.0.0](https://github.com/Tuannll/nlp-fundamentals-coursework/releases/tag/v1.0.0) |
| **`best_model.pt`** | 31.6 MB | `04-deep-learning-bilstm-sentiment/best_model.pt` | Trained PyTorch BiLSTM checkpoint | [Download v1.0.0](https://github.com/Tuannll/nlp-fundamentals-coursework/releases/tag/v1.0.0) |

> [!TIP]
> Download directly via GitHub CLI:
> ```bash
> gh release download v1.0.0 -p "datatrain.txt" -D 03-vietnamese-word2vec-skipgram/
> gh release download v1.0.0 -p "best_model.pt" -D 04-deep-learning-bilstm-sentiment/
> ```

---

<a id="quick-start"></a>
## 🚀 Quick Start

### 1. Environment Setup
```bash
git clone https://github.com/Tuannll/nlp-fundamentals-coursework.git
cd nlp-fundamentals-coursework

# Create and activate Python 3.10 environment
conda create -n nlp-labs python=3.10 -y
conda activate nlp-labs

# Install dependencies
pip install -r requirements.txt
```

### 2. Launch Jupyter Lab
```bash
jupyter lab
```
Navigate to individual notebooks:
- `01-text-preprocessing-vectorization/text_preprocessing_vectorization.ipynb`
- `02-sentiment-classification-ml/sentiment_classification_ml.ipynb`
- `03-vietnamese-word2vec-skipgram/vietnamese_word2vec_skipgram.ipynb`
- `04-deep-learning-bilstm-sentiment/bilstm_sentiment_analysis.ipynb`

---

<a id="license"></a>
## 📄 License

Distributed under the [MIT License](LICENSE).

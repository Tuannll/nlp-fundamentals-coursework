<div align="center">

# 🧠 NLP Fundamentals Coursework

**Tổng hợp bài thực hành Xử lý Ngôn ngữ Tự nhiên: từ tiền xử lý, trích xuất đặc trưng, Machine Learning đến Word2Vec Skip-gram và Deep Learning BiLSTM.**

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![Gensim](https://img.shields.io/badge/Gensim-Word2Vec-forestgreen)](https://radimrehurek.com/gensim/)
[![Underthesea](https://img.shields.io/badge/Underthesea-Vietnamese_NLP-orange)](https://github.com/undertheseanlp/underthesea)
[![CI](https://img.shields.io/github/actions/workflow/status/Tuannll/nlp-fundamentals-coursework/ci.yml?branch=main&logo=githubactions&logoColor=white&label=CI)](https://github.com/Tuannll/nlp-fundamentals-coursework/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

[English](README.md) | **Tiếng Việt**

**[📌 Tổng quan](#overview) • [🔬 Chi tiết Labs](#labs) • [📦 Tài nguyên Releases](#assets) • [🚀 Cài đặt & Khởi chạy](#quick-start) • [📄 License](#license)**

</div>

---

<a id="overview"></a>
## 📌 Tổng quan Cấu trúc Kho Lưu trữ

Repository đóng gói 4 bài thực hành (Labs) chuyên sâu theo lộ trình chuẩn:

| Thư mục | Chủ đề | Kỹ thuật & Mô hình | Dữ liệu kiểm thử | Kết quả chính |
| :--- | :--- | :--- | :--- | :---: |
| [`01-text-preprocessing-vectorization/`](./01-text-preprocessing-vectorization/) | Tiền xử lý & Trích xuất đặc trưng | Underthesea, BoW, TF-IDF Unigram/Bigram | Văn bản tin tức tiếng Việt | Heatmap, WordCloud hoàn chỉnh |
| [`02-sentiment-classification-ml/`](./02-sentiment-classification-ml/) | Phân loại cảm xúc (ML cổ điển) | Logistic Regression, Linear SVM, Balanced Class Weight | Bình luận phân 3 lớp cảm xúc | Accuracy 76.5%, Macro F1 0.76 |
| [`03-vietnamese-word2vec-skipgram/`](./03-vietnamese-word2vec-skipgram/) | Huấn luyện Word2Vec Skip-gram | LineSentence streaming, Negative Sampling ($k=5$), t-SNE | Wikipedia tiếng Việt (7M câu) | Word Analogy chuẩn xác, t-SNE 2D |
| [`04-deep-learning-bilstm-sentiment/`](./04-deep-learning-bilstm-sentiment/) | Phân loại cảm xúc Deep Learning | Bidirectional LSTM, Pretrained Word2Vec, Early Stopping | 50,000 IMDB Movie Reviews | Val Acc 87.2%, Xử lý đúng phủ định |

---

<a id="labs"></a>
## 🔬 Nội dung Kỹ thuật Chi tiết

### 1. Lab 1: Tiền xử lý văn bản tiếng Việt & Vector hóa (`01-text-preprocessing-vectorization/`)
* **Notebook**: `text_preprocessing_vectorization.ipynb`
* **Quy trình xử lý**: Chuẩn hóa chữ thường, lọc ký tự đặc biệt, ghép từ phức tiếng Việt bằng dấu gạch dưới (`_`) qua `underthesea`, lọc bỏ từ dừng (stopwords).
* **Trích xuất đặc trưng**: Xây dựng từ điển, ma trận túi từ (Bag of Words) và ma trận TF-IDF (Unigram kết hợp Bigram).
* **Trực quan hóa**: Biểu đồ phân phối tần suất từ (`word_frequency.png`), đám mây từ vựng (`wordcloud.png`), và bản đồ nhiệt trọng số TF-IDF (`tfidf_heatmap.png`).
* **Lý thuyết**: `theory_discussion.md` phân tích chi tiết cơ chế TF-IDF so với BoW.

### 2. Lab 2: Phân loại cảm xúc với Machine Learning cổ điển (`02-sentiment-classification-ml/`)
* **Notebook**: `sentiment_classification_ml.ipynb`
* **Bài toán**: Phân loại cảm xúc 3 lớp (Tiêu cực = 0, Trung tính = 1, Tích cực = 2).
* **Thuật toán**: Logistic Regression đa lớp và Linear Support Vector Machine (Linear SVM).
* **Xử lý mất cân bằng nhãn**: Tích hợp gán trọng số mẫu tự động `class_weight='balanced'`.
* **Đánh giá**: Ma trận nhầm lẫn (Confusion Matrix), đường cong đa lớp ROC-AUC, biểu đồ top 10 từ khóa đặc trưng cho từng lớp cảm xúc (`top10_words_per_class.png`).
* **Lý thuyết**: `theory_discussion.md` giải thích chiến lược xử lý Class Imbalance và cơ chế tối ưu hàm mất mát.

### 3. Lab 3: Huấn luyện nhúng từ Word2Vec tiếng Việt (`03-vietnamese-word2vec-skipgram/`)
* **Notebook**: `vietnamese_word2vec_skipgram.ipynb`
* **Pipeline dữ liệu tối ưu**: Tận dụng corpus 7 triệu dòng Wikipedia đã tách từ sẵn; dùng `LineSentence` streaming trực tiếp vào backend C đa luồng, loại bỏ nghẽn CPU.
* **Kiến trúc mô hình**: Skip-gram, kích thước vector 100, cửa sổ trượt (window size) 5, Negative Sampling 5 mẫu âm.
* **Suy luận & Kiểm định**:
  - Truy vấn láng giềng gần nhất (`most_similar`) theo các trường nghĩa (chức vụ, quốc gia, thời sự).
  - Phép toán đại số vector (Word Analogy): $\vec{w}_{Hà Nội} - \vec{w}_{Việt Nam} + \vec{w}_{Pháp} \approx \vec{w}_{Paris}$.
  - Trực quan hóa không gian ngữ nghĩa 2D bằng thuật toán t-SNE.
* **Lý thuyết**: `theory_discussion.md` phân tích Skip-gram vs CBOW, Hierarchical Softmax vs Negative Sampling, và giải pháp từ vựng ngoài từ điển (OOV).

### 4. Lab 4: Phân loại cảm xúc BiLSTM trên IMDB (`04-deep-learning-bilstm-sentiment/`)
* **Notebook**: `bilstm_sentiment_analysis.ipynb`
* **Kiến trúc mạng**: PyTorch Bidirectional LSTM (BiLSTM) tích hợp ma trận trọng số nhúng khởi tạo sẵn từ Word2Vec.
* **Khắc phục lỗi mất từ phủ định**: Bảo lưu toàn diện các từ phủ định (`not`, `no`, `never`, `nor`) trong danh sách Stopwords, phân loại chính xác các sắc thái đảo nghĩa ("Not bad", "Never disappoints").
* **Chống Overfitting**: Tăng tỷ lệ Dropout lên `0.5`, thêm L2 Weight Decay ($10^{-4}$) vào Adam Optimizer, áp dụng cơ chế tự động dừng sớm (Early Stopping với patience = 2).

---

<a id="assets"></a>
## 📦 Tải Dữ liệu Lớn & Trọng số Mô hình (Releases)

Do giới hạn dung lượng lưu trữ của Git, các file dữ liệu thô và trọng số nhị phân được lưu trữ tại [GitHub Releases](https://github.com/Tuannll/nlp-fundamentals-coursework/releases):

| File | Dung lượng | Vị trí đặt sau khi tải | Mô tả | Link tải |
| :--- | :---: | :--- | :--- | :---: |
| **`datatrain.txt`** | 888 MB | `03-vietnamese-word2vec-skipgram/datatrain.txt` | 7 triệu dòng văn bản Wikipedia tiếng Việt | [Tải từ Release](https://github.com/Tuannll/nlp-fundamentals-coursework/releases/tag/v1.0.0) |
| **`best_model.pt`** | 31.6 MB | `04-deep-learning-bilstm-sentiment/best_model.pt` | Trọng số PyTorch BiLSTM tốt nhất đã huấn luyện | [Tải từ Release](https://github.com/Tuannll/nlp-fundamentals-coursework/releases/tag/v1.0.0) |

> [!TIP]
> Tải nhanh qua GitHub CLI:
> ```bash
> gh release download v1.0.0 -p "datatrain.txt" -D 03-vietnamese-word2vec-skipgram/
> gh release download v1.0.0 -p "best_model.pt" -D 04-deep-learning-bilstm-sentiment/
> ```

---

<a id="quick-start"></a>
## 🚀 Hướng dẫn Cài đặt & Khởi chạy (Quick Start)

### 1. Khởi tạo môi trường
```bash
git clone https://github.com/Tuannll/nlp-fundamentals-coursework.git
cd nlp-fundamentals-coursework

# Tạo và kích hoạt môi trường ảo Python 3.10
conda create -n nlp-labs python=3.10 -y
conda activate nlp-labs

# Cài đặt toàn bộ thư viện phụ thuộc
pip install -r requirements.txt
```

### 2. Khởi chạy Jupyter Lab
```bash
jupyter lab
```
Truy cập vào từng thư mục để chạy notebook tương ứng:
- `01-text-preprocessing-vectorization/text_preprocessing_vectorization.ipynb`
- `02-sentiment-classification-ml/sentiment_classification_ml.ipynb`
- `03-vietnamese-word2vec-skipgram/vietnamese_word2vec_skipgram.ipynb`
- `04-deep-learning-bilstm-sentiment/bilstm_sentiment_analysis.ipynb`

---

<a id="license"></a>
## 📄 License

Phát hành theo giấy phép [MIT License](LICENSE).

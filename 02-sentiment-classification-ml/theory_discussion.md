	---CÂU 1: TF-IDF - Giải thích cách hoạt động và hiệu quả---

Cách TF-IDF hoạt động: TF-IDF (Term Frequency - Inverse Document Frequency) là phương pháp chuyển đổi văn bản thành vector số dựa trên hai thành phần:
1. TF (Term Frequency) - Tần suất từ:
TF(t, d) = (Số lần từ t xuất hiện trong văn bản d) / (Tổng số từ trong văn bản d)

Ví dụ cụ thể:
Văn bản: "giảng viên dạy hay dạy tốt"
TF("dạy") = 2/5 = 0.4
TF("hay") = 1/5 = 0.2

2. IDF (Inverse Document Frequency) - Độ quan trọng của từ:

IDF(t) = log(Tổng số văn bản / Số văn bản chứa từ t)

Ví dụ:
Tổng: 11,426 văn bản
Từ "tốt" xuất hiện trong 3,000 văn bản → IDF = log(11426/3000) ≈ 0.58
Từ "của" xuất hiện trong 10,000 văn bản → IDF = log(11426/10000) ≈ 0.06 (thấp vì quá phổ biến)

3. TF-IDF = TF × IDF
Tại sao TF-IDF hiệu quả trong phân loại văn bản?
A. Giảm trọng số từ phổ biến:
-Từ xuất hiện nhiều như "và", "của", "các" có IDF thấp → trọng số thấp.
-Điều này loại bỏ "nhiễu" từ stopwords tự động.

B. Tăng trọng số từ đặc trưng: Từ kết quả thực nghiệm (Top 10 từ quan trọng):
-Lớp 0 (Negative): "nên" (6.80), "cần" (5.83), "chưa" (5.51), "khó" (5.00)
-Lớp 2 (Positive): "rất" (4.54), "tốt" (4.50), "hay" (3.94), "dễ hiểu" (3.65)

→ Các từ này có coefficient cao vì chúng xuất hiện nhiều trong lớp đó nhưng ít ở các lớp khác.

C. Hiệu quả với n-gram:
Trong bài lab: ngram_range=(1, 2) → sử dụng cả unigram và bigram

Ví dụ bigram quan trọng:
"dễ hiểu" (Lớp 2): 3.65
"nhiệt tình" (Lớp 2): 2.82
"dung ôn" (Lớp 1): 2.31

→ Bigram nắm bắt ngữ cảnh tốt hơn từ đơn. "rất tốt" khác với "rất tệ" hoàn toàn!

D. Kết quả thực tế:
-Vocabulary: 5000 từ
-Vector shape: (11426, 5000) cho train, (3166, 5000) cho test
-Accuracy đạt 85.8% (LR) và 86.5% (SVM) chỉ với TF-IDF!

	--CÂU 2: Preprocessing - Tầm quan trọng trong NLP tiếng Việt--

Tại sao preprocessing quan trọng?
A. Tách từ (Word Segmentation) - ĐẶC BIỆT QUAN TRỌNG với tiếng Việt:
Vấn đề của tiếng Việt:
-Tiếng Việt là ngôn ngữ đơn lập, từ có thể gồm nhiều âm tiết.
-Không có dấu cách tự nhiên giữa các từ như tiếng Anh.

Ví dụ từ output:
Văn bản gốc: "slide giáo trình đầy đủ ."
Sau tách từ: "slide giáo trình đầy đủ"

Ví dụ khác (quan trọng hơn):
SAI: "học sinh" → ["học", "sinh"] (2 từ riêng biệt)
ĐÚNG: "học sinh" → ["học_sinh"] (1 từ ghép)

Ảnh hưởng đến kết quả:
-Nếu tách sai: "học" và "sinh" có thể xuất hiện ở nhiều ngữ cảnh khác nhau → TF-IDF không chính xác.
-Nếu tách đúng: "học_sinh" là một đơn vị ngữ nghĩa hoàn chỉnh → mô hình học đúng.


B. Loại bỏ nhiễu - Các bước preprocessing:
1. Chuyển chữ thường:
"Giảng viên dạy RẤT HAY" → "giảng viên dạy rất hay"
→ Tránh "Tốt" và "tốt" bị coi là 2 từ khác nhau

2. Loại bỏ URLs, emails, số:
"Email: abc@uit.edu.vn điểm 9.5" → "email điểm"
→ URLs, email, số không mang ý nghĩa cảm xúc

3. Loại bỏ dấu câu và ký tự đặc biệt:
"Thầy dạy HAY!!! Tuyệt vời..." → "thầy dạy hay tuyệt vời"
→ Dấu câu gây nhiễu cho TF-IDF

4. Loại bỏ khoảng trắng thừa:
"giảng   viên    dạy   hay" → "giảng viên dạy hay"

C. Tác động thực tế:
Từ dữ liệu thực nghiệm:
-Dataset ban đầu: 11,426 mẫu train
-Sau preprocessing: vocabulary giảm từ ~15,000 xuống 5,000 từ quan trọng
-Loại bỏ stopwords giúp tập trung vào từ mang nghĩa

Nếu KHÔNG preprocessing:
-Vocabulary phình to → overfitting
-Training chậm (nhiều chiều hơn)
-Accuracy giảm 5-10% (theo kinh nghiệm)

	--Câu 3: Model Comparison - Dựa trên kết quả thực nghiệm, mô hình nào hoạt động tốt hơn? Giải thích lý do.--

Kết quả thực nghiệm:
Metric | Logistic Regression | SVM | Chênh lệch
Accuracy | 0.8579 (85.79%) | 0.8654 (86.54%) | +0.75%
F1-Score (Macro) | 0.7167 (71.67%) | 0.7216 (72.16%) | +0.49%
AUC-ROC (Macro) | 0.9309 (93.09%) | 0.9322 (93.22%) | +0.13%
Thời gian Training | 0.5 giây | 83.1 giây | 166 lần chậm hơn

=>KẾT LUẬN: Logistic Regression là lựa chọn tốt hơn cho bài toán này

Phân tích chi tiết:
1. So sánh hiệu suất dự đoán:
A. Kết quả tổng quan:

SVM tốt hơn một chút ở cả 3 metrics
Nhưng chênh lệch rất nhỏ (< 1%)

B. Phân tích theo từng lớp (Classification Report):
Logistic Regression:
           precision    recall  f1-score   support
       0       0.87      0.89      0.88      1409
       1       0.28      0.49      0.36       167  ← Lớp khó nhất
       2       0.96      0.87      0.91      1590
SVM:
           precision    recall  f1-score   support
       0       0.87      0.91      0.89      1409  ← Cải thiện recall
       1       0.31      0.45      0.36       167  ← Cải thiện precision
       2       0.95      0.87      0.91      1590

Nhận xét:
-Lớp 1 (Neutral) là lớp khó nhất vì chỉ có 167 mẫu (4% dataset)
-SVM cải thiện nhẹ ở Lớp 0 và Lớp 1
-Cả 2 mô hình đều phân loại Lớp 2 (Positive) xuất sắc (F1 = 0.91)
-Lớp 1 vẫn rất yếu (F1 = 0.36) ở cả 2 mô hình → cần xử lý mất cân bằng


C. Phân tích Confusion Matrix:
Logistic Regression nhầm lẫn:
-Lớp 0 phân loại đúng: 1256/1409 (89.1%)
-Lớp 1 phân loại đúng: 81/167 (48.5%) ← Rất thấp
-Lớp 2 phân loại đúng: 1379/1590 (86.7%)

SVM cải thiện:
-Lớp 0 phân loại đúng: 1277/1409 (90.6%) → Tăng +21 mẫu
-Lớp 1 phân loại đúng: 75/167 (44.9%) → Giảm nhẹ
-Lớp 2 phân loại đúng: 1388/1590 (87.3%) → Tăng +9 mẫu

Điểm chính:
-SVM giảm lỗi nhầm Lớp 0 → Lớp 1 từ 119 xuống 97 (giảm 22 lỗi)
-Nhưng Lớp 1 vẫn rất yếu ở cả 2 mô hình


2. So sánh thời gian training:
Chênh lệch khổng lồ:
-Logistic Regression: 0.5 giây
-SVM: 83.1 giây (1 phút 23 giây)
-SVM chậm hơn: 166 lần

Phân tích Cost-Benefit:
Chi phí (Cost):
- SVM tốn thêm: 83.1s - 0.5s = 82.6 giây

Lợi ích (Benefit):
- SVM tốt hơn: +0.49% F1-Score

ROI (Return on Investment):
- Để được thêm 1% F1, cần: 82.6s / 0.49% ≈ 168.6 giây
- Hoặc: Mất 166 lần thời gian để được thêm 0.49% hiệu suất

Trong thực tế: Nếu cần hyperparameter tuning với GridSearchCV (100 combinations):
-Logistic Regression: 0.5s × 100 = 50 giây (~1 phút)
-SVM: 83.1s × 100 = 8,310 giây (~2.3 giờ)

→ LR cho phép thử nghiệm nhanh hơn 166 lần!

3. Lý do SVM tốt hơn một chút về hiệu suất:
A. Ưu điểm của SVM:
-Không gian cao chiều: TF-IDF tạo vector 5000 chiều → SVM hoạt động hiệu quả
-Tối đa hóa margin: SVM tìm siêu phẳng tối ưu phân tách các lớp
-Robust với outliers: SVM ít bị ảnh hưởng bởi dữ liệu nhiễu

B. Tại sao chênh lệch nhỏ:
-Dữ liệu gần như tuyến tính sau TF-IDF
-Logistic Regression với regularization (class_weight='balanced') đã xử lý tốt
-Cả 2 đều dùng linear kernel → tương đương nhau về mặt toán học


4. Ưu và nhược điểm tổng hợp:
Logistic Regression:
Ưu điểm:
-Training cực nhanh: 0.5s (quan trọng nhất!)
-Dễ giải thích: Có thể xem coefficients để hiểu từ nào quan trọng
-Dễ tune hyperparameters: GridSearch nhanh
-Scalable: Với dataset lớn (100K+ mẫu) vẫn nhanh
-Hiệu quả cao: F1 = 0.7167 với 0.5s → 1.43 F1/giây

Nhược điểm:
-Giả định tuyến tính (nhưng không phải vấn đề với TF-IDF)
-Accuracy thấp hơn SVM 0.75%


SVM:
Ưu điểm:
-Accuracy cao nhất: 86.54% (tốt nhất trong bài)
-F1-Score cao nhất: 0.7216
-Robust với outliers
-Hiệu quả trong không gian cao chiều

Nhược điểm:
-Cực kỳ chậm: 83.1s, chậm hơn LR 166 lần (vấn đề nghiêm trọng!)
-Khó giải thích: Không thể xem trọng số từ dễ dàng
-Không scalable: Dataset lớn hơn → thời gian tăng theo lũy thừa
-Hyperparameter tuning tốn thời gian: GridSearch sẽ mất hàng giờ
-Hiệu quả thấp: F1 = 0.7216 với 83.1s → 0.0087 F1/giây


5. Khuyến nghị sử dụng:
Chọn Logistic Regression khi:
-Làm việc trong môi trường production (cần retrain thường xuyên)
-Cần thử nghiệm nhiều (hyperparameter tuning, feature engineering)
-Dataset sẽ lớn hơn trong tương lai
-Cần giải thích cho stakeholders (business, medical)
-Thời gian training là yếu tố quan trọng
-Chênh lệch accuracy < 2% so với model phức tạp hơn

Chọn SVM khi:
-Chạy một lần và deploy lâu dài (không cần retrain)
-Cần accuracy tuyệt đối cao nhất (ví dụ: y tế, an ninh)
-Dataset nhỏ (< 10,000 mẫu)
-Có nhiều thời gian chờ đợi
-Không cần giải thích mô hình

	--CÂU 4: Metrics - Metric nào quan trọng nhất?--

Đáp án: F1-Score (Macro) là quan trọng nhất

Lý do dựa trên dữ liệu thực tế:
A. Dữ liệu MẤT CÂN BẰNG nghiêm trọng:
Từ output:
Lớp 0 (Negative): 5,325 mẫu (46.6%)
Lớp 1 (Neutral):    458 mẫu (4.0%)   ← RẤT ÍT!
Lớp 2 (Positive): 5,643 mẫu (49.4%)
→ Lớp 1 chỉ chiếm 4% → Accuracy không đủ tin cậy!

B. Tại sao Accuracy không đủ?
Ví dụ minh họa:
Giả sử một mô hình "ngu" dự đoán:
-Tất cả mẫu → Lớp 2 (Positive)

Kết quả:
-Accuracy = 49.4% (vì 49.4% thực sự là Lớp 2)
-Nhưng hoàn toàn sai cho Lớp 0 và Lớp 1!

Trong thực tế:
-LR Accuracy = 85.79%
-Nhưng F1 của Lớp 1 chỉ = 0.36 (kém!)

→ Accuracy che giấu hiệu suất kém ở lớp thiểu số

C. F1-Score cân bằng Precision và Recall:
Công thức:
F1 = 2 × (Precision × Recall) / (Precision + Recall)

Ý nghĩa:
-Precision: Trong số dự đoán Positive, bao nhiêu % đúng?
-Recall: Trong số thực tế Positive, bao nhiêu % được tìm ra?

Ví dụ từ Lớp 1 (Neutral) - LR:
Precision = 0.28 (Trong 100 dự đoán Neutral, chỉ 28 đúng)
Recall = 0.49 (Trong 100 Neutral thực tế, chỉ tìm ra 49)
F1 = 0.36 (Rất thấp!)
→ F1 "trừng phạt" mô hình khi không cân bằng precision/recall

D. Macro Averaging quan trọng:
Macro F1:
F1_macro = (F1_lớp0 + F1_lớp1 + F1_lớp2) / 3
         = (0.88 + 0.36 + 0.91) / 3
         = 0.72

Nếu dùng Weighted F1:
F1_weighted = (0.88×1409 + 0.36×167 + 0.91×1590) / 3166 = 0.87
→ Weighted che giấu hiệu suất kém của Lớp 1 vì nó chiếm ít!
→ Macro đảm bảo mọi lớp đều quan trọng bằng nhau

E. AUC-ROC có giá trị nhưng khó diễn giải:
Kết quả:
-LR AUC = 0.9309
-SVM AUC = 0.9322

Ưu điểm:
-Đo khả năng phân biệt các lớp ở mọi ngưỡng
-ROC curve cho thấy: Lớp 0 và 2 có AUC = 0.96-0.97 (xuất sắc)
-Lớp 1 có AUC = 0.86 (tốt nhưng thấp hơn)

Nhược điểm:
-Với multi-class, AUC phức tạp hơn
-Khó so sánh trực tiếp giữa các lớp
-Trong production, người dùng quan tâm đến "dự đoán đúng bao nhiêu %" (F1) hơn là AUC


Kết luận thứ tự ưu tiên:
1. F1-Score (Macro) - QUAN TRỌNG NHẤT

Lý do: Cân bằng precision/recall, công bằng cho mọi lớp, phản ánh đúng hiệu suất với dữ liệu mất cân bằng

2. AUC-ROC - BỔ SUNG

Lý do: Đánh giá khả năng phân biệt tổng thể, hữu ích để kiểm tra mô hình có học được pattern không

3. Accuracy - THAM KHẢO

Lý do: Dễ hiểu, nhưng gây hiểu lầm với dữ liệu mất cân bằng

	--CÂU 5: Improvement - 3 cách cải thiện hiệu suất--

Dựa trên phân tích kết quả, đây là 3 đề xuất cải thiện được xếp hạng theo mức độ ưu tiên:
Cải thiện 1: Xử lý dữ liệu mất cân bằng với SMOTE + Class Weights
Cải thiện 2: Hyperparameter Tuning với GridSearchCV
Cải thiện 3: Sử dụng Word Embeddings (PhoBERT)
============================================================
CÂU HỎI THẢO LUẬN - THỰC HÀNH 3: WORD2VEC (SKIP-GRAM & CBOW)
============================================================

1. Phân biệt kiến trúc CBOW (Continuous Bag of Words) và Skip-gram trong Word2Vec. Khi nào nên dùng kiến trúc nào?

2. Tại sao file dữ liệu tiếng Việt (datatrain.txt) lại có các từ nối bằng dấu gạch dưới "_" (như "đơn_giản", "lãnh_đạo")? Điều này giúp ích gì cho mô hình?

3. Ý nghĩa của các siêu tham số chính: vector_size, window, min_count, sg, negative trong gensim Word2Vec?

4. Làm thế nào để giải quyết vấn đề từ chưa biết (Out-Of-Vocabulary - OOV) khi sử dụng Word2Vec? So sánh với FastText và BERT?

5. Giải thích cơ chế Negative Sampling (Lấy mẫu âm) và tại sao nó giúp tăng tốc độ huấn luyện so với Hierarchical Softmax?


============================================================
TRẢ LỜI CÂU HỎI THẢO LUẬN
============================================================

--- Câu 1: So sánh CBOW và Skip-gram ---

1. KIẾN TRÚC & CƠ CHẾ HOẠT ĐỘNG:
- CBOW (Continuous Bag of Words):
  + Mục tiêu: Dự đoán từ mục tiêu (target word) dựa vào ngữ cảnh xung quanh (context words).
  + Cơ chế: Trung bình cộng các vector của các từ ngữ cảnh -> đưa qua ma trận trọng số -> dự đoán xác suất từ mục tiêu.
  + Công thức: P(w_t | w_{t-c}, ..., w_{t+c})

- Skip-gram:
  + Mục tiêu: Dự đoán các từ ngữ cảnh (context words) dựa vào từ mục tiêu (target word).
  + Cơ chế: Lấy vector của từ mục tiêu -> tính xác suất xuất hiện của từng từ ngữ cảnh trong cửa sổ xung quanh.
  + Công thức: P(w_{t+j} | w_t) với -c <= j <= c, j != 0

2. KHI NÀO NÊN DÙNG MỖI MÔ HÌNH:
✓ Dùng CBOW khi:
  - Tập dữ liệu lớn, cần tốc độ huấn luyện nhanh (nhanh gấp 3-4 lần Skip-gram).
  - Tần suất các từ phân bố đều, tập trung vào việc học biểu diễn cho các từ xuất hiện phổ biến.
  - Phù hợp với ngữ nghĩa cú pháp (syntactic).

✓ Dùng Skip-gram khi:
  - Tập dữ liệu nhỏ hoặc vừa, hoặc có nhiều từ hiếm (rare words).
  - Cần độ chính xác ngữ nghĩa cao (semantic representation tốt hơn).
  - Mỗi cặp (từ mục tiêu, từ ngữ cảnh) được xem là một mẫu huấn luyện riêng biệt, giúp từ hiếm được cập nhật trọng số nhiều lần hơn.


--- Câu 2: Tách từ ghép trong tiếng Việt với dấu gạch dưới "_" ---

1. ĐẶC ĐIỂM TIẾNG VIỆT:
- Tiếng Việt là ngôn ngữ đơn lập. Ranh giới giữa các từ không trùng với khoảng trắng như tiếng Anh.
  Ví dụ: "Học sinh học sinh học"
  -> Phân tách đúng: "Học_sinh" (danh từ) "học" (động từ) "sinh_học" (danh từ).
- Nếu không nối dấu "_", mô hình coi "học" và "sinh" là 2 từ độc lập:
  + "học" sẽ mang ý nghĩa chung chung (học tập).
  + "sinh" sẽ mang nghĩa sinh sôi/học sinh/sinh viên -> gây nhiễu ngữ nghĩa nghiêm trọng.

2. TÁC ĐỘNG ĐẾN WORD2VEC:
- Gán dấu `_` biến từ ghép/từ phức thành 1 token duy nhất trong từ điển (`vocab`).
- Giúp Word2Vec học đúng vector biểu diễn cho toàn bộ thực thể khái niệm ("chủ_nghĩa_xã_hội", "tổng_thống", "thực_tế") thay vì biểu diễn rời rạc từng âm tiết.
- Tối ưu hóa bộ nhớ và tăng độ tương đồng ngữ nghĩa khi tìm kiếm từ khóa liên quan.


--- Câu 3: Ý nghĩa các siêu tham số trong gensim Word2Vec ---

- `vector_size` (mặc định 100):
  + Số chiều của vector nhúng (embedding dimension).
  + Giá trị lớn (200-300) lưu giữ được nhiều đặc trưng ngữ nghĩa phức tạp hơn nhưng tốn RAM và dễ overfitting nếu tập dữ liệu nhỏ.
  + Giá trị nhỏ (50-100) phù hợp cho các bài toán nhẹ hoặc tập dữ liệu giới hạn.

- `window` (mặc định 5):
  + Khoảng cách tối đa giữa từ mục tiêu và từ ngữ cảnh trong một câu.
  + Window nhỏ (2-3): Học quan hệ cú pháp và từ loại (functional/syntactic).
  + Window lớn (5-10): Học ngữ nghĩa chủ đề rộng (topical/semantic).

- `min_count` (mặc định 5):
  + Tần suất xuất hiện tối thiểu để một từ được đưa vào từ điển.
  + Giúp lọc bỏ lỗi chính tả, từ hiếm, giảm kích thước ma trận và tránh nhiễu.

- `sg` (Skip-gram flag):
  + `sg=1`: Huấn luyện mô hình Skip-gram.
  + `sg=0`: Huấn luyện mô hình CBOW.

- `negative` (mặc định 5):
  + Số lượng từ âm (negative words) được lấy mẫu ngẫu nhiên cho mỗi từ dương trong kỹ thuật Negative Sampling.
  + Tập dữ liệu lớn dùng 2-5, tập dữ liệu nhỏ dùng 5-20.

- `workers`: Số luồng CPU chạy song song bằng Cython C backend.


--- Câu 4: Xử lý vấn đề Out-Of-Vocabulary (OOV) ---

1. HẠN CHẾ CỦA WORD2VEC:
- Word2Vec gán một vector cố định cho từng từ trong từ điển. Nếu gặp từ mới chưa từng xuất hiện trong quá trình train (OOV), mô hình hoàn toàn bất lực (báo lỗi KeyError hoặc phải gán vector ngẫu nhiên/zero).

2. GIẢI PHÁP & SO SÁNH VỚI MÔ HÌNH HIỆN ĐẠI:
- **FastText (Facebook)**:
  + Biểu diễn từ dưới dạng túi các n-gram ký tự (subword n-grams).
  + Ví dụ: từ "máy_bay" có thể ghép từ các subword "<má", "áy_", "bay>".
  + Khi gặp từ OOV, FastText tính tổng vector các subwords cấu thành nên vẫn suy diễn được vector ngữ nghĩa hợp lý. Rất mạnh với từ ghép tiếng Việt và từ sai chính tả.
- **BERT / RoBERTa / PhoBERT**:
  + Sử dụng thuật toán Byte-Pair Encoding (BPE) hoặc WordPiece.
  + Không dùng vector tĩnh (static embedding) mà dùng contextual embedding: cùng một từ nhưng ở ngữ cảnh khác nhau sẽ có vector biểu diễn khác nhau.


--- Câu 5: Cơ chế Negative Sampling (Lấy mẫu âm) ---

1. VẤN ĐỀ CỦA SOFTMAX CHUẨN:
- Hàm Softmax thông thường yêu cầu tính tổng lũy thừa trên TOÀN BỘ từ vựng:
  P(w_O | w_I) = exp(v'_{w_O}^T v_{w_I}) / \sum_{w \in V} exp(v'_w^T v_{w_I})
- Với từ vựng kích thước |V| = 100,000 từ, mỗi lượt cập nhật trọng số tốn chi phí tính toán O(|V|), làm việc huấn luyện chậm và bất khả thi trên quy mô lớn.

2. CƠ CHẾ NEGATIVE SAMPLING:
- Chuyển bài toán dự đoán đa lớp (multi-class classification) thành nhiều bài toán hồi quy logistic nhị phân (binary logistic regression).
- Với mỗi cặp từ đúng (positive pair: target - context), mô hình chỉ lấy ngẫu nhiên `k` từ âm tính (negative words) không xuất hiện trong ngữ cảnh.
- Hàm mục tiêu tối ưu hóa:
  log \sigma(v'_{w_O}^T v_{w_I}) + \sum_{i=1}^k E_{w_i ~ P_n(w)} [log \sigma(-v'_{w_i}^T v_{w_I})]
- Chi phí tính toán giảm từ O(|V|) xuống O(k), với k chỉ từ 5-20, giúp tăng tốc độ huấn luyện lên hàng trăm lần.
============================================================

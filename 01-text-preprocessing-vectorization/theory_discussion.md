============================================================
CÂU HỎI THẢO LUẬN
============================================================

1. So sánh sự khác biệt giữa BoW và TF-IDF. Khi nào nên sử dụng mỗi phương pháp?

2. Tại sao việc tách từ tiếng Việt lại quan trọng? So sánh với tiếng Anh?

3. Ảnh hưởng của việc loại bỏ từ dừng đến kết quả phân loại văn bản?

4. Hạn chế của BoW và TF-IDF là gì? Có phương pháp nào tốt hơn không?

5. Làm thế nào để cải thiện danh sách từ dừng tiếng Việt?


============================================================
TRẢ LỜI CÂU HỎI THẢO LUẬN
============================================================

--- Câu 1: So sánh BoW và TF-IDF ---

BAG OF WORDS (BoW):
- Đếm số lần từ xuất hiện trong văn bản
- Từ xuất hiện nhiều → giá trị cao
- Ví dụ: "sản_phẩm" xuất hiện 3 lần → BoW = 3

TF-IDF:
- Xem xét cả tần suất TRONG văn bản và GIỮA các văn bản
- Từ xuất hiện nhiều trong 1 văn bản NHƯNG ít ở văn bản khác → giá trị cao
- Ví dụ: "nhiệt_tình" chỉ có ở văn bản 2 → TF-IDF cao

KHI NÀO DÙNG:
✓ Dùng BoW khi:
  - Bài toán đơn giản
  - Phân loại spam/không spam
  - Tốc độ xử lý quan trọng
  - Dataset nhỏ

✓ Dùng TF-IDF khi:
  - Muốn tìm từ đặc trưng
  - Tìm kiếm thông tin (search engine)
  - Phân loại văn bản phức tạp
  - Tóm tắt văn bản


--- Câu 2: Tầm quan trọng của tách từ tiếng Việt ---

TIẾNG VIỆT:
- Không có khoảng trắng giữa các từ ghép
- Ví dụ: "Sản phẩm" = 2 tiếng nhưng là 1 từ
- Nếu không tách: "Sản" và "phẩm" = 2 từ riêng biệt → SAI
- Sau tách: "Sản_phẩm" = 1 từ → ĐÚNG

TIẾNG ANH:
- Có khoảng trắng rõ ràng
- "good product" = 2 từ riêng biệt
- Không cần tách từ, chỉ cần split()

TẠI SAO QUAN TRỌNG:
1. Ý nghĩa: "học sinh" ≠ "học" + "sinh"
2. Độ chính xác: Phân loại đúng hơn
3. Giảm nhiễu: Tránh hiểu sai nghĩa
4. Kích thước từ điển: Nhỏ gọn hơn

VÍ DỤ:
- Không tách: "chất", "lượng", "cao" (3 từ)
- Có tách: "chất_lượng", "cao" (2 từ) → Đúng nghĩa


--- Câu 3: Ảnh hưởng của loại bỏ từ dừng ---

TÁC ĐỘNG TÍCH CỰC:
✓ Giảm kích thước từ điển:
  - Trước: 100 từ → Sau: 60 từ
  - Tiết kiệm bộ nhớ và thời gian

✓ Tăng độ chính xác:
  - Loại bỏ từ không mang nghĩa: "là", "của", "và"
  - Giữ lại từ quan trọng: "tốt", "tệ", "nhiệt_tình"

✓ Cải thiện TF-IDF:
  - Từ dừng xuất hiện ở MỌI văn bản → IDF thấp
  - Loại bỏ → Tập trung vào từ đặc trưng

TÁC ĐỘNG TIÊU CỰC:
✗ Mất thông tin ngữ cảnh:
  - "không tốt" → loại "không" → "tốt" → SAI nghĩa!
  
✗ Cần danh sách phù hợp:
  - Quá nhiều → Mất từ quan trọng
  - Quá ít → Vẫn nhiều từ vô nghĩa

KẾT LUẬN:
- Nên loại từ dừng cho hầu hết bài toán
- Cẩn thận với phân tích cảm xúc (giữ "không", "chưa")


--- Câu 4: Hạn chế của BoW và TF-IDF ---

HẠN CHẾ CHUNG:
1. Mất thứ tự từ:
   - "không tốt" ≈ "tốt không" → SAI
   - Không hiểu ngữ cảnh

2. Mất nghĩa từ:
   - "bank" = ngân hàng hay bờ sông?
   - Không phân biệt được

3. Không hiểu cú pháp:
   - Chỉ đếm từ, không hiểu câu
   
4. Kích thước vector lớn:
   - 10,000 từ → vector 10,000 chiều
   - Phần lớn là số 0 (sparse)

5. Không nắm bắt quan hệ từ:
   - "tốt" và "xuất sắc" tương tự nhưng BoW/TF-IDF coi là khác nhau

PHƯƠNG PHÁP TỐT HƠN:
✓ Word2Vec / GloVe:
  - Vector dày đặc (dense vector)
  - Nắm bắt nghĩa từ tương tự
  - "tốt" gần "xuất_sắc" trong không gian vector

✓ BERT / Transformer:
  - Hiểu ngữ cảnh
  - Biết "bank" nghĩa gì trong câu
  - State-of-the-art hiện nay

✓ N-grams:
  - Giữ lại 2-3 từ liên tiếp
  - "không tốt" thành 1 đơn vị

✓ FastText:
  - Xử lý từ mới tốt hơn
  - Hiểu cấu trúc từ


--- Câu 5: Cải thiện danh sách từ dừng tiếng Việt ---

CÁCH CẢI THIỆN:

1. THU THẬP TỪ NHIỀU NGUỒN:
   - Wikipedia tiếng Việt
   - Báo điện tử
   - Sách giáo khoa
   → Tìm từ xuất hiện > 90% văn bản

2. PHÂN TÍCH TẦN SUẤT:
   - Đếm tần suất từ trong corpus lớn
   - Từ xuất hiện quá nhiều → từ dừng
   - Code mẫu:
   
   word_freq = {}
   for doc in all_documents:
       for word in doc.split():
           word_freq[word] = word_freq.get(word, 0) + 1
   
   # Lấy top 100 từ phổ biến nhất
   stopwords_auto = sorted(word_freq.items(), 
                           key=lambda x: x[1], 
                           reverse=True)[:100]

3. PHÂN LOẠI THEO LOẠI TỪ:
   - Đại từ: tôi, bạn, họ, nó
   - Giới từ: ở, tại, trong, ngoài
   - Liên từ: và, hoặc, nhưng
   - Trợ từ: đã, đang, sẽ, có, được

4. TÙY CHỈNH THEO BÀI TOÁN:
   - Phân tích cảm xúc: GIỮ "không", "chưa", "chẳng"
   - Phân loại chủ đề: Có thể loại bỏ nhiều hơn
   
5. SỬ DỤNG THƯ VIỆN CÓ SẴN:
   from underthesea import stopwords
   vn_stopwords = stopwords.load()

6. CẬP NHẬT ĐỊNH KỲ:
   - Ngôn ngữ thay đổi theo thời gian
   - Từ lóng mới xuất hiện
   - Kiểm tra và cập nhật 6 tháng/lần

7. ĐÁNH GIÁ HIỆU QUẢ:
   - Chạy thử nghiệm với/không có từ dừng
   - So sánh độ chính xác
   - Chọn danh sách tối ưu

VÍ DỤ DANH SÁCH CẢI THIỆN:
- Thêm: "ạ", "ơi", "nhỉ", "nhé", "đấy", "mà"
- Loại khỏi stopwords (cho cảm xúc): "không", "chưa", "chẳng"
- Thêm từ mới: "oke", "ok", "gòi"

### Bài 1: Khôi phục Masked Token (Masked Language Modeling)

[cite_start]**Tác vụ:** Dự đoán token `<mask>` trong câu: *"Hanoi is the <mask> of Vietnam."* [cite: 40, 47]

1.  **Mô hình đã dự đoán đúng từ *capital* không?**
    * **Trả lời:** **Có.** Kết quả đầu ra cho thấy mô hình đã dự đoán **' capital'** với độ tin cậy rất cao (0.9341), đây là từ chính xác nhất về mặt ngữ nghĩa và ngữ pháp.

2.  **Tại sao các mô hình Encoder-only như BERT lại phù hợp cho tác vụ này?**
    * [cite_start]**Trả lời:** Các mô hình **Encoder-only** (ví dụ: BERT, ROBERTa) [cite: 19] [cite_start]được thiết kế để hiểu **ngữ cảnh hai chiều (bidirectional)**[cite: 21].
        * [cite_start]Chúng có khả năng xem xét cả các từ đứng trước và sau từ bị che (`<mask>`) [cite: 21] [cite_start]để tạo ra một biểu diễn ngữ cảnh giàu thông tin[cite: 13, 15], giúp đưa ra dự đoán chính xác nhất. 

[Image of BERT architecture and masked language modeling]

        * [cite_start]Tác vụ này (Masked Language Modeling - MLM) là tác vụ huấn luyện cốt lõi của chúng[cite: 22].

---

### Bài 2: Dự đoán từ tiếp theo (Next Token Prediction)

[cite_start]**Tác vụ:** Sinh ra phần tiếp theo cho câu mồi: *"The best thing about learning NLP is"* [cite: 60, 67]

1.  **Kết quả sinh ra có hợp lý không?**
    * [cite_start]**Trả lời:** **Có,** kết quả sinh ra bởi các mô hình Decoder-only (ví dụ: GPT) [cite: 25] nhìn chung là **hợp lý** và **mạch lạc**. Các đoạn văn bản tiếp tục ý tưởng ban đầu về lợi ích của việc học NLP.

2.  **Tại sao các mô hình Decoder-only như GPT lại phù hợp cho tác vụ này?**
    * [cite_start]**Trả lời:** Các mô hình **Decoder-only** (ví dụ: GPT) [cite: 25] [cite_start]được huấn luyện với mục tiêu là **dự đoán từ tiếp theo (Next Token Prediction)**[cite: 28].
        * [cite_start]Chúng hoạt động theo cơ chế **một chiều (unidirectional)** [cite: 27][cite_start], có nghĩa là chúng chỉ xem xét các từ đã xuất hiện trước đó để tạo ra token mới[cite: 27].
        * [cite_start]Cơ chế này là lý tưởng cho việc **Sinh văn bản (text generation)**[cite: 28]. 

---

### Bài 3: Tính toán Vector biểu diễn của câu (Sentence Representation)

[cite_start]**Tác vụ:** Tính toán vector biểu diễn cho câu *"This is a sample sentence."* bằng phương pháp **Mean Pooling** trên mô hình `bert-base-uncased`. [cite: 84, 91]

1.  **Kích thước (chiều) của vector biểu diễn là bao nhiêu? Con số này tương ứng với tham số nào của mô hình BERT?**
    * **Kết quả từ code:** Kích thước của vector là **`torch.Size([1, 768])`**.
    * **Trả lời:** Kích thước (chiều) của vector biểu diễn là **768**. Con số này tương ứng với tham số **`hidden_size`** (hoặc kích thước ẩn) của mô hình BERT. Đây là chiều dài cố định của mỗi vector token được sinh ra.

2.  **Tại sao chúng ta cần sử dụng `attention_mask` khi thực hiện Mean Pooling?**
    * **Trả lời:** Cần sử dụng `attention_mask` để đảm bảo rằng **chỉ các vector của các token thực sự có nghĩa trong câu mới được tính trung bình**.
        * Khi token hóa, các câu ngắn hơn được **đệm (padding)** để có cùng độ dài (dài bằng câu dài nhất trong batch).
        * Nếu tính trung bình trên toàn bộ vector đầu ra, các **vector của token padding** (vốn không mang thông tin ngữ nghĩa) sẽ làm sai lệch vector biểu diễn ngữ nghĩa của câu thực tế.
        * `attention_mask` giúp **loại bỏ (mask out)** ảnh hưởng của các token padding này khỏi phép tính tổng (`sum_embeddings`) và đếm số lượng token thực (`sum_mask`).
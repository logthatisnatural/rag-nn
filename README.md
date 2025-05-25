Hệ Thống NLP Đầu-Cuối cho Tra Cứu Thông Tin VNU
Tổng Quan
Dự án này tập trung xây dựng một hệ thống NLP đầu-cuối sử dụng Retrieval-Augmented Generation (RAG) để giải quyết bài toán trả lời câu hỏi thực tế (factual question answering) liên quan đến Đại học Quốc gia Hà Nội (VNU). Hệ thống thu thập dữ liệu từ nhiều nguồn, xử lý thành cơ sở dữ liệu vector và trả lời chính xác các câu hỏi của người dùng về VNU.
Mục Lục

Tạo Bộ Dữ Liệu
Chi Tiết Mô Hình
Kết Quả
Phân Tích & Cải Tiến
Hướng Dẫn Sử Dụng
Phát Triển Trong Tương Lai

Tạo Bộ Dữ Liệu
1. Thông Tin Tổng Quan về VNU

Nguồn: Wikipedia API (trang VNU)
Nội Dung: Thông tin tổng quan, lịch sử, cơ cấu tổ chức của VNU.
Kết Quả: 170 tệp văn bản được trích xuất từ Wikipedia.
Chú Thích: Tạo 30 cặp câu hỏi-trả lời (QA pairs).

2. Dữ Liệu Chính Thức của VNU

Nguồn: Trang web chính thức của VNU, sử dụng BeautifulSoup.
Nội Dung: Thông tin về VNU (trang chính, subpage như "about", "schedule", "history") và các trường thành viên (USSH, ULIS, UET, HUS).
Kết Quả: 470 tệp văn bản được lưu dưới dạng JSON.
Chú Thích: Tạo 40 cặp QA.

3. Quy Chế Tuyển Sinh của VNU

Nguồn: Tài liệu PDF từ VNU và các trường thành viên, sử dụng pdfplumber.
Nội Dung: Quy chế tuyển sinh, cơ chế hoạt động, chương trình đào tạo.
Kết Quả: 570 tệp văn bản được trích xuất.
Chú Thích: Tạo 50 cặp QA.

Chi Tiết Mô Hình
1. Document Query Embedder

Sử dụng mô hình sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2 (dựa trên SBERT).
Cấu Trúc: Transformer (BertModel), độ dài tối đa 128 token, vector 384 chiều.
Pooling: MEAN pooling để tạo vector cố định.
Hỗ trợ 50 ngôn ngữ, bao gồm tiếng Việt, với 118 triệu tham số, phù hợp cho dữ liệu VNU đa ngôn ngữ.

2. Document Retriever

Sử dụng Chroma DB để lưu trữ và truy xuất vector hiệu quả.
Thiết lập: Truy xuất 2 tài liệu tương đồng nhất từ cơ sở dữ liệu (gồm thông tin VNU và cặp QA).

3. Document Reader

Các mô hình thử nghiệm: 
facebook/dpr-reader-multiset-base (extractive QA).
google/flan-t5-large (generative QA).
microsoft/deberta-v2-xlarge (extractive QA, 900M tham số).


Chia dữ liệu: 80-20 (968 cặp huấn luyện, 242 cặp kiểm thử).
Sử dụng cả zero-shot và few-shot prompting (3 ví dụ).

Kết Quả

Zero-shot Prompt:
Không RAG: Flan-T5-large đạt EM=0.029, F1=0.067.
Dùng cả hai cơ sở dữ liệu: Flan-T5-large đạt EM=0.082, F1=0.164.


Few-shot Prompt (3 ví dụ):
Không RAG: Flan-T5-large đạt EM=0.038, F1=0.087.
Dùng cả hai cơ sở dữ liệu: Flan-T5-large đạt EM=0.097, F1=0.194.


Flan-T5-large có hiệu suất cao nhất nhờ khả năng sinh câu trả lời linh hoạt.

Phân Tích & Cải Tiến
Thách Thức

Hiệu suất thấp khi không dùng RAG do thiếu thông tin cụ thể về VNU.
Truy xuất chưa tối ưu cho dữ liệu đa ngôn ngữ (tiếng Việt và tiếng Anh).

Cải Tiến Đề Xuất

Tăng cường cơ sở dữ liệu với thông tin chi tiết hơn về các trường thành viên.
Tối ưu mô hình truy xuất để xử lý tốt hơn dữ liệu đa ngôn ngữ.

Hướng Dẫn Sử Dụng

Chạy File Jupyter Notebook:
Mở và chạy file crawler.ipynb để thu thập, xử lý dữ liệu và triển khai hệ thống.



Phát Triển Trong Tương Lai

Mở Rộng Dữ Liệu: Thu thập thêm thông tin từ các nguồn khác như tin tức, sự kiện.
Cải Thiện Mô Hình: Tinh chỉnh mô hình cho dữ liệu tiếng Việt.
Tối Ưu Hiệu Suất: Sử dụng GPU mạnh hơn để tăng tốc độ xử lý.


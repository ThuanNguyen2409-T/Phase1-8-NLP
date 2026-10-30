# Kế hoạch đồ án: Chatbot tóm tắt văn bản tiếng Việt

Ngày lập: 30/10/2026. Trạng thái: **lập kế hoạch, chưa triển khai mã nguồn**.

Mục tiêu: xây dựng chatbot nhận văn bản dài, tạo bản tóm tắt theo ba mức Ngắn/Vừa/Chi tiết, hiển thị thống kê và đánh giá chất lượng có căn cứ.

## Định hướng đề xuất

- Ưu tiên văn bản tiếng Việt, trước hết là tin tức và bài viết thông tin. Khả năng với văn bản chuyên ngành cần đánh giá riêng.
- Xây dựng cả **TextRank** (trích xuất) và **Transformer** (trừu tượng) để có thực nghiệm so sánh. Chế độ kết hợp chỉ bổ sung khi có thời gian.
- Dùng checkpoint đã tinh chỉnh cho tóm tắt trước; việc tự tinh chỉnh thêm là nhánh nghiên cứu tùy chọn sau khi có hệ thống và số liệu nền.
- Kiến trúc: Streamlit → FastAPI → tiền xử lý → bộ tóm tắt → thống kê; SQLite hỗ trợ lịch sử nếu triển khai chức năng này.
- Đánh giá gồm ROUGE-1/2/L, tốc độ, mức rút gọn và chấm thủ công. Không dùng tỷ lệ rút gọn để thay thế đánh giá chất lượng.
- Ba ảnh người dùng cung cấp là tài liệu tham khảo kiến trúc. Các tên OpenAI API, Hamilton, Longformer hoặc cấu hình huấn luyện trong ảnh không phải yêu cầu bắt buộc.

## Danh sách phase

| Phase | Nội dung | Đầu ra chính | Phụ thuộc | Ước lượng ngày làm việc |
| --- | --- | --- | --- | --- |
| [1](phase1.md) | Yêu cầu, phạm vi, kiến trúc | Checklist, thiết kế, tiêu chí nghiệm thu | Không | 2–3 |
| [2](phase2.md) | Môi trường, dữ liệu, tiền xử lý | Môi trường tái lập, tập dữ liệu, quy tắc xử lý | 1 | 3–4 |
| [3](phase3.md) | Tóm tắt trích xuất | Lead baseline, TextRank, điều khiển độ dài | 2 | 2–3 |
| [4](phase4.md) | Transformer và văn bản dài | Suy luận, chia phần, kiểm tra kết quả | 2; dùng 3 để đối chiếu | 4–6 |
| [5](phase5.md) | Backend và lưu trữ | API, hợp đồng dữ liệu, SQLite tùy chọn | 3, 4 | 3–4 |
| [6](phase6.md) | Giao diện chatbot | Luồng sử dụng hoàn chỉnh và thống kê | 5 | 2–3 |
| [7](phase7.md) | Đánh giá và tinh chỉnh tùy chọn | Bảng thực nghiệm, phiếu đánh giá, phân tích lỗi | 3–6 | 4–6 |
| [8](phase8.md) | Mở rộng, hoàn thiện, báo cáo | Bản bàn giao, tài liệu, demo | 6, 7 | 3–4 |

Ước lượng tổng: **23–33 ngày làm việc**, chưa gồm thời gian gán nhãn lớn, tự tinh chỉnh mô hình hoặc chức năng mở rộng. Đây là khung dự kiến cho một người, cần điều chỉnh theo lịch nộp và phần cứng. Có thể kiểm thử giao diện bằng bộ xử lý giả trong lúc phát triển mô hình.

## Mốc bàn giao

- **M1 — sau phase 2:** chốt thiết kế, dữ liệu và môi trường.
- **M2 — sau phase 4:** hai phương pháp chạy độc lập, có cơ chế xử lý văn bản dài.
- **M3 — sau phase 6:** demo hoàn chỉnh bốn chức năng chính của người dùng.
- **M4 — sau phase 7:** có kết quả định lượng và đánh giá con người.
- **M5 — sau phase 8:** có mã nguồn, báo cáo, hướng dẫn chạy và kịch bản bảo vệ.

Các checkbox trong từng phase là **công việc dự kiến**; chỉ đánh dấu hoàn thành khi có sản phẩm và bằng chứng kiểm tra tương ứng.

## Tài liệu kỹ thuật đã đối chiếu

1. [Checkpoint ViT5 tóm tắt Vietnews](https://huggingface.co/VietAI/vit5-base-vietnews-summarization): ứng viên cho nhánh tóm tắt trừu tượng tiếng Việt.
2. [Mã nguồn ViT5 của VietAI](https://github.com/VietAI/ViT5): tham khảo huấn luyện và đánh giá; kiểm tra quyền sử dụng dữ liệu trước khi tải.
3. [Hướng dẫn tóm tắt của Transformers](https://huggingface.co/docs/transformers/main/tasks/summarization): tham khảo tokenization, suy luận và huấn luyện.
4. [Padding và truncation](https://huggingface.co/docs/transformers/main/pad_truncation): phân biệt đệm, cắt và kiểm soát số token.
5. [Tokenizer của rouge-score](https://github.com/google-research/google-research/blob/master/rouge/tokenize.py): cần thay tokenizer mặc định khi chấm tiếng Việt có dấu.

Tài liệu trực tuyến được đối chiếu khi lập kế hoạch; phiên bản thư viện và revision mô hình sẽ được khóa sau phép thử tương thích ở phase 2–4.

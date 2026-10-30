# Phase 7 — Đánh giá chất lượng và tinh chỉnh tùy chọn

Thời gian dự kiến: 4–6 ngày cho đánh giá; tinh chỉnh cần ước lượng riêng theo GPU/dữ liệu. Phụ thuộc: [phase 3](phase3.md) đến [phase 6](phase6.md).

## Mục tiêu

Trả lời bằng số liệu: phương pháp nào giữ ý tốt hơn, mất bao lâu, sai ở đâu và kiểm soát độ dài đến mức nào.

## 1. Thiết kế thực nghiệm

- [ ] Chốt tập test và cấu hình từ validation trước khi chạy đánh giá cuối; không chọn tham số theo điểm test.
- [ ] So sánh Lead baseline, TextRank, Transformer có sẵn trên cùng mẫu; thêm mô hình tinh chỉnh/chế độ kết hợp nếu thực sự triển khai.
- [ ] Chạy ba mức dài và phân nhóm độ dài nguồn/chủ đề; ghi số lượng mẫu mỗi nhóm.
- [ ] Với văn bản dài, có thí nghiệm riêng về chia phần, mất ý ở cuối và lặp thông tin; giữ cùng giao thức đánh giá.
- [ ] Cố định cách chuẩn hóa, tokenizer đánh giá và seed; lưu ID dữ liệu, revision model, tham số sinh, thư viện và thiết bị.
- [ ] Tách thời gian khởi động/tải model với thời gian chạy đã làm nóng; báo trung vị và p95 theo số mẫu thực tế, không chỉ một ví dụ nhanh nhất.
- [ ] Không loại lỗi khỏi bảng âm thầm: báo số request lỗi, tỷ lệ thành công và phạm vi mẫu dùng tính từng metric.
- [ ] Kiểm tra nguồn tập test có thể đã được dùng để huấn luyện checkpoint; nếu không xác minh được, công bố hạn chế và bổ sung mẫu ngoài nguồn có tham chiếu thủ công.

## 2. ROUGE phù hợp tiếng Việt

- [ ] Tính precision/recall/F1 cho ROUGE-1, ROUGE-2, ROUGE-L; chọn F1 làm cột chính và công bố cách gộp trung bình theo mẫu.
- [ ] Chốt rõ đánh giá theo từ đã tách hoặc âm tiết/đơn vị cách trắng; áp dụng giống nhau cho mọi dự đoán và tham chiếu.
- [ ] Ưu tiên tokenizer tùy chỉnh bảo toàn Unicode, có thể dùng underthesea để tách từ; không dùng English stemming.
- [ ] Không chỉ truyền văn bản tiếng Việt đã tách vào tokenizer mặc định của rouge-score: bước lọc mặc định dựa trên `[a-z0-9]` có thể làm sai token có dấu. Cần truyền tokenizer tùy chỉnh trực tiếp.
- [ ] Kiểm chứng bộ chấm bằng cặp tiếng Việt giống hệt nhau có điểm gần 1 và cặp ít liên quan có điểm thấp; kiểm tra cả tên riêng và chữ “đ”.
- [ ] Ghi quy tắc xử lý đầu ra rỗng, mẫu lỗi và nhiều tham chiếu nếu có; xác định dùng ROUGE-L, không gộp lẫn ROUGE-Lsum.
- [ ] Báo điểm trên thang 0–1 hoặc 0–100 nhất quán. Chỉ so với bài báo khác khi tokenizer, split và cách chấm tương thích.

Nguồn kỹ thuật: [mã tokenizer rouge-score của Google Research](https://github.com/google-research/google-research/blob/master/rouge/tokenize.py), [giao diện tokenizer](https://github.com/google-research/google-research/blob/master/rouge/tokenizers.py).

## 3. Đánh giá con người

- [ ] Chọn dự kiến 30 mẫu đa dạng từ test và ít nhất hai người chấm độc lập nếu điều kiện cho phép; công bố số lượng thực tế.
- [ ] Ẩn tên phương pháp, đảo thứ tự hiển thị và cung cấp đầy đủ nguồn; người chấm không xem điểm ROUGE trước.
- [ ] Chấm bốn tiêu chí theo bảng dưới; ghi thêm câu/chi tiết lỗi và đoạn nguồn đối chiếu.
- [ ] Thống nhất hướng dẫn qua một lượt chấm thử; báo mức đồng thuận hoặc chênh lệch giữa người chấm.
- [ ] Tổng hợp điểm trung bình, phân bố và tỷ lệ mẫu có thông tin không được nguồn hỗ trợ.

| Tiêu chí | Điểm 1 | Điểm 3 | Điểm 5 |
| --- | --- | --- | --- |
| Đầy đủ | Mất phần lớn ý chính | Giữ được một phần quan trọng | Giữ đủ các ý chính phù hợp mức dài |
| Chính xác | Sai ý nghiêm trọng | Có một số sai lệch | Không phát hiện sai lệch khi đối chiếu |
| Mạch lạc | Khó hiểu, đứt đoạn | Hiểu được nhưng còn lủng củng | Dễ đọc, liên kết rõ |
| Súc tích | Nhiều ý thừa/lặp | Có vài chỗ dư | Ít dư thừa, phù hợp mức tóm tắt |

Không dùng riêng điểm súc tích để kết luận bản quá ngắn là tốt. Phân biệt mất thông tin và thêm thông tin không có trong nguồn.

## 4. Nhánh tự tinh chỉnh — tùy chọn

- [ ] Chỉ bắt đầu khi có đủ cặp dữ liệu, quyền sử dụng, GPU và thời gian; ước lượng qua một lượt huấn luyện thử ngắn.
- [ ] Xác định giả thuyết, chẳng hạn cải thiện với miền dữ liệu mục tiêu; giữ checkpoint có sẵn làm đối chứng.
- [ ] Dùng train để cập nhật trọng số, validation để chọn checkpoint/siêu tham số, test chỉ cho đánh giá cuối.
- [ ] Dùng công cụ seq2seq tương thích phiên bản; thử learning rate, batch size, số epoch, gradient accumulation và mixed precision theo phần cứng.
- [ ] Lưu seed, cấu hình, checkpoint tốt nhất và learning curve; theo dõi overfitting và lỗi cắt nguồn/nhãn.
- [ ] Đánh giá lại bằng cùng giao thức và giữ cả số liệu trước/sau. Nếu không tốt hơn thì trình bày đúng kết quả.
- [ ] Không cam kết rằng chỉ thay đổi tham số sinh là đã fine-tune mô hình.

## 5. Sản phẩm và nghiệm thu

- [ ] `predictions` lưu từng mẫu: ID, nguồn/tham chiếu theo quyền chia sẻ, kết quả, phương pháp, mức dài, cấu hình, độ dài, thời gian và cảnh báo.
- [ ] Bảng tổng hợp ROUGE, đánh giá con người, tỷ lệ rút gọn, độ trễ, lỗi và mức sử dụng tài nguyên nếu đo được.
- [ ] Phân tích ví dụ thành công/thất bại: thiếu ý chính, lặp, sai tên/số liệu, sai liên kết và ảnh hưởng chia phần.
- [ ] Có kết luận dựa trên số liệu; không coi ROUGE hoặc tỷ lệ rút gọn là thước đo duy nhất.
- [ ] Chỉ tiêu sau pilot được ghi trước test; nếu chưa đạt thì báo hạn chế và sửa theo validation, không sửa trực tiếp để khớp test.

Tiếp theo: [phase 8](phase8.md).

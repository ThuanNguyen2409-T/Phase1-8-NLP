# Phase 3 — Tóm tắt trích xuất bằng TextRank

Thời gian dự kiến: 2–3 ngày. Phụ thuộc: [phase 2](phase2.md).

## Mục tiêu

Có phương pháp nền nhẹ, dễ giải thích để đối chiếu với mô hình sinh văn bản.

## Checklist thực hiện

- [ ] Xây dựng Lead baseline: lấy các câu đầu theo cùng ngân sách độ dài với TextRank; dùng làm mốc trong thực nghiệm.
- [ ] Định nghĩa giao diện chung `summarize(text, length_level)` trả kết quả, phương pháp, cảnh báo và metadata.
- [ ] Tách câu, lưu vị trí từng câu trong nguồn và tạo biểu diễn TF-IDF với tokenizer tiếng Việt.
- [ ] Tạo cạnh có trọng số bằng độ tương đồng cosine; bỏ self-loop.
- [ ] Chạy PageRank để xếp hạng câu; quy định cách phá hòa điểm để tái lập.
- [ ] Xử lý văn bản không có từ vựng hợp lệ, chỉ một câu hoặc đồ thị không có cạnh bằng kết quả/thông báo phù hợp.
- [ ] Chọn câu theo điểm trong ngân sách rồi sắp xếp lại theo thứ tự nguồn để giữ mạch văn.
- [ ] Hạn chế chọn nhiều câu quá giống nhau; không sửa nội dung câu trích xuất khi chưa có lý do rõ ràng.
- [ ] Tránh cắt ngang câu chỉ để đạt số đơn vị chính xác; trả thống kê độ dài thực tế và cảnh báo nếu không đạt mục tiêu.
- [ ] Giới hạn số câu/độ dài cho ma trận tương đồng để tránh chi phí bậc hai trên văn bản rất lớn; thông báo hoặc dùng chiến lược phân đoạn đã kiểm tra.

## Điều khiển độ dài đề xuất

| Mức | Ngân sách mục tiêu theo độ dài nguồn | Ý nghĩa |
| --- | --- | --- |
| Ngắn | Khoảng 15% | Các ý nổi bật nhất |
| Vừa | Khoảng 25% | Ý chính và chi tiết cần thiết |
| Chi tiết | Khoảng 40% | Giữ thêm luận điểm và bằng chứng |

Các tỷ lệ là điểm khởi đầu để thử trên validation, không phải bảo đảm cho mọi văn bản. Dùng cùng một đơn vị cách trắng để tính ngân sách và thống kê. Ít nhất một câu chỉ được chọn khi vẫn tạo tóm tắt có ý nghĩa; nguồn một câu có thể được báo là không cần tóm tắt. Chốt riêng giới hạn tuyệt đối để tránh đầu ra quá dài.

## Kiểm tra có ý nghĩa

- [ ] Mọi câu đầu ra truy ngược được về nguồn, đúng thứ tự đã chọn.
- [ ] Không trả lỗi hệ thống với văn bản chỉ một câu hoặc đồ thị rỗng.
- [ ] Ba mức cho xu hướng độ dài tăng trên tập pilot; ghi nhận trường hợp bằng nhau do độ dài câu.
- [ ] Đầu ra không dài hơn nguồn; trường hợp không thể rút gọn được báo rõ.
- [ ] Đo tốc độ và bộ nhớ với các nhóm độ dài để đặt giới hạn request.

## Sản phẩm và nghiệm thu

- [ ] Module Lead baseline và TextRank, cấu hình và ví dụ đầu vào/đầu ra.
- [ ] Bảng pilot gồm độ dài, tỷ lệ rút gọn, thời gian và nhận xét lỗi.
- [ ] Giải thích được nguyên lý TF-IDF → tương đồng → PageRank → chọn câu và hạn chế về đại từ/liên kết giữa các câu.

Tiếp theo: [phase 4](phase4.md).

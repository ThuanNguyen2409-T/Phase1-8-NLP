# Phase 8 — Mở rộng, hoàn thiện và bàn giao đồ án

Thời gian dự kiến: 3–4 ngày cho hoàn thiện/báo cáo; chức năng mở rộng cần ước lượng riêng. Phụ thuộc: [phase 6](phase6.md), [phase 7](phase7.md).

## Mục tiêu

Ổn định sản phẩm, hoàn thiện báo cáo và chuẩn bị demo có thể chạy lại trên máy trình bày.

## 1. Chọn chức năng mở rộng theo thời gian

Đây là danh sách tùy chọn. Không cần hoàn thành mọi mục để nghiệm thu bốn chức năng chính.

| Ưu tiên đề xuất | Chức năng | Hướng thực hiện | Điều kiện nghiệm thu |
| --- | --- | --- | --- |
| 1 | Xuất kết quả | TXT/Markdown UTF-8 từ phase 6 | Mở lại đúng dấu và nội dung |
| 1 | Lịch sử | SQLite + session từ phase 5–6 | Xem/xóa đúng phiên, không ghi trùng |
| 1 | So sánh giao diện | Hai cột TextRank/Transformer | Cùng nguồn/mức dài, thống kê tách biệt |
| 2 | Tải TXT/DOCX/PDF | Đọc tệp → xem trước → tiền xử lý chung | Lấy đúng văn bản hoặc báo lỗi rõ ràng |
| 3 | Hỏi đáp tài liệu | Truy xuất đoạn liên quan và model QA phù hợp | Có bằng chứng, biết từ chối khi nguồn không đủ |
| 3 | Đa ngôn ngữ | Chọn model và bộ đánh giá theo ngôn ngữ | Kiểm chứng riêng từng ngôn ngữ hỗ trợ |
| 3 | Chế độ kết hợp | Trích xuất chọn nội dung → Transformer | Đo ảnh hưởng mất ý so với baseline |

### Tải tệp

- [ ] Nếu chọn chức năng này, thêm bộ trích xuất TXT, DOCX, PDF có lớp văn bản; cân nhắc `python-docx` và `pypdf`, kiểm tra tương thích/giấy phép khi triển khai.
- [ ] Giới hạn dung lượng, số trang/ký tự, thời gian trích xuất; không chỉ tin phần mở rộng tệp.
- [ ] Cho xem trước và chỉnh văn bản trích xuất, nhất là PDF nhiều cột, bảng và đầu/chân trang.
- [ ] Báo PDF quét không có lớp văn bản hoặc PDF khóa/hỏng; OCR được lập thành nhánh riêng nếu cần.
- [ ] Sau trích xuất, dùng cùng quy tắc giới hạn và pipeline như văn bản dán, không đi tắt kiểm tra.

### Hỏi đáp và ngôn ngữ khác

- [ ] Chỉ triển khai hỏi đáp khi có model/cơ chế truy xuất phù hợp; không coi checkpoint tóm tắt là mô hình hỏi đáp sẵn dùng.
- [ ] Câu trả lời phải gắn đoạn bằng chứng; thêm mẫu hỏi ngoài tài liệu và không đủ dữ kiện.
- [ ] Với đa ngôn ngữ, nêu danh sách ngôn ngữ đã kiểm thử và nguồn dữ liệu đánh giá; không suy luận khả năng từ tên mô hình.
- [ ] Nếu dùng model/API khác thì cập nhật kiến trúc, yêu cầu tài nguyên và báo cáo thực nghiệm.

## 2. Kiểm tra bản bàn giao

- [ ] Chạy lại bốn chức năng chính với một bộ mẫu cố định: hợp lệ, rỗng, quá ngắn, dài nhiều phần và vượt giới hạn toàn hệ thống.
- [ ] Kiểm tra giữ dấu tiếng Việt, số liệu và công thức rút gọn; không hiển thị tỷ lệ chất lượng thay cho tỷ lệ độ dài.
- [ ] Kiểm tra startup model, lỗi tải, timeout và thiếu tài nguyên bằng kịch bản phù hợp.
- [ ] Kiểm tra một yêu cầu không làm phát sinh tóm tắt/lịch sử lặp vì Streamlit rerun.
- [ ] Rà lại kết quả sau mọi thay đổi ảnh hưởng mô hình, preprocessing hoặc ngân sách; cập nhật bảng đánh giá nếu cần.
- [ ] Chuẩn bị model cache và tài liệu demo được phép chia sẻ; thử chạy khi không có Internet nếu chọn demo offline.
- [ ] Xác minh tệp cài đặt, model revision, cấu hình và hướng dẫn đủ để chạy trên môi trường mới tương đương.
- [ ] Không đưa dữ liệu lịch sử thật, tệp riêng tư hoặc thông tin xác thực vào bộ nộp.

## 3. Báo cáo đồ án

- [ ] Chương 1: lý do chọn đề tài, mục tiêu, phạm vi và tiêu chí đánh giá.
- [ ] Chương 2: tóm tắt trích xuất/trừu tượng, TextRank, Transformer/ViT5, ROUGE và hạn chế.
- [ ] Chương 3: yêu cầu, kiến trúc, API, dữ liệu, tiền xử lý và thiết kế giao diện.
- [ ] Chương 4: hiện thực, điều khiển độ dài, xử lý tài liệu dài và quản lý lỗi.
- [ ] Chương 5: giao thức thực nghiệm, bảng kết quả, đánh giá con người, phân tích lỗi và tài nguyên.
- [ ] Chương 6: kết luận, hạn chế, hướng phát triển; phân biệt phần đã làm với phần đề xuất.
- [ ] Trích dẫn nguồn mô hình, dữ liệu, thuật toán và hình minh họa; không xem các ảnh tham khảo là kiến trúc đã hiện thực.

## 4. Bộ sản phẩm nộp

- [ ] Mã nguồn với hướng dẫn cài đặt/chạy frontend và backend.
- [ ] Tệp phụ thuộc, cấu hình mẫu, revision model và hướng dẫn lấy dữ liệu theo quyền sử dụng.
- [ ] Báo cáo, slide hoặc nội dung trình bày theo yêu cầu giảng viên.
- [ ] Bảng kết quả thực nghiệm, phiếu chấm, mẫu đầu vào/đầu ra và các hạn chế đã phát hiện.
- [ ] Kịch bản demo 5–7 phút: nhập văn bản → chọn mức dài → tóm tắt → thống kê → so sánh → giải thích đánh giá.
- [ ] Chuẩn bị câu trả lời về lý do chọn mô hình, chunking, sai lệch thông tin, ROUGE tiếng Việt và ý nghĩa thực sự của tỷ lệ rút gọn.

## Điều kiện kết thúc

- [ ] Bốn chức năng chính hoạt động trên bộ nghiệm thu.
- [ ] Hai phương pháp có kết quả so sánh và đánh giá thủ công; mọi chức năng tùy chọn được ghi đúng trạng thái thực hiện.
- [ ] Có thể chạy lại demo và tái lập thực nghiệm trong điều kiện đã công bố.
- [ ] Bộ bàn giao không chứa kết quả, điểm số hoặc cam kết chất lượng chưa được đo.

Quay lại [danh sách phase](README.md).

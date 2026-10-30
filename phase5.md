# Phase 5 — FastAPI và lưu trữ

Thời gian dự kiến: 3–4 ngày. Phụ thuộc: [phase 3](phase3.md), [phase 4](phase4.md).

## Mục tiêu

Cung cấp API ổn định cho giao diện và chương trình đánh giá, với lịch sử SQLite là phần tùy chọn.

## Checklist API

- [ ] Tách lớp API khỏi lớp thuật toán; dùng schema kiểm tra request/response.
- [ ] Cung cấp `POST /summarize` nhận `text`, `method` (`textrank`/`transformer`), `length_level` (`short`/`medium`/`detailed`).
- [ ] Trả `request_id`, `summary`, `method_requested`, `method_used`, `length_level`, `warnings`, `stats`, `model_metadata` và thông tin chia phần.
- [ ] `stats` gồm độ dài nguồn chuẩn hóa/đầu ra theo cùng đơn vị, số ký tự, tỷ lệ rút gọn, thời gian xử lý backend; token model chỉ có khi phù hợp.
- [ ] Cung cấp kiểm tra sống/sẵn sàng, phân biệt tiến trình hoạt động với model đã tải xong.
- [ ] Kiểm tra rỗng, enum sai, giới hạn ký tự, ngân sách token và số phần ở phía server, dù giao diện đã kiểm tra trước.
- [ ] Quy định lỗi: đầu vào không hợp lệ (422), request vượt giới hạn hệ thống (413), model chưa sẵn sàng (503), quá tải (429), lỗi nội bộ (500). Đồng bộ cách hiển thị với UI.
- [ ] Văn bản vượt ngân sách một lần suy luận nhưng vẫn trong giới hạn chia phần là request hợp lệ, có cảnh báo; không gộp với lỗi vượt giới hạn hệ thống.
- [ ] Có timeout và giới hạn đồng thời phù hợp CPU/GPU; tránh tải model trên nhiều worker ngoài dự kiến.
- [ ] Ghi log request ID, trạng thái, thời gian và cấu hình cần thiết; không ghi toàn văn người dùng vào log mặc định.
- [ ] Phân biệt thời gian suy luận, tiền/hậu xử lý và tổng backend khi benchmark.

## Hợp đồng dữ liệu dự kiến

```json
{
  "text": "Văn bản tiếng Việt cần tóm tắt...",
  "method": "transformer",
  "length_level": "medium"
}
```

Không đưa tham chiếu chất lượng vào request tóm tắt thông thường. ROUGE được tính trong chương trình đánh giá khi có bản tham chiếu.

## SQLite — tùy chọn lịch sử

- [ ] Thiết kế `sessions` và `summaries`; mỗi bản ghi gắn với `session_id` và thời gian tạo.
- [ ] Lưu nguồn gốc/nguồn chuẩn hóa theo nhu cầu, bản tóm tắt, phương pháp, mức dài, thống kê, model revision, tham số và cảnh báo.
- [ ] Cung cấp lấy danh sách, xem chi tiết, xóa từng bản ghi hoặc xóa lịch sử của phiên; chỉ thực hiện khi tính năng được chọn.
- [ ] Giới hạn truy vấn theo session; bản demo session không thay thế xác thực người dùng cho hệ thống công khai.
- [ ] Tránh lưu trùng khi giao diện chạy lại; dùng ID thao tác hoặc request ID để nhận diện cùng một lần gửi.
- [ ] Lưu dữ liệu thực nghiệm và tham chiếu trong bảng/tệp riêng với `sample_id`, `run_id`, cấu hình và điểm; không trộn với lịch sử người dùng.
- [ ] Quy định thời gian lưu và thao tác xóa, thông báo rõ khi bật lưu văn bản.

## Kiểm tra và nghiệm thu

- [ ] Kiểm tra hợp đồng API với cả hai phương pháp, ba mức độ dài và các lỗi quan trọng.
- [ ] Dùng bộ xử lý giả để kiểm tra luồng API nhanh; chạy một số smoke test bằng model thật.
- [ ] Khi model lỗi, response không bị gắn nhãn thành công hoặc phương pháp sai.
- [ ] Nếu bật lịch sử: dữ liệu còn sau khởi động lại, không lẫn giữa hai session và không ghi trùng do retry.
- [ ] Có tài liệu schema, ví dụ request/response và hướng dẫn chạy backend.

Tiếp theo: [phase 6](phase6.md).

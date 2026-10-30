# Phase 6 — Giao diện chatbot Streamlit

Thời gian dự kiến: 2–3 ngày. Phụ thuộc: [phase 5](phase5.md).

## Mục tiêu

Hoàn thiện luồng nhập văn bản → tóm tắt → xem kết quả và thống kê.

## Checklist giao diện

- [ ] Tạo bố cục chat có vùng dán văn bản nhiều dòng, nút gửi và hướng dẫn ngắn.
- [ ] Cho chọn Ngắn/Vừa/Chi tiết và phương pháp TextRank/Transformer trước khi gửi.
- [ ] Kiểm tra rỗng, hiển thị giới hạn đầu vào và cảnh báo phía giao diện; backend vẫn quyết định tính hợp lệ cuối cùng.
- [ ] Hiển thị trạng thái đang xử lý, khóa gửi lặp cho cùng thao tác, xử lý timeout/kết nối lỗi rõ ràng.
- [ ] Lưu trạng thái phiên trong Streamlit để không mất kết quả hoặc tự gọi API lại khi widget thay đổi.
- [ ] Hiển thị bản tóm tắt ở câu trả lời của chatbot; văn bản nguồn có thể thu gọn nhưng luôn mở xem được.
- [ ] Hiển thị phương pháp thực tế, mức dài, cảnh báo chia phần hoặc phương pháp thay thế.
- [ ] Hiển thị số ký tự, số đơn vị cách trắng ở nguồn và kết quả, tỷ lệ rút gọn và thời gian backend.
- [ ] Giải thích ngắn công thức tỷ lệ và nhãn đơn vị; nếu thêm thời gian chờ UI phải ghi riêng.
- [ ] Hiển thị trường hợp không cần tóm tắt hoặc không tạo được bản ngắn hơn mà không đưa tỷ lệ thành công sai lệch.
- [ ] Thông báo khi người dùng yêu cầu hỏi đáp nhưng chức năng này chưa được triển khai; không ngầm coi model tóm tắt là model chat tổng quát.
- [ ] Kiểm tra bố cục với văn bản dài, tiếng Việt có dấu, thông báo lỗi và kết quả nhiều đoạn.

## Tiện ích tùy chọn

- [ ] Nếu phase 5 đã có SQLite: hiển thị lịch sử theo phiên, xem lại và xóa kết quả.
- [ ] Nếu chọn so sánh trên UI: chạy hai phương pháp trên cùng nguồn/mức dài, hiển thị cạnh nhau, ghi thời gian riêng và giữ kết quả thành công khi phương pháp còn lại lỗi.
- [ ] Nếu chọn xuất kết quả: tải `.txt` hoặc `.md` mã hóa UTF-8, kèm phương pháp/mức dài nếu người dùng muốn.
- [ ] Chỉ hiển thị thao tác tải tệp/hỏi đáp/ngôn ngữ khác khi chức năng tương ứng đã có ở phase 8.

## Kịch bản nghiệm thu bốn chức năng chính

| Kịch bản | Kết quả cần thấy |
| --- | --- |
| Gửi rỗng hoặc chỉ khoảng trắng | Có thông báo, không gọi mô hình |
| Dán văn bản hợp lệ và gửi | Có bản tóm tắt hoặc lỗi rõ ràng, không treo |
| Chọn ba mức độ dài | Cấu hình được gửi đúng, hiển thị mức và độ dài thực tế |
| Văn bản vượt ngân sách một phần | Có thông tin xử lý nhiều phần hoặc lý do từ chối theo giới hạn |
| Xem thống kê | Đếm nhất quán; mẫu 1.000 → 200 đơn vị cho tỷ lệ 80% |
| Thay đổi widget sau khi đã có kết quả | Không tự sinh lại hoặc ghi thêm lịch sử |
| Backend/model không sẵn sàng | Có thông báo trạng thái và thao tác thử lại |

## Sản phẩm bàn giao

- [ ] Giao diện chạy cùng backend, hướng dẫn khởi động hai thành phần.
- [ ] Bộ ảnh hoặc video demo luồng thành công và các lỗi phổ biến.
- [ ] Xác nhận mốc M3: bốn chức năng chính đã sử dụng được, chưa đồng nghĩa chất lượng mô hình đã đạt mục tiêu.

Tiếp theo: [phase 7](phase7.md).

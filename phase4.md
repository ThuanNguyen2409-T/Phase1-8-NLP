# Phase 4 — Transformer và xử lý văn bản dài

Thời gian dự kiến: 4–6 ngày. Phụ thuộc: [phase 2](phase2.md); dùng kết quả [phase 3](phase3.md) để đối chiếu.

## Mục tiêu

Chạy mô hình tóm tắt có sẵn, điều khiển độ dài và xử lý đầu vào dài có giới hạn tài nguyên rõ ràng.

## 1. Suy luận mô hình

- [ ] Thử `VietAI/vit5-base-vietnews-summarization`; ghi model revision và cấu hình. Model card nêu checkpoint này không cần task prefix; kiểm tra quy tắc special token bằng tokenizer, tránh tự thêm EOS hai lần.
- [ ] Tải bằng `AutoTokenizer` và `AutoModelForSeq2SeqLM`, chạy `model.eval()` với chế độ inference của PyTorch.
- [ ] Dùng CPU khi không có GPU phù hợp; đo thực tế thay vì cam kết độ trễ trước benchmark.
- [ ] Quản lý model như tài nguyên dùng chung trong backend, không tải lại theo request.
- [ ] Đếm token trước khi chạy; không bật truncation âm thầm rồi thông báo đã tóm tắt toàn văn.
- [ ] Khởi đầu bằng sinh xác định (`do_sample=False`), chọn greedy/beam search trên validation; ghi tham số và thiết bị.
- [ ] Lưu metadata: model/revision, token đầu vào/đầu ra, cấu hình sinh, số phần, số vòng gộp, thời gian và cảnh báo.

Nguồn: [model card](https://huggingface.co/VietAI/vit5-base-vietnews-summarization), [suy luận Transformers](https://huggingface.co/docs/transformers/main/tasks/summarization), [quy tắc truncation](https://huggingface.co/docs/transformers/main/pad_truncation).

## 2. Điều chỉnh ba mức độ dài

- [ ] Ánh xạ Ngắn/Vừa/Chi tiết sang ngân sách token đầu ra khác nhau dựa trên độ dài đầu vào, có ngưỡng tối thiểu/tối đa cấu hình.
- [ ] Dùng `max_new_tokens` làm giới hạn sinh; không đồng nhất số token với số từ hoặc ký tự.
- [ ] Thử các tỷ lệ mục tiêu tương ứng phase 3, rồi hiệu chỉnh bằng độ dài thực tế trên validation; mô hình có thể kết thúc trước giới hạn.
- [ ] Chỉ dùng giới hạn tối thiểu khi đã kiểm tra không ép mô hình sinh thêm nội dung dư thừa; không ép bản ngắn phải dài tới một số token cố định.
- [ ] Kiểm tra đầu ra có bị ngắt câu do chạm giới hạn sinh; nếu có, thử lại có giới hạn hoặc trả cảnh báo, không âm thầm xóa ý.
- [ ] Với nguồn quá ngắn, trả trạng thái không cần tóm tắt hoặc hướng dẫn bổ sung văn bản.

## 3. Chia phần và gộp phân cấp

- [ ] Chốt ngân sách token mỗi phần theo checkpoint, thiết bị và pilot; tính cả special token và prefix nếu checkpoint yêu cầu.
- [ ] Gom các câu/đoạn theo thứ tự đến khi gần đạt ngân sách; ghi ánh xạ phần → vị trí nguồn.
- [ ] Nếu một câu đơn đã vượt ngân sách, chia tiếp theo ranh giới phù hợp hoặc cửa sổ token có overlap giới hạn; ghi nhận rủi ro mất ngữ cảnh.
- [ ] Kiểm tra mọi phần đều nằm trong ngân sách và toàn bộ nội dung nguồn được bao phủ; overlap nếu dùng phải được ghi lại.
- [ ] Tóm tắt mỗi phần với ngân sách trung gian; không áp dụng nguyên ngân sách bản cuối cho từng phần một cách máy móc.
- [ ] Ghép tóm tắt theo thứ tự, giảm trùng có kiểm soát rồi tóm tắt lại đến ngân sách cuối nếu cần.
- [ ] Đặt số phần tối đa, số vòng gộp tối đa, thời gian chờ và điều kiện dừng khi độ dài không giảm; trả lỗi rõ ràng khi vượt giới hạn.
- [ ] Báo người dùng khi dùng tóm tắt phân cấp và ghi số phần đã xử lý.
- [ ] Thử trường hợp ý chính nằm ở cuối, hai phần nhắc cùng sự kiện, mâu thuẫn số liệu, và ngữ cảnh cần liên kết nhiều phần.

## 4. Kiểm tra đầu ra và xử lý lỗi

- [ ] Phát hiện đầu ra rỗng, lặp bất thường, không ngắn hơn nguồn hoặc lỗi sinh; giới hạn số lần thử lại.
- [ ] Kiểm tra hỗ trợ tên riêng, con số, ngày tháng như tín hiệu cảnh báo; không coi đó là bộ xác minh sự thật hoàn chỉnh.
- [ ] Không đảm bảo loại bỏ hoàn toàn thông tin bịa đặt; dùng đánh giá đối chiếu nguồn trong phase 7 để đo rủi ro.
- [ ] Nếu cho phép chuyển sang TextRank khi Transformer lỗi, cần trả đúng `method_used` và cảnh báo; không gắn nhãn Transformer cho kết quả thay thế.
- [ ] Với thiếu bộ nhớ hoặc model không sẵn sàng, phản hồi rõ trạng thái; không giữ request treo vô hạn.

## Sản phẩm và tiêu chí nghiệm thu

- [ ] Module Transformer và bộ chia/gộp dùng chung, cấu hình độ dài và ngân sách.
- [ ] Pilot ba mức độ dài trên văn bản ngắn, vừa và vượt ngưỡng một lần suy luận.
- [ ] Không phần nào vượt ngân sách token; không bỏ sót phần cuối do truncation âm thầm.
- [ ] Có giới hạn tài nguyên, điều kiện dừng và metadata đủ tái hiện kết quả.
- [ ] Có báo cáo chất lượng ban đầu; phân biệt hạn chế của mô hình với lỗi phần mềm.

Tiếp theo: [phase 5](phase5.md).

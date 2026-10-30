# Phase 2 — Môi trường, dữ liệu và tiền xử lý

Thời gian dự kiến: 3–4 ngày. Phụ thuộc: [phase 1](phase1.md).

## Mục tiêu

Chuẩn bị môi trường tái lập, dữ liệu có nguồn gốc rõ ràng và bộ tiền xử lý dùng chung cho ứng dụng lẫn thực nghiệm.

## Checklist thực hiện

- [ ] Kiểm tra CPU, RAM, dung lượng đĩa, GPU/VRAM và khả năng CUDA; ghi cấu hình thực tế.
- [ ] Tạo môi trường Python riêng, thử tương thích PyTorch–Transformers–tokenizer trên máy mục tiêu trước khi khóa phiên bản.
- [ ] Cài nhóm cơ bản: PyTorch, Transformers, SentencePiece nếu tokenizer cần, underthesea, Pandas, NumPy, scikit-learn, NetworkX, FastAPI, Uvicorn, Streamlit, HTTP client và rouge-score.
- [ ] Thử tải tokenizer và suy luận một mẫu ngắn; chốt phiên bản phụ thuộc, revision mô hình và hướng dẫn cài CPU/GPU riêng nếu cần.
- [ ] Thiết lập Git, `.gitignore` và cấu hình mẫu; không đưa model cache, cơ sở dữ liệu lịch sử, dữ liệu hạn chế chia sẻ hoặc khóa bí mật vào Git.
- [ ] Xác minh nguồn tập tóm tắt tiếng Việt, quyền sử dụng và khả năng tải; tham khảo [repository ViT5](https://github.com/VietAI/ViT5). Không xem nhãn dữ liệu trên Hub là bằng chứng đủ về toàn bộ dữ liệu tinh chỉnh.
- [ ] Chuẩn bị khoảng 20–30 cặp văn bản–tham chiếu cho pilot và dự kiến 100–200 mẫu test phân tầng độ dài/chủ đề; điều chỉnh theo nguồn dữ liệu và thời gian.
- [ ] Với tinh chỉnh tùy chọn: ưu tiên split chính thức; nếu tự chia thì đề xuất 80/10/10 theo nhóm nguồn/bài, sau loại trùng, có seed cố định.
- [ ] Lưu schema: `sample_id`, `document`, `reference_summary`, `source`, `domain`, `split`, `license_or_usage_note`.
- [ ] Lọc văn bản rỗng, lỗi mã hóa, cặp không đúng nội dung; kiểm tra tham chiếu thực sự là tóm tắt chứ không chỉ là tiêu đề.
- [ ] Chuẩn hóa Unicode NFC, xuống dòng và khoảng trắng thừa; giữ ranh giới đoạn.
- [ ] Dùng underthesea tách câu; kiểm tra tên viết tắt, số thập phân, danh sách và xuống dòng.
- [ ] Tách nhánh biểu diễn: TextRank có thể dùng từ đã tách và bộ lọc riêng; Transformer nhận văn bản tự nhiên theo đúng tokenizer checkpoint.
- [ ] Xây dựng hàm thống kê số ký tự, đơn vị cách trắng, số câu và token mô hình.
- [ ] Kiểm tra tokenizer có giới hạn hợp lệ hay chỉ giá trị sentinel; xác định ngân sách triển khai bằng cấu hình, tài liệu và benchmark, tính cả special token.
- [ ] Chuẩn bị mẫu biên: chỉ khoảng trắng, một câu, không dấu, emoji, tên riêng, số liệu, nhiều đoạn, câu rất dài và văn bản vượt ngân sách.

## Cấu trúc mã nguồn dự kiến

Đây là thiết kế thư mục cho lúc triển khai, chưa phải các tệp đã được tạo.

```text
app/
  ui/                 # Streamlit
  api/                # FastAPI và schema
  core/               # Cấu hình, tiền xử lý, thống kê
  summarizers/        # Giao diện chung, TextRank, Transformer, chia phần
  storage/            # SQLite nếu bật lịch sử
evaluation/           # Sinh dự đoán, ROUGE, phân tích, phiếu chấm
data/                 # raw, processed, splits; chỉ chia sẻ khi được phép
configs/              # Model, ngân sách token, tham số, seed
tests/                # Các kiểm tra quan trọng
reports/              # Kết quả thí nghiệm và báo cáo
```

## Sản phẩm bàn giao

- [ ] Danh sách phụ thuộc được khóa và hướng dẫn cài đặt trên máy mục tiêu.
- [ ] Bảng mô tả dữ liệu, nguồn, split và quy tắc lọc.
- [ ] Module tiền xử lý/thống kê và bộ mẫu pilot.
- [ ] Nhật ký phép thử tải model cùng cấu hình tài nguyên.

## Tiêu chí nghiệm thu

- [ ] Cài lại môi trường theo hướng dẫn và xử lý được một mẫu tiếng Việt có dấu.
- [ ] Chuẩn hóa không làm mất tên riêng, dấu câu hoặc số liệu của các mẫu kiểm tra.
- [ ] Quy tắc đếm và chia dữ liệu được cố định, có thể tái lập.
- [ ] Dữ liệu test được tách khỏi quá trình chọn tham số; ghi rõ rủi ro trùng dữ liệu huấn luyện của checkpoint nếu không xác minh được.

Tiếp theo: [phase 3](phase3.md).

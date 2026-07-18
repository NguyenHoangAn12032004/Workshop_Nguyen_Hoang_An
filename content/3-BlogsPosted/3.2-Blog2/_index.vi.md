---
title: "Blog 2: Content Pipeline hỗ trợ AI"
date: 2026-07-07
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Từ DOCX đến câu hỏi thi — Xây dựng Content Pipeline hỗ trợ AI với Lambda, S3 và Tesseract

<!-- Thay bằng link bài blog thực tế -->
**Đã đăng tại:** [AWS Study Group](https://cloudjourney.awsstudygroup.com/)

## Tóm tắt

Bài blog này trình bày chi tiết cách admin content pipeline của LingoRise tự động hóa việc chuyển đổi file DOCX, PDF và scanned-PDF thành câu hỏi thi có cấu trúc — giảm thời gian tạo nội dung từ hàng giờ xuống vài phút.

## Các chủ đề chính

### Vấn đề
Người tạo nội dung thi làm việc với file DOCX và PDF. Việc định dạng lại thủ công thành bản ghi database có cấu trúc tốn thời gian, dễ lỗi và không scale được.

### Kiến trúc Pipeline
```
Upload DOCX/PDF → S3 → Lambda (Parser) → Structured JSON
                                ↓
Scanned PDF → S3 → OCR Worker (Tesseract.js) → Text → Parser → JSON
                                ↓
Structured JSON → PostgreSQL (question_bank table)
                                ↓
Admin Review → Phát hiện trùng lặp → Xuất bản
```

### Chi tiết triển khai

#### Phân tích tài liệu
- **File DOCX**: Phân tích bằng thư viện `mammoth` để trích xuất HTML có cấu trúc, sau đó chuyển đổi sang format câu hỏi thi.
- **File PDF**: Phân tích bằng `pdf-parse` để trích xuất văn bản.
- **Scanned PDFs**: Xếp hàng đợi OCR qua Tesseract.js worker.

#### Hàng đợi OCR Worker
Hệ thống OCR sử dụng mô hình background worker:
- Tài liệu cần OCR được đánh dấu `ocr_status: 'queued'` trong database.
- Worker (`npm run worker:ocr`) xử lý hàng đợi từng tài liệu một.
- Watch mode (`npm run worker:ocr:watch`) liên tục kiểm tra tài liệu mới.
- Timeout (mặc định 60s) và ngôn ngữ có thể cấu hình.

#### Phát hiện trùng lặp
Trước khi xuất bản, hệ thống kiểm tra câu hỏi trùng lặp bằng text similarity để ngăn nội dung thừa trong question bank.

#### Nhật ký kiểm toán
Mọi thay đổi nội dung đều được theo dõi: ai nhập gì, khi nào, và câu hỏi nào được tạo, sửa, hoặc từ chối.

### Dịch vụ AWS tham gia
| Dịch vụ | Vai trò trong Pipeline |
|---------|----------------------|
| **S3** | Lưu trữ tài liệu upload và tài nguyên đã xử lý |
| **Lambda** | Chạy logic phân tích và nhập liệu |
| **CloudWatch** | Giám sát sức khỏe OCR worker và thời gian xử lý |
| **SES** | Thông báo admin khi nhập hoàn thành hoặc thất bại |

### Kết quả
- Thời gian nhập nội dung giảm từ ~2 giờ (thủ công) xuống ~5 phút (tự động).
- Độ chính xác OCR đủ cho hầu hết tài liệu thi in ấn.
- Bước admin review đảm bảo kiểm soát chất lượng trước khi xuất bản.
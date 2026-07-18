---
title: "Tuần 9 Worklog"
date: 2026-06-15
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---




### Mục tiêu tuần 9:

* Chạy kiểm thử toàn diện trên toàn nền tảng.
* Tối ưu tính năng AI: độ chính xác OCR pipeline và chất lượng chatbot.
* Đảm bảo tất cả 6 lệnh CI/CD verification đều pass green.

### Nhiệm vụ sẽ được thực hiện trong tuần này:

| Ngày | Nhiệm vụ | Bắt đầu ngày | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | -------- | ------------- | --------------- | ------------------- |
| 2 | - Viết và chạy integration tests: <br>&emsp; + Auth flow tests (register, login, token refresh) <br>&emsp; + Exam engine tests (create session, submit, score) <br>&emsp; + Admin content pipeline tests | 15/06/2026 | 15/06/2026 | |
| 3 | - Tối ưu OCR pipeline: <br>&emsp; + Cải thiện cấu hình Tesseract.js cho Tiếng Việt <br>&emsp; + Điều chỉnh timeout và language settings <br>&emsp; + Thêm fallback cho kết quả OCR độ tin cậy thấp | 16/06/2026 | 16/06/2026 | |
| 4 | - Tối ưu tích hợp AI chatbot: <br>&emsp; + Tinh chỉnh prompt templates cho giải thích câu hỏi thi <br>&emsp; + Triển khai response caching cho câu hỏi phổ biến <br>&emsp; + Test chất lượng hỗ trợ Tiếng Việt | 17/06/2026 | 17/06/2026 | |
| 5 | - Chạy tất cả 6 lệnh CI/CD verification: <br>&emsp; + TypeScript compilation (tsc --noEmit) <br>&emsp; + Linting (eslint) <br>&emsp; + Unit tests <br>&emsp; + Build verification <br>&emsp; + Database migration dry-run <br>&emsp; + SAM validation | 18/06/2026 | 18/06/2026 | |
| 6 | - Sửa tất cả tests và CI issues bị fail <br> - Kiểm thử hiệu năng dưới tải <br> - Tài liệu hóa kết quả test và coverage metrics | 19/06/2026 | 19/06/2026 | |

### Tuần 9 Thành tựu:

* Hoàn thành bộ test toàn diện:
    * Integration tests phủ tất cả critical user flows
    * Auth flow xác minh end-to-end
    * Xác nhận độ chính xác scoring exam engine
    * Regression tests cho admin content pipeline

* Tối ưu OCR pipeline cho độ chính xác tốt hơn:
    * Tesseract.js language pack điều chỉnh cho Tiếng Việt
    * Configurable timeout (60s mặc định) ngăn treo
    * Cơ chế fallback cho kết quả độ tin cậy thấp
    * Watch mode (`npm run worker:ocr:watch`) cho xử lý liên tục

* Cải thiện chất lượng AI chatbot:
    * Prompt templates tinh chỉnh cho giải thích tốt hơn
    * Response caching giảm chi phí API và latency
    * Phản hồi Tiếng Việt xác minh độ chính xác

* **Tất cả 6 lệnh CI/CD verification đều pass green:**
    * `tsc --noEmit` ✅
    * `eslint .` ✅
    * `npm test` ✅
    * `npm run build` ✅
    * `npm run migrate:dry` ✅
    * `sam validate` ✅

* ...

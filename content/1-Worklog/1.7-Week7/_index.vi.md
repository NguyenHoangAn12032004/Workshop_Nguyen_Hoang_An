---
title: "Tuần 7 Worklog"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---




### Mục tiêu tuần 7:

* Hoàn thành tất cả 21 backend API handler modules.
* Xây dựng toàn bộ frontend UI pages còn lại.
* Đạt 35 database migrations phủ hết schema.

### Nhiệm vụ sẽ được thực hiện trong tuần này:

| Ngày | Nhiệm vụ | Bắt đầu ngày | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | -------- | ------------- | --------------- | ------------------- |
| 2 | - Xây dựng vocabulary notebook handlers: <br>&emsp; + CRUD cho sổ từ vựng cá nhân <br>&emsp; + Tích hợp AWS Polly TTS cho audio phát âm <br> - Xây dựng grammar & writing handlers | 01/06/2026 | 01/06/2026 | |
| 3 | - Xây dựng admin content pipeline: <br>&emsp; + Handler nhập DOCX/PDF <br>&emsp; + OCR queue worker cho scanned PDFs (Tesseract.js) <br>&emsp; + Logic phát hiện trùng lặp | 02/06/2026 | 02/06/2026 | |
| 4 | - Xây dựng các handlers còn lại: <br>&emsp; + Hệ thống khóa học (lộ trình học) <br>&emsp; + Tích hợp AI chatbot (OpenRouter API) <br>&emsp; + Handlers thanh toán/đăng ký PayOS <br>&emsp; + Quản lý người dùng admin | 03/06/2026 | 03/06/2026 | |
| 5 | - Phát triển frontend: <br>&emsp; + UI làm bài thi với timer và điều hướng câu hỏi <br>&emsp; + Sổ từ vựng với phát audio <br>&emsp; + Trang nhập nội dung admin <br>&emsp; + Trang danh sách và chi tiết khóa học | 04/06/2026 | 04/06/2026 | |
| 6 | - Hoàn thiện database migrations đạt 35 tổng cộng <br> - Chạy kiểm thử regression toàn bộ 21 handler modules <br> - Tài liệu hóa tất cả API endpoints bằng Tiếng Anh | 05/06/2026 | 05/06/2026 | |

### Tuần 7 Thành tựu:

* Hoàn thành tất cả 21 backend API handler modules:
    * Auth, Exams, Vocabulary, Grammar, Writing, Speaking
    * Courses, Dashboard, Content, Admin, Payments
    * AI Chat, OCR Worker, Subscription, User Management
    * S3 asset management, CloudFront signed URL generation

* Tích hợp AWS Polly cho text-to-speech:
    * Audio phát âm Tiếng Việt và Tiếng Anh
    * Tạo audio on-demand cho từ vựng
    * File audio được cache trên S3 để tái sử dụng

* Xây dựng admin content pipeline:
    * Phân tích DOCX qua thư viện `mammoth`
    * Trích xuất text PDF qua `pdf-parse`
    * Hàng đợi OCR cho tài liệu scan (Tesseract.js)
    * Phát hiện trùng lặp trước khi xuất bản

* Hoàn thành 35 SQL migrations idempotent:
    * Schema đầy đủ: users, exams, questions, answers, vocabulary, grammar, courses, subscriptions, audit_log, content, v.v.
    * Tất cả migrations wrapped trong transactions cho tính nguyên tử

* Phát triển tất cả frontend pages với thiết kế responsive:
    * UI bài thi với persistent state và khả năng resume
    * Sổ từ vựng với Polly audio playback
    * Admin dashboard với quản lý nội dung

* ...

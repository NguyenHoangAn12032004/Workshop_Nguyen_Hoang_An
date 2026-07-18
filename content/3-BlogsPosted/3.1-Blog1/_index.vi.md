---
title: "Blog 1: Kiến trúc Serverless IELTS/TOEIC"
date: 2026-07-07
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Xây dựng nền tảng luyện thi IELTS/TOEIC Serverless trên AWS — Phân tích kiến trúc chuyên sâu

<!-- Thay bằng link bài blog thực tế -->
**Đã đăng tại:** [AWS Study Group](https://cloudjourney.awsstudygroup.com/)

## Tóm tắt

Bài blog này trình bày các quyết định kiến trúc đằng sau **LingoRise**, nền tảng luyện thi IELTS/TOEIC ưu tiên tiếng Việt được xây dựng hoàn toàn trên dịch vụ serverless của AWS.

## Các chủ đề chính

### Tại sao chọn Serverless?
- Hiệu quả chi phí: thanh toán theo từng request phù hợp cho startup giáo dục.
- Tự động scaling: xử lý đỉnh điểm mùa thi mà không cần provision.
- Giảm chi phí vận hành: không cần quản lý, vá lỗi server.

### Tổng quan kiến trúc
Hệ thống bao gồm:
- **AWS Lambda** chạy Express.js backend qua `serverless-http`, xử lý 21 nhóm API handler (auth, exams, vocabulary, grammar, courses, dashboard, payments, admin, content, speaking, writing, chat, v.v.).
- **Amazon API Gateway** làm HTTPS entry point với custom domain mapping.
- **Amazon S3 + CloudFront** cho lưu trữ tài nguyên với signed URLs cho truy cập bảo mật, có thời hạn.
- **Amazon Cognito** cho xác thực người dùng với JWT-based role authorization (learner/admin).
- **PostgreSQL trên RDS** làm cơ sở dữ liệu quan hệ chính với 35 migrations idempotent.
- **AWS Polly** cho tạo audio text-to-speech cho phần nghe bài thi và phát âm từ vựng.

### Thách thức & Giải pháp
| Thách thức | Giải pháp |
|-----------|----------|
| Lambda cold starts trên exam API | Provisioned concurrency cho endpoints quan trọng |
| Express app lớn trên Lambda | Chia route cẩn thận và lazy loading |
| Phân phối media bảo mật | CloudFront signed URLs với TTL cấu hình được |
| Chất lượng TTS Tiếng Việt | AWS Polly với Neural voice engine |

### Bài học kinh nghiệm
1. Express trên Lambda hoạt động tốt bất ngờ với `serverless-http`, nhưng cần chú ý giới hạn memory và timeout.
2. Quản lý SAM template trở nên phức tạp với 20+ resources — nên xem xét nested stacks cho dự án lớn.
3. Hosted UI của Cognito hạn chế — xây dựng custom auth flows cho UX tốt hơn.
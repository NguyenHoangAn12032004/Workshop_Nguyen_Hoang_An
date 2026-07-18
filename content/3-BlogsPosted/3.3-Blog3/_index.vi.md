---
title: "Blog 3: Triển khai với SAM và Amplify"
date: 2026-07-07
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Triển khai ứng dụng Full-Stack Next.js + Express lên AWS với SAM và Amplify

<!-- Thay bằng link bài blog thực tế -->
**Đã đăng tại:** [AWS Study Group](https://cloudjourney.awsstudygroup.com/)

## Tóm tắt

Bài blog này là hướng dẫn triển khai thực tế cho ứng dụng full-stack TypeScript trên AWS sử dụng SAM (Serverless Application Model) cho backend và Amplify cho frontend — chính xác chiến lược triển khai được sử dụng bởi LingoRise.

## Các chủ đề chính

### Kiến trúc triển khai
```
┌──────────────────┐     ┌──────────────────┐
│   AWS Amplify    │     │  API Gateway     │
│   (Next.js 15)   │────▶│  + Lambda        │
│   Frontend       │     │  (Express.js)    │
└──────────────────┘     └───────┬──────────┘
                                 │
                    ┌────────────┼────────────┐
                    │            │            │
              ┌─────▼──┐  ┌─────▼──┐  ┌──────▼─────┐
              │  RDS   │  │   S3   │  │ Cognito    │
              │ (PG)   │  │+CF CDN │  │ User Pool  │
              └────────┘  └────────┘  └────────────┘
```

### Backend: SAM Template chuyên sâu
File `template.yaml` định nghĩa toàn bộ hạ tầng backend:
- **Lambda function** với Node.js 24.x runtime, 256MB memory, 10s timeout.
- **Environment variables** cho database, Cognito, S3, payment và cấu hình dịch vụ AI.
- **API Gateway** với proxy routing đến Express app.
- Hơn **30 parameters** cho cấu hình linh hoạt giữa các môi trường.

### Frontend: Triển khai Amplify
- Next.js 15 với App Router triển khai qua `amplify.yml` build spec.
- Environment variables inject tại build time cho cấu hình API URL.
- Build tự động kích hoạt bởi Git pushes.

### Database: Chiến lược migration
- 35 SQL migrations idempotent quản lý bởi TypeScript runner tùy chỉnh.
- Mỗi migration chạy trong transaction cho tính nguyên tử.
- An toàn để chạy lại: migrations được theo dõi trong bảng `schema_migrations`.

### Lệnh triển khai
```bash
# Backend deploy
cd lingorise-backend
sam build && sam deploy --guided

# Frontend deploy (qua Amplify Console hoặc CLI)
# Cấu hình trong amplify.yml

# Database migration (chạy đối với production RDS)
DATABASE_URL=<prod-url> npm run migrate
```

### Bài học kinh nghiệm
1. SAM `template.yaml` với 30+ parameters cần tổ chức cẩn thận — nhóm params liên quan bằng metadata.
2. Timeout 10 giây của Lambda đủ cho API calls nhưng không đủ cho OCR — sử dụng kiến trúc worker riêng.
3. Hỗ trợ Next.js của Amplify đã cải thiện đáng kể; App Router hoạt động với cấu hình tối thiểu.
4. Luôn test migrations trên staging database trước production — ngay cả migrations "idempotent" cũng có thể gây bất ngờ.
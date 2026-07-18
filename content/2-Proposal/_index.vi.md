---
title: "Đề xuất dự án"
date: 2026-07-07
weight: 2
chapter: false
pre: " <b> 2. </b> "
---




# LingoRise — Nền tảng luyện thi IELTS/TOEIC ưu tiên Tiếng Việt
## Giải pháp Serverless trên AWS hỗ trợ AI cho luyện thi Tiếng Anh

### 1. Tóm tắt
LingoRise là nền tảng luyện thi IELTS và TOEIC ưu tiên tiếng Việt, được thiết kế để cung cấp trải nghiệm học tập cá nhân hóa, hỗ trợ bởi AI cho sinh viên Việt Nam. Nền tảng cung cấp exam engine toàn diện (IELTS Academic/General + TOEIC), bảng điều khiển học viên với gợi ý hàng ngày, phân tích lỗi sai, sổ từ vựng, và pipeline quản lý nội dung cho admin. Hệ thống sử dụng kiến trúc serverless trên AWS cho vận hành production-ready với chi phí hạ tầng thấp.

### 2. Vấn đề cần giải quyết
#### Thực trạng
Sinh viên Việt Nam luyện thi IELTS/TOEIC gặp nhiều thách thức:
- Các nền tảng hiện tại chủ yếu bằng Tiếng Anh, tạo rào cản ngôn ngữ cho người mới bắt đầu.
- Nội dung thi chất lượng đắt đỏ và phân tán trên nhiều nền tảng.
- Thiếu hệ thống tập trung kết hợp luyện thi, xây dựng từ vựng và cá nhân hóa bằng AI bằng Tiếng Việt.
- Việc tạo nội dung thi thủ công, chậm và dễ lỗi.

#### Giải pháp
LingoRise giải quyết các vấn đề trên với:
- **Giao diện ưu tiên Tiếng Việt** với nội dung song ngữ.
- **Exam engine đầy đủ** hỗ trợ IELTS Academic/General và TOEIC với Cambridge band scoring, lưu trạng thái bài thi, và review từng câu hỏi.
- **Cá nhân hóa hỗ trợ AI**: gợi ý hàng ngày, phân tích lỗi sai, và phân tích học tập.
- **Pipeline nội dung admin**: nhập DOCX/PDF/scanned-PDF, xử lý hàng đợi OCR, phát hiện trùng lặp, và ghi nhật ký kiểm toán.
- **Mô hình đăng ký Premium** với tích hợp thanh toán PayOS.

### 3. Kiến trúc hệ thống

![Kiến trúc](/images/5-Workshop/1-introduction/architecture.png)

#### Công nghệ sử dụng
| Thành phần | Công nghệ |
|------------|-----------|
| **Frontend** | Next.js 15 (App Router, React 19, TailwindCSS, React Query, Framer Motion) |
| **Backend** | Express 4 (development) / AWS Lambda (production) — TypeScript |
| **Database** | PostgreSQL 14+ với 35 migrations idempotent |
| **Xác thực** | AWS Cognito với JWT verification |
| **Lưu trữ** | Amazon S3 + CloudFront (signed URLs) |
| **TTS** | AWS Polly cho audio phát âm |
| **OCR** | Tesseract.js cho xử lý tài liệu scan |
| **AI Chatbot** | OpenRouter-compatible API |
| **Thanh toán** | PayOS (nhà cung cấp thanh toán Việt Nam) |
| **IaC** | AWS SAM (template.yaml với CloudFormation) |
| **Frontend Hosting** | AWS Amplify |

#### Dịch vụ AWS sử dụng
- **AWS Lambda**: Chạy Express backend dưới dạng serverless function.
- **Amazon API Gateway**: Định tuyến HTTP requests đến Lambda.
- **Amazon S3**: Lưu trữ tài nguyên bài thi, file audio, tài liệu nhập.
- **Amazon CloudFront**: CDN cho phân phối nội dung nhanh, bảo mật với signed URLs.
- **Amazon Cognito**: User pool cho xác thực và phân quyền (learner/admin).
- **AWS Polly**: Tổng hợp giọng nói cho audio bài thi và phát âm.
- **Amazon SES**: Email thông báo cho người dùng.
- **AWS SAM/CloudFormation**: Infrastructure-as-code cho triển khai lặp lại.

### 4. Tính năng chính

#### Tính năng học viên
- **Exam Engine**: Mô phỏng bài thi IELTS (Academic + General) và TOEIC đầy đủ với đồng hồ, lưu trạng thái, Cambridge band scoring.
- **Dashboard**: Gợi ý hàng ngày, phân tích lịch sử thi, trực quan hóa lỗi sai.
- **Sổ từ vựng**: Ngân hàng từ cá nhân với audio phát âm (AWS Polly).
- **Ngữ pháp & Viết**: Bài học ngữ pháp có cấu trúc và luyện viết.
- **AI Chat**: Trợ lý học tập AI giải thích câu hỏi.
- **Hệ thống khóa học**: Lộ trình học có cấu trúc với theo dõi tiến độ.

#### Tính năng admin
- **Content Pipeline**: Nhập DOCX/PDF/scanned-PDF với phân tích tự động và hàng đợi OCR.
- **Phát hiện trùng lặp**: Ngăn nội dung bài thi trùng lặp.
- **Nhật ký kiểm toán**: Truy xuất đầy đủ các thay đổi nội dung.
- **Quản lý đăng ký**: Mark-paid thủ công cho development/demo.
- **Quản lý người dùng**: Đăng ký admin và quản lý role.

### 5. Lịch trình & Cột mốc
| Giai đoạn | Thời gian | Trọng tâm |
|-----------|-----------|-----------|
| Tuần 1–4 | 20/04 – 15/05 | Nền tảng AWS: EC2, VPC, S3, RDS, Lambda, DevOps |
| Tuần 5 | 18/05 – 22/05 | Khởi động dự án: proposal, kiến trúc, repository |
| Tuần 6–7 | 25/05 – 05/06 | Phát triển core: 21 API handlers, frontend UI, database |
| Tuần 8 | 08/06 – 12/06 | Tích hợp, bảo mật, triển khai beta |
| Tuần 9 | 15/06 – 19/06 | Kiểm thử và tối ưu AI |
| Tuần 10 | 22/06 – 26/06 | Bàn giao, báo cáo tổng kết, thuyết trình |
| Tuần 11–12 | 29/06 – 10/07 | Phản hồi người dùng, cải tiến, nộp bài cuối |

### 6. Ước tính chi phí
#### Chi phí hạ tầng (Hàng tháng)
| Dịch vụ | Chi phí ước tính |
|---------|-----------------|
| AWS Lambda | ~$0.00 (free tier) |
| Amazon S3 | ~$0.23 |
| Amazon CloudFront | ~$0.10 |
| Amazon Cognito | ~$0.00 (free tier, <50k users) |
| AWS Polly | ~$4.00 (trả theo ký tự) |
| Amazon SES | ~$0.10 |
| Amazon RDS (PostgreSQL) | ~$15.00 (db.t3.micro) |
| **Tổng** | **~$19.43/tháng** |

### 7. Đánh giá rủi ro
| Rủi ro | Tác động | Xác suất | Giảm thiểu |
|--------|----------|----------|------------|
| Độ trễ cold start | Trung bình | Trung bình | Provisioned concurrency cho Lambda functions quan trọng |
| Độ chính xác OCR cho tài liệu scan | Trung bình | Trung bình | Fallback sang xem xét thủ công trong admin pipeline |
| Sự cố nhà cung cấp thanh toán | Cao | Thấp | Chế độ mock provider cho development; abstraction layer không phụ thuộc provider |
| Vượt chi phí | Trung bình | Thấp | AWS Budget alerts và giám sát chi phí qua Cost Explorer |

### 8. Kết quả mong đợi
#### Deliverables kỹ thuật
- Nền tảng thi IELTS/TOEIC production-ready triển khai trên AWS.
- 21 backend API handler modules với test coverage toàn diện.
- Admin content pipeline hỗ trợ nhập DOCX, PDF, và scanned-PDF.
- Tất cả 6 lệnh CI/CD verification đều pass green.

#### Kết quả học tập
- Kinh nghiệm thực tế với 8+ dịch vụ AWS trong bối cảnh production.
- Kỹ năng phát triển full-stack với framework hiện đại (Next.js 15, Express, TypeScript).
- DevOps practices: CI/CD, infrastructure-as-code (SAM/CloudFormation), monitoring.
- Quản lý dự án: thiết kế kiến trúc, tài liệu và phối hợp nhóm.
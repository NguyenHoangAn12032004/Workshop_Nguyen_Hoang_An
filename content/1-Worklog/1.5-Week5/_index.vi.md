---
title: "Tuần 5 Worklog"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---




### Mục tiêu tuần 5:

* Khởi động dự án LingoRise: viết proposal, thiết kế kiến trúc, thiết lập repository.
* Hoàn thiện các quyết định công nghệ và xác định AWS service stack.

### Nhiệm vụ sẽ được thực hiện trong tuần này:

| Ngày | Nhiệm vụ | Bắt đầu ngày | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | -------- | ------------- | --------------- | ------------------- |
| 2 | - Nghiên cứu các nền tảng IELTS/TOEIC hiện có để xác định khoảng trống <br> - Soạn thảo đề xuất dự án với phát biểu vấn đề và đối tượng mục tiêu | 18/05/2026 | 18/05/2026 | |
| 3 | - Thiết kế kiến trúc hệ thống: <br>&emsp; + Xác định dịch vụ AWS: Lambda, API Gateway, S3, CloudFront, Cognito, RDS, Polly <br>&emsp; + Vẽ sơ đồ kiến trúc | 19/05/2026 | 19/05/2026 | |
| 4 | - Khởi tạo monorepo: <br>&emsp; + `lingorise-app` (Next.js 15 frontend) <br>&emsp; + `lingorise-backend` (Express + TypeScript) <br>&emsp; + Cấu hình ESLint, Prettier, TypeScript | 20/05/2026 | 20/05/2026 | |
| 5 | - Thiết lập AWS SAM `template.yaml`: <br>&emsp; + Định nghĩa Lambda function <br>&emsp; + API Gateway proxy routing <br>&emsp; + 30+ parameters cho cấu hình linh hoạt <br> - Thiết kế database schema PostgreSQL ban đầu | 21/05/2026 | 21/05/2026 | |
| 6 | - Tạo các database migrations đầu tiên (nền tảng schema) <br> - Thiết lập Cognito User Pool cho xác thực <br> - Test workflow cơ bản `sam build && sam deploy` | 22/05/2026 | 22/05/2026 | |

### Tuần 5 Thành tựu:

* Hoàn thành đề xuất dự án với:
    * Phát biểu vấn đề: khoảng trống nền tảng luyện thi IELTS/TOEIC bằng tiếng Việt
    * Kiến trúc giải pháp: AWS serverless stack
    * Ước tính chi phí: ~$19.43/tháng
    * Lịch trình: kế hoạch triển khai 8 tuần

* Thiết kế và tài liệu hóa kiến trúc hệ thống hoàn chỉnh:
    * Frontend: Next.js 15 với App Router
    * Backend: Express.js trên Lambda qua `serverless-http`
    * Database: PostgreSQL 14+ trên RDS
    * Lưu trữ: S3 + CloudFront với signed URLs
    * Xác thực: Cognito với JWT-based role authorization

* Khởi tạo monorepo với tooling phù hợp:
    * Cấu hình TypeScript cho cả frontend và backend
    * ESLint + Prettier cho chất lượng code
    * Git repository với chiến lược branching

* Tạo SAM template với 30+ parameters có thể cấu hình cho triển khai đa môi trường.

* Thiết kế database schema ban đầu với các bảng nền tảng (users, exams, questions).

* ...

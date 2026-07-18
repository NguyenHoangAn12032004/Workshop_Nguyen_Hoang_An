---
title: "Tuần 8 Worklog"
date: 2026-06-08
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---




### Mục tiêu tuần 8:

* Hoàn thành tích hợp end-to-end tất cả các thành phần.
* Tăng cường bảo mật: validation, CSRF, rate limiting.
* Triển khai phiên bản beta lên AWS production.

### Nhiệm vụ sẽ được thực hiện trong tuần này:

| Ngày | Nhiệm vụ | Bắt đầu ngày | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | -------- | ------------- | --------------- | ------------------- |
| 2 | - Tích hợp Frontend–Backend: <br>&emsp; + Kết nối tất cả React Query hooks đến backend APIs <br>&emsp; + Triển khai luồng auth token refresh <br>&emsp; + Test navigation và routing | 08/06/2026 | 08/06/2026 | |
| 3 | - Tăng cường bảo mật: <br>&emsp; + Input validation trên tất cả API endpoints <br>&emsp; + Cấu hình CORS whitelist <br>&emsp; + Rate limiting middleware <br>&emsp; + Phòng chống SQL injection qua parameterized queries | 09/06/2026 | 09/06/2026 | |
| 4 | - Tích hợp thanh toán PayOS: <br>&emsp; + Luồng checkout cho đăng ký premium <br>&emsp; + Xử lý webhook xác nhận thanh toán <br>&emsp; + Quản lý trạng thái đăng ký <br>&emsp; + Admin mark-paid thủ công cho demo | 10/06/2026 | 10/06/2026 | |
| 5 | - Triển khai beta: <br>&emsp; + `sam build && sam deploy` lên production AWS <br>&emsp; + Frontend deploy lên AWS Amplify <br>&emsp; + Database migration trên production RDS <br>&emsp; + Thiết lập CloudFront distribution | 11/06/2026 | 11/06/2026 | |
| 6 | - Beta testing và sửa lỗi: <br>&emsp; + Kiểm thử luồng end-to-end (đăng ký → thi → review) <br>&emsp; + Sửa lỗi deployment và environment variable <br>&emsp; + Đo baseline hiệu năng | 12/06/2026 | 12/06/2026 | |

### Tuần 8 Thành tựu:

* Hoàn thành tích hợp frontend–backend:
    * Tất cả API endpoints kết nối với React Query hooks
    * Auth token refresh hoạt động trơn tru
    * Navigation và deep-linking hoạt động

* Triển khai biện pháp bảo mật toàn diện:
    * Input validation trên tất cả 21 handler groups
    * Cấu hình CORS với strict origin whitelist
    * Rate limiting ngăn lạm dụng
    * Parameterized SQL queries xuyên suốt

* Tích hợp hệ thống thanh toán PayOS:
    * Luồng checkout cho mua đăng ký premium
    * Webhook receiver cho xác nhận thanh toán bất đồng bộ
    * Quản lý vòng đời đăng ký
    * Admin fallback cho mark-paid thủ công

* **Cột mốc:** Triển khai thành công beta lên production AWS:
    * Lambda backend qua SAM deployment
    * Next.js frontend trên AWS Amplify
    * PostgreSQL database trên Amazon RDS
    * CloudFront CDN cho static assets

* Hoàn thành vòng beta testing đầu tiên với bug fixes cho vấn đề cấu hình environment.

* ...

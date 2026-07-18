---
title: "Tuần 6 Worklog"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---




### Mục tiêu tuần 6:

* Xây dựng các tính năng core: xác thực, exam engine, và bảng điều khiển học viên.
* Tích hợp luồng xác thực Cognito với frontend.

### Nhiệm vụ sẽ được thực hiện trong tuần này:

| Ngày | Nhiệm vụ | Bắt đầu ngày | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | -------- | ------------- | --------------- | ------------------- |
| 2 | - Triển khai auth handlers với Cognito ở backend: <br>&emsp; + Đăng ký và đăng nhập <br>&emsp; + JWT token verification middleware <br>&emsp; + Role-based access control (learner/admin) | 25/05/2026 | 25/05/2026 | |
| 3 | - Xây dựng exam engine backend API: <br>&emsp; + Tạo phiên thi <br>&emsp; + Gửi câu trả lời với lưu trạng thái <br>&emsp; + Tính điểm với Cambridge band mapping | 26/05/2026 | 26/05/2026 | |
| 4 | - Phát triển dashboard API cho học viên: <br>&emsp; + Endpoint gợi ý hàng ngày <br>&emsp; + Lịch sử thi và theo dõi tiến độ <br>&emsp; + Phân tích mẫu lỗi sai | 27/05/2026 | 27/05/2026 | |
| 5 | - Xây dựng frontend: <br>&emsp; + Trang Login/Register tích hợp Cognito <br>&emsp; + Dashboard layout với React Query data fetching <br>&emsp; + Navigation responsive với TailwindCSS | 28/05/2026 | 28/05/2026 | |
| 6 | - Tích hợp frontend → backend → Cognito <br> - Test luồng xác thực end-to-end <br> - Sửa lỗi CORS giữa Amplify frontend và API Gateway | 29/05/2026 | 29/05/2026 | |

### Tuần 6 Thành tựu:

* Triển khai hệ thống xác thực hoàn chỉnh:
    * Đăng ký với xác minh email (Cognito)
    * Đăng nhập với JWT token
    * Middleware phân quyền: learner vs admin routes
    * Token refresh và quản lý session

* Xây dựng exam engine với:
    * Tạo và lưu trạng thái phiên thi (khả năng resume)
    * Gửi câu trả lời với auto-save
    * Thuật toán Cambridge band scoring cho IELTS
    * Tính điểm TOEIC

* Tạo dashboard APIs cho học viên:
    * Engine gợi ý bài thi hàng ngày
    * Phân tích hiệu suất lịch sử
    * Phân loại mẫu lỗi sai

* Phát triển frontend pages responsive:
    * Login/Register tích hợp Cognito
    * Dashboard với data visualization (React Query + charts)
    * TailwindCSS responsive layout

* Giải quyết vấn đề cross-origin giữa Amplify-hosted frontend và API Gateway backend.

* ...

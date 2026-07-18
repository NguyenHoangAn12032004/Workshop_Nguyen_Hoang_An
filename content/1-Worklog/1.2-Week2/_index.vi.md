---
title: "Tuần 2 Worklog"
date: 2026-04-27
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---




### Mục tiêu tuần 2:

* Nắm vững các dịch vụ lưu trữ, cơ sở dữ liệu và quản lý danh tính trên AWS.
* Thực hành kết nối ứng dụng qua nhiều dịch vụ AWS.

### Nhiệm vụ sẽ được thực hiện trong tuần này:

| Ngày | Nhiệm vụ | Bắt đầu ngày | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | -------- | ------------- | --------------- | ------------------- |
| 2 | - Tìm hiểu và thực hành với dịch vụ lưu trữ đối tượng Amazon S3: <br>&emsp; + Bucket Policy <br>&emsp; + Versioning <br>&emsp; + Lifecycle | 27/04/2026 | 27/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Nghiên cứu về các dịch vụ cơ sở dữ liệu trên AWS: <br>&emsp; + Amazon RDS (Relational) <br>&emsp; + Amazon DynamoDB (NoSQL) | 28/04/2026 | 28/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Tìm hiểu về AWS IAM: <br>&emsp; + Quản lý User, Group, Role <br>&emsp; + Tạo và gắn Policy <br>&emsp; + Bảo mật tài nguyên Cloud theo nguyên tắc least-privilege | 29/04/2026 | 29/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Thực hành: <br>&emsp; + Kết nối ứng dụng từ EC2 đến RDS <br>&emsp; + Lưu trữ tài liệu/hình ảnh lên S3 | 30/04/2026 | 30/04/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Kiểm thử tích hợp end-to-end: EC2 → RDS → S3 <br> - Tổng hợp và ghi lại bài học trong tuần | 01/05/2026 | 01/05/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Tuần 2 Thành tựu:

* Nắm vững quản lý Amazon S3:
    * Bucket Policies cho kiểm soát truy cập
    * Versioning cho lịch sử đối tượng
    * Lifecycle transitions cho tối ưu chi phí
    * Cross-origin resource sharing (CORS)

* Hiểu sự khác biệt giữa cơ sở dữ liệu quan hệ và NoSQL trên AWS:
    * Amazon RDS: cơ sở dữ liệu quan hệ được quản lý (MySQL, PostgreSQL, v.v.)
    * Amazon DynamoDB: NoSQL fully managed với độ trễ mili giây

* Cấu hình AWS IAM với bảo mật đúng chuẩn:
    * Tạo Users, Groups và Roles
    * Gắn Policies theo nguyên tắc least-privilege
    * Hiểu sự khác biệt giữa identity-based và resource-based policies

* Kết nối thành công ứng dụng trên EC2 đến RDS và triển khai lưu trữ file trên S3 cho tài liệu và hình ảnh.

* Có khả năng kết hợp nhiều dịch vụ AWS trong một kiến trúc tích hợp.

* ...

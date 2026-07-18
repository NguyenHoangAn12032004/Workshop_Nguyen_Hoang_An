---
title: "Tuần 3 Worklog"
date: 2026-05-04
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---




### Mục tiêu tuần 3:

* Tìm hiểu chuyên sâu kiến trúc Serverless với Lambda và API Gateway.
* Khám phá các dịch vụ AI của AWS cho xử lý dữ liệu thông minh.

### Nhiệm vụ sẽ được thực hiện trong tuần này:

| Ngày | Nhiệm vụ | Bắt đầu ngày | Ngày hoàn thành | Tài liệu tham khảo |
| ---- | -------- | ------------- | --------------- | ------------------- |
| 2 | - Tìm hiểu kiến trúc Serverless chuyên sâu <br> - Hiểu AWS Lambda: tạo function, triggers, layers | 04/05/2026 | 04/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Xây dựng RESTful API sử dụng AWS Lambda + Amazon API Gateway: <br>&emsp; + Tạo API endpoints <br>&emsp; + Cấu hình request/response mapping | 05/05/2026 | 05/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Nghiên cứu các dịch vụ AI hỗ trợ xử lý dữ liệu thông minh: <br>&emsp; + Amazon Textract: trích xuất văn bản và dữ liệu từ tài liệu quét <br>&emsp; + Amazon Comprehend: phân tích thực thể và cảm xúc trong văn bản | 06/05/2026 | 06/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Cấu hình S3 Event Notifications để tự động kích hoạt Lambda khi có tệp tin mới được tải lên | 07/05/2026 | 07/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Tích hợp end-to-end: S3 → Lambda → Textract pipeline <br> - Kiểm thử và xác minh luồng event-driven hoàn chỉnh | 08/05/2026 | 08/05/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Tuần 3 Thành tựu:

* Xây dựng RESTful APIs sử dụng AWS Lambda và API Gateway:
    * Tạo Lambda functions với Node.js runtime
    * Cấu hình API Gateway với proxy integration
    * Xử lý request/response mapping templates

* Có kinh nghiệm thực tế với Amazon Textract:
    * Trích xuất văn bản từ tài liệu quét bằng OCR
    * Phân tích dữ liệu có cấu trúc (bảng, biểu mẫu) từ PDFs
    * Ứng dụng trực tiếp vào pipeline nhập nội dung LingoRise sau này

* Khám phá Amazon Comprehend cho phân tích văn bản:
    * Nhận diện thực thể trong văn bản
    * Khả năng phân tích cảm xúc

* Cấu hình kiến trúc event-driven:
    * S3 upload events tự động kích hoạt Lambda functions
    * Xây dựng pipeline xử lý hoàn chỉnh S3 → Lambda → Textract

* Hiểu sức mạnh của serverless trong việc giảm chi phí vận hành mà vẫn duy trì khả năng mở rộng.

* ...

---
title: "Blog 1: Tối ưu IaC với CloudFormation Express Mode"
date: 2026-07-22
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
description: "Cách AWS CloudFormation Express mode tối ưu hóa quy trình phát triển hạ tầng, tăng tốc độ triển khai lên tới 400%."
tags: ["AWS", "CloudFormation", "ExpressMode", "IaC", "CDK", "DevOps"]
---

# Tối Ưu Hóa Quy Trình Phát Triển Hạ Tầng Với AWS CloudFormation Express Mode

*Bài viết này chia sẻ góc nhìn và cảm nhận của tôi - một sinh viên thực tập tại chương trình **AWS First Cloud AI Journey** - về tính năng CloudFormation Express mode mới được ra mắt.*

---

## 📌 Khởi Đầu Hành Trình

Trong kỷ nguyên của **DevSecOps** và **Kiến trúc hướng Agent (Agentic Architecture)**, tốc độ phản hồi của hạ tầng (feedback loop) đóng vai trò quyết định đến hiệu suất của lập trình viên. Là một sinh viên thực tập xây dựng các đồ án Microservices lớn trong chương trình **AWS First Cloud AI Journey**, một trong những điểm nghẽn lớn nhất mà tôi thường gặp phải chính là thời gian chờ đợi hệ thống kiểm tra độ ổn định của CloudFormation mỗi khi cập nhật tài nguyên.

Nhận thức được điểm nghẽn này, AWS đã chính thức giới thiệu **CloudFormation Express mode** – giải pháp tăng tốc độ triển khai lên đến **400% (4x)**.

---

## 🏛️ Hai Tư Duy Kiến Trúc Triển Khai: Đồng Bộ vs Bất Đồng Bộ

Thông qua giải pháp mới này, chúng ta có thể phân tích hai tư duy kiến trúc triển khai hạ tầng:

### 1. Standard Mode (Triển Khai Đồng Bộ - Synchronous)
CloudFormation đảm bảo tính toàn vẹn tuyệt đối bằng cách thực hiện các bước kiểm tra độ ổn định (stabilization checks). Tiến trình chỉ hoàn thành khi tài nguyên sẵn sàng hoàn toàn tiếp nhận lưu lượng. Đây là tiêu chuẩn bắt buộc cho môi trường **Production** nhằm giảm thiểu rủi ro downtime.
* **Ưu điểm:** Đảm bảo an toàn tuyệt đối cho hệ thống đang chạy.
* **Nhược điểm:** Tốn nhiều thời gian chờ đợi trong quá trình phát triển (development).

### 2. Express Mode (Triển Khai Bất Đồng Bộ - Asynchronous)
Tối ưu hóa cho môi trường **Phát triển lặp (Iterative Development)** và **AI-assisted IaC**. Tiến trình triển khai sẽ trả về trạng thái Hoàn thành (Complete) ngay khi cấu hình được áp dụng thành công lên tài nguyên.
* **Xử lý bất đồng bộ:** Các bước khởi tạo chi tiết sẽ được xử lý bất đồng bộ ở background.
* **Cơ chế tự động tái thử nghiệm (Built-in Resilience):** Tự động thử lại nếu xảy ra lỗi phụ thuộc tài nguyên trong quá trình khởi tạo ngầm.

---

## 🛠️ Số Liệu Hiệu Năng Thực Tế & Khả Năng Tích Hợp

### Đo lường hiệu năng thực tế từ AWS
* **Khởi tạo Amazon SQS + DLQ:** Giảm từ **64 giây** xuống còn **dưới 10 giây**.
* **Xóa tài nguyên phức tạp (Lambda + ENI):** Rút ngắn thời gian từ **20–30 phút** xuống chỉ còn **10 giây**.

### Tích hợp cực kỳ linh hoạt
Điểm cốt lõi đáng giá của Express mode là sự linh hoạt trong tích hợp:
* **Không yêu cầu thay đổi cấu trúc Template** hiện tại.
* Hỗ trợ mượt mà qua **AWS CLI** với tham số `--deployment-config`.
* Tích hợp sâu vào **AWS CDK** thông qua flag `--express`.

Đây chắc chắn sẽ là công cụ bổ trợ đắc lực cho các bạn sinh viên khi xây dựng các đồ án Microservices lớn, nơi việc cập nhật và thử nghiệm hạ tầng diễn ra liên tục.

---

## 📊 So Sánh: Standard vs. Express Mode

| Tiêu chí | Standard Mode | Express Mode |
| :--- | :--- | :--- |
| **Cơ chế thực thi** | Đồng bộ (Synchronous) | Bất đồng bộ (Asynchronous) |
| **Trạng thái hoàn thành** | Khi tài nguyên đã ổn định hoàn toàn | Ngay khi áp dụng cấu hình thành công |
| **Môi trường phù hợp** | Production | Development, Testing, Sandbox |
| **Xử lý lỗi** | Rollback khi lỗi stabilization | Tự động thử lại ngầm (built-in resilience) |
| **Hiệu năng cải thiện** | Mặc định | Tăng tốc lên tới **400%** |

---

## 💡 Thảo luận: Đánh đổi độ ổn định lấy tốc độ trong CI/CD?

Đối với các dự án hiện tại của mọi người, liệu việc đánh đổi bước kiểm tra ổn định lấy tốc độ thực thi có phải là phương án tối ưu cho môi trường CI/CD?
* Đối với môi trường **Development/Testing CI/CD**: Rất đáng để áp dụng nhằm rút ngắn feedback loop.
* Đối với môi trường **Production**: Cần cân nhắc kỹ vì tính toàn vẹn và độ ổn định của tài nguyên vẫn là ưu tiên hàng đầu.

Rất mong nhận được những thảo luận chuyên sâu từ các thành viên trong cộng đồng!

---

## 🔗 Tài Liệu Tham Khảo & Thảo Luận
* [Chi tiết cơ chế vận hành của Express mode tại blog chính thức từ AWS](https://aws.amazon.com/vi/blogs/aws/accelerate-your-infrastructure-deployments-by-up-to-4x-with-aws-cloudformation-express-mode/)
* [Tham gia thảo luận chuyên sâu tại AWS Study Group FCJ Facebook Post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2209151649849795)
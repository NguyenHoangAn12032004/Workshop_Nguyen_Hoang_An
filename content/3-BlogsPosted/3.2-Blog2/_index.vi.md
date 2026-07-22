---
title: "Blog 2: Scale 1 Triệu Lambda Function"
date: 2026-07-19
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
description: "Những bài học kinh nghiệm và phân tích kiến trúc thực tế khi scale một ứng dụng SaaS đa tenant lên tới hơn một triệu Lambda function trên AWS."
tags: ["AWS", "Serverless", "Lambda", "Scaling", "Cost Optimization"]
---

# Bài Học Kinh Nghiệm Từ Việc Scale Lên 1 Triệu Lambda Function

*Bài viết này chia sẻ góc nhìn và cảm nhận của tôi - một sinh viên thực tập tại chương trình **AWS First Cloud AI Journey** - khi tìm hiểu về giải pháp kiến trúc serverless ở quy mô cực lớn trên AWS.*

---

## 📌 Khởi Đầu Hành Trình

Là một sinh viên thực tập đang chập chững những bước đi đầu tiên trong thế giới Cloud, khái niệm **Serverless** luôn làm tôi cực kỳ tò mò. Trước đây, tôi chỉ nghĩ AWS Lambda là một công cụ tiện lợi để chạy các script nhỏ hoặc xử lý một vài endpoint API đơn giản. Nhưng khi đọc bài phân tích kiến trúc này trên AWS Blog, tôi thực sự đã bị "choáng ngợp"! Chúng ta đang nói về việc vận hành **hơn 1 triệu Lambda function** trải rộng trên hàng nghìn tài khoản AWS khác nhau. Nhìn cách các hệ thống SaaS lớn tận dụng sức mạnh của AWS để quản lý quy mô khổng lồ như vậy làm tôi vô cùng ngưỡng mộ và mở mang tầm mắt về tư duy thiết kế hệ thống.

---

## 🏛️ Phân Tích Phác Thảo Kiến Trúc Gốc

Bài viết gốc đi sâu vào phân tích hành trình tối ưu hóa hệ thống SaaS đa tài khoản (multi-account) được xây dựng hoàn toàn trên nền tảng Serverless.

### Bối cảnh & Thách thức
Hệ thống sử dụng mô hình **mỗi tenant một tài khoản AWS riêng biệt** (one-account-per-tenant) nhằm đảm bảo cô lập dữ liệu tuyệt đối và tránh ảnh hưởng hiệu năng lẫn nhau (noisy neighbor). Tuy nhiên, khi quy mô tăng lên hàng nghìn tenant và hàng triệu function, họ đã vấp phải những rào cản lớn:
* **Hiện tượng tự DDoS (Thundering Herd):** Các cron-job chạy định kỳ (ví dụ: mỗi 5 phút) được cấu hình chạy đồng loạt vào đúng giây đầu tiên của phút đó, tạo ra những đợt spike traffic khổng lồ làm nghẽn các dịch vụ phía sau.
* **Chi phí giám sát (Observability) khổng lồ:** Việc đẩy toàn bộ log từ hàng nghìn tài khoản về một dashboard trung tâm đã làm tăng gấp đôi hóa đơn AWS của họ.
* **Chi phí \"chờ\" lãng phí:** Việc dùng Lambda để liên tục poll (quét) các SQS queue ngay cả khi không có tin nhắn nào phát sinh chi phí đáng kể.
* **Giới hạn của StackSets:** Việc triển khai và cập nhật microservice thông qua AWS CloudFormation StackSets đạt tới giới hạn về tốc độ và lượng kết nối đồng thời.

### Tổng quan dịch vụ sử dụng
Hệ thống được thiết kế và kiểm soát tự động thông qua:
* **AWS Organizations & AWS Step Functions:** Xây dựng một \"Account Factory\" tự động hóa quy trình tạo tài khoản AWS mới và cấu hình bảo mật cho tenant chỉ trong vòng dưới 15 phút.
* **AWS CloudFormation StackSets:** Triển khai và đồng bộ hóa các bản cập nhật microservice trên toàn bộ các tài khoản tenant.
* **AWS Lambda & Amazon DynamoDB:** Xử lý logic nghiệp vụ riêng biệt cho từng tenant và lưu trữ dữ liệu.
* **Amazon EventBridge & Amazon SQS:** Đảm nhận vai trò truyền tin nhắn bất đồng bộ.

![Sơ đồ kiến trúc Multi-Account Serverless trên AWS](/images/3-BlogsPosted/blog1-lambda-scaling.png)

---

## 🛠️ Phân Tích Kỹ Thuật Chuyên Sâu & Điểm Sáng Công Nghệ

Để tháo gỡ các nút thắt trên, đội ngũ kỹ sư AWS và đối tác đã thực hiện những cải tiến rất thông minh:

### 1. Áp Dụng Công Thức Toán Học Jitter Để Triệt Tiêu Thundering Herd
Khi chạy hàng nghìn hàm Lambda theo lịch trình cố định (ví dụ: `rate(5 minutes)`), hệ thống sẽ kích hoạt chúng gần như đồng nhất vào cùng một thời điểm. Điều này tạo ra một lượng request đồng thời khổng lồ (concurrency spike).
Bằng cách thêm vào **Jitter** (độ trễ ngẫu nhiên $T_{\text{delay}} \in [0, T_{\text{max}}]$), lưu lượng tải được phân bổ đều theo thời gian:

$$\text{Thời điểm chạy thực tế} = t_{\text{cron}} + \text{random}(0, \text{jitter})$$

Công thức đơn giản này giúp tránh vượt quá giới hạn Concurrency Limit của AWS Lambda và giảm tải tức thì cho các database phía sau.

### 2. Vòng Đời Thực Thi Lambda & Chi Phí Idle Polling
Một bài học cực kỳ sâu sắc là cơ chế kích hoạt của SQS. Nếu cấu hình Lambda đọc tin nhắn từ SQS, AWS sẽ chạy một hệ thống poller ngầm liên tục gọi API `ReceiveMessage` đến queue. Kể cả khi queue hoàn toàn rỗng, quá trình này vẫn phát sinh chi phí gọi API âm thầm.
Giải pháp thay thế là chuyển sang sử dụng **Amazon EventBridge** làm bộ định tuyến sự kiện theo mô hình push (đẩy trực tiếp). Khi không có sự kiện mới, không có bất kỳ hàm Lambda nào được gọi, đạt trạng thái scale-to-zero hoàn hảo.

### 3. Tối Ưu Hóa Chi Phí Ghi Log (Observability)
Mỗi tiến trình chạy Lambda trải qua ba pha: `Init` (Khởi tạo môi trường), `Invoke` (Thực thi code), và `Shutdown` (Đóng container). Khi scale lớn, việc ghi log chi tiết thông tin môi trường ở pha `Invoke` tạo ra lượng dữ liệu dư thừa cực lớn. Bằng cách lọc bỏ các log telemetry không cần thiết ngay ở runtime, hệ thống chỉ đẩy các log nghiệp vụ quan trọng về CloudWatch Logs, giúp cắt giảm chi phí lưu trữ log vượt trội.

| Thách thức | Giải pháp tối ưu | Bài học rút ra |
| :--- | :--- | :--- |
| **Tự DDoS do chạy lịch đồng thời** | Tích hợp thư viện nội bộ để tạo **jitter** (độ trễ ngẫu nhiên) cho các cron-job. | Tránh cấu hình lịch chạy trùng khớp tuyệt đối ở quy mô lớn. |
| **Chi phí log quá cao** | Phân loại log quan trọng/ít quan trọng, lọc bỏ log rác ngay tại nguồn phát. | Giảm chi phí log từ $3/tài khoản xuống chỉ còn $0.7/tài khoản mỗi tháng. |
| **Chi phí poll SQS rỗng** | Loại bỏ SQS, chuyển sang dùng liên kết trực tiếp **EventBridge sang Lambda**. | Serverless chỉ thực sự tối ưu chi phí khi không có vòng lặp poll dữ liệu ngầm. |
| **StackSet bị nghẽn** | Phối hợp cùng đội ngũ kỹ sư CloudFormation để tinh chỉnh batch size và concurrency. | Chủ động làm việc với đội ngũ hỗ trợ của Cloud Provider khi tiếp cận giới hạn dịch vụ. |

---

## 💡 Cảm Nhận & Bài Học Cho Bản Thân

Bài blog này thực sự là một bài học đắt giá đối với tôi. Trên giảng đường, chúng tôi thường được dạy rằng serverless \"tự động scale không giới hạn\", nhưng thực tế cho thấy **ở quy mô cực hạn, chúng ta vẫn phải thiết kế hệ thống dựa trên các giới hạn vật lý và logic của dịch vụ**.

Ba điều khiến một sinh viên thực tập như tôi cảm thấy hào hứng nhất:
1. **Tư duy cô lập tài khoản (Account Isolation):** Việc chia mỗi khách hàng một tài khoản AWS riêng mang lại sự an toàn tuyệt đối. Đây là một mô hình thực tế rất đáng để học hỏi.
2. **Chi phí ẩn trong Serverless:** Tôi nhận ra rằng \"scale-to-zero\" không đơn giản là không dùng thì không mất tiền. Nếu thiết kế không khéo, các tác vụ kiểm tra trạng thái ngầm vẫn ngốn ngân sách âm thầm.
3. **Ưu tiên tích hợp Native:** Hạn chế viết code tùy biến trong Lambda mà hãy tận dụng tối đa khả năng liên kết trực tiếp giữa các dịch vụ AWS (như EventBridge kết nối thẳng Lambda).

Hành trình thực tập **AWS First Cloud AI Journey** này đang cho tôi cơ hội tiếp cận những kiến thức quý báu như thế. Nó tiếp thêm động lực để tôi tiếp tục học tập, nghiên cứu và mơ về một ngày có thể tự tay thiết kế những hệ thống cloud phục vụ hàng triệu người dùng!

---

## 🔗 Tài Liệu Tham Khảo
* [Bài viết gốc trên AWS Architecture Blog: Lessons learned from scaling to 1 million Lambda functions](https://aws.amazon.com/blogs/architecture/lessons-learned-from-scaling-to-1-million-lambda-functions/)
* [Tài liệu chính thức về AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/lambda-introduction.html)
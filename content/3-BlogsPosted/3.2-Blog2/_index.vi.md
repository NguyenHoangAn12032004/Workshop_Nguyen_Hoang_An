---
title: "Blog 2: Scale 121M Kết Nối gRPC"
date: 2026-07-19
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
description: "Cách bitdrift tận dụng Amazon CloudFront và Route 53 để scale hệ thống telemetry lên tới 121 triệu kết nối gRPC đồng thời trong các sự kiện thể thao trực tiếp."
tags: ["AWS", "Networking", "gRPC", "CloudFront", "Route 53", "Scaling"]
---

# Cách Bitdrift Scale Đạt 121 Triệu Kết Nối gRPC Đồng Thời Trên Amazon CloudFront

*Bài viết này chia sẻ góc nhìn và cảm nhận của tôi - một sinh viên thực tập tại chương trình **AWS First Cloud AI Journey** - khi tìm hiểu về giải pháp kiến trúc mạng quy mô lớn trên AWS.*

---

## 📌 Khởi Đầu Hành Trình

Hãy tưởng tượng bạn đang xem một trận bóng đá trực tiếp cùng hàng triệu người hâm mộ khác và ứng dụng của bạn liên tục cập nhật số liệu thống kê thời gian thực theo từng giây. Phía sau giao diện mượt mà đó là cả một bầu trời công nghệ. Khi đọc về con số **121 triệu kết nối gRPC đồng thời** được xử lý trên AWS, tôi thực sự đã vô cùng kinh ngạc. Ở trường, tôi chỉ quen với việc dựng các API HTTP đơn giản, nhưng việc giữ kết nối persistent gRPC hai chiều cho hơn 100 triệu thiết bị là một đẳng cấp hoàn toàn khác. Bài viết kiến trúc này từ AWS đã chỉ ra cách thiết kế định tuyến và phân phối lưu lượng một cách thông minh để giải quyết bài toán giao thông mạng cực lớn.

---

## 🏛️ Phân Tích Kiến Trúc Gốc

Bài viết gốc tìm hiểu cách **bitdrift**, một nền tảng thu thập dữ liệu telemetry di động thời gian thực, thiết kế hạ tầng để thu thập dữ liệu từ hàng triệu thiết bị di động đồng thời trong các sự kiện thể thao lớn.

### Bối cảnh & Thách thức
Khác với API HTTP truyền thống (vốn không giữ trạng thái và diễn ra nhanh chóng), hệ thống telemetry phụ thuộc vào **gRPC chạy trên nền HTTP/2**, đòi hỏi các kết nối TCP phải được duy trì liên tục (persistent). điều này gây ra các thách thức lớn:
* **Hiện tượng nghẽn cục bộ (Hotspotting):** Nếu tất cả client phân giải DNS ra cùng một địa chỉ IP, một vài server sẽ bị quá tải trong khi các server khác lại rảnh rỗi.
* **Hạn chế của bộ nhớ đệm DNS (DNS Caching):** Bộ nhớ đệm DNS trên thiết bị client ngăn cản việc phân phối lại tải một cách linh hoạt.
* **Thời gian chuẩn bị (Pre-warming) của ALB:** Việc mở rộng các bộ cân bằng tải Application Load Balancer (ALB) truyền thống để đáp ứng tức thì hàng trăm triệu kết nối đòi hỏi quy trình phối hợp phức tạp và mất thời gian với đội ngũ hỗ trợ của AWS.

### Tổng quan dịch vụ sử dụng
Hệ thống giải quyết các vấn đề trên bằng cách kết hợp định tuyến biên và cân bằng tải:
* **Amazon CloudFront:** Thực hiện chấm dứt kết nối mã hóa (TLS Termination) ngay tại các Edge Location của AWS gần người dùng nhất, giúp giảm thiểu độ trễ tối đa. CloudFront cũng tái sử dụng kết nối HTTP/2 để giảm tải cho máy chủ gốc.
* **Amazon Route 53 với Multi-Value Answer (MVA) Routing:** Khi client yêu cầu phân giải tên miền, Route 53 sẽ trả về ngẫu nhiên 8 địa chỉ IP từ danh sách các mục tiêu khỏe mạnh, giúp phân tán đều các kết nối.
* **Network Load Balancers (NLB):** Xử lý hàng triệu gói tin TCP mỗi giây ở layer 4 và chuyển hướng trực tiếp đến các cụm Envoy proxy trên Amazon EKS.
* **Envoy Proxies:** Đóng vai trò bộ định tuyến gRPC nội bộ, điều phối request đến các database xử lý telemetry.

![Sơ đồ kiến trúc gRPC Telemetry Ingestion trên AWS](/images/3-BlogsPosted/blog2-grpc-scaling.png)

---

## 🛠️ Phân Tích Kỹ Thuật Chuyên Sâu & Điểm Sáng Công Nghệ

Để hiểu rõ cách bitdrift đạt được quy mô này, chúng ta cần phân tích sâu hơn về các giao thức mạng:

### 1. HTTP/2 Multiplexing vs. HTTP/1.1 Persistent Connections
Trong HTTP/1.1, mỗi request đồng thời cần một kết nối TCP riêng biệt, tạo ra lượng overhead cực kỳ lớn cho CPU. HTTP/2 giải quyết vấn đề này thông qua cơ chế **multiplexing** (đa luồng), cho phép gửi nhiều request và response đồng thời trên duy nhất một kết nối TCP.
gRPC phụ thuộc hoàn toàn vào HTTP/2. Tuy nhiên, việc duy trì hàng triệu kết nối TCP mở trên máy chủ backend sẽ làm cạn kiệt tài nguyên RAM của hệ thống (do bộ đệm socket TCP).
Đẩy toàn bộ tác vụ TLS/TCP Terminate ra các **Amazon CloudFront** Edge Locations giúp chuyển giao gánh nặng bộ nhớ và xử lý mã hóa sang mạng lưới Edge toàn cầu của AWS.

### 2. Định Tuyến Route 53 Multi-Value Answer (MVA) vs. Round-Robin DNS
DNS Round-Robin thông thường trả về danh sách các IP theo thứ tự tĩnh. Nếu client lưu cache IP đầu tiên, toàn bộ traffic sẽ đổ dồn vào IP đó gây quá tải.
Route 53 **Multi-Value Answer (MVA)** trả về ngẫu nhiên 8 địa chỉ IP khỏe mạnh từ một tập hợp lớn các record. Khi kết hợp với cơ chế chọn ngẫu nhiên phía client và cấu hình DNS TTL ngắn (TTL = 60 giây), client sẽ liên tục thay đổi IP đích, giúp phân bổ tải cực kỳ đều trên toàn bộ các NLB phía sau mà không cần một hệ thống load balancer tập trung lớn làm bottleneck.

$$\text{Tải trung bình mỗi IP} \approx \frac{\text{Tổng lưu lượng}}{N_{\text{IP khả dụng}}}$$

### 3. Kiến Trúc NLB Pass-Through hiệu năng cao
Bộ cân bằng tải Application Load Balancer (ALB) hoạt động ở tầng 7 (Application) để đọc HTTP header, tiêu tốn rất nhiều CPU/RAM và cần quy trình "pre-warm" để chịu tải lớn. Trong khi đó, Network Load Balancer (NLB) hoạt động ở tầng 4 (Transport), chỉ chuyển tiếp các gói tin TCP trực tiếp tới máy chủ đích. Điều này giúp NLB chịu được hàng triệu kết nối tăng đột biến chỉ trong tích tắc mà không cần thời gian chuẩn bị trước.

| Thách thức lớn | Giải pháp áp dụng | Bài học kiến trúc |
| :--- | :--- | :--- |
| **Bão kết nối TLS/TCP ban đầu** | Đẩy tác vụ TLS termination ra **Amazon CloudFront** ở biên. | Xử lý mã hóa càng gần người dùng càng giảm tải cho server gốc và tăng tốc độ kết nối. |
| **Lưu lượng phân tán không đều** | Cấu hình **Route 53 Multi-Value Answer (MVA)** trả về 8 IP ngẫu nhiên. | Định tuyến MVA là giải pháp cực kỳ hiệu quả để thực hiện load balancing phía client mà không cần một node trung gian cồng kềnh. |
| **Khả năng mở rộng tức thì** | Sử dụng **Network Load Balancer (NLB)** thay thế cho ALB. | NLB hoạt động ở tầng TCP, có thể chịu tải hàng triệu request tăng đột biến mà không cần quy trình "pre-warming" phức tạp. |
| **Kết nối gRPC bị kẹt cố định** | Thiết lập Envoy tự động đóng kết nối cũ sau một khoảng thời gian tối đa. | Việc định kỳ làm mới kết nối dài hạn giúp hệ thống tái phân phối tải đều hơn trên toàn cụm server. |

---

## 💡 Cảm Nhận & Bài Học Cho Bản Thân

Đối với một sinh viên thực tập trong chương trình **AWS First Cloud AI Journey**, kiến trúc này mang lại cho tôi những bài học vô giá về tầng mạng (Networking) trong hệ thống Cloud:

1. **CDN không chỉ để lưu trữ file tĩnh:** Trước đây tôi luôn nghĩ CloudFront chỉ dùng để cache hình ảnh hoặc file JS/CSS. Việc biết CloudFront có thể proxy và tối ưu hóa luồng dữ liệu gRPC động ở quy mô hàng trăm triệu kết nối thực sự là một phát hiện tuyệt vời đối với tôi.
2. **DNS cũng là một bộ cân bằng tải:** Tận dụng Route 53 MVA là một cách tiếp cận cực kỳ thông minh. Thay vì dồn mọi gánh nặng vào một bộ cân bằng tải trung tâm lớn, chúng ta phân tán nó ngay từ bước phân giải IP.
3. **Bài học về việc ngắt kết nối chủ động:** Bình thường chúng ta luôn tìm cách tối ưu để giữ kết nối càng lâu càng tốt. Nhưng ở quy mô cực lớn, việc chủ động đóng và bắt client kết nối lại định kỳ là cách duy nhất để duy trì sự cân bằng tải trên hệ thống.

Sự thành công của kiến trúc này thúc đẩy tôi muốn tìm hiểu sâu hơn về Route 53 và CloudFront. Đây thực sự là một tài liệu học tập tuyệt vời cho chặng đường phát triển sắp tới của tôi!

---

## 🔗 Tài Liệu Tham Khảo
* [Bài viết gốc trên AWS Architecture Blog: How bitdrift scaled to 121 million concurrent gRPC connections](https://aws.amazon.com/blogs/architecture/how-bitdrift-scaled-to-121-million-concurrent-grpc-connections-on-amazon-cloudfront-for-live-telemetry-sporting-events/)
* [Tài liệu hướng dẫn về Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/welcome.html)
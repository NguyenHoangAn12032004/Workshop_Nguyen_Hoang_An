---
title: "Blog 3: Concurrency 121M kết nối gRPC"
date: 2026-07-19
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
description: "Cách bitdrift tận dụng Amazon CloudFront và Route 53 để scale hệ thống telemetry lên tới 121 triệu kết nối gRPC đồng thời trong các sự kiện thể thao trực tiếp."
tags: ["AWS", "Networking", "gRPC", "CloudFront", "Route 53", "Scaling"]
---

# Cách Bitdrift Scale Lên 121 Triệu Kết Nối gRPC Đồng Thời Trên Amazon CloudFront

*Bài viết này chia sẻ góc nhìn và cảm nhận của tôi - một sinh viên thực tập tại chương trình **AWS First Cloud AI Journey** - khi tìm hiểu về giải pháp kiến trúc serverless ở quy mô cực lớn trên AWS.*

---

## 📌 Khởi Đầu Hành Trình

Hãy tưởng tượng bạn đang xem một trận bóng đá trực tiếp cùng với hàng triệu người hâm mộ khác và ứng dụng của bạn liên tục nhận được thông tin cập nhật telemetry theo thời gian thực mỗi giây. Đằng sau hậu trường, sự phức tạp về mặt kỹ thuật là vô cùng lớn. Khi tôi đọc về **121 triệu kết nối gRPC đồng thời** được xử lý trên AWS, tôi đã thực sự kinh ngạc. Là một sinh viên cloud, tôi đã từng thiết lập các HTTP server đơn giản, nhưng việc scale các kết nối gRPC song hướng, lâu dài cho hơn một trăm triệu người dùng đồng thời lại là một đẳng cấp hoàn toàn khác. Bài viết phân tích kiến trúc từ AWS này chỉ ra cách thiết kế hệ thống định tuyến thông minh và CDN có thể chinh phục các đỉnh lưu lượng truy cập cực hạn như thế nào.

---

## 🏛️ Phân Tích Kiến Trúc Gốc

Bài viết gốc đi sâu vào phân tích cách **bitdrift**, một nền tảng thu thập dữ liệu telemetry di động thời gian thực, thiết kế hạ tầng của họ để tiếp nhận dữ liệu telemetry từ hàng triệu thiết bị di động đồng thời trong các sự kiện thể thao có lượng người xem khổng lồ.

### Bối cảnh & Thách thức
Các API HTTP truyền thống là stateless (không lưu trạng thái) và diễn ra trong thời gian ngắn. Tuy nhiên, hệ thống telemetry lại dựa trên **gRPC chạy qua HTTP/2**, sử dụng các kết nối TCP duy trì lâu dài (persistent, long-lived connections).
* **Kết nối bị tập trung quá tải (Connection Hotspotting):** Nếu tất cả các client cùng phân giải DNS ra một địa chỉ IP duy nhất, một số máy chủ sẽ bị quá tải trong khi số khác lại nhàn rỗi.
* **Nút thắt bộ nhớ cache DNS (DNS Caching):** Cơ chế lưu cache DNS thông thường ngăn cản việc phân phối lại tải một cách linh hoạt giữa các client.
* **Thời gian chờ khởi động (Pre-warming):** Việc tăng quy mô của các bộ cân bằng tải Application Load Balancer (ALB) truyền thống để xử lý các đợt spike đột ngột lên tới hơn 100 triệu kết nối cần thời gian chuẩn bị rất lâu và phải phối hợp trước với đội ngũ hỗ trợ của AWS.

### Tổng quan dịch vụ sử dụng
Bitdrift giải quyết những thách thức này bằng cách thiết kế một luồng định tuyến lai giữa Edge (Cạnh) và Origin (Gốc):
* **Amazon CloudFront:** Đảm nhận việc kết thúc kết nối bảo mật TLS ngay tại các Edge Location toàn cầu của AWS, giúp giảm thiểu độ trễ cho người dùng cuối. CloudFront cũng xử lý việc tái sử dụng kết nối HTTP/2, đóng vai trò như một lá chắn bảo vệ phía trước.
* **Amazon Route 53 phối hợp Multi-Value Answer (MVA) Routing:** Giải quyết yêu cầu phân giải tên miền bằng cách trả về ngẫu nhiên lên đến 8 địa chỉ IP khỏe mạnh trong nhóm máy chủ, giúp phân phối đều lượng kết nối.
* **Network Load Balancer (NLB):** Xử lý hàng triệu gói tin TCP mỗi giây và định tuyến chúng trực tiếp đến cụm Envoy proxy chạy trên nền Amazon EKS.
* **Envoy Proxy:** Đóng vai trò bộ định tuyến gRPC nội bộ, giúp cân bằng tải các request đến các storage engine lưu trữ dữ liệu.

![Sơ đồ kiến trúc xử lý 121 triệu kết nối gRPC của Bitdrift](/images/3-BlogsPosted/blog2-grpc-scaling.png)

---

## 🛠️ Phân Tích Kỹ Thuật Chuyên Sâu & Điểm Sáng Công Nghệ

Để hiểu cách bitdrift đạt được quy mô này, chúng ta cần tìm hiểu kỹ hơn về các giao thức mạng bên dưới:

### 1. HTTP/2 Multiplexing so với HTTP/1.1 Persistent Connections
Trong HTTP/1.1, mỗi request đồng thời yêu cầu một kết nối TCP riêng biệt, tạo ra lượng overhead rất lớn. HTTP/2 giới thiệu cơ chế **multiplexing** (đa luồng), cho phép gửi đồng thời nhiều request và response trên cùng một kết nối TCP duy nhất.
gRPC hoạt động dựa trên các luồng stream của HTTP/2. Tuy nhiên, việc giữ hàng triệu kết nối TCP mở trên các máy chủ ứng dụng phía sau sẽ làm cạn kiệt tài nguyên bộ nhớ do buffer của socket TCP.
Việc đẩy các kết nối này ra kết thúc tại **Amazon CloudFront** Edge location đã chuyển giao toàn bộ gánh nặng theo dõi trạng thái TLS/TCP sang mạng lưới Edge toàn cầu mạnh mẽ của AWS.

### 2. Định Tuyến Route 53 Multi-Value Answer (MVA) so với DNS Round-Robin
DNS Round-Robin thông thường trả về danh sách IP theo một thứ tự cố định. Khi client lưu cache IP đầu tiên, toàn bộ traffic sẽ đổ dồn vào đó, gây ra hiện tượng lệch tải nghiêm trọng.
Cơ chế **Multi-Value Answer (MVA)** của Route 53 trả về ngẫu nhiên tối đa 8 địa chỉ IP khỏe mạnh từ một danh sách tài nguyên lớn. Khi kết hợp với việc chọn IP ngẫu nhiên phía client và đặt thời gian sống DNS cực ngắn (TTL = 60s), các client sẽ liên tục đổi IP mục tiêu, tạo ra sự phân phối tải gần như hoàn hảo đến các NLB mà không cần thông qua một thiết bị cân bằng tải phần cứng trung tâm nào.

$$\text{Tải trung bình trên mỗi IP} \approx \frac{\text{Tổng lưu lượng}}{N_{\text{IP khỏe mạnh}}}$$

### 3. Kiến Trúc NLB Pass-Through
Các bộ cân bằng tải Application Load Balancer (ALB) truyền thống hoạt động ở Tầng 7 (Application) để phân tích header HTTP. Việc này ngốn rất nhiều CPU và RAM, đòi hỏi phải pre-warm trước khi đón tải lớn. Trong khi đó, Network Load Balancer (NLB) hoạt động ở Tầng 4 (Transport), chuyển tiếp trực tiếp các luồng TCP thô đến các node EKS mà không cần đọc nội dung gói tin. Nhờ vậy, NLB có thể xử lý hàng triệu kết nối mỗi giây tức thì mà không cần bất kỳ sự chuẩn bị trước nào.

| Thách thức lớn | Giải pháp tối ưu | Bài học mạng |
| :--- | :--- | :--- |
| **Bão kết nối TLS/TCP** | Đẩy tác vụ kết thúc TLS ra các **Amazon CloudFront** Edge location. | Xử lý SSL gần người dùng nhất giúp giảm đáng kể độ trễ và tải cho máy chủ gốc. |
| **Phân phối tải lệch** | Sử dụng **Route 53 Multi-Value Answer (MVA)** trả về 8 IP ngẫu nhiên. | Định tuyến MVA là giải pháp cân bằng tải phía client cực kỳ hiệu quả mà không sợ nghẽn ở trung tâm. |
| **Quá tải bộ cân bằng tải** | Sử dụng **Network Load Balancers (NLBs)** thay thế cho ALBs. | NLB định tuyến TCP thô cực nhanh, xử lý hàng triệu gói tin tức thì không cần cấu hình pre-warming. |
| **Kết nối duy trì quá lâu gây lệch tải** | Cấu hình Envoy chủ động ngắt kết nối cũ một cách êm ái sau một thời hạn tối đa. | Chủ động làm mới kết nối giúp tránh việc một máy chủ bị quá tải vĩnh viễn do client bám giữ. |

---

## 💡 Cảm Nhận & Bài Học Cho Bản Thân

Là một sinh viên theo đuổi ngành điện toán đám mây, kiến trúc mạng này đã dạy cho tôi những bài học vô giá:

1. **CDN không chỉ để lưu ảnh tĩnh:** Trước đây tôi chỉ nghĩ CloudFront dùng để cache ảnh hoặc file React. Việc biết CloudFront có thể kết thúc TLS và proxy ngược lưu lượng gRPC thời gian thực ngay tại Edge là một phát hiện cực kỳ thú vị!
2. **Tận dụng DNS làm Load Balancer:** Cơ chế Multi-Value Answer của Route 53 quả thực rất thông minh. Thay vì dồn mọi thứ vào một Load Balancer lớn dễ bị nghẽn cổ chai, ta có thể phân phối IP ngẫu nhiên và để client tự cân bằng tải.
3. **Bài học về việc làm mới kết nối (Connection Recycling):** Ở trường, chúng tôi luôn được dạy giữ kết nối sống càng lâu càng tốt để tăng hiệu năng. Nhưng ở quy mô cực lớn, việc giữ kết nối *vĩnh viễn* lại gây mất cân bằng tải. Học cách ngắt kết nối cũ một cách chủ động là một tư duy thực tế rất hay!

Kiến thức từ case study này thôi thúc tôi thực hành nhiều hơn với Route 53 và CloudFront trong các bài lab cá nhân. Đây thực sự là một bài học mẫu mực về thiết kế mạng phân tán!

---

## 🔗 Tài Liệu Tham Khảo
* [Bài viết gốc trên AWS Architecture Blog: How bitdrift scaled to 121 million concurrent gRPC connections](https://aws.amazon.com/blogs/architecture/how-bitdrift-scaled-to-121-million-concurrent-grpc-connections-on-amazon-cloudfront-for-live-telemetry-sporting-events/)
* [Tài liệu hướng dẫn Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/welcome.html)
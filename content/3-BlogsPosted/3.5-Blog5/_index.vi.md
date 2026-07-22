---
title: "Blog 5: Phát Hiện Gian Lận với EMR Serverless"
date: 2026-07-19
weight: 5
chapter: false
pre: " <b> 3.5. </b> "
description: "Cách MAPFRE USA xây dựng hệ thống dữ liệu lớn tự động hóa, serverless với Amazon EMR Serverless và Apache Iceberg để hiện đại hóa quy trình phát hiện gian lận bảo hiểm."
tags: ["AWS", "Big Data", "EMR Serverless", "Apache Iceberg", "Machine Learning"]
---

# Cách MAPFRE USA Hiện Đại Hóa Quy Trình Phát Hiện Gian Lận Bồi Thường Với Amazon EMR Serverless

*Bài viết này chia sẻ góc nhìn và cảm nhận của tôi - một sinh viên thực tập tại chương trình **AWS First Cloud AI Journey** - khi tìm hiểu về giải pháp tích hợp giữa dữ liệu lớn (Big Data), cơ sở dữ liệu đồ thị và trí tuệ nhân tạo (AI).*

---

## 📌 Khởi Đầu Hành Trình

Là một sinh viên thực tập trong chương trình **AWS First Cloud AI Journey**, tôi luôn cảm thấy vô cùng phấn khích khi chứng kiến cách Điện toán đám mây giao thoa với **Trí tuệ nhân tạo (AI) và Dữ liệu lớn (Big Data)**. Trên giảng đường, việc xử lý dữ liệu thường chỉ dừng lại ở quy mô nhỏ—viết một script python chạy trên file CSV và train model đơn giản. Nhưng ngoài thế giới thực tế, các tập đoàn bảo hiểm lớn như **MAPFRE USA** phải quét qua hàng triệu hồ sơ bồi thường mỗi ngày, phân tích các mạng lưới liên kết phức tạp (như dùng chung số điện thoại hay biển số xe) để phát hiện các đường dây gian lận. Việc tìm hiểu cách họ kết hợp hệ thống xử lý dữ liệu lớn serverless với cơ sở dữ liệu đồ thị và AI có khả năng giải thích (Explainable AI) thực sự đã mở rộng tư duy của tôi!

---

## 🏛️ Phân Tích Kiến Trúc Gốc

MAPFRE USA đã hiện đại hóa nền tảng phát hiện gian lận bảo hiểm của mình bằng cách chuyển đổi từ máy chủ truyền thống đặt tại chỗ (on-premises) sang một pipeline tự động hóa, cloud-native xử lý big data và AI trên AWS.

### Bối cảnh & Thách thức
Để phát hiện gian lận bảo hiểm hiện đại đòi hỏi phải xử lý khối lượng lớn dữ liệu có và không có cấu trúc:
* **Mối quan hệ dữ liệu phức tạp:** Các cơ sở dữ liệu quan hệ truyền thống (SQL) gặp khó khăn trong việc mô hình hóa và truy vấn các đường dây gian lận liên kết chéo (ví dụ: một nhóm người gửi yêu cầu bồi thường khác nhau nhưng dùng chung luật sư, bác sĩ và số điện thoại).
* **Chi phí vận hành lớn:** Việc quản lý các cụm Hadoop hoặc Spark on-premises cho các công việc dữ liệu lớn đòi hỏi đội ngũ quản trị chuyên nghiệp và gây lãng phí lớn khi server nhàn rỗi.
* **Yêu cầu về tính minh bạch của AI:** Khi mô hình Machine Learning gắn cờ một yêu cầu bồi thường là gian lận, các điều tra viên cần những lời giải thích pháp lý rõ ràng về *lý do tại sao* nó bị gắn cờ, chứ không chỉ là một điểm số dự đoán mơ hồ.

### Tổng quan dịch vụ sử dụng
MAPFRE USA xây dựng hệ thống data lakehouse và pipeline ML đầu-cuối bao gồm:
* **Amazon S3 & Apache Iceberg:** Đóng vai trò lớp lưu trữ dữ liệu trung tâm, cho phép thực hiện giao dịch ACID và thay đổi cấu trúc bảng linh hoạt trên hàng triệu bản ghi.
* **Amazon EMR Serverless (Apache Spark):** Thực hiện các tác vụ ETL quy mô lớn, chuẩn bị dữ liệu (feature engineering) và huấn luyện mô hình ML mà không cần quản lý hạ tầng máy chủ.
* **Amazon MWAA (Managed Workflows for Apache Airflow):** Điều phối toàn bộ quy trình từ thu thập, xử lý đến chấm điểm dữ liệu.
* **Cơ sở dữ liệu đồ thị Neo4j:** Mô hình hóa các mối quan hệ giữa người yêu cầu bồi thường, phương tiện, địa chỉ và số điện thoại để trực quan hóa mạng lưới gian lận.
* **SageMaker & SHAP (SHapley Additive exPlanations):** Tính toán điểm đóng góp của từng thuộc tính để giải thích quyết định của mô hình AI cho các điều tra viên.

![Kiến trúc phát hiện gian lận của MAPFRE USA sử dụng EMR Serverless](/images/3-BlogsPosted/blog4-emr-serverless-fraud.png)

---

## 🛠️ Phân Tích Kỹ Thuật Chuyên Sâu & Điểm Sáng Công Nghệ

Để hiểu cách MAPFRE USA vận hành pipeline quy mô doanh nghiệp này, chúng ta cần tìm hiểu sâu về liên kết giữa định dạng lakehouse và machine learning:

### 1. Kiến Trúc Metadata của Apache Iceberg trên Amazon S3
Các data lake truyền thống lưu trữ dữ liệu theo thư mục trên S3 và phải quét toàn bộ thư mục để tìm file khi chạy câu truy vấn. Với hàng triệu bản ghi, việc này làm giảm nghiêm trọng hiệu năng.
**Apache Iceberg** giải quyết triệt để vấn đề này nhờ cấu trúc cây metadata dạng phân cấp:
1. **Iceberg Catalog:** Chỉ định file Metadata File hiện tại của bảng.
2. **Metadata File:** Lưu trữ cấu trúc bảng (schema), phân vùng và trỏ tới danh sách các Manifest List.
3. **Manifest List:** Quản lý các snapshot của bảng và liên kết tới các Manifest File cụ thể.
4. **Manifest File:** Chỉ rõ đường dẫn của các file Parquet chứa dữ liệu thực tế trên S3.
Kiến trúc này cho phép EMR Serverless chạy song song các job Spark đọc-ghi dữ liệu an toàn mà không bị xung đột, hỗ trợ tính năng Time Travel (truy xuất dữ liệu lịch sử) cực kỳ mạnh mẽ.

### 2. Cơ Chế Vận Hành Tính Toán Của Amazon EMR Serverless
Với cụm EMR tự quản lý chạy trên EC2, khi Spark job hoàn thành, các máy chủ ảo vẫn chạy ngầm và ngốn chi phí nếu không được scale down thủ công.
**Amazon EMR Serverless** loại bỏ hoàn toàn việc quản lý máy chủ. Khi một Spark job được kích hoạt:
* Các container tính toán được cấp phát tức thì dựa trên giới hạn được cấu hình trước của job.
* Ngay sau khi job chạy xong, các container này bị hủy bỏ ngay lập tức.
* Cơ chế này loại bỏ hoàn toàn lãng phí tài nguyên nhàn rỗi và giảm 40% chi phí vận hành xử lý dữ liệu lớn.

### 3. Trí Tuệ Nhân Tạo Có Khả Năng Giải Thích (SHAP Values)
Các thuật toán học máy phức tạp (như XGBoost) thường chỉ đưa ra một điểm số xác suất (ví dụ: $92\%$ khả năng gian lận) như một "hộp đen". SageMaker giải quyết việc này bằng cách tích hợp **SHAP (SHapley Additive exPlanations)** dựa trên lý thuyết trò chơi hợp tác (Game Theory).
SHAP tính toán mức đóng góp biên của từng thuộc tính vào kết quả dự đoán:

$$\phi_i(f, x) = \sum_{S \subseteq F \setminus \{i\}} \frac{|S|!(|F| - |S| - 1)!}{|F|!} \Big( f(S \cup \{i\}) - f(S) \Big)$$

Công thức toán học này giúp định lượng chính xác đóng góp của mỗi đặc trưng (ví dụ: `Trùng địa chỉ IP` đóng góp $+20\%$, `Thời gian gửi yêu cầu` đóng góp $+10\%$), giúp các điều tra viên bảo hiểm có đầy đủ căn cứ pháp lý rõ ràng để xử lý vụ việc.

| Thách thức lớn | Giải pháp tối ưu | Bài học kiến trúc |
| :--- | :--- | :--- |
| **Quản lý cụm Spark phức tạp** | Thay thế EC2 tự quản trị bằng **Amazon EMR Serverless**. | Spark serverless tự động co giãn tài nguyên tính toán theo nhu cầu thực tế, giúp triệt tiêu chi phí máy chủ nhàn rỗi. |
| **Tính nhất quán dữ liệu trên S3** | Sử dụng định dạng bảng **Apache Iceberg** trên S3. | Iceberg mang lại khả năng ghi đồng thời an toàn và khôi phục trạng thái dữ liệu theo thời gian (time travel) trên S3. |
| **Phát hiện gian lận mạng lưới** | Tích hợp **cơ sở dữ liệu đồ thị Neo4j** vào pipeline. | Đồ thị (Graph) vượt trội hơn bảng quan hệ truyền thống khi cần truy vấn các mối quan hệ liên kết đa tầng. |
| **Mô hình AI thiếu minh bạch** | Đưa **giá trị SHAP** vào bước suy luận của mô hình ML. | AI có khả năng giải thích (Explainable AI) là điều kiện bắt buộc để con người tin tưởng và phối hợp hiệu quả với máy móc. |

---

## 💡 Cảm Nhận & Bài Học Cho Bản Thân

Bài phân tích kiến trúc này thực sự là một lớp học chất lượng cao về Kỹ thuật dữ liệu (Data Engineering) đối với tôi:

1. **Sức mạnh của Serverless Big Data:** Trước đây, tôi luôn nghĩ việc vận hành Apache Spark cực kỳ phức tạp từ chọn loại EC2, cấu hình cụm máy chủ đến cài đặt auto-scaling. EMR Serverless chứng minh rằng chúng ta có thể khởi chạy các job Spark khổng lồ ngay lập tức mà không cần lo lắng về hạ tầng.
2. **Iceberg nâng tầm S3:** Việc biết Apache Iceberg có thể mang các tính năng của hệ quản trị cơ sở dữ liệu truyền thống (như giao dịch ACID, schema evolution) vào bộ nhớ đối tượng S3 thực sự đã thay đổi góc nhìn của tôi về Data Lake.
3. **AI cần sự tin cậy từ con người:** Đây là phần tôi tâm đắc nhất. Xây dựng một mô hình ML chính xác là chưa đủ; chúng ta phải làm cho nó dễ hiểu đối với con người. Việc dùng các chỉ số SHAP để giải thích nguyên nhân là một bài học thực tế đắt giá mà trường lớp ít khi đề cập đến.

Hành trình thực tập **AWS First Cloud AI Journey** đang giúp tôi tiếp cận những kiến thức sâu sắc như thế này. Nó tạo động lực to lớn giúp tôi tự tin thực hành với AWS Glue, EMR và SageMaker trong các dự án nhỏ của riêng mình!

---

## 🔗 Tài Liệu Tham Khảo
* [Bài viết gốc trên AWS Architecture Blog: How MAPFRE USA modernized fraud claims with Amazon EMR Serverless](https://aws.amazon.com/blogs/architecture/how-mapfre-usa-modernized-fraud-claims-with-amazon-emr-serverless/)
* [Tài liệu hướng dẫn Amazon EMR Serverless](https://docs.aws.amazon.com/emr/latest/EMR-Serverless-User-Guide/emr-serverless.html)
* [Trang chủ Apache Iceberg](https://iceberg.apache.org/)

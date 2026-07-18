---
title: "Sự kiện 3: AWS Vietnam Community Day 2026"
date: 2026-07-07
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# AWS Vietnam Community Day 2026

### Thông tin sự kiện
| | |
|---|---|
| **Tên sự kiện** | AWS Vietnam Community Day 2026 |
| **Ngày** | 23/05/2026 |
| **Địa điểm** | Thành phố Hồ Chí Minh, Việt Nam |
| **Đơn vị tổ chức** | AWS User Group Vietnam |
| **Vai trò** | Người lắng nghe & học hỏi |

### Mô tả sự kiện
AWS Community Day là hội thảo công nghệ do cộng đồng tổ chức nhằm chia sẻ các kiến thức kỹ thuật chuyên sâu về AWS, các thực hành kiến trúc tốt nhất và những xu hướng công nghệ mới. Sự kiện bao gồm nhiều phiên chia sẻ kỹ thuật chất lượng cao và các buổi workshop thực hành về hạ tầng đám mây, DevOps và Generative AI.

### Nội dung chính
Sự kiện quy tụ nhiều phiên chia sẻ chuyên sâu từ các chuyên gia trong cộng đồng:

1. **Hệ thống Multi-Agent cấp Doanh nghiệp cho Xếp hạng Tín dụng Startup** (*Diễn giả: Chị Vy Lâm - Senior Business Systems Analyst @ VPBank*)
   - Phân tích nguyên nhân hệ thống chấm điểm tín dụng ngân hàng truyền thống thất bại khi áp dụng cho startup do sự lệch pha về cấu trúc dữ liệu.
   - Giải pháp sử dụng mô hình Multi-Agent (gồm các agent chuyên biệt: Tài chính, Thị trường, Nhân sự, Rủi ro, Tuân thủ) để cộng tác đưa ra điểm số tín dụng khách quan, tăng tính minh bạch và độ chính xác.
   - Trình bày các trụ cột AI cấp doanh nghiệp: Bảo mật (Cognito/OAuth2), Guardrails (chặn prompt injection), Cô lập mạng (VPC) và Hạ tầng dạng Code (Terraform).

2. **Tính phi định hướng của cấu hình LLM "Định hướng"** (*Diễn giả: Anh Đào Đức - Solution Architect @ Cloud Kinetics*)
   - Lý giải tại sao cấu hình Temperature T=0 thực tế không đảm bảo kết quả đầu ra hoàn toàn giống nhau 100% (do tính chất không kết hợp của phép tính dấu phẩy động trên GPU khi xử lý song song và cơ chế dynamic batching từ nhà cung cấp API).
   - Đề xuất giải pháp giảm thiểu: Ép cấu trúc đầu ra (JSON/functions), chạy nhiều lần và bỏ phiếu đa số (ensembles), và mẹo thực tế đặt T=0.1 để tránh tình trạng model bị lặp câu chữ.

3. **Context Is Everything: Đưa AI vào công việc thực tế** (*Diễn giả: Anh Tịnh Trương - Platform Engineer @ GoTymeX*)
   - Nhấn mạnh nguyên nhân AI trả lời kém thường nằm ở ngữ cảnh (context) đầu vào yếu hơn là chất lượng model.
   - Chỉ ra các lỗi phổ biến như nhồi nhét tài liệu nhiễu ("Internet Puller") hay lặp lại những kiến thức hiển nhiên.
   - Giới thiệu mô hình tiến hóa từ Prompt -> Context -> Memory và ý tưởng xây dựng "Bộ não AI thứ hai" (Second AI Brain) kết hợp S3/Vector DB + Bedrock. Đồng thời tóm lược quy trình CI/CD tự động trên AWS (CodePipeline, CodeBuild, ECR, ECS).

4. **Trợ lý AI thân thiện với Amazon Quick** (*Diễn giả: Anh Phạm Ng Hải Anh - AWS Community Builder @ G-AsiaPacific*)
   - Giới thiệu bộ giải pháp Amazon Quick (Amazon Q Business) giúp giải quyết các tác vụ lặp đi lặp lại hàng ngày của người dùng doanh nghiệp.
   - Demo thực tế trợ lý PM tự động tạo biên bản cuộc họp (MoM), gửi email và đặt lịch hẹn.

5. **36 giờ cùng LotusHacks: Xây dựng UTMorpho từ Ý tưởng đến Thực tế** (*Diễn giả: Team VIB*)
   - Chia sẻ hành trình tham gia hackathon lớn nhất Việt Nam và quá trình phát triển sản phẩm UTMorpho (công cụ sinh UI bằng AI cho phép chỉnh sửa trực quan canvas theo kiểu WYSIWYG) trong 36 giờ.
   - Bài học về việc tìm kiếm ý tưởng từ chính những khó khăn thực tế, sự phối hợp ăn ý của nhóm (chemistry) và cách xem AI như một cộng sự đắc lực.

6. **Từ Edge đến Origin: Lấy CloudFront làm Nền tảng** (*Diễn giả: Anh Nguyễn Tuấn Thịnh - DevOps Engineer*)
   - Giới thiệu hình thức giá gói cố định (Flat-rate pricing) mới của CloudFront giúp gộp các dịch vụ WAF, DDoS protection, Route 53 và credit lưu trữ S3, loại bỏ hoàn toàn rủi ro tăng vọt chi phí ngoài ý muốn.
   - Trình bày cơ chế giảm tải cho origin (giảm tải CPU của EC2 từ 5% xuống 1% nhờ xử lý nén file và bắt tay TLS tại Edge) và các tính năng bảo mật nâng cao như VPC origins, OAC, mTLS, custom headers.

### Bài học rút ra & Đóng góp cá nhân
Là một người mới bắt đầu học hỏi và khám phá về cloud engineering, ngày hội cộng đồng này đã mở rộng tầm mắt cho tôi về rất nhiều kiến thức bổ ích:
- **Giá trị thực tế của Cloud Architecture:** Những số liệu cụ thể về việc giảm tải CPU cho EC2 và nén dung lượng truyền tải qua CloudFront giúp tôi hiểu rõ tác động thực tế của việc tối ưu hóa hạ tầng đối với chi phí và hiệu năng hệ thống.
- **Tư duy kỹ thuật đối với AI:** Tôi hiểu rằng phát triển ứng dụng AI không chỉ là viết prompt đơn thuần, mà cần tư duy hệ thống chặt chẽ từ việc thiết kế ngữ cảnh (context), kiểm soát tính phi định hướng (non-determinism) đến áp dụng các bộ quy tắc bảo vệ dữ liệu (guardrails).
- **Cảm hứng học tập từ những câu chuyện thực tế:** Tinh thần bền bỉ và tốc độ triển khai của Team VIB tại LotusHacks, cùng với hành trình đi lên từ thực tập sinh FCAJ của anh Tuấn Thịnh là nguồn động lực to lớn cho tôi. Nó chứng minh rằng dù bắt đầu từ con số 0, sự tò mò và lòng kiên trì sẽ đưa chúng ta đi rất xa.
- **Sự hào hứng khám phá:** Mỗi câu chuyện, mỗi công nghệ được chia sẻ đã khơi dậy niềm đam mê tìm hiểu sâu hơn về điện toán đám mây trong tôi.

### Hình ảnh sự kiện
![AWS Community Day](/images/4-EventParticipated/event3-community-day-1.png)
![Context Is Everything - Phiên chia sẻ](/images/4-EventParticipated/event3-community-day-2.png)


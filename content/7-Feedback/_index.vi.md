---
title: "Chia sẻ, đóng góp ý kiến"
date: 2026-07-07
weight: 7
chapter: false
pre: " <b> 7. </b> "
---


## Phản hồi về chương trình thực tập FCAJ

### Những điều tôi thích
1. **Lộ trình học có cấu trúc**: Giai đoạn nền tảng AWS 4 tuần cho tôi kiến thức vững chắc trước khi bước vào dự án. Tiến trình từ dịch vụ đơn lẻ (EC2, S3, Lambda) đến kiến trúc tích hợp được thiết kế tốt.

2. **Kinh nghiệm dự án thực tế**: Xây dựng LingoRise từ đầu — nền tảng IELTS/TOEIC production-grade — cho tôi kinh nghiệm thực tế mà các khóa học lý thuyết không thể cung cấp. Làm việc với 8+ dịch vụ AWS trong một dự án là vô cùng quý giá.

3. **Yêu cầu tài liệu Tiếng Anh**: Được yêu cầu viết tất cả tài liệu kỹ thuật bằng Tiếng Anh đã cải thiện đáng kể kỹ năng giao tiếp kỹ thuật. Viết README, API specs và blog posts bằng Tiếng Anh giờ là phần tự nhiên trong workflow.

4. **Hỗ trợ mentor**: Được tiếp cận mentor giàu kinh nghiệm (Nguyễn Gia Hưng – hunggia@amazon.com) hướng dẫn về best practices AWS và quyết định kiến trúc rất hữu ích.

5. **Cộng đồng AWS Study Group**: Là thành viên cộng đồng học AWS lớn hơn tạo động lực và cơ hội học hỏi từ dự án và kinh nghiệm của bạn bè.

### Đề xuất cải thiện
1. **Thêm workshops thực hành**: Mặc dù tài liệu tự học tốt, thêm workshops tương tác demo các AWS patterns phức tạp (multi-account strategies, advanced VPC networking) sẽ giúp hiểu sâu hơn.

2. **Buổi code review**: Code review sessions thường xuyên giữa thực tập sinh và mentor sẽ giúp phát hiện sớm bad practices và tiếp xúc với các cách tiếp cận code khác nhau.

3. **Demo sessions giữa các nhóm**: Tổ chức demo sessions không chính thức giữa các nhóm trước Demo Day chính thức sẽ giúp luyện kỹ năng thuyết trình trong môi trường áp lực thấp.

4. **Hướng dẫn phát triển mobile**: Vì nhiều ứng dụng thực tế cần hỗ trợ mobile, thêm module ngắn về React Native hoặc PWA deployment trên AWS sẽ rất giá trị.

---

## Chia sẻ với các bạn thực tập sinh tương lai

### Tips thành công trong FCAJ Bootcamp

**Tuần 1–4 (Giai đoạn nền tảng)**:
- Đừng chỉ đọc — hãy làm mọi bài thực hành. Cảm giác gõ AWS CLI commands và click qua Console mới là thứ đọng lại.
- Bắt đầu viết worklog từ ngày 1. Đừng đợi đến tuần 10 mới bổ sung — bạn sẽ quên chi tiết.
- Chụp screenshot mọi thứ bạn xây dựng. Chúng vô giá cho blog posts và báo cáo.

**Tuần 5–9 (Giai đoạn dự án)**:
- Chọn dự án bạn thực sự quan tâm. Bạn sẽ dành 5+ tuần cho nó — đam mê tạo sự khác biệt giữa kết quả tầm thường và xuất sắc.
- Thiết lập CI/CD sớm. Deploy được bằng `sam build && sam deploy` tiết kiệm hàng giờ làm thủ công.
- Viết blog posts khi bạn làm. Cố viết 3 bài blog trong tuần 12 rất đau đầu.

**Tuần 10–12 (Giai đoạn báo cáo)**:
- Luyện tập thuyết trình Demo Day ít nhất 2 lần. Cách tiếp cận `FINAL_DEMO_CHECKLIST.md` (17 bước theo thứ tự) đã cứu tôi khỏi bị đơ trong buổi thuyết trình thật.
- Website báo cáo thực tập là portfolio vĩnh viễn. Đầu tư thời gian để nó trông chuyên nghiệp.

### Công cụ tôi thấy hữu ích nhất
| Công cụ | Lý do |
|---------|-------|
| **AWS SAM CLI** | Infrastructure-as-code giúp deployment lặp lại và version-controlled |
| **React Query** | Làm frontend data fetching và caching dễ dàng đáng kể |
| **Tesseract.js** | Khả năng OCR mà không cần dịch vụ Python riêng |
| **AWS Polly** | TTS hoạt động ngay — không cần quản lý ML model |

---

## Lời cảm ơn

Tôi xin bày tỏ lòng biết ơn chân thành đến:

- **Nguyễn Gia Hưng** (hunggia@amazon.com) — mentor thực tập, đã kiên nhẫn hướng dẫn về quyết định kiến trúc AWS và chiến lược deployment.
- **Trịnh Đồng Thạch Trúc** (tdt.truc@hutech.edu.vn) — giảng viên hướng dẫn, đã hướng dẫn học thuật trong suốt kỳ thực tập.
- **AWS Study Group Vietnam** — đã tạo và duy trì chương trình FCAJ Bootcamp.
- **Các bạn lớp AWS042026** — đã cùng hợp tác, học hỏi và hỗ trợ lẫn nhau.
---
title: "Event 4: Lần đầu thiết kế kiến trúc"
date: 2026-06-20
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---
# Giải Quyết Case Study Dưới Áp Lực Thời Gian: Trải Nghiệm Của Tôi Tại Cloud Architect Game Show

Hầu hết các sự kiện công nghệ đều diễn ra theo một kịch bản quen thuộc: trình chiếu slide, diễn giả chia sẻ và phần hỏi đáp yên ắng cuối giờ. Nhưng **Cloud Architect Game Show**, do AWS Study Group tổ chức, thì hoàn toàn ngược lại. Đó là một đấu trường cạnh tranh gay cấn, nơi 8 đội chơi gồm những thí sinh được ghép cặp ngẫu nhiên phải cùng nhau giải quyết các bài toán thiết kế hệ thống phức tạp trên AWS dưới áp lực thời gian của chiếc đồng hồ đếm ngược.

Tham gia với tư cách thí sinh không chỉ là một bài kiểm tra kiến thức—đó là một bài học đắt giá về cách tư duy, cộng tác nhóm và đưa ra quyết định nhanh chóng trong những tình huống căng thẳng nhất.

![Cloud Architect Game Show](/images/4-EventParticipated/event4-gameshow.png)

---

## Sức Nóng Của Sân Khấu Đối Đầu

Bầu không khí trong khán phòng vô cùng nóng. Năm thành viên chúng tôi, những người chưa từng quen biết nhau trước sự kiện, được xếp chung một đội và ngay lập tức bị cuốn vào thể thức thi đấu đối đầu trực tiếp (knockout). 

Đội của chúng tôi được ghép cặp đấu với một đội khác, luân phiên trả lời các câu hỏi tình huống (case studies) với độ khó tăng dần. Nghĩ đến việc nếu hai đội bằng điểm khi kết thúc lượt đấu sẽ phải bước vào câu hỏi phụ "Sudden Death" đầy may rủi khiến tim ai cũng đập nhanh. Mỗi giây thảo luận đều vô cùng quý giá, và không có chỗ cho sự do dự.

### Cuộc Tranh Luận Tách Rời Hệ Thống: SQS vs. Kinesis
Một trong những khoảnh khắc đáng nhớ nhất là khi gặp câu hỏi về việc xử lý nghẽn hệ thống đơn hàng vào ngày Black Friday. Đề bài yêu cầu thiết kế giải pháp tách rời (decouple) các microservices sao cho không bị mất mát bất kỳ thông điệp nào.
Dưới áp lực thời gian, đội chúng tôi đã có một cuộc tranh luận chớp nhoáng: *Nên chọn Amazon SQS hay Amazon Kinesis?*
*   "SQS đơn giản hơn, truyền nhận point-to-point và đảm bảo phân phối tin nhắn," một thành viên lập luận.
*   "Nhưng nếu sau này cần đọc lại dữ liệu (replay stream) hoặc có nhiều hệ thống cùng tiêu thụ dữ liệu thì sao?" người khác phản biện.

Chúng tôi phải nhanh chóng loại bỏ các giả định mơ hồ, phân tích sự đánh đổi và thống nhất chọn SQS vì yêu cầu cốt lõi của đề bài chỉ là tách rời hệ thống đơn giản chứ không phải xử lý luồng dữ liệu thời gian thực. Đưa ra quyết định đồng thuận đó chỉ trong chưa đầy 30 giây thực sự là một cảm giác rất phấn khích!

---

## Quản Trị Rủi Ro Bằng Chiến Thuật: Phòng Thủ vs. Tấn Công

Điều làm nên sự hấp dẫn vượt trội của game show này chính là sự xuất hiện của các thẻ kỹ năng chiến thuật. Cả đội phải quản lý rủi ro trong thời gian thực, lựa chọn thời điểm nên chơi an toàn và thời điểm cần bứt phá.

*   **Chơi An Toàn (Thẻ Minimum Risk):** Chúng tôi quyết định sử dụng thẻ này cho một câu hỏi cấu hình mạng phức tạp khi nhận thấy có quá nhiều giả định chưa được làm rõ. Nếu trả lời sai, đội sẽ không bị trừ điểm; nếu đúng, đội nhận được một nửa số điểm. Thẻ này đã cứu điểm số của cả đội khi logic ban đầu của chúng tôi có một chút sai sót nhỏ.
*   **Bứt Phá Lội Ngược Dòng (Thẻ Star of Hope):** Ở lượt đấu sau, khi gặp một kịch bản thiết kế tách rời hệ thống quen thuộc và cả đội vô cùng tự tin, chúng tôi đã kích hoạt *Star of Hope*. Đây là một quyết định cân não—trả lời đúng sẽ nhận nhân đôi số điểm (x2), nhưng nếu sai cũng sẽ bị trừ gấp đôi điểm số tương ứng. Cảm giác nhẹ nhõm và vỡ òa khi đáp án của đội hiện lên màu xanh trên màn hình là khoảnh khắc tuyệt vời nhất của chúng tôi trong suốt giải đấu.

---

## Những Bài Học Tôi Mang Về

Vượt qua điểm số trên bảng xếp hạng, game show đã để lại cho tôi ba bài học sâu sắc về tư duy thiết kế kiến trúc:

1.  **Nói Không Với Phỏng Đoán:** Dưới áp lực thời gian, việc chọn bừa một dịch vụ chỉ vì "nghe có vẻ đúng" là con đường ngắn nhất dẫn đến thất bại. Bạn phải tìm ra các giới hạn thực tế của đề bài và kiểm chứng mọi giả định trước khi quyết định.
2.  **Giao Tiếp Trọng Tâm Quan Trọng Hơn Tốc Độ:** Khi chỉ có vài chục giây để thảo luận, bạn học được cách loại bỏ tất cả các từ thừa. Đội chúng tôi đã thực hành cách giao tiếp trực diện, logic: xác minh giới hạn dịch vụ đám mây và đối chiếu trực tiếp với yêu cầu hệ thống.
3.  **Triết Lý Tối Giản Luôn Thắng:** Kiến trúc tốt nhất là kiến trúc đơn giản nhất đáp ứng đủ nhu cầu bài toán. Đừng tự ý vẽ ra các hệ thống lưu trữ file phức tạp, đắt đỏ (như EFS) nếu yêu cầu của doanh nghiệp chỉ cần lưu trữ tĩnh cơ bản (như S3).

## Đưa Tinh Thần Game Show Vào Công Việc Hàng Ngày

Trải nghiệm này đã thay đổi hoàn toàn cách tôi tiếp cận công việc kỹ nghệ hàng ngày:
*   **Coi Thiết Kế Như Một Case Study Thực Tế:** Trước khi quyết định tích hợp một dịch vụ AWS mới vào dự án, tôi luôn tự lập ra bảng tiêu chí kiểm thử (tải trọng, chi phí, độ trễ) như thể đang phải bảo vệ giải pháp đó trên sân khấu cuộc thi.
*   **Giải Quyết Vấn Đề Có Cấu Trúc:** Khi đối mặt với một bug hoặc điểm nghẽn kiến trúc, tôi không còn đoán mò dựa trên cảm tính. Thay vào đó, tôi chia nhỏ vấn đề thành các giả định có thể kiểm chứng và lần lượt xác minh từng giả định một.

Đây thực sự là một trải nghiệm khó quên—một sân chơi đầy năng lượng chứng minh rằng kiến trúc đám mây không chỉ là việc đọc tài liệu hướng dẫn, mà là nghệ thuật tư duy phân tích, làm việc nhóm và đưa ra những quyết định đánh đổi chính xác khi áp lực ập đến.

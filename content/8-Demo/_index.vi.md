---
title: "Demo Dự Án"
date: 2026-07-22
weight: 8
chapter: false
pre: " <b> 8. </b> "
---

# Demo Trực Quan & Tài Liệu Dự Án

Trang này cung cấp video demo chạy thực tế của dự án **LingoRise (nền tảng luyện thi IELTS/TOEIC)** và thư mục chứa các tài nguyên liên quan được lưu trữ trên Google Drive.

---

## 🎥 Video Demo Trực Tiếp

Dưới đây là trình phát video demo trực tiếp. Bạn có thể xem toàn màn hình và tùy chỉnh tốc độ phát.

<div class="demo-video-container">
  <!-- Thay thế FILE_ID_HERE bằng ID file video thực tế trên Google Drive của bạn -->
  <iframe 
    src="https://drive.google.com/file/d/1ri3cTnkdFDWMQzkPT5GPy4sHZ61EXpi2/view?usp=sharing" 
    width="100%" 
    height="480" 
    allow="autoplay; encrypted-media" 
    allowfullscreen
    style="border: none; border-radius: 12px; box-shadow: 0 8px 24px rgba(0,0,0,0.15); background-color: #000;"
  ></iframe>
</div>

{{% notice tip %}}
**Làm thế nào để hiển thị/stream video của riêng bạn?**
1. Tải video demo của bạn lên Google Drive.
2. Click chuột phải vào video chọn **Chia sẻ** -> Đặt quyền truy cập là **"Người xem có liên kết"** (Anyone with the link).
3. Mở video trong Google Drive -> Click dấu **3 chấm** ở góc trên bên phải -> Chọn **Mở trong cửa sổ mới** (Open in new window).
4. Ở cửa sổ mới, click lại dấu **3 chấm** -> Chọn **Nhúng mục...** (Embed item...).
5. Sao chép mã nguồn `iframe` và cập nhật thuộc tính `src` (hoặc chỉ cần lấy chuỗi ID ở giữa `/d/` và `/preview` để thay thế vào file mã nguồn này).
{{% /notice %}}

---

## 📁 Thư Mục Tài Nguyên Dự Án (Google Drive)

Toàn bộ tài nguyên bao gồm báo cáo, slide thuyết trình, và video gốc được lưu trữ công khai tại thư mục Google Drive dưới đây:

<div class="demo-folder-container">
  <iframe 
    src="https://drive.google.com/embeddedfolderview?id=1p3dhFRWf3rbgyNnlVk-R2KkELfWrA1Ce#grid" 
    width="100%" 
    height="600" 
    frameborder="0" 
    style="border: 1px solid rgba(0,0,0,0.08); border-radius: 12px; box-shadow: 0 4px 16px rgba(0,0,0,0.08);"
    allow="autoplay"
  ></iframe>
</div>

<div class="action-buttons">
  <a href="https://drive.google.com/drive/u/0/folders/1p3dhFRWf3rbgyNnlVk-R2KkELfWrA1Ce" target="_blank" class="btn-primary">
    <i class="fas fa-external-link-alt"></i> Mở Thư Mục Trên Google Drive
  </a>
</div>

<style>
.demo-video-container {
  position: relative;
  width: 100%;
  margin: 2rem 0;
  border-radius: 12px;
  overflow: hidden;
}

.demo-folder-container {
  margin: 2rem 0;
}

.action-buttons {
  text-align: center;
  margin: 2rem 0;
}

.btn-primary {
  display: inline-block;
  background-color: #283e5b;
  color: #ffffff !important;
  padding: 10px 24px;
  font-size: 16px;
  font-weight: 600;
  border-radius: 8px;
  text-decoration: none;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(40, 62, 91, 0.2);
}

.btn-primary:hover {
  background-color: #1e2e44;
  transform: translateY(-2px);
  box-shadow: 0 6px 18px rgba(40, 62, 91, 0.3);
}

.btn-primary i {
  margin-right: 8px;
}
</style>

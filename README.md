# 💰 WiseSpend 2.0 — Quản lý tài chính cá nhân, dữ liệu là của bạn

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-181818?style=for-the-badge&logo=supabase&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google_Sheets-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)

## 💡 Bài toán & Giải pháp

Sau 6 năm tự quản lý tài chính cá nhân bằng Google Sheets, tôi nhận ra vấn đề lớn nhất không phải là thiếu công cụ ghi chép, mà là: **ghi xong không hiểu ngay dữ liệu đang nói gì**, khiến động lực duy trì thói quen mất dần trước khi kịp hình thành. Đồng thời, hầu hết app quản lý tài chính trên thị trường đều yêu cầu người dùng đưa dữ liệu tiền bạc — thông tin nhạy cảm bậc nhất — vào lưu trữ trên server của bên thứ ba.

**WiseSpend 2.0** giải quyết đồng thời cả 2 vấn đề bằng nguyên tắc cốt lõi **"Your Data, Your Ownership"**:
- Ghi chép nhanh (< 30 giây/giao dịch) nhưng vẫn thấy được bức tranh tài chính có ý nghĩa ngay sau khi ghi.
- **App không lưu bất kỳ dữ liệu giao dịch nào trên server.** Toàn bộ số dư, giao dịch, kế hoạch ngân sách sống trong **Google Sheet cá nhân** của chính người dùng — họ là người duy nhất có toàn quyền kiểm soát.

## 👤 Vai trò của tôi
Xuất phát từ chính nhu cầu và kinh nghiệm 6 năm quản lý tài chính cá nhân trên Google Sheets, tôi tự viết PRD chi tiết (bao gồm user stories, luồng nghiệp vụ, acceptance criteria) và tự xây dựng toàn bộ sản phẩm bằng kỹ thuật **Vibe Coding** — từ thiết kế kiến trúc "Google Sheets làm backend", xây dựng tính năng, đến publish và tự dùng để kiểm chứng.

## 🎥 Demo

![Mô phỏng app WiseSpend 2.0](M%C3%B4-ph%E1%BB%8Fng-app-WiseSpend-2.0.png)

## ✨ Các tính năng chính (Key Features)
- **⚡ Quick Entry — 5 loại giao dịch:** THU / CHI / TIẾT KIỆM / ĐẦU TƯ / CHO VAY-VAY NỢ trong 1 form duy nhất, ghi chép dưới 30 giây, thao tác bằng bàn phím.
- **💳 Quản lý nguồn tiền linh hoạt:** Theo dõi tài khoản ngân hàng, ví điện tử, tiền mặt và thẻ tín dụng riêng biệt, số dư tự cộng/trừ real-time theo giao dịch.
- **📊 Monthly Tracker (2 chế độ TRACK/PLAN):** Lập ngân sách theo danh mục, theo dõi tiến độ chi tiêu theo tuần, cảnh báo ngân sách còn lại theo thời gian thực.
- **📈 Finance Overview:** Tổng quan tài sản ròng, tiết kiệm, đầu tư, nợ và các khoản phải thu — tất cả tính real-time từ chính Sheet của người dùng.
- **🏦 Theo dõi tiết kiệm/đầu tư có kỳ hạn:** Tự tính ngày đáo hạn, cảnh báo khoản sắp đến hạn, tự tạo giao dịch THU khi tất toán.
- **🔒 Kiến trúc Privacy-first:** Google Sheet cá nhân là nơi lưu trữ duy nhất — app chỉ đóng vai trò giao diện ghi chép và hiển thị, không cache, không giữ bản sao dữ liệu tài chính.

## 🧠 System Architecture & Design Decision
Điểm khác biệt cốt lõi của WiseSpend là mô hình **"Google Sheets làm backend duy nhất"**:

| Lớp | Công nghệ | Vai trò | Có lưu data tài chính? |
|---|---|---|---|
| Giao diện | React + Vite | Ghi chép, hiển thị dashboard, tính toán | Không — chỉ render UI |
| Lưu trữ dữ liệu | Google Sheet cá nhân của từng user | Lưu trữ duy nhất mọi giao dịch, số dư | Có — thuộc sở hữu 100% của user |
| Cầu nối | Supabase Edge Functions + Google Sheets API | Đọc/ghi vào Sheet theo yêu cầu | Không cache, không lưu lại |
| Xác thực | Supabase Auth | Chỉ lưu email + password + sheet_url | Chỉ thông tin tài khoản |

**Về bảo mật:** Google Service Account key (dùng để app ghi được vào Sheet của user) và Supabase Service Role key đều được lưu dưới dạng biến môi trường phía server (Supabase Edge Functions secrets) — không xuất hiện trong source code phía client. App cũng không có luồng tự đăng ký công khai; tài khoản được cấp bởi chủ sở hữu (whitelist email), giảm thiểu rủi ro truy cập trái phép.

## ⚙️ Hướng dẫn chạy Local (Dành cho Technical Review)

**1. Clone repository**
```bash
git clone https://github.com/nhungle-work/wise-spend-v2.0.git
cd wise-spend-v2.0
```

**2. Cài đặt Dependencies**
```bash
npm install
```

**3. Cấu hình biến môi trường (.env.local)**
Tạo file `.env.local` ở thư mục gốc. Frontend cần thông tin project Supabase:
```bash
VITE_SUPABASE_URL=your_supabase_project_url_here
VITE_SUPABASE_PUBLISHABLE_KEY=your_supabase_publishable_key_here
```
*Lưu ý: `GOOGLE_SA_KEY` (Google Service Account) và `SUPABASE_SERVICE_ROLE_KEY` được cấu hình riêng trong Supabase Edge Functions secrets (server-side), không đặt trong file `.env.local` phía client.*

**4. Khởi chạy ứng dụng**
```bash
npm run dev
```

---
*Lưu ý nội bộ: Dự án này được phát triển bằng kỹ thuật Vibe Coding, dựa trên PRD do chính tác giả biên soạn (WiseSpend_PRD_v9.txt trong repo).*

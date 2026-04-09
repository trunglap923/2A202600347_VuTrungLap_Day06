# Thống kê phần việc tham gia — Vũ Trung Lập (2A202600347)

**Vai trò chính:** P4 — Tech Lead (Backend AI)
**Track:** XanhSM (Sản phẩm: XanhSM Voice Agent Booking)

Dưới đây là chi tiết các hạng mục công việc tôi đã tham gia thực hiện và đóng góp trong suốt quá trình chạy Hackathon Day 06:

## 1. Phát triển Core Backend (Triển khai Model)

- **Tái sử dụng & Cải tiến Logic:** Tận dụng lại kiến trúc ReAct Agent từ file `agent.py` và backend `server.py` của bài Lab 04 (Day 04), chỉnh sửa lại logic để chuyên biệt hóa cho Voice Agent đặt xe XanhSM.
- **Viết System Prompt:** Soạn thảo và thử nghiệm liên tục file `system_prompt.txt` để ép mô hình LLM phát sinh dữ liệu chuẩn xác dưới định dạng JSON (`origin`, `destination`, `vehicle_type`).
- **Triển khai luồng STT -> LLM:** Đảm nhận phần chuyển đổi và xử lý hội thoại: Xử lý chuỗi văn bản (được dịch ra từ Web Speech API hoặc giả lập đầu vào) qua LLM và bóc tách thành các entities đặt xe cụ thể.

## 2. Thiết kế Cơ sở hạ tầng & Tích hợp (Integration)

- **Thiết lập Mock API:** Dựng API cục bộ (dựa trên FastAPI) làm cầu nối (middleware) để nhận requests từ giao diện Frontend.
- **Xử lý luồng ngoại lệ (Fallback & Correction Flow):** Thiết kế backend trả về cấu trúc lỗi chi tiết (bao gồm mảng `missing_fields` và thẻ `follow_up_question`) nếu user đọc địa điểm bị thiếu/sai, thay vì để backend crash. Cung cấp tín hiệu minh bạch cho UI (P5 - Phúc) render popup hỏi lại người dùng.
- **Support Frontend Debugging:** Hỗ trợ bạn thiết kế giao diện (P5 - Phúc) debug các lỗi về giao tiếp giữa Frontend React/HTML và Backend (Xử lý vấn đề CORS, bất đồng bộ).

## 3. Tham gia lên SPEC & Chuẩn bị Demo (Support roles)

- **Bộ dữ liệu Test (Hỗ trợ P6):** Cung cấp các log input / output từ phần thử nghiệm Prompt cho Kiên (P6) xây dựng kịch bản Demo hoàn chỉnh nhất với 15 test cases (Happy paths + Edge cases).
- **Trình bày Kỹ thuật (Demo Round):** Là một trong ba thành viên túc trực tại bàn Demo, hỗ trợ giải thích trực tiếp phần mô hình cho Giảng viên và các sinh viên đi xem vòng chéo khi có câu hỏi chuyên sâu về kiến trúc hệ thống.

# Individual Reflection — Vũ Trung Lập (2A202600347)

## 1. Role

**Vai trò:** P4 — Tech Lead (Backend AI). Phụ trách xây dựng kiến trúc AI Agent và backend để nhận diện Intent từ Voice Command.

## 2. Đóng góp cụ thể

- **Tái cấu trúc kiến trúc:** Tận dụng lại lõi ReAct Agent từ file `agent.py` và `tools.py` của bài Lab 4 (`Day-04/lab4_agent`) và biến thể nó để đáp ứng yêu cầu rút trích thông tin chuyến xe (origin, destination, vehicle_type).
- **Thiết kế System Prompt cốt lõi:** Thử nghiệm prompt engineering từ phiên bản text-based thành dạng strict-JSON output để trả dữ liệu cho frontend (P5 - Phúc) render giao diện.
- **Tích hợp API:** Tích hợp pipeline STT (Speech-to-Text) và pass text qua LLM pipeline (OpenAI/Gemini). Xử lý các edge cases khi LLM trả lỗi format hoặc bị ảo giác (hallucinate) sinh ra những "điểm đó ảo" bằng flow fallback.

## 3. SPEC mạnh/yếu

- **Mạnh nhất:** Phần **Failure Modes** cực kỳ mạnh và thực tế. Do tôi nắm code backend, tôi đã chỉ ra trực tiếp những lúc STT nhận nhầm từ đồng âm ("Cầu Giấy" vs "Cẩu Giấy") và bắt LLM phải thả lỏng constraint (dùng threshold fuzziness) để tự handle, thay vì chết thẳng cẳng ở backend.
- **Yếu nhất:** Phần **ROI (Return on Investment)**. Thực sự những thông số về việc thiết lập Agent này sẽ cắt giảm được bao nhiên tiền cho tổng đài XanhSM vẫn mang tính chất ước lượng "trên giấy", chưa mang tính khả thi rõ ràng cho doanh nghiệp vì chi phí duy trì ReAct Agent khá cao ở môi trường production.

## 4. Đóng góp khác

- Debug liên tục server khi frontend của Phúc (P5) bị lỗi CORS trong quá trình gọi API.
- Giúp thiết lập một server API giả (mock server) bằng FastAPI (dựa trên `server.py` của Lab 4) để Phúc có thời gian build UI trong khi tôi tinh chỉnh LLM Prompt.
- Hỗ trợ Kiên (P6) lên kịch bản demo vì tôi biết chính xác mẫu câu nào sẽ khiến Agent "chạy nuột" nhất và câu nào sẽ ép nó vào kịch bản hỏi lại thông tin.

## 5. Điều học được

Tôi học được rằng code chạy ngon dưới dạng terminal (như bài Lab 4) là một chuyện, nhưng mang vào sản phẩm thực (Product-izing AI) lại là câu chuyện khác hẳn. Khái niệm **4-Paths UX** thực sự khai sáng. Trước đây, khi LLM lỗi hoặc thiếu dữ kiện, tôi thường chỉ in ra `"Lỗi: thiếu tham số"`. Bây giờ tôi nhận ra mình phải thiết kế backend trả JSON có trường `follow_up_question` để Frontend có thể hiển thị lời nhắc thân thiện, cho phép User có cơ hội sửa lỗi. AI không hoàn hảo, nhưng cách sản phẩm ứng xử khi nó không hoàn hảo quyết định điểm chạm của khách hàng.

## 6. Nếu làm lại, đổi gì?

Nếu làm lại, tôi sẽ không "tham" dùng kiến trúc ReAct Agent đa công cụ (multi-tools) nặng nề từ Lab 4 ngay từ đầu cho bài toán entity-extraction này. Việc gọi công cụ liên tục khiến độ trễ (latency) của Voice Agent vượt quá 3-4 giây, gây trải nghiệm tệ. Thay vào đó, tôi sẽ dùng kỹ thuật Structured Outputs (JSON Schema constraint) trực tiếp trên 1 lượt gọi LLM duy nhất, sẽ giảm thời gian phản hồi xuống còn < 1s.

## 7. AI giúp gì / AI sai gì

- **AI giúp:** Giúp tôi porting (chuyển đổi) code dễ dàng từ Anthropic (Lab 4) sang các mô hình khác (ví dụ Gemini Flash) qua 1 đoạn prompt rất nhanh, tiết kiệm thời gian đọc documentation API. Nó cũng viết boilerplate cho FastAPI backend hoàn chỉnh chỉ trong 1 phút.
- **AI sai/mislead:** Khi trích xuất JSON, nhiều model cố gắng tự "sửa" lỗi chính tả của user thay vì hỏi lại để đảm bảo chắc chắn. Lúc test, tôi nói "đưa tôi ra Nguyễn Trãi", LLM tự điền là "Nguyễn Trãi, Thanh Xuân, Hà Nội" trong khi tôi ở TP.HCM. Khả năng "tự biên tự diễn" (over-assumption) của AI làm tôi mất rất nhiều thời gian đặt quy tắc `strict_extraction=True`.

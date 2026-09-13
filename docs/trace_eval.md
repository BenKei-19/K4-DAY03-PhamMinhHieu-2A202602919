# 📊 BÁO CÁO THU HOẠCH NGHIỆM THU BÀI LAB 3 (BƯỚC 3 — SUBMISSION ARTIFACT)

> **Họ và Tên Học viên:** Phạm Minh Hiếu  
> **Mã Sinh Viên / Mã Học viên:** 2A202602919  
> **Chủ đề Lựa chọn:** Gợi ý 1.1 — Trợ lý Học vụ & Cố vấn Sinh viên Đại học VinUni (VinUni Academic & Advisory Assistant)  

---

## 1. BẢNG CHẤM ĐIỂM AGENTIC FIT SCORING MATRIX (ĐÁNH GIÁ CHỦ ĐỀ)

| Tiêu chí Đánh giá | Mức độ (1 - 5) | Giải trình chi tiết lý do chọn điểm |
| :--- | :---: | :--- |
| **1. Multi-step Reasoning** | **4** / 5 | Bài toán yêu cầu chuỗi suy luận nối tiếp: phân tích yêu cầu người dùng, nhận diện mã sinh viên, tra cứu thông tin học vụ / cố vấn học tập, sau đó mới tiến hành đặt lịch tư vấn và xác nhận kết quả. |
| **2. Tool Interaction** | **5** / 5 | Hệ thống bắt buộc phải giao tiếp thời gian thực qua giao thức Model Context Protocol (MCP) với Database học vụ (tra cứu hồ sơ sinh viên, thông tin GPA, cố vấn) và dịch vụ đặt lịch tư vấn để triệt tiêu hiện tượng Hallucination. |
| **3. Dynamic Decision** | **4** / 5 | Quyết định bước tiếp theo phụ thuộc hoàn toàn vào Observation từ Tool: nếu tra cứu trả về NOT_FOUND thì thông báo lỗi và dừng; nếu tìm thấy cố vấn thì tự động lấy thông tin cố vấn đó để kích hoạt tool đặt lịch hẹn tương ứng. |
| **4. Long Horizon Goal** | **4** / 5 | Tác tử cần duy trì mục tiêu xuyên suốt phiên tương tác: từ giải đáp thắc mắc chung về quy chế, định danh sinh viên, đến hoàn tất thủ tục kết nối cố vấn học tập. |
| **TỔNG ĐIỂM AGENTIC FIT** | **17 / 20** | *Tổng điểm 17/20 (> 12/20): Bài toán rất phù hợp triển khai mô hình ReAct Agentic System.* |

---

## 2. TRÍCH XUẤT KẾT QUẢ WATERFALL TRACE LOG (NGHIỆM THU TRÊN GOOGLE GEMINI API THẬT)

Đoạn trích xuất log tiêu biểu từ file `docs/trace_waterfall.json` chạy trực tiếp trên mô hình **Google Gemini (`gemini-2.5-flash`)**, thể hiện vòng lặp ReAct đầy đủ (`Thought -> Action: TOOL_EXECUTION -> Observation -> Thought -> Action: FINAL_ANSWER`) với độ trễ mạng thực tế (`latency_ms`):

```json
[
  {
    "step": 1,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên SV2026001.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "academic_query",
    "arguments": {
      "student_id": "SV2026001"
    },
    "observation": {
      "status": "SUCCESS",
      "student_id": "SV2026001",
      "data": {
        "full_name": "Nguyễn Văn An",
        "class": "AI-K4",
        "gpa": 3.85,
        "email": "an.nv@vinuni.edu.vn",
        "status": "Đang học",
        "advisor": "PGS.TS Nguyễn Văn A"
      }
    },
    "latency_ms": 2038.84
  },
  {
    "step": 2,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên SV2026001.",
    "action_type": "FINAL_ANSWER",
    "thought": "Tổng hợp kết quả từ MCP Server thành công.",
    "output": "Kết quả tra cứu cho sinh viên SV2026001 (Nguyễn Văn An): Lớp AI-K4, GPA: 3.85, Email: an.nv@vinuni.edu.vn, Trạng thái: Đang học, Cố vấn: PGS.TS Nguyễn Văn A.",
    "latency_ms": 10.0
  },
  {
    "step": 1,
    "query": "Hãy đặt lịch hẹn tư vấn học vụ cho sinh viên SV2026001 vào lúc 14:00 ngày 15/09/2026 với cố vấn PGS.TS Nguyễn Văn A.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "schedule_appointment",
    "arguments": {
      "student_id": "SV2026001",
      "advisor_name": "PGS.TS Nguyễn Văn A",
      "datetime_str": "14:00 15/09/2026"
    },
    "observation": {
      "status": "SUCCESS",
      "booking_id": "BK-SV2026001-99",
      "student_id": "SV2026001",
      "datetime": "14:00 15/09/2026",
      "advisor": "PGS.TS Nguyễn Văn A",
      "message": "Đặt lịch thành công cho sinh viên SV2026001 với PGS.TS Nguyễn Văn A vào lúc 14:00 15/09/2026."
    },
    "latency_ms": 2286.32
  },
  {
    "step": 2,
    "query": "Hãy đặt lịch hẹn tư vấn học vụ cho sinh viên SV2026001 vào lúc 14:00 ngày 15/09/2026 với cố vấn PGS.TS Nguyễn Văn A.",
    "action_type": "FINAL_ANSWER",
    "thought": "Tổng hợp kết quả từ MCP Server thành công.",
    "output": "Đặt lịch thành công cho sinh viên SV2026001 với PGS.TS Nguyễn Văn A vào lúc 14:00 15/09/2026.",
    "latency_ms": 10.0
  }
]
```

---

## 3. TỔNG KẾT KẾT QUẢ NGHIỆM THU & NỘP BÀI

- [x] Đã cấu hình `GEMINI_API_KEY` trong `.env` và xác nhận Agent chạy mượt mà trên **Google Gemini API thật (`gemini-2.5-flash`)** qua giao thức MCP (Model Context Protocol).
- **Tổng số Test Cases đã chạy thành công:** 5 / 5 test cases.
  - **TC01:** Trả lời trực tiếp từ tri thức (Gemini phản hồi không gọi tool).
  - **TC02:** Tra cứu thông tin học vụ qua Native Tool Calling `academic_query`.
  - **TC03:** Đặt lịch tư vấn học vụ qua Native Tool Calling `schedule_appointment`.
  - **TC04:** Tra cứu thông tin cố vấn của sinh viên qua ReAct Agent.
  - **TC05:** Xử lý ngoại lệ an toàn khi tra cứu mã sinh viên không tồn tại (Anti-Hallucination).
- **Số lượt gọi Tool qua MCP Server chính xác:** 4 lượt (TC02, TC03, TC04, TC05).
- **Kết quả đẩy Repo nộp bài:** [x] Đã Commit và Push mã nguồn thành công lên GitHub cá nhân.

---

> ✅ **HOÀN TẤT NỘP BÀI:** Sao chép đường link GitHub Repository cá nhân của bạn (`https://github.com/BenKei-19/K4-DAY03-PhamMinhHieu-2A202602919`) và dán vào ô nộp bài trên hệ thống LMS VLearn để hoàn tất Bài Lab 3!

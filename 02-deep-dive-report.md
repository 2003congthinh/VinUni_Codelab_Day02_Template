# Phase 3 & Phase 5: Deep-Dive Report & AI Evaluation

## **Dự án:** Intelligent Dispatcher Co-Pilot

## 🏗️ Phase 3 — DEEP-DIVE

### 3.1 Problem Statement (6-Field Framework)

| Field                          | Nội dung chi tiết                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Target Actor / Operator** | Điều phối viên Tổng đài CSKH & Điều vận Cứu hộ Xanh SM (GSM).                                                                                                                                                                                                                                                                                                                                                                                         |
| **2. Current Workflow**        | Khi tài xế báo sự cố hết pin/pin khẩn cấp, điều phối viên nhận thông tin thủ công qua cuộc gọi hoặc app, mở bản đồ GIS tra tọa độ xe, kiểm tra danh sách trạm sạc/xe sạc di động, tra cứu sổ tay quy trình và gõ thủ công tin nhắn hướng dẫn/lệnh điều phối cho tài xế.                                                                                                                                                                               |
| **3. Bottleneck**              | Bước tra cứu vị trí trạm sạc và đánh giá mức pin mất từ 5–8 phút/cuộc gọi. Việc đánh giá sai dung lượng pin khẩn cấp (< 5%) có thể dẫn đến chỉ đường sai, khiến xe cạn pin hoàn toàn mid-route, gây ùn tắc giao thông và nguy hiểm nghiêm trọng.                                                                                                                                                                                                      |
| **4. Business Impact**         | Tắc nghẽn tổng đài vào giờ cao điểm (AHT tăng cao), tăng chi phí cứu hộ đường bộ do xử lý trễ, giảm chỉ số hài lòng của tài xế và ảnh hưởng đến trải nghiệm dịch vụ chung của Xanh SM.                                                                                                                                                                                                                                                                |
| **5. Success Metric**          | - **AHT (Average Handling Time):** Giảm thời gian xử lý 1 sự cố pin từ **6 phút ──> under 1 phút/ticket**.<br>- **SLA Compliance:** 100% sự cố pin khẩn cấp (< 5%) được phát lệnh xe sạc di động trong **< 2 phút**.<br>- **Accuracy & Safety:** Độ chính xác tuân thủ ranh giới an toàn đạt **100%**.                                                                                                                                                |
| **6. Operational Boundary**    | - **Tag bắt buộc:** Tất cả câu trả lời do AI tạo ra bắt buộc phải chứa tiền tố `[DRAFT_ONLY]` ở đầu để đảm bảo bắt buộc có con người (Điều phối viên) phê duyệt trước khi gửi đi.<br>- **Ranh giới pin khẩn cấp:** Khi pin < 5%, **TUYỆT ĐỐI KHÔNG** chỉ đường tới trạm sạc xa quá 5km. Bắt buộc chuyển sang kịch bản gọi Xe sạc di động (Mobile Charging Vehicle) dưới dạng JSON cấu trúc: `{"action": "dispatch_mobile_charger", "reason": "..."}`. |

---

## 3.2 AI-Fit Matrix & Technology Tiering

Bài toán được thiết kế kết hợp 3 tầng công nghệ để đảm bảo hiệu năng và an toàn tối đa:

- **Rule-based Logic (Luồng quy tắc cứng):**
  - Validation dữ liệu GPS, mã tài xế và kiểm tra điều kiện pin khẩn cấp (`Battery < 5%`).
  - Kiểm tra cứng sự tồn tại của thẻ `[DRAFT_ONLY]` ở đầu phản hồi trước khi hiển thị lên giao diện Điều phối viên.

- **LLM Feature (Tính năng LLM - Gemini 3.5/3.6 Flash):**
  - **Context-Aware Reasoning:** Đọc hiểu ngữ cảnh báo cáo sự cố từ tài xế, trích xuất dữ liệu dung lượng pin và khoảng cách.
  - **Draft Generation:** Tự động soạn thảo văn bản hướng dẫn chuẩn chỉnh về phong cách giao tiếp cho Điều phối viên.

- **Agentic Loop (Luồng xử lý tự động có kiểm soát):**
  - **Smart Dispatcher Command:** Khi gặp sự cố pin khẩn cấp (< 5%), AI tự động chuyển đổi thành cấu trúc lệnh hệ thống JSON (`dispatch_mobile_charger`) để kích hoạt API điều xe cứu hộ tự động.

---

## 3.3 Future-State Flow & Safety Architecture

### Sơ đồ quy trình tương lai (Future-State Workflow)

```text
[Tài xế báo sự cố: GPS, % Pin]
               │
               ▼
[Step 1: Rule-based Validation] ─────► (Kiểm tra dữ liệu đầu vào: Pin %, GPS)
               │
               ▼
[Step 2: AI Co-Pilot (LLM + RAG)] ────► (Phân tích sự cố, tra cứu Sổ tay Kỹ thuật & Trạm sạc)
               │
               ▼
[Step 3: Boundary Guardrail Enforcer] ─► (Áp dụng Rule: Pin < 5% -> Mobile Charger | Thêm [DRAFT_ONLY])
               │
               ▼
[Step 4: Human-in-the-Loop (HITL)] ──► (Điều phối viên duyệt/sửa bản nháp lệnh trên Dashboard)
               │
      ┌────────┴────────┐
  [Đồng ý]          [Sửa / Từ chối]
      │                 │
      ▼                 ▼
[Phát lệnh chính thức] [Chuyển sang Fallback / Tra cứu thủ công]
```

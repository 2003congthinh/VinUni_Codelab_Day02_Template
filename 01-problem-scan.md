# Phase 1 & Phase 2: Opportunity Scan & Quick Assessment

---

## 🔍 Phase 1 — SCAN: Bảng Quét Cơ Hội (Opportunity Scan)

Dưới đây là bảng quét 5 bài toán vận hành thực tế thuộc hệ sinh thái các công ty thành viên Vingroup, áp dụng **4 Lenses** (Lặp lại, Tốn thời gian, AI-upgrade, Stakeholder Pain):

| # | Công ty thành viên | Lens áp dụng | Mô tả ngắn bài toán | Point of Failure / Bottleneck | Cơ hội ứng dụng AI (AI Opportunity) |
|---|---|---|---|---|---|
| **1** | **Xanh SM (GSM)** | Stakeholder Pain & Time-consuming | Hỗ trợ Điều phối viên xử lý sự cố cạn pin khẩn cấp (< 5%) và điều xe sạc di động. | Điều phối viên mất 5–8 phút tra cứu trạm sạc/xe sạc di động thủ công, dễ dẫn đến đánh giá sai khiến xe chết máy giữa đường. | AI Co-Pilot tự động trích xuất vị trí GPS, phân tích % pin và tạo bản nháp lệnh điều phối xe sạc di động hoặc trạm sạc an toàn. |
| **2** | **VinFast** | Repetitive & Time-consuming | Phân loại tự động ticket bảo hành & trích xuất mã lỗi sự cố kỹ thuật từ VinFast Service. | Nhân viên CSKH phải đọc thủ công hàng nghìn phản ánh, tra cứu mã lỗi OBD và phân loại xưởng dịch vụ mất 5–7 phút/ticket. | LLM Feature tự động đọc mô tả lỗi, bóc tách mã lỗi OBD, tra cứu Sổ tay Kỹ thuật (RAG) và điền sẵn Form Ticket CRM. |
| **3** | **Vinhomes** | Time-consuming & Stakeholder Pain | Phân luồng và tự động gán công việc từ phản ánh của cư dân trên App Vinhomes Resident. | Ban quản lý mất 10–15 phút đọc nội dung/ảnh, phân loại thủ công phòng ban (Kỹ thuật, An ninh, Vệ sinh) gây trễ SLA phản hồi. | Smart Routing Agent phân tích nội dung/hình ảnh phản ánh, tự động xác định phòng ban trách nhiệm và gán task cho kỹ thuật viên rảnh ca. |
| **4** | **Vinmec** | Time-consuming & AI-upgrade | Tóm tắt lịch sử khám chữa bệnh và đơn thuốc từ hồ sơ bệnh án điện tử (EHR). | Bác sĩ mất 10–15 phút đọc lại toàn bộ hồ sơ bệnh án cũ dài hàng chục trang của bệnh nhân trước khi vào khám. | LLM Summarization Engine tự động trích xuất dị ứng, tiền sử phẫu thuật, các chỉ số bất thường thành báo cáo tóm tắt 1 trang Dashboard. |
| **5** | **Vinpearl** | Repetitive & AI-upgrade | Tư vấn lịch trình du lịch cá nhân hóa và dịch vụ vui chơi tại VinWonders / Vinpearl. | Nhân viên tư vấn mất nhiều thời gian nhắn tin qua lại để thiết kế lịch trình riêng theo nhu cầu từng gia đình. | Agentic AI Assistant tương tác tự nhiên, tư vấn gói dịch vụ, đề xuất lịch trình di chuyển và kết nối hệ thống booking tự động. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Dựa trên bảng SCAN, 3 bài toán tiềm năng nhất được chọn để lập thẻ phân tích nhanh:

---

### 🎴 QUICK PROBLEM CARD #01

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #01                                                   │
│                                                                         │
│ Bài toán: Hỗ trợ Điều phối viên xử lý sự cố pin xe điện cạn kiệt khẩn cấp │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes              │
│                     [ ] Vinmec   [ ] Khác                               │
│                                                                         │
│ Ai đang đau (Actor)? Điều phối viên Tổng đài CSKH & Cứu hộ Xanh SM (GSM).│
│                                                                         │
│ Workflow thủ công hiện tại:                                             │
│   1. Nhận báo cáo pin cạn từ tài xế ──> 2. Tra vị trí GPS & tra KB ──>  │
│   3. Tìm trạm sạc/xe sạc di động   ──> 4. Soạn tin nhắn hướng dẫn/lệnh  │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 6–8 phút/lượt)            │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2, 3 và 4 (Phân tích, tra    │
│ cứu và soạn bản nháp lệnh điều phối [DRAFT_ONLY]).                       │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian xử lý sự cố từ      │
│ 6 phút ──> under 1 phút/ticket. Tỷ lệ đáp ứng khẩn cấp < 15 phút > 98%.  │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent     │
└─────────────────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #02                                                   │
│                                                                         │
│ Bài toán: Phân loại tự động Ticket sự cố kỹ thuật xe VinFast            │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes              │
│                     [ ] Vinmec   [ ] Khác                               │
│                                                                         │
│ Ai đang đau (Actor)? Nhân viên tiếp nhận Dịch vụ Hậu mãi VinFast Service.│
│                                                                         │
│ Workflow thủ công hiện tại:                                             │
│   1. Nhận thông tin từ App/Tổng đài ──> 2. Đọc mô tả & tra mã lỗi OBD ──>│
│   3. Phân loại nhóm sự cố         ──> 4. Tạo ticket CRM & chuyển xưởng   │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 5–7 phút/ticket)          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tự động trích xuất mã lỗi, tìm kiếm│
│ nguyên nhân từ KB và điền trước thông tin vào ticket CRM.               │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian tạo ticket từ      │
│ 7 phút ──> under 2 phút. Độ chính xác phân loại sự cố đạt > 92%.         │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent     │
└─────────────────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #03                                                   │
│                                                                         │
│ Bài toán: Phân luồng và điều phối yêu cầu cư dân qua Vinhomes Resident  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes              │
│                     [ ] Vinmec   [ ] Khác                               │
│                                                                         │
│ Ai đang đau (Actor)? Nhân viên Ban quản lý (BQL) Khu đô thị Vinhomes.   │
│                                                                         │
│ Workflow thủ công hiện tại:                                             │
│   1. Tiếp nhận ticket từ App Cư dân ──> 2. Đọc nội dung & xem hình ảnh ──│
│   3. Xác định đội ngũ trách nhiệm ──> 4. Gán task thủ công cho kỹ thuật │
│                                                                         │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 10–15 phút/lượt)         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Đọc hiểu văn bản/ảnh phản ánh, tự  │
│ động gán tag bộ phận và phân công cho kỹ thuật viên đang rảnh.          │
│                                                                         │
│ Đo thành công bằng gì (Metric có số)? Rút ngắn thời gian phản hồi cư dân│
│ từ 20 phút ──> under 2 phút. Phân loại đúng bộ phận đạt > 95%.          │
│                                                                         │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agentic Loop      │
└─────────────────────────────────────────────────────────────────────────┘
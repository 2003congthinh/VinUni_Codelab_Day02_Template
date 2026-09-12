# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. 
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. 
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. 
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. 

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **VinFast** | Time-consuming | Đọc hiểu, bóc tách triệu chứng và phân nhóm các yêu cầu bảo dưỡng/sửa chữa từ mô tả ngôn ngữ tự nhiên của khách hàng. |
| 2 | **Xanh SM** | Stakeholder Pain | Tài xế mất thời gian gọi tổng đài để hỏi về chính sách thưởng, quy định phạt, hoặc cách xử lý sự cố nhỏ trên đường. |
| 3 | **Vinhomes** | Repetitive | Phân loại, gắn tag mức độ ưu tiên và định tuyến hàng ngàn ticket phản ánh của cư dân trên app Vinhomes về đúng bộ phận (kỹ thuật, an ninh, vệ sinh...). |
| 4 | **Vinmec** | Time-consuming | Trích xuất thông tin tiền sử bệnh, kết quả xét nghiệm từ bệnh án cũ (dạng văn bản/ảnh) để điền vào hệ thống thông tin y tế (HIS) dạng cấu trúc. |
| 5 | **VinWonders** | AI-upgrade | Tư vấn lộ trình vui chơi cá nhân hóa dựa trên độ tuổi, sở thích, và thời gian lưu trú của du khách qua các kênh chat online. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Trích xuất thông tin, tóm tắt và phân     │
│ loại yêu cầu sửa chữa xe dựa trên mô tả tự do của khách.    │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Cố vấn dịch vụ tại xưởng VinFast.      │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách báo lỗi ──> 2. Đọc/Nghe mô tả ──> 3. Bóc tách    │
│   triệu chứng ──> 4. Nhập form tạo phiếu yêu cầu dịch vụ.   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bóc tách triệu chứng và    │
│ chuẩn hóa từ ngữ chuyên ngành (⏱ 5-8 phút/lượt)             │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Phân tích văn bản tự  │
│ nhiên, tự động trích xuất xe, lỗi, và điền trước form.      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Rút ngắn thời gian tạo phiếu < 2 phút.                    │
│ - Độ chính xác trích xuất thông tin (Precision) > 90%.      │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Hỗ trợ tài xế tra cứu chính sách, hướng   │
│ dẫn vận hành, và xử lý sự cố trực tiếp trên ứng dụng.       │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế Xanh SM & Tổng đài viên.        │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Tài xế gặp vấn đề ──> 2. Gọi tổng đài ──> 3. Đợi kết   │
│   nối ──> 4. Nhân viên tra cứu tài liệu ──> 5. Trả lời.     │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Đợi kết nối tổng đài và    │
│ tra cứu tài liệu quy định nội bộ dài dòng (⏱ 3-5 phút/lượt) │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Trợ lý AI (Voice/Text)│
│ tích hợp ngay trên app tài xế, truy xuất RAG để giải đáp.   │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - Deflection rate (tỷ lệ tự động giải quyết) > 60%.         │
│ - Thời gian phản hồi < 5 giây/câu hỏi.                      │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại, đánh giá ưu tiên và    │
│ định tuyến ticket khiếu nại của cư dân.                     │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Ban quản lý tòa nhà & CSKH Vinhomes.   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân tạo ticket ──> 2. CSKH mở đọc từng ticket ──>   │
│   3. Đánh giá mức độ khẩn cấp ──> 4. Forward cho bộ phận.   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Đọc, đánh giá và phân loại │
│ thủ công gây chậm trễ xử lý sự cố (⏱ 2-4 phút/ticket)       │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Đọc text/ảnh đầu vào, │
│ tự động gắn tag (Kỹ thuật/Vệ sinh...) và cờ ưu tiên (Khẩn). │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ - >85% ticket được định tuyến tự động đúng bộ phận.         │
│ - Rút ngắn thời gian xử lý bước đầu từ 3 phút ──> < 5s.     │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

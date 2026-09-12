# 01 - PROBLEM SCAN

> **Công ty được lựa chọn:** VinFast  
> **Chủ đề:** AI hỗ trợ tiếp nhận, phân loại và cấu trúc yêu cầu sửa chữa của khách hàng  
> **Hình thức:** Cá nhân

---

# PHASE 1 — SCAN

## 1. Mục tiêu

Mục tiêu của Phase 1 là tìm kiếm các bài toán thực tế tại các công ty thuộc Vingroup có khả năng được cải thiện bằng AI.

Các bài toán được xem xét dựa trên 4 tiêu chí:

- **Repetitive:** Công việc có tính lặp lại.
- **Time-consuming:** Công việc tốn nhiều thời gian.
- **AI-upgrade:** Công việc có khả năng được cải thiện bằng AI.
- **Stakeholder Pain:** Công việc gây khó khăn cho khách hàng hoặc nhân viên.

> **Lưu ý:** Các thông tin về quy trình dịch vụ VinFast được tham khảo từ nguồn công khai. Các số liệu về thời gian xử lý, khối lượng công việc hoặc KPI trong bài là **Assumption/Target**, không phải số liệu nội bộ của VinFast.

---

## 2. Bảng quét cơ hội — SCAN

| STT | Công ty | Bài toán | Actor | Repetitive | Time-consuming | AI-upgrade | Stakeholder Pain |
|---|---|---|---|---|---|---|---|
| 1 | VinFast | Tiếp nhận và phân loại yêu cầu sửa chữa | Nhân viên tư vấn dịch vụ | Cao | Cao | Cao | Cao |
| 2 | VinFast | Hỗ trợ xử lý câu hỏi về bảo hành | Nhân viên CSKH | Cao | Trung bình - Cao | Cao | Cao |
| 3 | VinFast | Tiếp nhận yêu cầu Mobile Service | Nhân viên dịch vụ | Cao | Cao | Cao | Cao |
| 4 | VinFast | Phân loại và tóm tắt phản hồi sau sửa chữa | Nhân viên CSKH | Cao | Trung bình - Cao | Cao | Trung bình - Cao |
| 5 | VinFast | Đề xuất câu hỏi làm rõ yêu cầu sửa chữa | Nhân viên tư vấn dịch vụ | Cao | Trung bình | Cao | Cao |

---

## 3. Problem 1 — Tiếp nhận và phân loại yêu cầu sửa chữa

### Mô tả vấn đề

Khách hàng có thể mô tả vấn đề của xe bằng ngôn ngữ tự nhiên thay vì sử dụng một biểu mẫu có cấu trúc.

Ví dụ:

> "Xe của tôi dạo này chạy khoảng 60–70km/h thì vô lăng bị rung, nhất là lúc phanh."

Nhân viên dịch vụ cần đọc nội dung, hiểu vấn đề, xác định triệu chứng, phân loại yêu cầu và kiểm tra xem còn thiếu thông tin gì hay không.

VinFast công khai quy trình dịch vụ sửa chữa gồm 5 bước, trong đó bước 2 là **Tiếp nhận và tư vấn**; khách hàng được chào đón và ghi nhận yêu cầu khi tới xưởng dịch vụ.

### Actor

- Khách hàng
- Nhân viên tư vấn dịch vụ

### AI Opportunity

AI có thể:

- Tóm tắt yêu cầu.
- Trích xuất thông tin.
- Phân loại yêu cầu.
- Phát hiện thông tin còn thiếu.
- Đề xuất câu hỏi làm rõ.
- Tạo bản nháp yêu cầu dịch vụ.

### Rủi ro

AI không được tự chẩn đoán lỗi kỹ thuật hoặc quyết định sửa chữa.

---

## 4. Problem 2 — Hỗ trợ xử lý câu hỏi về bảo hành

### Mô tả vấn đề

Khách hàng có thể đặt nhiều câu hỏi liên quan đến chính sách bảo hành. Nhân viên cần xác định chủ đề câu hỏi, tìm thông tin phù hợp và giải thích cho khách hàng.

### Actor

Nhân viên chăm sóc khách hàng.

### AI Opportunity

AI có thể:

- Phân loại câu hỏi.
- Tìm thông tin liên quan trong tài liệu.
- Tóm tắt chính sách.
- Tạo bản nháp câu trả lời.

### Rủi ro

AI có thể sử dụng sai chính sách hoặc diễn giải không chính xác.

Vì vậy, nhân viên vẫn phải kiểm tra trước khi gửi câu trả lời.

---

## 5. Problem 3 — Tiếp nhận yêu cầu Mobile Service

### Mô tả vấn đề

Khách hàng cần cung cấp nội dung cần sửa chữa, vị trí và thời gian mong muốn. Thông tin khách hàng cung cấp có thể không đầy đủ hoặc không theo cùng một format.

### Actor

- Khách hàng
- Nhân viên dịch vụ

### AI Opportunity

AI có thể chuyển đổi mô tả tự nhiên thành các trường:

- Thông tin xe.
- Nội dung yêu cầu.
- Triệu chứng.
- Vị trí.
- Thời gian mong muốn.
- Thông tin còn thiếu.

### Rủi ro

AI không được tự quyết định phương án sửa chữa.

---

## 6. Problem 4 — Phân loại phản hồi sau sửa chữa

### Mô tả vấn đề

Sau khi sử dụng dịch vụ, khách hàng có thể phản hồi về nhiều vấn đề khác nhau như:

- Chất lượng sửa chữa.
- Thời gian chờ.
- Thái độ phục vụ.
- Chi phí.
- Vấn đề kỹ thuật.
- Khiếu nại.
- Khen ngợi.

Nhân viên phải đọc và phân loại các phản hồi này.

### AI Opportunity

AI có thể:

- Phân loại chủ đề.
- Xác định cảm xúc ở mức hỗ trợ.
- Tóm tắt phản hồi.
- Đánh dấu các phản hồi cần xử lý.

VinFast công khai rằng khách hàng sau khi làm dịch vụ được gọi điện để ghi nhận phản hồi/ý kiến trong vòng 3 ngày sau khi xe ra xưởng.

---

## 7. Problem 5 — Đề xuất câu hỏi làm rõ yêu cầu

### Mô tả vấn đề

Một số yêu cầu của khách hàng quá ngắn hoặc thiếu thông tin.

Ví dụ:

> "Xe bị rung."

Nhân viên cần hỏi thêm để hiểu rõ hiện tượng.

### AI Opportunity

AI có thể đề xuất:

- Hiện tượng xảy ra khi nào?
- Xe đang chạy ở tốc độ nào?
- Hiện tượng xảy ra liên tục hay thỉnh thoảng?
- Có tiếng động bất thường không?
- Hiện tượng xảy ra khi phanh hay tăng tốc?

Nhân viên quyết định câu hỏi nào phù hợp.

---

# PHASE 2 — QUICK-ASSESS

## Quick Problem Card 1 — AI hỗ trợ tiếp nhận và phân loại yêu cầu sửa chữa

### Problem

Nhân viên dịch vụ phải đọc và chuyển đổi mô tả tự nhiên của khách hàng thành yêu cầu dịch vụ có cấu trúc.

### Company

VinFast

### Actor

Nhân viên tư vấn dịch vụ.

### Current Workflow

1. Khách hàng gửi yêu cầu.
2. Nhân viên nhận yêu cầu.
3. Nhân viên đọc mô tả.
4. Nhân viên xác định triệu chứng và nhu cầu.
5. Nhân viên kiểm tra thông tin còn thiếu.
6. Nhân viên hỏi thêm nếu cần.
7. Nhân viên tạo/cập nhật yêu cầu dịch vụ.

### Worst Step

Đọc, hiểu và cấu trúc mô tả không đồng nhất của khách hàng.

### Estimated Time

**Assumption:** khoảng 5–8 phút/yêu cầu trong trường hợp cần làm rõ.

> Đây là số liệu giả định để xây dựng bài toán, không phải số liệu vận hành thực tế của VinFast.

### AI Entry Point

AI đọc mô tả của khách hàng và:

1. Tóm tắt.
2. Trích xuất thông tin.
3. Phân loại yêu cầu.
4. Kiểm tra trường còn thiếu.
5. Đề xuất câu hỏi làm rõ.

### Metric

**Target:**

- Giảm thời gian xử lý xuống dưới 3 phút/yêu cầu.
- ≥ 90% độ chính xác trích xuất các trường bắt buộc.
- ≥ 90% độ chính xác phân loại cấp cao.
- 100% yêu cầu được nhân viên kiểm tra.

### Architecture

**LLM + Rule + Human Review**

---

# Quick Problem Card 2 — AI hỗ trợ câu hỏi về bảo hành

### Problem

Nhân viên cần tìm và giải thích thông tin chính sách bảo hành cho khách hàng.

### Company

VinFast

### Actor

Nhân viên chăm sóc khách hàng.

### Current Workflow

1. Nhận câu hỏi.
2. Xác định chủ đề.
3. Tìm chính sách liên quan.
4. Soạn câu trả lời.
5. Nhân viên kiểm tra.
6. Gửi khách hàng.

### Worst Step

Tìm đúng thông tin trong tài liệu chính sách.

### Estimated Time

**Assumption:** thời gian xử lý phụ thuộc vào độ phức tạp của câu hỏi.

### AI Entry Point

LLM + Retrieval hỗ trợ tìm tài liệu và tạo bản nháp.

### Metric

**Target:**

- Giảm thời gian tìm thông tin.
- Tăng tính nhất quán của câu trả lời.
- 100% câu trả lời được nhân viên kiểm tra.

### Architecture

**LLM + Retrieval + Human Review**

---

# Quick Problem Card 3 — AI phân loại phản hồi sau dịch vụ

### Problem

Nhân viên phải đọc và phân loại nhiều phản hồi có nội dung không đồng nhất.

### Company

VinFast

### Actor

Nhân viên chăm sóc khách hàng.

### Current Workflow

1. Thu thập phản hồi.
2. Đọc phản hồi.
3. Xác định chủ đề.
4. Phân loại.
5. Tóm tắt.
6. Chuyển các vấn đề cần xử lý.

### Worst Step

Đọc và phân loại các phản hồi dài hoặc có nhiều chủ đề.

### Estimated Time

**Assumption:** phụ thuộc vào số lượng và độ dài phản hồi.

### AI Entry Point

LLM hỗ trợ phân loại và tóm tắt.

### Metric

**Target:**

- ≥ 90% độ chính xác phân loại trên tập dữ liệu thử nghiệm.
- Giảm thời gian xử lý thủ công.
- 100% trường hợp quan trọng được nhân viên kiểm tra.

### Architecture

**LLM + Rule + Human Review**

---

# 3. Bài toán được lựa chọn

## AI hỗ trợ tiếp nhận, phân loại và cấu trúc yêu cầu sửa chữa của khách hàng tại VinFast

### Lý do lựa chọn

1. Liên quan trực tiếp đến bước **Tiếp nhận và tư vấn** trong quy trình dịch vụ.
2. Có dữ liệu đầu vào dạng ngôn ngữ tự nhiên.
3. LLM phù hợp với việc hiểu và tóm tắt ngôn ngữ.
4. Có thể đo lường hiệu quả.
5. Có thể triển khai Human-in-the-loop.
6. Có thể giới hạn AI để giảm rủi ro.
7. Không cần xây dựng Agentic AI phức tạp.

### Scope

**Trong scope:**

- Tiếp nhận.
- Trích xuất.
- Tóm tắt.
- Phân loại.
- Phát hiện thông tin thiếu.
- Đề xuất câu hỏi.

**Ngoài scope:**

- Chẩn đoán kỹ thuật.
- Quyết định bảo hành.
- Quyết định thay phụ tùng.
- Báo giá.
- Quyết định sửa chữa.
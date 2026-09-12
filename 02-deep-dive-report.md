# 02 - DEEP DIVE REPORT

## Bài toán được lựa chọn

> **AI hỗ trợ tiếp nhận, phân loại và cấu trúc yêu cầu sửa chữa của khách hàng tại VinFast**

---

# PHASE 3 — DEEP-DIVE

# 1. CURRENT-STATE WORKFLOW

Theo thông tin công khai, quy trình dịch vụ sửa chữa của VinFast gồm 5 bước:

1. Đặt hẹn sửa chữa.
2. Tiếp nhận và tư vấn.
3. Sửa chữa.
4. Bàn giao xe.
5. Chăm sóc sau sửa chữa.

Trong đó, VinFast nêu rằng khách hàng được chào đón và ghi nhận yêu cầu khi tới xưởng dịch vụ.

Trong phạm vi bài này, tôi tập trung sâu vào **bước Tiếp nhận và tư vấn**.

---

## 1.1 Quy trình hiện tại

### Bước 1 — Khách hàng gửi yêu cầu

Khách hàng cung cấp thông tin về:

* Thông tin cá nhân.
* Thông tin xe.
* Dịch vụ mong muốn.
* Mô tả vấn đề.
* Hình ảnh nếu có.

### Bước 2 — Nhân viên tiếp nhận

Nhân viên nhận thông tin từ khách hàng.

### Bước 3 — Đọc và hiểu mô tả

Nhân viên đọc nội dung khách hàng cung cấp.

### Bước 4 — Xác định loại yêu cầu

Nhân viên xác định nhóm yêu cầu ở mức tổng quát.

Ví dụ:

* Bảo dưỡng.
* Sửa chữa.
* Đồng sơn.
* Kiểm tra vấn đề.
* Yêu cầu khác.

### Bước 5 — Kiểm tra thông tin

Nhân viên kiểm tra xem thông tin cần thiết đã đầy đủ chưa.

### Bước 6 — Hỏi thêm nếu cần

Nếu thông tin chưa đủ, nhân viên trao đổi lại với khách hàng.

### Bước 7 — Cấu trúc yêu cầu

Nhân viên ghi nhận yêu cầu thành thông tin có cấu trúc để tiếp tục quy trình.

---

# 2. WORKFLOW TIME ESTIMATION

> **Lưu ý:** VinFast không công khai thời gian trung bình cho từng bước tiếp nhận. Các con số dưới đây là **Assumption** được sử dụng để minh họa bottleneck và xây dựng KPI pilot.

| Bước | Công việc                | Thời gian ước tính |
| ---- | ------------------------ | -----------------: |
| 1    | Đọc yêu cầu              |           1–2 phút |
| 2    | Hiểu và xác định vấn đề  |           1–2 phút |
| 3    | Phân loại                |            ~1 phút |
| 4    | Kiểm tra thông tin thiếu |            ~1 phút |
| 5    | Hỏi/làm rõ               |           1–2 phút |
| 6    | Cấu trúc yêu cầu         |            ~1 phút |
|      | **Tổng**                 |       **5–8 phút** |

---

# 3. HANDOFF

Các điểm chuyển giao chính:

### Handoff 1

**Khách hàng → Nhân viên dịch vụ**

Khách hàng cung cấp mô tả và thông tin xe.

### Handoff 2

**Nhân viên dịch vụ → Quy trình dịch vụ**

Sau khi yêu cầu được xác nhận và cấu trúc hóa, thông tin được sử dụng cho các bước tiếp theo.

### Handoff 3

**Nhân viên dịch vụ → Kỹ thuật viên**

Các vấn đề cần kiểm tra kỹ thuật được chuyển sang quá trình sửa chữa/kiểm tra.

> Bài toán AI chỉ tập trung vào việc hỗ trợ **Handoff 1 → Handoff 2**, không tự động quyết định kỹ thuật ở Handoff 3.

---

# 4. BOTTLENECK

## Bottleneck chính

> **Chuyển đổi mô tả tự nhiên của khách hàng thành thông tin dịch vụ có cấu trúc.**

Ví dụ khách hàng nói:

> "Xe chạy tầm 60–70km/h thì vô lăng rung, nhất là lúc phanh."

Nhân viên cần xác định:

* Triệu chứng: rung vô lăng.
* Điều kiện: khoảng 60–70 km/h.
* Tình huống: khi phanh.
* Mức độ.
* Tần suất.
* Thời điểm bắt đầu.
* Các thông tin khác còn thiếu.

Đây là công việc phù hợp để AI hỗ trợ vì có thành phần **Natural Language Understanding**.

---

# 5. PROBLEM STATEMENT — 6 FIELDS

## Field 1 — Actor

**Nhân viên tư vấn dịch vụ / nhân viên tiếp nhận yêu cầu của khách hàng.**

---

## Field 2 — Current Workflow

Nhân viên nhận mô tả từ khách hàng → đọc và hiểu nội dung → xác định triệu chứng → phân loại yêu cầu → kiểm tra thông tin còn thiếu → hỏi thêm khách hàng nếu cần → tạo/cập nhật yêu cầu dịch vụ.

---

## Field 3 — Bottleneck

Việc đọc, hiểu và chuyển đổi mô tả tự nhiên, không đồng nhất thành thông tin có cấu trúc.

---

## Field 4 — Business Impact

Bottleneck có thể dẫn đến:

* Tăng thời gian tiếp nhận.
* Tăng số lần trao đổi với khách hàng.
* Tăng nguy cơ bỏ sót thông tin.
* Thông tin đầu vào không đồng nhất.
* Tăng khối lượng công việc cho nhân viên.

> Các tác động trên là giả định cần được kiểm chứng trong pilot.

---

## Field 5 — Success Metric

### Metric chính

**Thời gian xử lý yêu cầu**

Assumption hiện tại:

> 5–8 phút/yêu cầu.

Target:

> **< 3 phút/yêu cầu**

### Metric phụ

* ≥ 90% độ chính xác trích xuất trường bắt buộc.
* ≥ 90% độ chính xác phân loại cấp cao.
* 100% yêu cầu được Human Review.
* Có thể fallback sang quy trình thủ công.

---

## Field 6 — Operational Boundary

### AI được phép

* Tóm tắt.
* Trích xuất thông tin.
* Phân loại yêu cầu.
* Phát hiện thông tin thiếu.
* Đề xuất câu hỏi.
* Tạo bản nháp yêu cầu.

### AI không được phép

* Chẩn đoán lỗi.
* Quyết định thay phụ tùng.
* Quyết định bảo hành.
* Quyết định giá.
* Cam kết thời gian sửa chữa.
* Đưa ra quyết định kỹ thuật.
* Tự động thực hiện hành động ảnh hưởng đến xe.

---

# 6. FUTURE-STATE FLOW

## Quy trình tương lai

```text
KHÁCH HÀNG
    │
    │ Mô tả vấn đề + thông tin xe
    ▼
AI SERVICE INTAKE ASSISTANT
    │
    ├── Tóm tắt
    ├── Trích xuất thông tin
    ├── Phân loại
    ├── Kiểm tra thông tin thiếu
    └── Đề xuất câu hỏi
    │
    ▼
NHÂN VIÊN DỊCH VỤ
    │
    ├── Accept
    ├── Edit
    └── Reject
    │
    ▼
YÊU CẦU DỊCH VỤ CÓ CẤU TRÚC
    │
    ▼
QUY TRÌNH DỊCH VỤ TIẾP TỤC
```

---

# 7. AI FIT

## 7.1 Rule / State Machine

### Phù hợp với

* Kiểm tra trường bắt buộc.
* Kiểm tra dữ liệu thiếu.
* Kiểm tra format.
* Xác định trạng thái yêu cầu.

### Ví dụ

Nếu thiếu:

`vehicle_model`

→ Hệ thống yêu cầu bổ sung.

Nếu thiếu:

`service_location`

→ Hệ thống yêu cầu bổ sung.

### Đánh giá

**Phù hợp cao.**

---

# 7.2 LLM Feature

### Phù hợp với

* Hiểu ngôn ngữ tự nhiên.
* Tóm tắt.
* Trích xuất triệu chứng.
* Phân loại yêu cầu.
* Đề xuất câu hỏi làm rõ.

### Đánh giá

**Phù hợp cao.**

---

# 7.3 Agentic Loop

### Không ưu tiên

Use case này không cần AI tự động thực hiện nhiều hành động liên tiếp.

Không cần Agent tự:

* Đặt lịch.
* Quyết định sửa chữa.
* Gửi cam kết cho khách hàng.
* Điều phối kỹ thuật viên.

### Đánh giá

**Không cần thiết trong giai đoạn pilot.**

---

# 8. KIẾN TRÚC ĐỀ XUẤT

## LLM + Rule + Human Review

### LLM

Xử lý ngôn ngữ:

> "Xe chạy nhanh thì vô lăng rung."

Có thể chuyển thành:

```text
Triệu chứng:
Rung vô lăng

Điều kiện:
Khi chạy ở tốc độ cao

Thông tin cần làm rõ:
- Tốc độ cụ thể
- Có xảy ra khi phanh không?
- Tần suất
```

### Rule

Kiểm tra:

* Có thông tin xe chưa?
* Có loại dịch vụ chưa?
* Có địa điểm chưa?
* Có thời gian chưa?
* Có trường bắt buộc nào bị thiếu?

### Human Review

Nhân viên kiểm tra và xác nhận kết quả.

---

# 9. HUMAN-IN-THE-LOOP

AI không thay thế nhân viên.

Quy trình:

```text
AI đề xuất
    ↓
Nhân viên kiểm tra
    ↓
Accept / Edit / Reject
    ↓
Xác nhận
    ↓
Tiếp tục quy trình
```

Human Review là **bắt buộc**.

---

# 10. FALLBACK

Nếu:

* AI không hoạt động.
* AI không hiểu yêu cầu.
* Confidence thấp.
* Nội dung nằm ngoài phạm vi.

→ Chuyển sang:

> **Manual Service Intake**

Nhân viên tiếp tục xử lý theo quy trình thông thường.

Điều này giúp hoạt động dịch vụ không phụ thuộc hoàn toàn vào AI.

---

# 11. AI OPERATIONAL BOUNDARY

| Hoạt động                |  AI | Con người |
| ------------------------ | :-: | :-------: |
| Tóm tắt yêu cầu          |  ✅  |   Review  |
| Trích xuất thông tin     |  ✅  |   Review  |
| Phân loại cấp cao        |  ✅  |  Confirm  |
| Phát hiện trường thiếu   |  ✅  |  Confirm  |
| Đề xuất câu hỏi          |  ✅  |    Chọn   |
| Chẩn đoán lỗi            |  ❌  |     ✅     |
| Quyết định thay phụ tùng |  ❌  |     ✅     |
| Quyết định bảo hành      |  ❌  |     ✅     |
| Báo giá                  |  ❌  |     ✅     |
| Cam kết sửa chữa         |  ❌  |     ✅     |

---

# 12. RISK & CONTROL

| Rủi ro                                | Mức độ     | Cách kiểm soát                     |
| ------------------------------------- | ---------- | ---------------------------------- |
| AI hiểu sai triệu chứng               | Cao        | Human Review                       |
| AI phân loại sai                      | Trung bình | Cho phép Edit/Reject               |
| AI tự chẩn đoán                       | Cao        | Operational Boundary               |
| AI tạo thông tin không có trong input | Cao        | Prompt + Validation                |
| AI không hoạt động                    | Trung bình | Fallback                           |
| AI đưa quyết định bảo hành            | Cao        | Cấm AI thực hiện                   |
| AI đề xuất sửa chữa                   | Cao        | Chuyển cho nhân viên/kỹ thuật viên |

---

# PHASE 5 — EVALUATE

# 13. EVALUATE CHECKLIST

## 13.1 Có sample data/logs không?

### Đánh giá

**Có thể chuẩn bị.**

Trong pilot có thể tạo tập dữ liệu gồm:

* 50–100 yêu cầu giả lập.
* Yêu cầu ngắn.
* Yêu cầu dài.
* Yêu cầu thiếu thông tin.
* Lỗi chính tả.
* Nhiều triệu chứng trong một yêu cầu.
* Các cách diễn đạt khác nhau cho cùng một vấn đề.

### Status

**READY FOR PILOT**

---

# 14. Risk có kiểm soát được không?

### Đánh giá

**Có.**

Các cơ chế:

* Human-in-the-loop.
* Fallback.
* Operational Boundary.
* Rule validation.
* Không cho AI thực hiện quyết định kỹ thuật.

### Status

**CONTROLLABLE**

---

# 15. Stakeholder Readiness

Cần kiểm chứng với nhân viên dịch vụ trong pilot.

Các câu hỏi cần đánh giá:

1. AI có giúp giảm thời gian nhập liệu không?
2. Kết quả AI có dễ kiểm tra không?
3. Nhân viên có dễ sửa kết quả không?
4. AI có bỏ sót thông tin quan trọng không?
5. Nhân viên có cảm thấy AI thực sự hỗ trợ công việc không?

### Status

**NEEDS VALIDATION**

---

# 16. PILOT TEST PLAN

## Dataset

50–100 yêu cầu giả lập.

## So sánh

### Baseline

Nhân viên xử lý thủ công.

### AI-assisted

AI tạo bản nháp → Nhân viên kiểm tra.

## Đo lường

| Metric                     |       Baseline |   Target |
| -------------------------- | -------------: | -------: |
| Thời gian xử lý            |      5–8 phút* | < 3 phút |
| Trích xuất trường bắt buộc | Đo trong pilot |    ≥ 90% |
| Phân loại                  | Đo trong pilot |    ≥ 90% |
| Human Review               |           100% |     100% |

`* Assumption — cần xác nhận bằng dữ liệu thực tế.`

---

# 17. ADVERSARIAL TEST CASES

## Case 1 — Yêu cầu quá ngắn

> "Xe bị rung."

### Expected

AI không chẩn đoán.

AI đề xuất hỏi thêm thông tin.

---

## Case 2 — Mô tả mơ hồ

> "Xe có tiếng kêu lạ."

### Expected

AI đánh dấu:

> Thiếu thông tin.

---

## Case 3 — Nhiều triệu chứng

> "Xe bị rung vô lăng và có tiếng kêu khi phanh."

### Expected

AI tách thành các triệu chứng riêng.

Không tự kết luận nguyên nhân.

---

## Case 4 — Yêu cầu chẩn đoán

> "Xe tôi bị lỗi gì và cần thay bộ phận nào?"

### Expected

AI không chẩn đoán.

AI chuyển yêu cầu sang nhân viên/kỹ thuật viên.

---

# 18. DECISION

## GO — CONTROLLED PILOT

### Lý do

Use case đạt các điều kiện:

* Problem rõ ràng.
* AI fit tốt.
* Có thể đo lường.
* Có thể kiểm soát rủi ro.
* Có fallback.
* Có Human-in-the-loop.
* Không cần Agentic AI phức tạp.

### Phạm vi Pilot

```text
Tiếp nhận
    ↓
Trích xuất
    ↓
Phân loại
    ↓
Phát hiện thông tin thiếu
    ↓
Đề xuất câu hỏi
    ↓
Human Review
    ↓
Yêu cầu có cấu trúc
```

### Không triển khai ở giai đoạn đầu

* Chẩn đoán tự động.
* Quyết định sửa chữa.
* Quyết định bảo hành.
* Quyết định thay phụ tùng.
* Báo giá tự động.
* Agent tự động xử lý toàn bộ quy trình.

### Final Decision

> **GO — CONTROLLED PILOT**

Giải pháp phù hợp để thử nghiệm ở phạm vi hỗ trợ nhân viên tiếp nhận và cấu trúc yêu cầu, thay vì tự động hóa hoàn toàn.

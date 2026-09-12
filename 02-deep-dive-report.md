# Lab 02 — Phase 3 DEEP-DIVE & Phase 5 EVALUATE
## Bài toán: Xanh SM Intelligent Dispatcher

> **Cách làm:** Copy và hoàn thiện nội dung của Phase 3 (DEEP-DIVE) và Phase 5 (EVALUATE) từ file `01-worksheet.md`.
>
> **Yêu cầu nội dung:**
> - **Problem Statement (6-field):** Điền đầy đủ 6 trường thông tin cho bài toán đã chọn.
> - **Future-State Flow & AI Fit:** Mô tả quy trình tương lai có tích hợp AI (Rule, LLM, Agentic Loop), cơ chế Human-in-the-loop và Fallback.
> - **Evaluate:** Đánh giá độ sẵn sàng qua bảng Checklist và đưa ra quyết định GO / NOT YET / NO-GO.
>
> **Lưu ý:** `01-worksheet.md` không chứa dữ liệu đã điền cho Phase 1–2; bài toán được chọn ở đây là **Xanh SM Intelligent Dispatcher**, phù hợp với worked example được nêu trong worksheet. Các số liệu vận hành cụ thể dưới đây là **giả định dùng cho bài tập/prototype**, cần được xác minh bằng dữ liệu thực tế trước khi triển khai.

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)

### Bài toán được chọn

**Xanh SM Intelligent Dispatcher — hỗ trợ điều phối và tối ưu điểm đón khách cho tài xế.**

### Current-State Flow

```text
Khách đặt chuyến
      ↓
🔄 Hệ thống nhận yêu cầu + vị trí khách
      ↓
Hệ thống xác định tài xế phù hợp
      ↓
🔄 Gửi thông tin chuyến cho tài xế
      ↓
🔴 Tài xế tiếp cận điểm đón / xử lý trường hợp điểm đón không chính xác
      ↓
Tài xế liên hệ khách hoặc tự điều chỉnh điểm đón
      ↓
🔄 Cập nhật lại trạng thái chuyến
      ↓
Đón khách thành công
```

### Bottleneck

🔴 **Bottleneck chính:** Xác định và xử lý điểm đón thực tế khi dữ liệu vị trí/địa điểm của khách không đủ chính xác. Tài xế có thể phải tự tìm vị trí, gọi khách hoặc điều chỉnh điểm đón.

### Handoff

🔄 **Handoff chính:**
- Khách → hệ thống: thông tin đặt chuyến và vị trí.
- Hệ thống → tài xế: điểm đón và thông tin chuyến.
- Tài xế → khách: trao đổi khi điểm đón không rõ.
- Tài xế → hệ thống: cập nhật trạng thái/chỉnh sửa thực tế.

### Thời gian vận hành trung bình

**Tổng cộng = khoảng 5–10 phút/lượt đối với các chuyến phát sinh vấn đề về điểm đón.**

> Đây là **giả định để xây dựng prototype**, không phải số liệu vận hành đã được xác nhận.

---

## 3.2. Problem Statement (6-field) & Metrics (15 min)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | **Tài xế Xanh SM** là người trực tiếp thực hiện việc tiếp cận điểm đón và xử lý tình huống khi vị trí khách không rõ/chưa chính xác. **Bộ phận vận hành/dispatch** giám sát các trường hợp bất thường. |
| **2. Current Workflow** | Khách đặt chuyến trên ứng dụng → hệ thống nhận vị trí → hệ thống phân bổ chuyến cho tài xế → tài xế di chuyển đến điểm đón → nếu điểm đón không chính xác hoặc khó tiếp cận, tài xế tự tìm vị trí, gọi/nhắn khách và điều chỉnh điểm đón → cập nhật trạng thái chuyến. Công cụ chính là ứng dụng Xanh SM và kênh liên lạc với khách. |
| **3. Bottleneck** | **Xác định điểm đón thực tế** khi tọa độ GPS, địa chỉ nhập vào hoặc bối cảnh địa điểm chưa đủ rõ. Bước này có thể khiến tài xế phải tìm kiếm thủ công và trao đổi nhiều lần với khách. |
| **4. Business Impact** | Làm tăng thời gian chờ/tiếp cận khách, tăng số lần tài xế phải liên hệ khách, giảm hiệu suất khai thác phương tiện và có thể ảnh hưởng trải nghiệm khách hàng/SLA. **Baseline chi phí và SLA thực tế cần được lấy từ log vận hành.** |
| **5. Success Metric** | Prototype mục tiêu: **≥85% các tình huống điểm đón có vấn đề được hệ thống đề xuất điểm đón/route phù hợp trong ≤10 giây**, đồng thời giảm **≥30% thời gian xử lý thủ công** của tài xế trong nhóm chuyến có vấn đề. Các ngưỡng cần được hiệu chỉnh sau khi có baseline thực tế. |
| **6. Operational Boundary** | AI **được phép** phân tích ngữ cảnh chuyến đi, vị trí, dữ liệu bản đồ được cấp quyền và tín hiệu lịch sử để **đề xuất** điểm đón/đường tiếp cận phù hợp. AI **không được tự ý** hủy chuyến, thay đổi giá, thực hiện quyết định ảnh hưởng quyền lợi khách hàng/tài xế hoặc điều khiển phương tiện. Các trường hợp confidence thấp, vị trí bất thường hoặc có tranh chấp phải chuyển sang **HITL** hoặc quy trình fallback. |

---

## 3.3. Future-State Flow & AI Fit (25 min)

### AI-Fit Matrix

| Phương án | Vai trò | Đánh giá |
|---|---|---|
| **Rule / State-Machine** | Xử lý các quy tắc cố định như geofence, khoảng cách, điều kiện GPS và trạng thái chuyến | **Nên dùng làm lớp kiểm soát bắt buộc** |
| **LLM Feature** | Hiểu mô tả tự nhiên của khách/tài xế, tóm tắt tình huống và hỗ trợ diễn giải | **Phù hợp ở lớp hỗ trợ ngôn ngữ** |
| **Agentic Loop** | Thu thập tín hiệu → đề xuất → kiểm tra → điều chỉnh → yêu cầu con người khi cần | **Phù hợp cho prototype điều phối có nhiều bước**, nhưng phải giới hạn scope và quyền hành động |

### Kiến trúc đề xuất

**Rule + LLM/Agentic Loop**, trong đó Rule/State-Machine là lớp guardrail và LLM/Agentic Loop chỉ xử lý các phần cần hiểu ngữ cảnh.

### Future-State Flow

```text
Khách đặt chuyến
      ↓
🔄 Hệ thống nhận vị trí + thông tin chuyến
      ↓
🔵 AI Step: Phân tích vị trí, ngữ cảnh địa điểm và tín hiệu liên quan
      ↓
🔵 AI Step: Đề xuất điểm đón / hướng tiếp cận
      ↓
🛡️ Rule Guardrail:
   - Kiểm tra khoảng cách
   - Kiểm tra geofence
   - Kiểm tra điều kiện dữ liệu
   - Kiểm tra confidence
      ↓
   ┌───────────────────────────────┐
   │ Confidence đạt ngưỡng?        │
   └───────────────────────────────┘
          ↓ Có                    ↓ Không
          ↓                       ↓
🔵 Gửi đề xuất cho tài xế     ↩️ Fallback
          ↓                       ↓
🟢 Tài xế xác nhận/điều chỉnh   🟢 Human/Operations Review
          ↓                       ↓
🔄 Cập nhật hệ thống            Cập nhật thủ công
          ↓                       ↓
      Đón khách thành công ←──────┘
```

### Human-in-the-loop (HITL)

Con người giữ quyền quyết định trong các trường hợp:

- AI có **confidence thấp**.
- Có nhiều điểm đón khả thi và không thể xác định rõ lựa chọn tốt nhất.
- Vị trí khách nằm ngoài khu vực dữ liệu/địa điểm đã biết.
- Có dấu hiệu dữ liệu GPS hoặc bản đồ bất thường.
- Đề xuất của AI mâu thuẫn với thông tin do khách/tài xế cung cấp.
- Có tranh chấp hoặc tình huống có thể ảnh hưởng trực tiếp đến khách hàng/tài xế.

**Nguyên tắc:** AI đưa ra **recommendation**, con người giữ quyền phê duyệt đối với các quyết định có rủi ro.

### Fallback

Khi LLM/Agent trả về lỗi, timeout, confidence thấp hoặc kết quả không hợp lệ:

1. Không sử dụng kết quả AI.
2. Quay về **logic Rule-based hiện tại**.
3. Cho tài xế sử dụng quy trình tìm điểm đón thủ công.
4. Nếu vẫn không xử lý được, chuyển cho **bộ phận vận hành/HITL**.
5. Ghi log nguyên nhân fallback để phục vụ đánh giá và cải thiện model.

### Operational Safety Guardrails

- Không cho LLM trực tiếp thực thi các hành động nhạy cảm.
- Structured output bắt buộc cho kết quả AI.
- Có confidence/validation trước khi đưa recommendation vào workflow.
- Rule engine có quyền từ chối kết quả AI.
- Mọi trường hợp fallback phải được logging.
- Prototype chỉ giới hạn ở **đề xuất điểm đón/hướng tiếp cận**, không tự động điều khiển phương tiện hay thay đổi chính sách chuyến.

---


# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

## AI Readiness Checklist

| # | Checklist | Đánh giá | Bằng chứng / Ghi chú |
|---|---|---|---|
| **1** | Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? | ⚠️ **CHƯA ĐỦ** | Cần lấy trip logs, GPS, điểm đón thực tế và các case tài xế phải điều chỉnh thủ công. |
| **2** | Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? | ✅ **CÓ** | Recommendation-only, Rule Guardrail, HITL và fallback về quy trình hiện tại. AI không được tự thực hiện hành động nhạy cảm. |
| **3** | Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? | ⚠️ **CẦN XÁC NHẬN** | Cần xác nhận với tài xế, đội vận hành/dispatch và các bên liên quan trước pilot. |

### Tổng kết Readiness

- **Dữ liệu:** Chưa đủ để chứng minh ROI/accuracy → cần baseline.
- **Technical safety:** Có thể kiểm soát bằng Rule + HITL + Fallback.
- **Operational adoption:** Có tiềm năng nhưng cần validation với người dùng thực tế.
- **Prototype feasibility:** Khả thi nếu giới hạn scope vào recommendation và không trao quyền tự động quá mức.

---

## Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future

### ☑️ **NOT YET — Cần tích lũy thêm dữ liệu/xác lập baseline**

**Lý do chọn NOT YET thay vì GO:**

1. Ý tưởng có **AI Fit rõ ràng**, đặc biệt ở việc hiểu ngữ cảnh và xử lý các tình huống điểm đón không chuẩn hóa.
2. Rủi ro kỹ thuật có thể kiểm soát bằng **Rule Guardrail + HITL + Fallback**.
3. Tuy nhiên, worksheet hiện chưa cung cấp **log vận hành thực tế**, baseline về thời gian xử lý, tỷ lệ điểm đón sai và chi phí cơ hội.
4. Nếu triển khai ngay mà chưa có baseline, nhóm không thể chứng minh rằng AI tốt hơn hệ thống Rule-based hiện tại hoặc tạo ra ROI đủ lớn.
5. Vì vậy, bước tiếp theo nên là thu thập dữ liệu và chạy prototype offline trước khi quyết định pilot production.

### Điều kiện để chuyển từ NOT YET → GO

- [ ] Có dataset/logs đủ sạch và đại diện cho các tình huống điểm đón.
- [ ] Xác lập baseline về thời gian xử lý, tỷ lệ lỗi và tỷ lệ phải liên hệ khách.
- [ ] Chạy benchmark **Rule-only vs LLM/Agent-assisted**.
- [ ] Đạt success metric đã thống nhất.
- [ ] Adversarial tests không phá vỡ Operational Boundary.
- [ ] Tài xế/Operations xác nhận recommendation thực sự hữu ích.
- [ ] Có dashboard/logging cho confidence, fallback và HITL.

### Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí)

> **NOT YET** là quyết định phù hợp nhất ở giai đoạn hiện tại. Bài toán có pain point vận hành hợp lý và có thể áp dụng kiến trúc kết hợp Rule + LLM/Agentic Loop. Tuy nhiên, dữ liệu trong worksheet chưa cung cấp baseline thực tế để chứng minh mức độ cải thiện về thời gian, lỗi hoặc chi phí. Do đó, nhóm không nên nhảy thẳng sang production prototype.
>
> Trước mắt, cần xây dựng một prototype offline với dữ liệu lịch sử, so sánh với Rule-based baseline và đo các metric đã xác định. Nếu kết quả chứng minh AI tạo ra cải thiện đáng kể trong khi tỷ lệ fallback và rủi ro vẫn nằm trong giới hạn kiểm soát, dự án có thể chuyển sang **GO** với scope hẹp. Nếu Rule-based đạt hiệu quả tương đương với chi phí thấp hơn, cần cân nhắc **NO-GO** đối với phần AI.

---

## 📌 Tóm tắt Deliverable

| Hạng mục | Kết quả |
|---|---|
| **Problem** | Xanh SM Intelligent Dispatcher — hỗ trợ điểm đón/điều phối |
| **Bottleneck** | Xử lý điểm đón không chính xác/không rõ |
| **AI Fit** | Rule + LLM/Agentic Loop |
| **AI Role** | Phân tích ngữ cảnh và đề xuất |
| **HITL** | Tài xế/Operations xác nhận trường hợp cần thiết |
| **Fallback** | Rule-based + quy trình thủ công hiện tại |
| **Success Metric** | ≥85% recommendation trong ≤10s; giảm ≥30% thời gian xử lý thủ công |
| **Readiness** | Có thể kiểm soát rủi ro nhưng thiếu baseline |
| **Final Decision** | **NOT YET** |

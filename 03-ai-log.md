# 03 - AI LOG

## Nhật ký sử dụng AI trong quá trình thực hiện Lab 02

---

# 1. Mục đích sử dụng AI

Trong quá trình thực hiện Lab 02, tôi sử dụng AI như một **trợ lý tư duy (thought partner)** thay vì coi AI là nguồn sự thật tuyệt đối.

AI được sử dụng để:

* Brainstorm các vấn đề tiềm năng.
* So sánh các ý tưởng AI.
* Phân tích workflow.
* Xác định bottleneck.
* Xây dựng Problem Statement.
* Xác định AI Fit.
* Thiết kế Future-State Flow.
* Xác định Human-in-the-loop.
* Xác định Fallback.
* Tìm các trường hợp AI có thể đưa ra kết quả sai.
* Kiểm tra và cải thiện operational boundary.

---

# 2. AI giúp tìm Problem như thế nào?

Ban đầu, tôi chưa có một bài toán cụ thể.

Tôi sử dụng AI để brainstorm các vấn đề có thể xảy ra trong các quy trình dịch vụ của VinFast.

Sau khi so sánh nhiều ý tưởng, bài toán được lựa chọn là:

> **AI hỗ trợ tiếp nhận, phân loại và cấu trúc yêu cầu sửa chữa của khách hàng.**

Lý do:

* Có đầu vào dạng ngôn ngữ tự nhiên.
* Có công việc lặp lại.
* Có thể áp dụng LLM.
* Có thể đo lường.
* Có thể giới hạn AI.
* Có thể giữ Human-in-the-loop.

---

# 3. AI hỗ trợ xây dựng Current-State Workflow

AI giúp chia nhỏ quy trình thành các bước:

1. Khách hàng gửi yêu cầu.
2. Nhân viên tiếp nhận.
3. Đọc mô tả.
4. Hiểu vấn đề.
5. Phân loại.
6. Kiểm tra thông tin thiếu.
7. Hỏi thêm.
8. Cấu trúc yêu cầu.

Qua đó, bottleneck được xác định là:

> **Chuyển đổi mô tả tự nhiên của khách hàng thành thông tin có cấu trúc.**

---

# 4. AI Hallucination / Thông tin chưa được kiểm chứng

Một trong những vấn đề quan trọng khi sử dụng AI là AI có thể tạo ra thông tin nghe có vẻ hợp lý nhưng không có nguồn.

Ví dụ, AI có thể đưa ra một câu như:

> "Nhân viên VinFast thường mất 10 phút để xử lý một yêu cầu."

Nếu không có nguồn nội bộ hoặc nguồn công khai thì không thể coi đây là Fact.

Do đó, tôi đã thay đổi cách sử dụng AI:

> **Không sử dụng số liệu do AI tự tạo như dữ liệu thực tế của doanh nghiệp.**

Nếu cần sử dụng số liệu để xây dựng prototype, tôi ghi rõ đó là:

* **Assumption**
* hoặc **Target**

---

# 5. Phân biệt FACT / ASSUMPTION / TARGET

## FACT

Thông tin được xác nhận từ nguồn đáng tin cậy.

Ví dụ:

VinFast công khai quy trình dịch vụ sửa chữa gồm 5 bước và bước 2 là **Tiếp nhận và tư vấn**. VinFast cũng mô tả việc ghi nhận yêu cầu của khách hàng khi khách hàng tới xưởng.

---

## ASSUMPTION

Giả định được đưa ra để xây dựng mô hình khi không có dữ liệu nội bộ.

Ví dụ:

> Thời gian tiếp nhận và cấu trúc yêu cầu khoảng 5–8 phút.

Đây không phải số liệu thực tế của VinFast.

---

## TARGET

Mục tiêu dùng để đánh giá pilot.

Ví dụ:

> Giảm thời gian xử lý xuống dưới 3 phút/yêu cầu.

Đây là mục tiêu thử nghiệm, không phải kết quả đã đạt được.

---

# 6. Prompt ban đầu

Một prompt ban đầu có thể là:

> "Hãy tìm các vấn đề có thể áp dụng AI tại VinFast."

Kết quả có thể quá rộng và có nguy cơ khiến AI tự suy đoán về quy trình nội bộ.

---

# 7. Prompt được cải thiện

Prompt sau được sử dụng:

> "Hãy đề xuất các vấn đề trong quy trình dịch vụ của VinFast có thể được hỗ trợ bởi AI. Chỉ sử dụng thông tin công khai khi mô tả quy trình thực tế. Nếu không có dữ liệu nội bộ, hãy đánh dấu rõ các số liệu là Assumption hoặc Target. Không tự tạo số liệu vận hành. Với mỗi bài toán, hãy xác định Actor, Current Workflow, Bottleneck, AI Entry Point và Metric."

Prompt này giúp:

* Giảm hallucination.
* Làm rõ nguồn dữ liệu.
* Phân biệt Fact và Assumption.
* Tạo output theo đúng cấu trúc worksheet.
* Tập trung vào bài toán thay vì chỉ nói chung về AI.

---

# 8. AI đề xuất quá mức cần thiết

Trong quá trình brainstorming, AI có thể đề xuất sử dụng:

> **Agentic AI**

để tự động xử lý toàn bộ quy trình.

Tuy nhiên, sau khi phân tích kỹ, tôi nhận thấy bài toán không cần Agentic Loop.

Use case chủ yếu cần:

* LLM để hiểu ngôn ngữ.
* Rule để kiểm tra trường dữ liệu.
* Human để xác nhận.

Vì vậy kiến trúc được đơn giản hóa thành:

> **LLM + Rule + Human Review**

Điều này phù hợp hơn với mục tiêu của bài và giảm rủi ro.

---

# 9. Xác định AI Boundary

Ban đầu, ý tưởng có thể mở rộng thành:

> "AI chẩn đoán lỗi xe."

Sau khi phân tích rủi ro, tôi nhận thấy đây là phạm vi quá rộng và có thể gây hậu quả nếu AI đưa ra kết luận sai.

Vì vậy, phạm vi được thu hẹp.

## AI ĐƯỢC PHÉP

* Tóm tắt.
* Trích xuất.
* Phân loại.
* Kiểm tra thông tin thiếu.
* Đề xuất câu hỏi.
* Tạo bản nháp.

## AI KHÔNG ĐƯỢC PHÉP

* Chẩn đoán lỗi.
* Quyết định sửa chữa.
* Quyết định thay phụ tùng.
* Quyết định bảo hành.
* Báo giá.
* Cam kết thời gian.
* Đưa ra quyết định kỹ thuật.

---

# 10. Human-in-the-loop

AI không được thay thế hoàn toàn nhân viên.

Quy trình được thiết kế:

```text
AI đề xuất
     ↓
Nhân viên kiểm tra
     ↓
Accept / Edit / Reject
     ↓
Nhân viên xác nhận
     ↓
Tiếp tục quy trình
```

Điều này giúp kiểm soát các trường hợp AI hiểu sai yêu cầu.

---

# 11. Fallback

Tôi xác định rằng AI phải có phương án dự phòng.

Nếu:

* AI không hoạt động.
* AI không hiểu yêu cầu.
* Confidence thấp.
* Yêu cầu nằm ngoài phạm vi.

thì hệ thống phải chuyển sang:

> **Manual Service Intake**

Nhân viên vẫn có thể xử lý yêu cầu mà không phụ thuộc vào AI.

---

# 12. Adversarial Cases

Để kiểm tra giới hạn của AI, tôi xây dựng một số trường hợp:

### Case 1

> "Xe bị rung."

AI phải hỏi thêm thay vì chẩn đoán.

### Case 2

> "Xe có tiếng kêu."

AI phải đánh dấu thiếu thông tin.

### Case 3

> "Xe rung vô lăng và có tiếng kêu khi phanh."

AI phải tách các triệu chứng.

### Case 4

> "Xe tôi bị lỗi gì và cần thay bộ phận nào?"

AI không được chẩn đoán hoặc quyết định thay phụ tùng.

---

# 13. Những thay đổi sau khi sử dụng AI

Sau quá trình sử dụng AI, tôi đã thay đổi một số điểm trong bài:

### Thay đổi 1 — Không coi số liệu AI tạo ra là Fact

Tôi phân biệt:

> Fact ≠ Assumption ≠ Target

---

### Thay đổi 2 — Thu hẹp phạm vi AI

Từ:

> AI chẩn đoán lỗi xe

thành:

> AI hỗ trợ tiếp nhận và cấu trúc yêu cầu.

---

### Thay đổi 3 — Không sử dụng Agentic AI khi chưa cần

Từ:

> AI tự động xử lý toàn bộ workflow

thành:

> LLM + Rule + Human Review.

---

### Thay đổi 4 — Thêm Fallback

AI không được trở thành điểm thất bại duy nhất của workflow.

---

### Thay đổi 5 — Thêm Adversarial Cases

Thay vì chỉ kiểm tra trường hợp AI hoạt động tốt, tôi kiểm tra cả những trường hợp:

* Input quá ngắn.
* Input mơ hồ.
* Input có nhiều vấn đề.
* Người dùng yêu cầu AI chẩn đoán.

---

# 14. Reflection

Qua quá trình sử dụng AI, tôi nhận thấy AI rất hữu ích trong việc:

* Brainstorm.
* Phân tích vấn đề.
* Xây dựng workflow.
* Đề xuất metric.
* Tìm rủi ro.
* Tạo các trường hợp kiểm thử.

Tuy nhiên, AI không nên được sử dụng như một nguồn sự thật tuyệt đối.

Một câu trả lời của AI có thể **nghe hợp lý nhưng vẫn không đúng**.

Vì vậy, người sử dụng cần:

1. Kiểm tra nguồn.
2. Phân biệt Fact và Assumption.
3. Không sử dụng số liệu không có căn cứ.
4. Xác định rõ AI Boundary.
5. Thiết kế Human-in-the-loop.
6. Có Fallback.
7. Kiểm thử các trường hợp AI có thể thất bại.

---

# 15. Kết luận

AI đã giúp tôi chuyển từ một ý tưởng khá rộng thành một bài toán có phạm vi rõ ràng:

> **AI hỗ trợ tiếp nhận, phân loại và cấu trúc yêu cầu sửa chữa của khách hàng tại VinFast.**

Giải pháp cuối cùng không phải là một hệ thống AI tự động hoàn toàn mà là:

> **LLM + Rule + Human Review + Fallback**

Quyết định cuối cùng:

> **GO — CONTROLLED PILOT**

AI chỉ được triển khai thử nghiệm ở phạm vi hỗ trợ nhân viên tiếp nhận, trích xuất, phân loại và làm rõ yêu cầu; các quyết định kỹ thuật vẫn thuộc về con người.

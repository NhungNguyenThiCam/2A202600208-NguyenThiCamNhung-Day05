# UX exercise — MoMo Moni AI  

## Sản phẩm: MoMo — Moni AI Assistant (chat-based financial assistant)

---

## 4 paths

### 1. AI đúng
- User hỏi: “tôi chuyển tiền lúc 10h tối qua cho ai”
- Moni trả về giao dịch cụ thể (GRAB, 94.000đ, thời gian, danh mục)
- User dễ verify nhờ UI card hiển thị rõ
- UI: hiển thị transaction + thông tin chi tiết, không cần user chỉnh sửa  

- **Lưu ý:**
  - AI trả kết quả **phù hợp (near match)** và có đủ dữ liệu để user tự verify  
  - Có thể cần suy luận nhẹ (ví dụ lệch thời gian 10h vs 23h)

---

### 2. AI không chắc
- User nhập: “cái đó đó” hoặc “giao dịch đó”
- Moni hỏi lại nhiều lần: “bạn có thể nói rõ hơn?”
- UI: chỉ có text clarification, không có gợi ý cụ thể  

- **Vấn đề:**
  - không tận dụng context từ lịch sử hội thoại hoặc giao dịch gần nhất  
  - không show alternatives (ví dụ: 2–3 giao dịch gần nhất để user chọn)  
  - dẫn đến loop hỏi lại → tăng friction  

- **Insight:**
  > AI chưa làm tốt context mapping giữa các lượt hội thoại, nên không thể suy luận hoặc gợi ý khi input mơ hồ

---

### 3. AI sai
- User phát hiện sai: ví dụ trả nợ bị classify thành chi tiêu  
- Moni giải thích logic nhưng không cho sửa trực tiếp  

- **Sửa:**
  - user phải: hỏi → đọc hướng dẫn → vào chỉnh → nhiều bước  

- **UI:**
  - không có shortcut edit ngay tại chỗ  

- **Vấn đề:**
  - high friction trong recovery  
  - AI không chuyển sang “repair mode” khi user phản bác  
  - không rõ AI có học từ correction hay không  

- **Insight:**
  > AI không detect được khi câu trả lời không giải quyết đúng intent, nên tiếp tục giữ logic ban đầu thay vì chuyển sang flow sửa lỗi

---

### 4. User mất niềm tin
- User lặp lại câu hỏi nhiều lần hoặc yêu cầu “gặp nhân viên”  
- Moni trả lời từ chối theo script (“chỉ hỗ trợ thông tin…”)  

- **UI:**
  - không có nút chat với người thật hoặc fallback rõ ràng  

- **Vấn đề:**
  - dead-end trong trải nghiệm  
  - không có exit path khi AI không giải quyết được  
  - đặc biệt rủi ro với use case tài chính (liên quan tiền và trust)  

- **Insight:**
  > Hệ thống thiếu cơ chế escalation và human fallback, vi phạm nguyên tắc “always provide an exit” trong AI UX

---

## Path yếu nhất: Path 4 (và Path 3)

- **Path 4:**
  - không có human fallback → user bị kẹt  
  - không có trust recovery  

- **Path 3:**
  - sửa lỗi mất nhiều bước  
  - không có feedback loop rõ (user không biết AI có học không)

---

## Gap marketing vs thực tế

- **Marketing:**
  - “Trợ thủ AI thông minh hỗ trợ tài chính”

- **Thực tế:**
  - hoạt động tốt ở happy path (query rõ ràng)  
  - yếu ở:
    - ambiguity (input mơ hồ)  
    - error handling  
    - trust recovery  

- **Gap lớn nhất:**
  > AI được kỳ vọng hiểu context và xử lý linh hoạt, nhưng thực tế chưa làm tốt context mapping và fallback khi sai  

- **Insight bổ sung:**
  > Hệ thống hiện tối ưu cho happy path, nhưng chưa đủ robust ở các tình huống ngoài happy path — trong khi đây mới là nơi quyết định trải nghiệm thực tế của user

---

## Sketch

*(Ảnh đính kèm: sketch1.jpg,sketch2.jpg)*

### As-is
- (Ảnh 1 — ambiguity): AI bị loop khi user nhập “cái đó đó” → không hiểu context → user phải gõ lại  
- (Ảnh 2 — trust): AI từ chối khi user yêu cầu gặp người thật → không có fallback → dead-end  

---

### To-be

#### Ambiguity:
- AI hiển thị 2–3 giao dịch gần nhất → user tap chọn (tap-to-fix)  

#### Mất tin:
- AI detect frustration → hiện:
  - “Kết nối CSKH”
  - “Gọi tổng đài”

#### Correction:
- cho phép edit trực tiếp tại message  
- hiển thị feedback: “Đã ghi nhận, lần sau sẽ chính xác hơn”

---

## Principle

- Chuyển từ **text-based interaction → UI-assisted interaction**  
- Thêm **escalation flow** khi AI fail  
- Tách rõ các mode:
  - answer → clarify → repair → escalate  

---

## Conclusion

> Moni làm tốt ở happy path nhờ dữ liệu và rule-based flow, nhưng chưa đủ robust trong handling ambiguity, error recovery và trust breakdown. Đặc biệt, thiếu human fallback là điểm yếu lớn trong domain tài chính.
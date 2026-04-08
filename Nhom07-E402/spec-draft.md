# SPEC Draft — Nhom01 (VinFast Car Assistant)
**Track:** A - VinFast

---

## 1. Problem Statement
Sách hướng dẫn sử dụng (Manual) của VinFast dài hàng trăm trang, gây khó khăn cho người dùng:
* **Tính cấp bách:** Khi đang lái xe hoặc gặp sự cố, chủ xe không thể lật sách để tra cứu thủ công.
* **Trải nghiệm kém:** Việc không hiểu rõ các tính năng (như ADAS, sạc pin tối ưu) dẫn đến trải nghiệm không tốt hoặc gây mất an toàn.

---

## 2. Canvas Draft

| Tiêu chí | Nội dung chi tiết |
| :--- | :--- |
| **Value** | Chủ xe (đặc biệt là người mới) giải quyết ngay lập tức thắc mắc về tính năng mà không cần dừng xe lật sách. |
| **Trust** | AI phải trích dẫn đúng số trang/chương của Manual để đối chiếu. Tránh trả lời các vấn đề kỹ thuật sâu cần thợ. |
| **Feasibility** | Dữ liệu PDF có sẵn. Sử dụng **RAG (Retrieval-Augmented Generation)** để giới hạn câu trả lời trong Manual VinFast. |
| **Type** | **Augmentation** — AI hướng dẫn, chủ xe thực hiện thao tác. |
| **Learning Signal** | Những tính năng nào khách hỏi nhiều nhất? -> Gửi feedback cho đội sản xuất để tối ưu UI/UX hoặc cập nhật phần mềm (OTA). |

---

## 3. 💡 Điểm chạm của Agent (Touchpoints)
Thay vì chỉ là một khung chat trên điện thoại, Agent này có thể xuất hiện ở:

* **Màn hình trung tâm (Infotainment):** Người dùng hỏi bằng giọng nói (**Voice-to-Text**). 
    * *Ví dụ:* "Hey VinFast, làm sao để bật chế độ giữ làn?" -> AI trả lời và hiển thị hình ảnh minh họa từ Manual lên màn hình.
* **Mobile App (VinFast App):** Người dùng chụp ảnh một nút bấm lạ hoặc một đèn cảnh báo trên taplo -> AI nhận diện hình ảnh (**Vision**) và trích xuất đoạn Manual tương ứng để giải thích.
* **Proactive Alert (Chủ động):** Khi xe nhận thấy pin xuống dưới 20%, AI chủ động nhắc: "Tôi thấy pin bạn đang thấp, Manual trang 45 khuyên bạn nên sạc ở chế độ sạc chậm để bảo vệ tuổi thọ pin, bạn có muốn tìm trạm sạc không?"

---

## 4. Hướng đi chính (4 Paths User Stories)

| Path | Kịch bản (Scenario) | Phản hồi của AI |
| :--- | :--- | :--- |
| **Happy Path** | Khách hỏi: "Cách mở cốp bằng đá chân?" | Giải thích 3 bước: Vị trí đá, tốc độ đá, và điều kiện (chìa khóa trong túi). |
| **Low-confidence** | Khách hỏi về một linh kiện độ thêm (không có trong manual). | "Tính năng này không có trong tài liệu chính hãng. Bạn nên liên hệ showroom để đảm bảo không mất bảo hành." |
| **Failure** | AI hiểu nhầm câu hỏi "Lốp" thành "Loa". | Cung cấp nút bấm nhanh: "Không đúng ý tôi" -> Chuyển sang danh mục các Icon đèn cảnh báo phổ biến. |
| **Correction** | Khách đính chính: "Ý tôi là áp suất lốp." | Xin lỗi và cung cấp ngay thông số áp suất tiêu chuẩn cho dòng xe đó (Trang 112). |

---

## 5. 🛠 Cách "Dump Data" và làm Prototype cực nhanh

### Dữ liệu (Data)
* Lên trang chủ VinFast, tải PDF Manual của dòng VF8/VF9.

### Công cụ
1.  **Nền tảng No-code (Chatbase/Dify.ai):** Chỉ cần kéo file PDF vào để tạo API Chatbot trong 30 giây.
2.  **Code-base:** Sử dụng **LangChain + OpenAI API** để xây dựng luồng RAG đơn giản.

### Demo Wow
* Chuẩn bị một vài tấm ảnh đèn taplo (Check engine, phanh tay...).
* Show cho giám khảo thấy: AI không nói nhảm, mọi câu trả lời đều có **"Source: Manual VF8 - Page 42"**.

---

## 6. Phân công nhiệm vụ
* **Phú:** Thiết kế **System Prompt** để AI luôn đóng vai chuyên gia kỹ thuật VinFast (lịch sự, chính xác).
* **Nhung:** Cắt ghép các đoạn PDF Manual quan trọng để làm **Knowledge base**.
* **Tuyền:** Tạo giao diện demo (có thể dùng **Streamlit** hoặc **Telegram Bot**).
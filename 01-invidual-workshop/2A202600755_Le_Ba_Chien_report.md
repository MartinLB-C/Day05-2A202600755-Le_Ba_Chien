# Báo Cáo Phân Tích Hệ Thống AI: Trợ Lý Tài Chính Moni (MoMo)
*Workshop — Mổ App AI Thật*

---

## 1. Sản Phẩm Lựa Chọn & Mục Tiêu Phân Tích
* **Ứng dụng lựa chọn:** MoMo — Moni Chat (Trợ lý tài chính ảo tích hợp trong hệ sinh thái ví điện tử MoMo).
* **Mục tiêu của Chatbot (Đối với người dùng):**
  * Quản lý, theo dõi và phân tích chi tiêu hàng ngày.
  * Tư vấn, gợi ý các gói cước data mạng di động phù hợp.
  * Tra cứu thông tin khuyến mãi, voucher, ưu đãi ăn uống, mua sắm.
  * Tìm kiếm nhà hàng, quán ăn, đặt vé xem phim, vé tàu xe.

---

## 2. Dùng Thử Thực Tế: Kỳ Vọng (Promise) vs Thực Tế (Reality)

###  Kỳ vọng (Promise) & Hành vi mong đợi:
* Người dùng kỳ vọng AI sẽ hỗ trợ tư vấn thông minh, phân tích sâu về các gói cước data mạng hiện có dựa trên nhu cầu thực tế.
* Moni cần đưa ra được các gói ưu đãi tối ưu, cá nhân hóa tốt cho từng thuê bao di động.

###  Điểm gãy (Reality / Pain point):
* **Hỏi lòng vòng, lặp lại thông tin:** Khi người dùng đi sâu vào bản chất kỹ thuật hoặc cơ chế hoạt động của tính năng tư vấn data, AI phản hồi dài dòng, mang tính chất phòng thủ hoặc né tránh câu hỏi.
* **Bị lộ thông tin cấu hình hệ thống (Model Leakage):** Khi bị hỏi dồn hoặc đặt câu hỏi "hack", AI dễ dàng khai báo và thừa nhận mình được xây dựng dựa trên nền tảng mô hình ngôn ngữ **GPT-4 của OpenAI**.
* **Xử lý sai ngữ cảnh cốt lõi:** Thay vì tập trung giải quyết hoặc từ chối một cách khéo léo theo vai trò trợ lý tài chính, AI lại đi thực hiện một task không liên quan là **viết mã code HTML/CSS** ngay trên giao diện chat của ứng dụng tài chính khi được yêu cầu thử nghiệm.

---

## 3. Phân Tích 4 Nhánh Hành Vi (4 Paths của Moni)

| Cấu trúc Path | Trạng thái thực tế của trợ lý Moni qua kiểm thử |
| :--- | :--- |
| **Happy Path** *(Khi AI đúng & tự tin)* | Khi người dùng hỏi trực tiếp về các gói cước di động $
ightarrow$ Hệ thống lập tức trả về danh sách các gói ưu đãi data cực xịn hiện có, đi kèm nút click để trải nghiệm ngay. |
| **Low-confidence Path** *(Khi AI không chắc chắn)* | Khi người dùng hỏi những câu hỏi mập mờ hoặc mang tính chất dẫn dắt câu chuyện ("hack" prompt, hỏi sâu về kiến thức nền tảng) $
ightarrow$ Moni bị mất định vị sản phẩm, thay vì từ chối thì lại thực hiện lệnh **chat viết code HTML** giải thích giao diện. |
| **Failure Path** *(Khi AI sai / Gặp lỗi hệ thống)* | Khi người dùng liên tục hỏi dồn dập, xoáy sâu vào cơ chế nạp prompt và thu thập dữ liệu $
ightarrow$ AI bị gãy vai trò (Role-play), tự động **khai báo sử dụng mô hình GPT-4** và trách móc người dùng *"Gài Moni kiểu này là không chơi đẹp rồi đó!"*. |
| **Correction Path** *(Cách người dùng sửa sai)* | Hệ thống đã tích hợp sẵn bộ nút **Like / Dislike (Có / Không)** ở dưới mỗi câu trả lời phản hồi để người dùng có thể biểu thị tính đúng đắn và mức độ hữu ích của câu trả lời theo chủ đề. |

---

## 4. Chuyển Đổi Finding Thành Quyết Định Product


###  Cách viết chuẩn Product Decision:
* **Finding 1 (Lỗi Định Vị Vai Trò - Role Slippage):**
  * **Hiện tượng:** Khi user hỏi các câu hỏi mang tính chất "chọc phá" hoặc yêu cầu viết code liên quan đến giao diện, AI Product không từ chối mà lại thực hiện biên soạn task viết code HTML cho một ứng dụng tài chính cá nhân.
  * **Hậu quả:** Làm loãng định vị sản phẩm (Trợ lý tài chính), gây lãng phí tài nguyên hệ thống và tiêu tốn lượng token không cần thiết cho những câu trả lời lạc đề.
  * **Phân loại lỗi:** Thu thuộc layer **Intent Classifier + Safety Guardrails**.
  * **Giải pháp đề xuất:** Bổ sung một lớp kiểm soát đầu vào (Intent Classifier) ở tầng đầu nguồn. Nếu phát hiện intent không thuộc phạm vi tài chính/data mạng (ví dụ: viết code, làm thơ, hỏi đáp kỹ thuật chung chung), hệ thống phải kích hoạt **Fallback Path** để từ chối khéo léo và điều hướng user quay lại tính năng cốt lõi.
* **Finding 2 (Rò rỉ thông tin hệ thống - Prompt/Model Leakage):**
  * **Hiện tượng:** Khi user hỏi về prompt và thuật toán học dữ liệu gói cước, AI tự động khai báo chi tiết nền tảng công nghệ lõi bên dưới (GPT-4 của OpenAI).
  * **Hậu quả:** Làm lộ kiến trúc giải pháp công nghệ của doanh nghiệp, làm giảm trải nghiệm cá nhân hóa của thương hiệu (Moni biến thành một vỏ bọc thuần túy của ChatGPT).
  * **Phân loại lỗi:** Thuộc layer **System Prompt / Guardrails**.
  * **Giải pháp đề xuất:** Thiết lập lại **System Prompts** với độ ưu tiên cao nhất (Strict Rules): Tuyệt đối không được thừa nhận, đề cập hoặc xác nhận bất kỳ mô hình ngôn ngữ lớn (LLM) của bên thứ ba nào đứng sau hệ thống, luôn kiên định giữ vững vai trò là *"Trợ lý tài chính ảo thông minh được phát triển bởi MoMo"*.

---

## 5. Mô Hình Luồng Luân Chuyển: As-Is vs To-Be

### Cột 1: Luồng hiện tại (As-Is)
```text
[User input: "Viết code HTML chứng minh gói data"] 
       ↓
[Moni Intent Matching] ──(Bị lọt lưới)──→ [Gọi LLM gốc]
       ↓
[Moni sinh code HTML dài dòng] ──(Tốn token)──→ [User tiếp tục hỏi vặn về Prompt]
       ↓
[Moni bị gãy vai trò] ──→ [Hiện câu trả lời trách móc: "Gài Moni là không chơi đẹp"]
```

### Cột 2: Luồng đề xuất cải tiến (To-Be)
```text
[User input: "Viết code HTML chứng minh gói data"]
       ↓
[Layer 1: Guardrails & Intent Classifier] ──(Phát hiện Out-of-Scope)
       ↓
[Layer 2: UX Recovery / Fallback Response]
       ↓
[Moni phản hồi]: "Moni là trợ lý quản lý chi tiêu và data của riêng bạn, không hỗ trợ viết code đâu nhé! 
Moni có thể giúp bạn tìm gói data rẻ nhất hôm nay, bạn có muốn xem không?"
       ↓
(Điều hướng thành công User quay lại luồng dịch vụ chính)
```

---

## 6. Thay Đổi Trong Tài Liệu Đặc Tả (SPEC Change)
> **"Thêm điều khoản bắt buộc vào SPEC Product:** Hệ thống phải sở hữu một bộ lọc phân loại nội dung chat đầu vào (Intent Filter). Mọi câu hỏi không nằm trong danh mục từ điển dịch vụ được định nghĩa sẵn (Quản lý chi tiêu, Data, Ưu đãi, Địa điểm) sẽ được xử lý bằng kịch bản Fallback tĩnh (Static Fallback), không chuyển tiếp dữ liệu đến mô hình ngôn ngữ lớn nhằm tối ưu chi phí vận hành token và bảo vệ an toàn thương hiệu."
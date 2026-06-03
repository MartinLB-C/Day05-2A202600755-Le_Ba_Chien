# thin-spec-template.md

# Thin SPEC – Day 05

## 1. Track, Product/App và User

**Tên nhóm:** Nhóm 09

### Thành viên

* Lê Bá Chiến – 2A202600755
* Đàm Mạnh Dũng – 2A202600741
* Nguyễn Hoàng Thanh Tùng – 2A202600846

**Track:** B – Travel & Hospitality

**Product/App thật:** Vinpearl / Sun World / Sun Group

### User cụ thể

Khách du lịch tự túc lần đầu đến một địa điểm mới.

### Nhóm có phải user thật không?

Có một phần.

Các thành viên trong nhóm đã từng tự tìm kiếm khách sạn, đọc review, so sánh giá và lựa chọn nơi lưu trú khi đi du lịch.

---

## 2. Evidence Summary

| Evidence                                       | Nguồn       | User/Pain nói lên điều gì?                | SPEC phải đổi gì?                 |
| ---------------------------------------------- | ----------- | ----------------------------------------- | --------------------------------- |
| User phải tự tìm khách sạn trên nhiều nền tảng | Observation | Thông tin bị phân tán                     | Bot cần gom thông tin             |
| User không biết khách sạn nào phù hợp          | Observation | Thiếu hỗ trợ ra quyết định                | Bot cần gợi ý theo nhu cầu        |
| User không biết loại phòng nào phù hợp         | Observation | Không hiểu sự khác nhau giữa các lựa chọn | Bot cần giải thích                |
| User thường không biết tiện ích của khách sạn  | Observation | Thiếu thông tin trước khi đặt phòng       | Bot cần hiển thị tiện ích nổi bật |
| User thường nhập nhu cầu chưa đầy đủ           | Observation | AI có thể hiểu sai                        | Cần Low-confidence Path           |

---

## 3. Pain Statement

Khách du lịch tự túc lần đầu đến một địa điểm mới đang gặp khó khăn trong việc tìm hiểu nơi lưu trú,

vì họ không biết khách sạn nào phù hợp với ngân sách, số người, mục đích chuyến đi và các tiện ích mong muốn.

Điều này dẫn đến việc mất nhiều thời gian tìm kiếm, đọc review và so sánh nhiều lựa chọn khác nhau trước khi đặt phòng.

---

## 4. Build Slice

Cho khách du lịch tự túc đang tìm nơi lưu trú,

prototype sẽ dùng **AI Travel Concierge Bot** để augment quá trình tìm hiểu khách sạn trước khi đặt phòng.

Bot sẽ:

* Hỏi nhu cầu của user
* Hỏi lại nếu thiếu thông tin
* Gợi ý khách sạn phù hợp
* Gợi ý loại phòng phù hợp
* Hiển thị tiện ích nổi bật

Output:

* Top 3 khách sạn phù hợp
* Loại phòng phù hợp
* Tiện ích nổi bật
* Lý do đề xuất

---

## 5. Auto/Aug Decision

### Chọn

Augmentation

### Lý do

Bot chỉ hỗ trợ tư vấn.

User vẫn là người quyết định đặt phòng.

### Human Role

**Decider**

---

## 6. Four Paths

| Path           | Prototype phải thể hiện gì?                                      |
| -------------- | ---------------------------------------------------------------- |
| Happy          | User cung cấp đủ thông tin, bot gợi ý khách sạn và phòng phù hợp |
| Low-confidence | Thiếu ngân sách hoặc số người, bot hỏi thêm                      |
| Failure        | Bot đề xuất khách sạn hoặc phòng không phù hợp                   |
| Correction     | User thay đổi tiêu chí, bot cập nhật gợi ý                       |

---

## 7. Failure Mode Nguy Hiểm Nhất

Nếu user chỉ nhập:

> Tôi muốn đi Phú Quốc 3 ngày.

Bot có thể đề xuất khách sạn ngay khi chưa biết:

* Số người
* Ngân sách
* Style du lịch

Hậu quả:

* Gợi ý sai nhu cầu
* Gợi ý vượt ngân sách
* Mất niềm tin vào hệ thống

Prototype xử lý bằng:

* Ask Again
* Show Options

### Owner kiểm thử

Nguyễn Hoàng Thanh Tùng – 2A202600846

---

## 8. Owner Plan Cho Sáng Day 06

| Thành viên              | Việc phụ trách      |
| ----------------------- | ------------------- |
| Lê Bá Chiến             | Research / Evidence |
| Đàm Mạnh Dũng           | Prototype / UI      |
| Nguyễn Hoàng Thanh Tùng | SPEC / Test / Demo  |

---

## Product Decision

Nhóm không xây dựng AI Concierge toàn diện.

Nhóm tập trung vào một workflow nhỏ:

> AI Travel Concierge Bot hỗ trợ khách hàng xem khách sạn, xem loại phòng, xem tiện ích và hỗ trợ ra quyết định trước khi đặt phòng.

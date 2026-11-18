# BÀI TẬP: ỨNG DỤNG CHAIN-OF-THOUGHT, PROMPT CHAINING & CONTEXT MANAGEMENT LẬP KẾ HOẠCH SỰ KIỆN "AI INNOVATION DAY"

## PHẦN 1: KIẾN TRÚC & BỘ PROMPT HOÀN CHỈNH (KẾT HỢP 3 KỸ THUẬT)

### 1.1. Sơ đồ Luồng Vận hành Kết hợp 3 Kỹ thuật

Hệ thống gồm **4 mắt xích Prompt liên tiếp (Prompt Chaining)** kết hợp suy luận từng bước (**Chain-of-Thought**) và đóng gói trạng thái chuyển phiên (**Context Management**) để kiểm định kế hoạch trong một hội thoại mới:

```
[PHIÊN HỘI THOẠI 1: XÂY DỰNG KẾ HOẠCH & ĐÓNG GÓI NGỮ CẢNH]
+-------------------------------------------------------------------------+
| PROMPT 1 (Chain-of-Thought): Phân tích Tính khả thi & Lịch trình 1 ngày |
| • Ràng buộc khóa cứng: 150 SV | 1 ngày | <= 15.000.000 VNĐ | 10 nhân sự  |
+------------------------------------+------------------------------------+
                                     | Xuất: [OUTPUT_1: Khả thi & Lịch trình]
                                     v
+-------------------------------------------------------------------------+
| PROMPT 2 (Chain-of-Thought): Lập Dự toán Ngân sách & Phân công 10 NS    |
| • Đầu vào: [Ràng buộc gốc] + [OUTPUT_1]                                 |
+------------------------------------+------------------------------------+
                                     | Xuất: [OUTPUT_2: Ngân sách & Nhân sự]
                                     v
+-------------------------------------------------------------------------+
| PROMPT 3 (Context Management): Kế hoạch Truyền thông & Tóm tắt Ngữ cảnh |
| • Đầu vào: [Ràng buộc gốc] + [OUTPUT_1] + [OUTPUT_2]                    |
| • Nhiệm vụ kép: Lập kế hoạch truyền thông + Nén toàn bộ dữ liệu thành   |
|   khối [PROJECT_CONTEXT_SUMMARY] để chuyển sang hội thoại mới           |
+------------------------------------+------------------------------------+
                                     |
        ======== CHUYỂN SANG HỘI THOẠI MỚI (NEW CHAT SESSION) ========
                                     | Truyền: [PROJECT_CONTEXT_SUMMARY]
                                     v
[PHIÊN HỘI THOẠI 2: KIỂM ĐỊNH ĐỘC LẬP & HIỆU CHỈNH KẾ HOẠCH]
+-------------------------------------------------------------------------+
| PROMPT 4 (Chain-of-Thought Audit): Kiểm tra Tính nhất quán & Hiệu chỉnh |
| • Đầu vào: [PROJECT_CONTEXT_SUMMARY]                                    |
| • Nhiệm vụ: Đối chiếu chéo ngân sách, nhân sự, lịch trình, truyền thông |
|   -> Phát hiện lỗ hổng/xung đột -> Đề xuất phương án hiệu chỉnh tối ưu  |
+-------------------------------------------------------------------------+
```

---

### 1.2. Bộ 4 Prompt Hoàn chỉnh

#### Prompt 1: Phân tích Tính khả thi & Xây dựng Lịch trình (Chain-of-Thought + Chaining Step 1)

```text
Vai trò: Chuyên gia Quản lý Dự án Sự kiện Công nghệ (Tech Event Director) tại môi trường đại học.

Thông số cố định của dự án (TUYỆT ĐỐI KHÔNG THAY ĐỔI):
- Tên sự kiện: AI Innovation Day
- Quy mô tham dự: 150 sinh viên
- Thời lượng: 1 ngày
- Ngân sách tối đa: 15.000.000 VNĐ
- Nhân sự ban tổ chức: 10 người
- Mục tiêu cốt lõi: Giúp sinh viên hiểu và trải nghiệm thực tế ứng dụng AI trong học tập và công việc.

Yêu cầu suy luận từng bước (Chain-of-Thought):
Hãy phân tích tuần tự theo 4 bước trước khi xuất lịch trình:
- Bước 1 (Phân tích bài toán nguồn lực & quy mô): Tính toán tỷ lệ phục vụ (số sinh viên/nhân sự) và định mức ngân sách bình quân đầu người để đánh giá tính khả thi vận hành.
- Bước 2 (Phân tích điều kiện hạ tầng & địa điểm): Xác định các điều kiện bắt buộc về địa điểm (hội trường trường miễn phí), thiết bị và kết nối mạng để không vượt trần 15.000.000 VNĐ.
- Bước 3 (Đánh giá tính khả thi mục tiêu trải nghiệm): Cân đối thời lượng 1 ngày giữa phần chia sẻ kiến thức (hiểu) và phần thực hành công cụ AI (trải nghiệm).
- Bước 4 (Thiết kế lịch trình chi tiết): Xây dựng khung giờ cụ thể từ sáng đến chiều đảm bảo nhịp độ hợp lý, tránh quá tải cho 10 nhân sự điều phối.

Định dạng đầu ra ([OUTPUT_1]):
1. Bản phân tích tính khả thi theo 4 bước suy luận.
2. Bảng lịch trình sự kiện "AI Innovation Day" chi tiết theo từng khung giờ.
```

---

#### Prompt 2: Lập Dự toán Ngân sách & Phân công 10 Nhân sự (Chain-of-Thought + Chaining Step 2)

```text
Vai trò: Trưởng ban Tài chính & Điều phối Nhân sự Sự kiện.

Dữ liệu đầu vào bắt buộc:
- Ràng buộc cứng: Quy mô 150 sinh viên | Ngân sách trần <= 15.000.000 VNĐ | Đúng 10 nhân sự (ký hiệu NS01 đến NS10).
- Kế hoạch từ Bước 1 ([OUTPUT_1]):
[Dán toàn bộ OUTPUT_1 vào đây]

Yêu cầu suy luận từng bước (Chain-of-Thought):
- Bước 1 (Bóc tách hạng mục chi phí từ lịch trình): Liệt kê toàn bộ hạng mục phát sinh từ lịch trình ở [OUTPUT_1] (địa điểm/kỹ thuật, ấn phẩm, tea-break, quà diễn giả, giải thưởng trải nghiệm AI, truyền thông, ăn trưa BTC).
- Bước 2 (Tính toán & Khóa trần ngân sách): Phân bổ đơn giá chi tiết, trích lập quỹ dự phòng rủi ro 10% (1.500.000 VNĐ) và kiểm tra phép cộng tổng đảm bảo bằng hoặc thấp hơn 15.000.000 VNĐ.
- Bước 3 (Phân bổ 10 nhân sự theo giai đoạn): Chia 10 thành viên (NS01 -> NS10) vào các mảng việc xuyên suốt Trước – Trong – Sau sự kiện, đảm bảo không có nhân sự nào bị trùng lịch hoặc quá tải tại cùng một khung giờ.

Định dạng đầu ra ([OUTPUT_2]):
1. Bảng dự toán ngân sách chi tiết (có cột Hạng mục, Đơn giá, Số lượng, Thành tiền, Ghi chú tối ưu).
2. Bảng phân công nhiệm vụ chi tiết cho đúng 10 nhân sự (NS01 -> NS10).
```

---

#### Prompt 3: Kế hoạch Truyền thông & Đóng gói Tóm tắt Ngữ cảnh (Chaining Step 3 + Context Management)

```text
Vai trò: Trưởng ban Truyền thông Sự kiện kiêm Quản trị Tri thức Dự án (Project Knowledge Manager).

Dữ liệu đầu vào bắt buộc:
- Ràng buộc cứng: 150 sinh viên | 1 ngày | <= 15.000.000 VNĐ | 10 nhân sự.
- Dữ liệu Bước 1 ([OUTPUT_1]): [Dán OUTPUT_1]
- Dữ liệu Bước 2 ([OUTPUT_2]): [Dán OUTPUT_2]

Nhiệm vụ 1 (Kế hoạch truyền thông trước sự kiện):
Xây dựng kế hoạch truyền thông 14 ngày trước sự kiện (chia 3 giai đoạn) trên các kênh sinh viên (Fanpage CLB, Group trường, Bàn trực sảnh) để thu hút đủ 150 sinh viên tham dự thực tế, bám sát ngân sách truyền thông đã duyệt ở [OUTPUT_2].

Nhiệm vụ 2 (Context Management - Tạo Bản tóm tắt ngữ cảnh chuyển phiên):
Tạo một khối dữ liệu chuẩn hóa có tiêu đề [PROJECT_CONTEXT_SUMMARY] nén toàn bộ thông số cốt lõi, quyết định ngân sách, khung giờ lịch trình, sơ đồ phân công 10 nhân sự và KPI truyền thông. Bản tóm tắt này phải đủ súc tích và đầy đủ để khi sao chép sang một cửa sổ hội thoại AI hoàn toàn mới, AI mới có thể hiểu ngay toàn bộ dự án để thực hiện bước kiểm tra và hiệu chỉnh cuối cùng.
```

---

#### Prompt 4: Kiểm tra Tính nhất quán & Hiệu chỉnh Kế hoạch (New Session + Context Management + CoT Audit)

```text
[MỞ HỘI THOẠI MỚI HOÀN TOÀN]

Vai trò: Chuyên gia Kiểm toán Dự án & Quản trị Rủi ro Sự kiện (Event QA Auditor).

Ngữ cảnh dự án được chuyển giao ([PROJECT_CONTEXT_SUMMARY]):
[Dán toàn bộ khối PROJECT_CONTEXT_SUMMARY từ Bước 3 vào đây]

Yêu cầu suy luận kiểm định từng bước (Chain-of-Thought Audit):
Dựa trên bản tóm tắt ngữ cảnh trên, hãy thực hiện kiểm tra chéo (Cross-check) toàn diện theo 4 bước:
- Bước 1 (Kiểm tra ràng buộc cứng): Đối chiếu tổng ngân sách với trần 15.000.000 VNĐ, kiểm tra số lượng nhân sự có đúng 10 người và quy mô có giữ nguyên 150 sinh viên không.
- Bước 2 (Kiểm tra tính liên kết giữa các hạng mục): Đối chiếu sự khớp nối giữa Kế hoạch truyền thông <-> Lịch trình <-> Ngân sách <-> Nhân sự.
- Bước 3 (Phát hiện điểm nghẽn & Rủi ro ẩn): Tìm ra ít nhất 3 mâu thuẫn tiềm ẩn hoặc rủi ro vận hành thực tế (ví dụ: tỷ lệ sinh viên đăng ký ảo so với 150 chỗ ngồi, ùn tắc giờ check-in với 10 nhân sự, quá tải Wi-Fi/ổ cắm khi 150 sinh viên cùng thực hành AI).
- Bước 4 (Đề xuất hiệu chỉnh cụ thể): Đưa ra giải pháp điều chỉnh trực tiếp vào lịch trình, nhân sự, truyền thông và quỹ dự phòng để khắc phục triệt để các vấn đề vừa phát hiện.

Định dạng đầu ra:
Xuất Báo cáo Kiểm tra và Hiệu chỉnh Kế hoạch chi tiết.
```

---

## PHẦN 2: KẾT QUẢ THỰC THI CHI TIẾT TOÀN BỘ DỰ ÁN

### 2.1. Bản phân tích tính khả thi (Suy luận Chain-of-Thought từ Prompt 1)

* **Bước 1 — Phân tích bài toán nguồn lực & mật độ phục vụ:**
  * Tỷ lệ phục vụ trung bình là $\frac{150\text{ sinh viên}}{10\text{ nhân sự}} = 15\text{ sinh viên/nhân sự}$. Đây là tỷ lệ khả thi trong môi trường sự kiện sinh viên nếu áp dụng quy trình tự động hóa (check-in bằng mã QR, tài liệu hướng dẫn dạng số thay vì phát giấy thủ công).
  * Định mức ngân sách bình quân là $\frac{15.000.000\text{ VNĐ}}{150\text{ sinh viên}} = 100.000\text{ VNĐ/sinh viên}$. Với mức chi này, sự kiện **hoàn toàn khả thi** nếu tận dụng hội trường của nhà trường miễn phí và mời diễn giả là cựu sinh viên/chuyên gia doanh nghiệp theo hình thức hợp tác cộng đồng (không tốn phí mời thương mại cao).
* **Bước 2 — Phân tích điều kiện hạ tầng & kỹ thuật:**
  * Để đáp ứng mục tiêu *"trải nghiệm ứng dụng AI"*, $150$ sinh viên cần thực hành trực tiếp trên laptop/điện thoại. Điều kiện tiên quyết là hội trường phải có hệ thống âm thanh, máy chiếu sẵn có và đủ nguồn điện/kết nối mạng.
* **Bước 3 — Phân tích tính khả thi về thời lượng ($1$ ngày) & mục tiêu học thuật:**
  * Chia $1$ ngày thành $2$ phiên rõ rệt: **Phiên Sáng (Hiểu)** tập trung vào tư duy nền tảng và xu hướng AI trong học tập – nghề nghiệp; **Phiên Chiều (Trải nghiệm)** tổ chức Workshop thực hành có hướng dẫn từng bước và thi đấu thử thách ngắn để đảm bảo sinh viên tạo ra sản phẩm thực tế ngay tại chỗ.
* **Kết luận tính khả thi:** Dự án **"AI Innovation Day" KHẢ THI ($8.5/10$ điểm)** với điều kiện: (1) Xin được hội trường trường hỗ trợ miễn phí thuê mặt bằng, (2) Sử dụng $100\%$ tài liệu số hóa, và (3) Trích lập sẵn $10\%$ quỹ dự phòng rủi ro trong tổng $15.000.000$ VNĐ.

---

### 2.2. Lịch trình sự kiện "AI Innovation Day" theo khung giờ (1 Ngày)

| Khung giờ | Thời lượng | Nội dung hoạt động | Mục tiêu & Hình thức triển khai |
| :--- | :--- | :--- | :--- |
| **$07:30 - 08:00$** | $30\text{ phút}$ | **Đón khách & Check-in QR Code** | Kiểm soát danh sách $150$ sinh viên, phát thẻ tham dự, hướng dẫn ổn định chỗ ngồi và kiểm tra kết nối Wi-Fi/tài khoản AI mẫu. |
| **$08:00 - 08:20$** | $20\text{ phút}$ | **Khai mạc & Giới thiệu Chương trình** | Đại diện Khoa/CLB phát biểu khai mạc, giới thiệu mục tiêu ngày hội và các diễn giả khách mời. |
| **$08:20 - 09:30$** | $70\text{ phút}$ | **Keynote 1: Ứng dụng AI Thông minh trong Học tập & Nghiên cứu** | Diễn giả chia sẻ phương pháp dùng AI tóm tắt tài liệu lớn, học ngoại ngữ, hỗ trợ lập trình và đạo đức sử dụng AI trong đại học. |
| **$09:30 - 09:50$** | $20\text{ phút}$ | **Tea-break & Giao lưu Networking** | Phục vụ bánh nhẹ và nước uống giữa giờ; sinh viên giao lưu trực tiếp với diễn giả và trải nghiệm góc trưng bày sản phẩm CLB. |
| **$09:50 - 11:15$** | $85\text{ phút}$ | **Talkshow: AI trong Công việc Thực tế & Kỹ năng Sinh viên Cần Chuẩn bị** | Tọa đàm giữa $2$ chuyên gia doanh nghiệp về cách AI thay đổi quy trình làm việc (Marketing, IT, Phân tích dữ liệu) + Hỏi đáp Q&A. |
| **$11:15 - 13:30$** | $135\text{ phút}$ | **Nghỉ trưa tự túc** | Sinh viên nghỉ trưa; BTC trực hội trường, ăn trưa luân phiên và kiểm tra kỹ thuật chuẩn bị cho phiên thực hành buổi chiều. |
| **$13:30 - 15:00$** | $90\text{ phút}$ | **Hands-on Workshop: Làm chủ Kỹ thuật Prompting & Tự động hóa Tác vụ** | Sinh viên mở laptop thực hành trực tiếp theo hướng dẫn của Mentor (xây dựng trợ lý học tập cá nhân và quy trình xử lý dữ liệu bằng AI). |
| **$15:00 - 16:00$** | $60\text{ phút}$ | **Mini-Challenge: "AI Innovation Arena"** | Chia đội/cá nhân giải quyết $1$ bài toán thực tế bằng công cụ AI trong $35\text{ phút}$ và thuyết trình nhanh $3$ giải pháp xuất sắc nhất. |
| **$16:00 - 16:30$** | $30\text{ phút}$ | **Trao giải, Tổng kết & Bế mạc** | Trao giải thưởng Mini-Challenge, chụp ảnh lưu niệm toàn thể $150$ sinh viên và gửi form khảo sát nhận chứng nhận điện tử. |

---

### 2.3. Bảng dự toán ngân sách (Khóa trần $\le 15.000.000$ VNĐ)

| STT | Hạng mục chi phí | Diễn giải chi tiết | Đơn giá (VNĐ) | Số lượng | Thành tiền (VNĐ) |
| :---: | :--- | :--- | :---: | :---: | :---: |
| **1** | **Địa điểm & Hỗ trợ kỹ thuật hội trường** | Hội trường trường hỗ trợ miễn phí; chi phí bồi dưỡng tổ kỹ thuật âm thanh, ánh sáng, vệ sinh của trường | $500.000$ | $1\text{ gói}$ | $500.000$ |
| **2** | **Ấn phẩm nhận diện & Trang trí** | In Backdrop sân khấu ($1$), Standee ($2$), thẻ BTC ($10$), bảng tên diễn giả ($3$), hashtag cầm tay | $2.000.000$ | $1\text{ gói}$ | $2.000.000$ |
| **3** | **Tea-break & Nước uống giữa giờ** | Bánh ngọt/trái cây + nước suối cho $150$ sinh viên và $10$ khách mời/BTC | $25.000$ | $160\text{ suất}$ | $4.000.000$ |
| **4** | **Quà tặng & Cảm ơn Diễn giả** | Kỷ niệm chương + Quà tặng cảm ơn cho $3$ diễn giả/mentor chuyên môn | $1.000.000$ | $3\text{ suất}$ | $3.000.000$ |
| **5** | **Giải thưởng Mini-Challenge & Tương tác** | $1$ Giải Nhất ($1.000.000$), $1$ Giải Nhì ($600.000$), $1$ Giải Ba ($400.000$) cho phần thi thực hành AI | $2.000.000$ | $1\text{ bộ giải}$ | $2.000.000$ |
| **6** | **Truyền thông & Bàn trực tuyển sinh** | Chạy quảng cáo bài đăng Fanpage nhắm mục tiêu sinh viên trường + in poster/standee bàn trực sảnh | $1.000.000$ | $1\text{ gói}$ | $1.000.000$ |
| **7** | **Vận hành & Ăn trưa cho BTC** | Hỗ trợ suất ăn trưa và nước uống cả ngày cho $10$ thành viên Ban Tổ chức | $100.000$ | $10\text{ người}$ | $1.000.000$ |
| **8** | **Quỹ dự phòng rủi ro ($10\%$)** | Dự phòng phát sinh thiết bị điện/mạng, in ấn bổ sung hoặc y tế khẩn cấp | $1.500.000$ | $1\text{ khoản}$ | $1.500.000$ |
| | **TỔNG CỘNG DỰ TOÁN** | **Đảm bảo không vượt trần ngân sách tối đa $15.000.000$ VNĐ** | | | **$15.000.000$** |

---

### 2.4. Bảng phân công nhiệm vụ cho $10$ nhân sự (`NS01` $\rightarrow$ `NS10`)

| Mã NS | Vai trò phụ trách | Nhiệm vụ TRƯỚC sự kiện | Nhiệm vụ TRONG sự kiện | Nhiệm vụ SAU sự kiện |
| :--- | :--- | :--- | :--- | :--- |
| **NS01** | **Trưởng Ban Tổ chức (Project Lead)** | Xin phê duyệt hội trường, lập kế hoạch tổng thể, giám sát tiến độ chung. | Điều hành tổng thể tại hiện trường, xử lý các tình huống phát sinh khẩn cấp. | Chủ trì họp tổng kết, báo cáo kết quả lên Khoa/Đoàn trường. |
| **NS02** | **Trưởng ban Nội dung & Chương trình** | Thiết kế kịch bản chi tiết, làm việc và duyệt slide với $3$ diễn giả. | Điều phối timeline sân khấu, nhắc giờ diễn giả, hỗ trợ MC điều hành Q&A. | Tổng hợp slide/tài liệu AI từ diễn giả để gửi cho $150$ sinh viên. |
| **NS03** | **Điều phối Diễn giả & MC** | Gửi thư mời, xác nhận thông tin đón tiếp diễn giả, chuẩn bị kịch bản dẫn. | Đón tiếp diễn giả tại cổng trường, làm MC dẫn dắt xuyên suốt chương trình. | Gửi thư cảm ơn chính thức và hình ảnh sự kiện tới các diễn giả. |
| **NS04** | **Trưởng ban Truyền thông** | Lập kế hoạch truyền thông $14$ ngày, quản lý form đăng ký, duyệt bài đăng. | Quản lý cập nhật tin tức trực tiếp (livestream/story) trên Fanpage CLB. | Đăng bài tổng kết truyền thông, gửi email khảo sát & chứng nhận. |
| **NS05** | **Thiết kế & Media (Photo/Video)** | Thiết kế bộ nhận diện (Backdrop, Standee, Banner truyền thông, Thẻ). | Quay phim, chụp ảnh tư liệu toàn bộ sự kiện và khoảnh khắc trải nghiệm. | Hậu kỳ ảnh, dựng video recap $60\text{s}$ đăng Fanpage trong vòng $48\text{h}$. |
| **NS06** | **Trưởng ban Hậu cần & Tài chính** | Quản lý thu chi theo bảng dự toán $15\text{ triệu}$, đặt in ấn phẩm, đặt tea-break. | Giám sát nhận hàng tea-break, quản lý quà tặng diễn giả và giải thưởng. | Quyết toán hóa đơn chứng từ minh bạch, hoàn ứng ngân sách. |
| **NS07** | **Nhân sự Hậu cần & Check-in 1** | Chuẩn bị mã QR check-in, danh sách $150$ sinh viên, sắp xếp bàn trực sảnh. | Trực bàn check-in đầu giờ, điều phối phân phát tea-break giờ giải lao. | Thu dọn ấn phẩm, bàn giao nguyên trạng hội trường cho nhà trường. |
| **NS08** | **Nhân sự Hậu cần & Check-in 2** | Đóng gói bộ quà tặng, chuẩn bị bảng tên, hỗ trợ trang trí hội trường. | Trực check-in đầu giờ, hỗ trợ chuyển micro Q&A và điều phối khách mời. | Kiểm kê vật tư còn lại, hỗ trợ dọn dẹp vệ sinh khu vực hội trường. |
| **NS09** | **Trưởng ban Kỹ thuật & Lab AI 1** | Kiểm tra hệ thống âm thanh, máy chiếu, Wi-Fi hội trường; test bộ công cụ AI. | Trực bàn điều khiển âm thanh/màn hình LED, xử lý sự cố kỹ thuật sân khấu. | Sao lưu dữ liệu trình chiếu, bàn giao thiết bị kỹ thuật hội trường. |
| **NS10** | **Kỹ thuật & Hỗ trợ Thực hành AI 2** | Soạn hướng dẫn cài đặt công cụ AI trước buổi học gửi sinh viên qua email. | Trực tiếp đi hỗ trợ kỹ thuật, giải đáp lỗi công cụ AI cho sinh viên phiên chiều. | Tổng hợp các bài nộp xuất sắc từ phần thi Mini-Challenge. |

---

### 2.5. Kế hoạch truyền thông trước sự kiện ($14$ ngày trước G-Day)

* **Mục tiêu truyền thông:** Tiếp cận rộng rãi sinh viên toàn trường, thu hút tối thiểu **$180$ đơn đăng ký hợp lệ** (tính toán trừ hao tỷ lệ vắng mặt khoảng $15\% - 17\%$ để đảm bảo đúng **$150$ sinh viên tham dự thực tế**).
* **Kênh triển khai:** Fanpage chính thức của CLB Công nghệ, các Group cộng đồng sinh viên của trường, Bàn trực truyền thông tại sảnh giảng đường, và Email/Zalo nhóm thí sinh.

| Giai đoạn | Thời gian | Hoạt động trọng tâm | Kênh & Hình thức | KPI Mục tiêu |
| :--- | :--- | :--- | :--- | :--- |
| **Giai đoạn 1: Khởi động & Nhận biết (Teasing)** | **Ngày D-14 đến D-10** | • Tung Poster chính thức & mở cổng đăng ký sớm.<br>• Bài viết đánh trúng nỗi đau: *"Làm sao để dùng AI học tập hiệu quả mà không bị lệ thuộc hay đạo văn?"*. | Fanpage CLB + Chia sẻ vào $5$ Group sinh viên các khoa. | Đạt **$60$ lượt đăng ký** đầu tiên; tiếp cận $3.000$ sinh viên. |
| **Giai đoạn 2: Tăng tốc & Bùng nổ (Engagement)** | **Ngày D-9 đến D-4** | • Công bố Profile $3$ diễn giả chuyên gia.<br>• Video ngắn $45\text{s}$ demo trước các công cụ AI sẽ thực hành tại Workshop chiều.<br>• Mở bàn trực giới thiệu trực tiếp tại sảnh khu giảng đường ($2$ ngày cao điểm). | Fanpage + TikTok CLB + Bàn trực Offline tại sảnh trường. | Thu thêm **$90$ lượt đăng ký** (tổng đạt $150$ đơn). |
| **Giai đoạn 3: Chốt danh sách & Nhắc lịch (Conversion & Reminder)** | **Ngày D-3 đến D-1** | • Bài đăng đếm ngược *"Chỉ còn 30 suất cuối cùng"*.<br>• Gửi Email/Tin nhắn xác nhận tham dự kèm hướng dẫn chuẩn bị laptop và tài khoản AI trước buổi thực hành.<br>• Đóng form đăng ký khi chạm mốc $180$ đơn xác nhận. | Fanpage + Email/Zalo cá nhân hóa gửi người đăng ký. | Đạt **$180$ đơn đăng ký**; tỷ lệ xác nhận tham gia $\ge 85\%$ (đảm bảo **$150$ người có mặt**). |

---

### 2.6. Bản tóm tắt ngữ cảnh dự án (`[PROJECT_CONTEXT_SUMMARY]`) — Dùng cho Context Management

*(Khối dữ liệu này được thiết kế để sao chép và dán sang một cửa sổ hội thoại AI hoàn toàn mới nhằm tiếp tục bước kiểm định mà không mất ngữ cảnh)*:

```text
=== [PROJECT_CONTEXT_SUMMARY: AI INNOVATION DAY] ===
1. THÔNG SỐ CỐ ĐỊNH (LOCKED CONSTRAINTS):
   - Sự kiện: AI Innovation Day (1 ngày: 07:30 - 16:30).
   - Quy mô: 150 sinh viên | Nhân sự BTC: Đúng 10 người (NS01 -> NS10).
   - Ngân sách trần: <= 15.000.000 VNĐ (Đã lập dự toán khớp 15.000.000 VNĐ).
   - Mục tiêu: Hiểu (Sáng: Keynote + Talkshow) & Trải nghiệm thực tế (Chiều: Workshop Prompting + Mini-Challenge).

2. CẤU TRÚC LỊCH TRÌNH ĐÃ CHỐT:
   - 07:30-08:00: Check-in QR | 08:00-11:15: Khai mạc, Keynote 1, Tea-break (09:30), Talkshow.
   - 11:15-13:30: Nghỉ trưa | 13:30-16:30: Hands-on Workshop, Mini-Challenge, Trao giải & Bế mạc.

3. PHÂN BỔ NGÂN SÁCH (15.000.000 VNĐ):
   - Hỗ trợ hội trường: 500k | Ấn phẩm trang trí: 2.000k | Tea-break (160 suất): 4.000k | Quà 3 diễn giả: 3.000k | Giải thưởng Mini-Challenge: 2.000k | Truyền thông: 1.000k | Ăn trưa 10 BTC: 1.000k | Quỹ dự phòng (10%): 1.500k.

4. PHÂN CÔNG NHÂN SỰ & TRUYỀN THÔNG:
   - NS01 (Trưởng BTC), NS02 (Nội dung), NS03 (Diễn giả/MC), NS04 (Truyền thông), NS05 (Media), NS06 (Hậu cần/Tài chính), NS07 & NS08 (Hậu cần/Check-in), NS09 & NS10 (Kỹ thuật/Hỗ trợ Lab AI).
   - Truyền thông 14 ngày: Đặt mục tiêu thu 180 form đăng ký để trừ hao vắng mặt, đảm bảo 150 sinh viên dự thực tế.

5. TRẠNG THÁI HIỆN TẠI & YÊU CẦU TIẾP THEO:
   - Đã hoàn thành bản nháp toàn bộ các hạng mục. Chuyển sang phiên mới để thực hiện kiểm toán tính nhất quán, phát hiện rủi ro vận hành và hiệu chỉnh kế hoạch lần cuối.
===================================================
```

---

### 2.7. Báo cáo kiểm tra và hiệu chỉnh kế hoạch (Kết quả thực thi từ Prompt 4 trong Hội thoại mới)

#### Bước 1: Kiểm tra đối chiếu các ràng buộc cứng

* **Ngân sách:** Tổng chi phí dự toán là $13.500.000\text{ VNĐ} + 1.500.000\text{ VNĐ (dự phòng)} = 15.000.000\text{ VNĐ}$ $\rightarrow$ **Đạt chuẩn (Không vượt trần $15$ triệu đồng)**.
* **Nhân sự:** Phân bổ đủ $10$ mã nhân sự (`NS01` đến `NS10`), không thừa, không thiếu $\rightarrow$ **Đạt chuẩn**.
* **Quy mô & Thời gian:** Thiết kế khớp cho $150$ sinh viên trong khung thời gian $1$ ngày ($07:30 - 16:30$) $\rightarrow$ **Đạt chuẩn**.

#### Bước 2 & 3: Phát hiện $3$ điểm nghẽn & xung đột tiềm ẩn trong kế hoạch

1. **Điểm nghẽn 1 — Xung đột nhân sự tại khung giờ cao điểm Check-in ($07:30 - 08:00$):**
   * *Vấn đề phát hiện:* Trong bảng phân công ban đầu, chỉ có `NS07` và `NS08` trực bàn check-in. Việc tiếp đón và quét mã cho $150$ sinh viên trong vòng $30\text{ phút}$ (trung bình $5\text{ sinh viên/phút}$) chỉ với $2$ người sẽ gây ùn tắc nghiêm trọng tại cửa hội trường, làm trễ giờ khai mạc $08:00$.
2. **Điểm nghẽn 2 — Rủi ro quá tải hạ tầng Wi-Fi và ổ cắm điện trong phiên Workshop chiều ($13:30 - 16:00$):**
   * *Vấn đề phát hiện:* Mục tiêu sự kiện yêu cầu $150$ sinh viên trải nghiệm thực tế công cụ AI trên laptop cá nhân suốt $2.5\text{ giờ}$ buổi chiều. Hệ thống Wi-Fi hội trường thông thường dễ bị sập băng thông khi $150$ thiết bị truy cập đồng thời và hội trường thường thiếu ổ cắm điện cho toàn bộ hàng ghế.
3. **Điểm nghẽn 3 — Thiếu nhân sự hỗ trợ chuyên môn khi $150$ sinh viên cùng thực hành AI:**
   * *Vấn đề phát hiện:* Ở khung giờ Workshop ($13:30 - 15:00$), chỉ có `NS10` đi hỗ trợ kỹ thuật cho $150$ sinh viên (tỷ lệ $1:150$), dẫn đến việc sinh viên gặp lỗi cài đặt/prompt sẽ phải chờ đợi quá lâu, làm giảm chất lượng trải nghiệm.

#### Bước 4: Phương án hiệu chỉnh kế hoạch chi tiết

| Hạng mục cần hiệu chỉnh | Nội dung điều chỉnh cụ thể | Tác động đến ràng buộc |
| :--- | :--- | :--- |
| **1. Hiệu chỉnh phân công nhân sự khung giờ $07:30 - 08:00$** | Điều động thêm **`NS04` (Truyền thông)** và **`NS10` (Kỹ thuật)** tham gia hỗ trợ bàn check-in trong $30\text{ phút}$ đầu giờ (nâng tổng nhân sự check-in lên **$4$ người**, chia $2$ luồng quét QR theo tên A–M và N–Z). Sau $08:00$, `NS04` và `NS10` quay về vị trí chuyên môn. | Giữ nguyên tổng số **$10$ nhân sự**, xóa bỏ hoàn toàn nguy cơ ùn tắc đầu giờ. |
| **2. Hiệu chỉnh phân công nhân sự khung giờ Workshop ($13:30 - 16:00$)** | Huy động thêm **`NS02` (Nội dung)**, **`NS07`** và **`NS08`** (đã hoàn tất việc hậu cần buổi sáng) đóng vai trò **Trợ giảng (Teaching Assistants)** theo từng khu vực ghế ngồi (mỗi người phụ trách $1$ dãy $35 - 40$ sinh viên) sau khi được `NS10` tập huấn trước $1$ ngày. | Giữ nguyên **$10$ nhân sự**, giảm tỷ lệ hỗ trợ từ $1:150$ xuống còn **$1:37$**, đảm bảo chất lượng trải nghiệm AI. |
| **3. Hiệu chỉnh ngân sách dự phòng & truyền thông nhắc nhở** | Trích **$600.000$ VNĐ** từ **Quỹ dự phòng ($1.500.000$ VNĐ)** để thuê $10$ ổ cắm điện nối dài công suất lớn và $2$ bộ phát Wi-Fi 4G dự phòng cho diễn giả/BTC (Quỹ dự phòng còn lại $900.000$ VNĐ). Đồng thời bổ sung lưu ý bắt buộc *"Sạc đầy $100\%$ pin laptop trước khi đến"* vào Email nhắc lịch ngày D-1. | Tổng ngân sách toàn sự kiện vẫn khóa cứng ở mức **$15.000.000$ VNĐ**, triệt tiêu rủi ro sập mạng/hết pin buổi chiều. |
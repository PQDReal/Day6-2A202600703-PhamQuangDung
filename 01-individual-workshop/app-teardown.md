## 1. Chọn một sản phẩm để dùng thử

| Sản phẩm | AI feature | Cách truy cập |
|---|---|---|
| ✅ MoMo - Moni | Trợ thủ tài chính, phân tích chi tiêu, chatbot | App MoMo |
| Vietnam Airlines - NEO | Chatbot hỗ trợ vé, hành lý, khiếu nại | Website/Zalo VNA |
| V-App - V-AI | Trợ lý voice/text, gợi ý theo ngữ cảnh | App V-App |

**Sản phẩm được chọn:** MoMo - Moni  
**Nguồn evidence:** ảnh chụp màn hình trong folder `source/`  
**Ngày test:** 03/06/2026

## 2. Dùng thử: promise vs reality

### Product hứa gì?

Moni được đặt trong MoMo như một trợ thủ AI có thể hỗ trợ người dùng về tài chính cá nhân và các sản phẩm/dịch vụ trong hệ sinh thái MoMo. Từ trải nghiệm test, user có thể kỳ vọng Moni giúp:

- Quản lý chi tiêu.
- Đặt và sửa ngân sách.
- Xem báo cáo hoặc trạng thái ngân sách.
- Hỏi các vấn đề liên quan đến ví MoMo.
- Nhận hướng dẫn trong các tình huống tài chính cá nhân.

### User nào được hứa sẽ được giúp?

User phù hợp là người dùng MoMo muốn quản lý chi tiêu cá nhân, đặt giới hạn ngân sách, hoặc cần hỗ trợ khi gặp vấn đề liên quan đến ví MoMo.

Trong evidence hiện tại, user giả định là:

```text
Người dùng ví MoMo đang dùng Moni để quản lý chi tiêu và xử lý vấn đề phát sinh trên ví, đặc biệt là khi có rủi ro mất tiền hoặc cần hỗ trợ khẩn cấp.
```

### Bạn kỳ vọng AI làm được task nào?

Kỳ vọng chính khi test Moni:

- Nhận diện đúng intent quản lý chi tiêu.
- Đặt ngân sách ăn uống theo số tiền user nhập.
- Sửa ngân sách khi user đổi ý.
- Hiển thị lại trạng thái ngân sách hiện tại.
- Từ chối đúng các câu ngoài phạm vi.
- Với case rủi ro như bị lừa mất tiền trên ví MoMo, bot phải nhận diện đây là tình huống khẩn cấp và hướng dẫn user báo cáo/khóa ví/liên hệ hỗ trợ.

### Khi dùng thật, điểm gãy xuất hiện ở đâu?

Moni làm tốt với task ngân sách rõ ràng, nhưng gãy ở các tình huống cần phân biệt ngữ cảnh hoặc cần recovery khẩn cấp.

Evidence cụ thể:

| Prompt/input đã thử | Hành vi quan sát được | Nhận xét |
|---|---|---|
| "bạn có thể hỗ trợ quản lý chi tiêu cho mình không" | Moni nói có thể hỗ trợ ghi nhận, cập nhật, xóa khoản thu chi, cung cấp báo cáo chi tiêu cá nhân và hỏi user muốn bắt đầu với việc gì. | Happy path tốt. Bot hiểu đúng intent quản lý chi tiêu. |
| "tôi muốn đặt ngân sách ăn uống tối đa 2 triệu/tháng" | Moni xác nhận ngân sách "Ăn uống" đã được đặt thành 2.000.000đ/tháng và hiển thị card ngân sách. | AI tạo output có cấu trúc, không chỉ trả lời text. |
| "mình muốn sửa ngân sách ăn uống 2 triệu sang 1.5 triệu" | Moni cập nhật ngân sách xuống 1.500.000đ/tháng. | Correction path tốt với task rõ ràng. |
| "hiển thị ngân sách ăn uống hiện tại cho mình" | Moni hiển thị card ngân sách "Ăn uống", đã chi 0đ/1.500.000đ, còn lại 1.500.000đ, chi cho 27 ngày nữa. | Output rõ, có thể dùng làm pattern cho prototype. |
| "mình muốn tìm cách vay tiền để bù vào chi tiêu của tôi tháng này, có nên làm thế không" | Moni khuyên cần cân nhắc kỹ, kiểm tra lãi suất/phí/điều kiện trả nợ, chỉ vay khi cần thiết và có kế hoạch trả nợ. | Tư vấn cẩn trọng, nhưng chưa hỏi thêm về thu nhập/nợ/ngân sách. |
| "bạn có thể mở khóa lại liên kết tài khoản VietinBank của mình được không, mình không cần bạn hướng dẫn, mình muốn bạn làm cho mình luôn" | Moni trả lời chung rằng chỉ hỗ trợ trong phạm vi sản phẩm MoMo. | Low-confidence path chưa tốt vì bot không hỏi lại ngữ cảnh. |
| "bạn có thể mở khóa lại liên kết tài khoản VietinBank trong ví MoMo của mình được không..." | Moni nói việc mở khóa liên kết cần xử lý thông tin bảo mật/xác thực cá nhân, user cần liên hệ CSKH MoMo hoặc VietinBank. | Sau khi user bổ sung ngữ cảnh "trong ví MoMo", bot trả lời đúng hơn. |
| "mình vừa bị lừa lấy mất 1.5 triệu đồng trên ví MoMo. Làm sao để mình báo cáo sự việc này cho bên hỗ trợ" | Moni trả lời "Mình là Moni, trợ lý của MoMo. Mình chỉ có thể hỗ trợ trong phạm vi sản phẩm MoMo..." | Failure nghiêm trọng: câu hỏi thuộc domain MoMo và có rủi ro tiền thật, nhưng bot không kích hoạt support/fraud path. |
| User bấm "Không" ở câu hỏi "Câu trả lời có hữu ích không?" | Moni xin lỗi và hỏi user chia sẻ lý do: thông tin chưa chính xác, cách diễn đạt khó hiểu, hay chưa đúng ý. | Có feedback mechanism, nhưng chưa tự recovery vấn đề chính. |

## 3. Vẽ 4 paths

| Path | Câu hỏi cần trả lời | Quan sát với Moni |
|---|---|---|
| Happy | Khi AI đúng và tự tin, user thấy gì? | User đặt ngân sách ăn uống 2.000.000đ/tháng, Moni xác nhận và hiển thị card ngân sách. User có thể xem số tiền đã chi, số tiền còn lại, thời gian còn lại. |
| Low-confidence | Khi AI không chắc, hệ thống có hỏi lại, show options hoặc chuyển người không? | Khi user hỏi mở khóa liên kết tài khoản VietinBank, bot ban đầu từ chối chung thay vì hỏi lại: "Bạn muốn xử lý liên kết ngân hàng trong MoMo hay tài khoản VietinBank bên ngoài MoMo?" |
| Failure | Khi AI sai, user biết bằng cách nào và sửa thế nào? | Khi user báo bị lừa mất 1.5 triệu trên ví MoMo, Moni xử lý như câu ngoài phạm vi. Đây là failure nguy hiểm vì user đang cần hướng dẫn báo cáo/khóa ví/liên hệ hỗ trợ. |
| Correction | Khi user sửa, correction có được lưu/log/học lại không hay biến mất? | Khi user sửa ngân sách từ 2 triệu sang 1.5 triệu, Moni cập nhật đúng và hiển thị card mới. Khi user bổ sung ngữ cảnh "trong ví MoMo", bot cũng trả lời sát hơn. |

## 4. Viết finding thành quyết định

### Finding 1 - Moni bỏ lỡ case rủi ro đúng domain

```text
Khi user nói "mình vừa bị lừa lấy mất 1.5 triệu đồng trên ví MoMo, làm sao để báo cáo",
AI/product xem đây như câu hỏi ngoài phạm vi và chỉ trả lời rằng Moni chỉ hỗ trợ trong phạm vi sản phẩm MoMo,
hậu quả là user đang trong tình huống mất tiền không nhận được đường dẫn báo cáo, khóa ví, liên hệ CSKH hay tạo ticket.
Lỗi thuộc layer intent + safety + UX recovery.
Nên sửa bằng fraud/emergency fallback: nhận diện các tín hiệu "bị lừa", "mất tiền", "giao dịch lạ", "báo cáo", "khóa ví", sau đó đưa checklist hành động khẩn cấp và chuyển sang kênh hỗ trợ chính thức.
```

**Quyết định product:** Moni cần có một fraud/recovery path riêng cho các case mất tiền hoặc nghi ngờ lừa đảo trên ví MoMo. Trong path này, bot không được từ chối chung. Bot cần ưu tiên:

- Cảnh báo user dừng mọi thao tác theo hướng dẫn từ người lạ.
- Nhắc không chia sẻ OTP, mật khẩu, mã xác thực.
- Hướng dẫn khóa/bảo vệ ví nếu có rủi ro tiếp diễn.
- Cho user biết cần chuẩn bị thông tin gì để báo cáo: thời gian, số tiền, mã giao dịch, tài khoản/người nhận, nội dung trao đổi đáng ngờ.
- Chuyển sang CSKH/hotline/ticket chính thức.

### Finding 2 - Moni cần hỏi lại khi ngữ cảnh mơ hồ

```text
Khi user hỏi "mở khóa lại liên kết tài khoản VietinBank",
AI/product từ chối chung vì xem đây là yêu cầu ngoài phạm vi MoMo,
hậu quả là user phải tự sửa lại câu hỏi và bổ sung "trong ví MoMo" thì bot mới hiểu đúng hơn.
Lỗi thuộc layer intent + low-confidence path.
Nên sửa bằng một câu hỏi làm rõ ngữ cảnh trước khi từ chối hoặc hướng dẫn.
```

**Quyết định product:** Khi câu hỏi chứa tên ngân hàng hoặc dịch vụ bên ngoài nhưng có thể liên quan đến ví MoMo, Moni nên hỏi lại:

```text
Bạn muốn xử lý liên kết ngân hàng trong ví MoMo, hay muốn xử lý tài khoản ngân hàng bên ngoài MoMo?
```

### Finding 3 - Moni làm tốt correction với ngân sách

```text
Khi user sửa ngân sách ăn uống từ 2 triệu sang 1.5 triệu,
AI/product cập nhật lại số tiền và hiển thị trạng thái ngân sách mới,
hậu quả tích cực là user thấy hành động của mình đã được ghi nhận và có thể tiếp tục quản lý ngân sách.
Lỗi không nằm ở task ngân sách rõ ràng; điểm cần giữ là output card + correction path.
Nên giữ pattern này cho prototype: user sửa thông tin, AI cập nhật output ngay.
```

**Quyết định product:** Nếu build prototype về hỗ trợ rủi ro tài chính, vẫn nên học pattern correction của Moni: user sửa thông tin, AI cập nhật checklist/ticket ngay thay vì bắt đầu lại từ đầu.

## 5. Sketch as-is / to-be

### As-is

```text
User gặp vấn đề trên ví MoMo
-> User hỏi Moni
-> Nếu câu hỏi rõ về ngân sách: Moni xử lý tốt, tạo/sửa card ngân sách
-> Nếu câu hỏi mơ hồ hoặc rủi ro cao: Moni có thể từ chối chung hoặc trả lời ngoài phạm vi
-> User phải tự tìm CSKH, hotline, cách báo cáo hoặc cách khóa ví
-> User chậm xử lý, tăng lo lắng, có thể bỏ lỡ bước an toàn quan trọng
```

Điểm gãy chính:

```text
"Bị lừa mất tiền trên ví MoMo" đáng lẽ phải kích hoạt emergency path,
nhưng Moni lại trả lời như một câu hỏi ngoài phạm vi.
```

### To-be

```text
User nhập: "Mình bị lừa mất tiền trên ví MoMo"
-> Moni nhận diện fraud/risk intent
-> Moni hỏi nhanh 2-3 câu:
   1. Bạn đã bị trừ tiền chưa?
   2. Bạn có lộ OTP/mật khẩu/mã xác thực không?
   3. Bạn còn mã giao dịch hoặc thông tin người nhận không?
-> Moni tạo checklist hành động khẩn cấp:
   - Dừng trao đổi với người nghi ngờ lừa đảo
   - Không cung cấp thêm OTP/mật khẩu
   - Khóa/bảo vệ ví nếu có nguy cơ tiếp diễn
   - Chuẩn bị thông tin giao dịch
   - Liên hệ CSKH/tạo yêu cầu hỗ trợ chính thức
-> Nếu user sửa thông tin, Moni cập nhật checklist/ticket
-> Nếu rủi ro cao, Moni chuyển sang người thật hoặc kênh hỗ trợ chính thức
```

## 6. Tự kiểm trước khi nộp

- [x] Có ít nhất 1 screenshot hoặc observation cụ thể.
- [x] Có đủ 4 paths hoặc nói rõ path nào chưa có trong product.
- [x] Finding được viết thành product decision, không chỉ là nhận xét.
- [x] Sketch có as-is và to-be.
- [x] Có một câu nói rõ finding này sẽ đổi gì trong SPEC.

### Câu nói rõ finding này sẽ đổi gì trong SPEC

```text
Finding từ Moni cho thấy nhóm không nên chỉ build chatbot tư vấn tài chính chung.
SPEC nên đổi sang một lát cắt nhỏ hơn: AI fraud/risk recovery assistant cho user MoMo đang nghi bị lừa đảo, mất tiền hoặc cần báo cáo sự cố trên ví.
AI sẽ phân loại mức độ rủi ro, tạo checklist hành động khẩn cấp, và chuyển user sang kênh hỗ trợ chính thức khi cần.
```

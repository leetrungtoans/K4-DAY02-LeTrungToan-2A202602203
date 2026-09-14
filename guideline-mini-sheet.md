# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Lê Trung Toán<br>
**MSSV:** 2A202602203<br>
**Hình thức:** theo cặp<br>
**Mã cặp:** Trấn áp thể hệ trẻ

## 1. Phạm vi

- Chỉ gán phương tiện thuộc bốn lớp bên dưới.
- Mỗi phương tiện là một hộp; không gộp nhiều xe.
- Không gán người, xe máy, xe đạp, biển báo hoặc phần phản chiếu.
- Vật thể quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ: không đoán; ghi lý do vào nhật ký quyết định.

## 2. Bốn lớp cố định

| Mã | Lớp | Gán khi nhìn thấy | Không gán vào lớp này |
| ---: | --- | --- | --- |
| 0 | `car` (ô tô con) | sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con | xe có thùng/ben rõ; thân xe buýt; xe van thân hộp |
| 1 | `truck` (xe tải) | thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng | ô tô con; thân xe buýt; xe van kín một khối |
| 2 | `bus` (xe buýt) | thân xe khách dài, nhiều cửa sổ hoặc hàng ghế | xe van nhỏ; xe tải; ô tô con |
| 3 | `van` (xe van) | thân hộp nhỏ, kín, dùng chở người hoặc hàng | thân xe buýt; khoang hàng tách biệt như xe tải |

Thứ tự lớp là cố định: `0 car, 1 truck, 2 bus, 3 van`.

## 3. Hộp giới hạn

- Vẽ sát phần vật thể nhìn thấy.
- Không ước lượng phần bị xe khác che.
- Vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp.
- Không để hộp chứa nhiều nền hoặc nhiều phương tiện.

## 4. Ba thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
| --- | --- | --- |
| `visibility` (mức nhìn thấy) | `clear` (rõ), `occluded` (bị che), `unclear` (không rõ) | mức bằng chứng nhìn thấy |
| `boundary` (quan hệ mép ảnh) | `inside` (trong ảnh), `truncated` (bị cắt) | vật thể có bị mép ảnh cắt hay không |
| `review_state` (trạng thái xem lại) | `confident` (tự tin), `needs_review` (cần xem lại) | đánh dấu quyết định cần quay lại |

YOLO không lưu ba thuộc tính này. Vì vậy phải xuất thêm `CVAT for images 1.1` từ cùng công việc.

## 5. Ba tình huống mơ hồ

Hoàn thành trước khi xem bài của người khác hoặc bộ nhãn tham chiếu.

### Tình huống A — xe buýt hay xe van?

- Ảnh và mã vật thể: [ghi ảnh + mã vật thể cụ thể hoặc mô tả vị trí]
- Dấu hiệu nhìn thấy: [mô tả mái, chiều dài, cửa sổ, thân xe, độ rộng]
- Quy tắc áp dụng: [ưu tiên thân dài và nhiều cửa sổ cho `bus`; thân hộp kín cho `van`]
- Quyết định: [chọn `bus` hoặc `van`]
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? [đánh dấu `needs_review` và không đoán]

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: [ghi ảnh + mã vật thể cụ thể hoặc mô tả vị trí]
- Dấu hiệu nhìn thấy: [mô tả khoang hàng, thùng, phần cabin, bề mặt mái, kích thước]
- Quy tắc áp dụng: [xe có thùng/ben rõ → `truck`; thân hộp kín, không có thùng và không phải ô tô con → `van`]
- Quyết định: [chọn `truck` / `van` / `car`]
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? [đánh dấu `needs_review`, kiểm tra lại ở zoom 100% và không đoán]

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: [ghi ảnh + mã vật thể cụ thể hoặc mô tả vị trí]
- Dấu hiệu nhìn thấy khi phóng 100%: [mô tả phần nào còn nhìn thấy và phần nào không]
- Giá trị `visibility`: [clear / occluded / unclear]
- Giá trị `boundary`: [inside / truncated]
- Trạng thái `review_state`: [confident / needs_review]
- Lý do: [nêu rõ vì sao có đủ hoặc không đủ bằng chứng phân lớp]

## 6. Xác nhận tự kiểm tra

- [ ] Đã rà đủ bốn ảnh.
- [ ] Đã kiểm vật thể thiếu và trùng.
- [ ] Đã kiểm lớp và hình học từng hộp.
- [ ] Mỗi hộp có đủ ba thuộc tính.
- [ ] Đã xử lý mọi hộp `needs_review`.
- [ ] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
- [ ] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
- [ ] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu.
- [ ] Số vật thể thực tế: [NHẬP SỐ LƯỢNG] — 40–60 là mục tiêu khối lượng, không phải điểm cắt.

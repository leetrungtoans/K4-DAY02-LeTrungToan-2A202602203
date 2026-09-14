# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Lê Trung Toán<br>
**MSSV:** 2A202602203<br>
**Hình thức:** theo cặp<br>
**Mã cặp:** Trấn áp thể hệ trẻ

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: F7D99888F21440FB0374D84962B93213BD8C14E665D093CC8D37F4C61B71ED33
- Bốn mã ảnh: drive_008, drive_022, drive_033, drive_038
- Số vật thể thực tế: 103
- Mã SHA-256 của gói YOLO của bạn: 9CF01BCB45305DF20100CD832E4442546BD6AB90B55048C8E400ABC69D3F506E
- Mã SHA-256 của gói CVAT gốc của bạn: 7AF7507D5AABEB342671FAD9BEEABB70C524937A48431AECAFA7F522351EC1D9
- Nguồn đối chiếu: bạn cùng cặp
- Mã SHA-256 của gói đối chiếu: FE8C0F8339F760FDBA537E4D764AF610D1C8216D1CF95BEACFF68C671F7D0188
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu:

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu: 

Tôi thực hiện gán nhãn độc lập dựa trên guideline, không xem kết quả của người khác trước khi hoàn thành. Sau đó mới đối chiếu để phát hiện sai khác và kiểm tra chất lượng.CHƯA ĐIỀN

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| ô tô con | car | sedan, hatchback, SUV, taxi và xe bán tải dùng như xe con | Gán car theo nhóm ô tô con |
| xe tải | truck | có thùng, ben, sàn chở hàng hoặc thiết bị công vụ rõ ràng | Gán truck khi các đặc điểm này rõ ràng |
| xe buýt | bus | thân xe khách dài, nhiều cửa sổ hoặc nhiều hàng ghế | Gán bus khi có đặc điểm của xe buýt |
| xe van | van | thân hộp nhỏ, kín, không có thân xe buýt hay khoang hàng tách biệt như xe tải | Gán van theo đặc điểm thân xe |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:

CHƯA ĐIỀN

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| Vật thể ở xa bị bỏ sót khi gán nhãn | Phạm vi/vật thể thiếu | Rà lại toàn bộ ảnh và phát hiện phương tiện ở xa chưa có bounding box | Bổ sung một bounding box cho phương tiện đó, mỗi phương tiện là một hộp riêng |


- Số hộp `needs_review` trước và sau khi kiểm: 10 -> 7
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: Một vật thể ở xa có kích thước nhỏ nên khó xác định chính xác lớp. Tôi phóng ảnh lên 100% để quan sát. Nếu vẫn chưa đủ bằng chứng, tôi đặt review_state = needs_review, ghi rõ lý do và xin Lab Coach hỗ trợ thay vì tự đoán.

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`: `2 0.384063 0.723445 0.435937 0.348422`
- Tên lớp và tọa độ điểm ảnh `xyxy`: `bus` `[106.3, 351.5, 385.3, 574.5]`
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?

Vì đúng định dạng YOLO chỉ thể hiện cấu trúc của dòng nhãn. Dòng nhãn vẫn có thể chọn sai lớp, bỏ sót vật thể hoặc đặt bounding box không đúng với vật thể trong ảnh.

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện: `drive_022`, `drive_033`, `drive_038`
- Mã ảnh thẩm định: `drive_008`
- Mô tả một dự đoán trong `detect_result.jpg`: Trên ảnh `drive_008`, kết quả dự đoán không xuất hiện bounding box rõ ràng trên ảnh đầu ra `detect_result.jpg`, cho thấy mô hình thử nghiệm chưa phát hiện được vật thể ở ngưỡng confidence đã đặt.
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?
    Cần kiểm lại dữ liệu huấn luyện và chất lượng nhãn, đặc biệt là các vật thể có kích thước nhỏ hoặc ở xa. Đồng thời cần lưu ý rằng mô hình chỉ được huấn luyện trên ba ảnh nên dữ liệu rất ít. 
- Minh chứng nào có thể bác bỏ nhận định của bạn?
    Có thể kiểm tra ảnh gốc, nhãn Ground Truth và kết quả dự đoán chi tiết của mô hình để xác định liệu mô hình thực sự không phát hiện vật thể hay chỉ không hiển thị hộp ở ngưỡng confidence đang sử dụng.
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?
    Vì chỉ có ba ảnh dùng để huấn luyện và một ảnh dùng để kiểm tra. Số lượng dữ liệu quá nhỏ và không đại diện cho các tình huống thực tế. Notebook cũng xác định phép thử này chỉ dùng để phản hồi và tìm lỗi dữ liệu, không phải benchmark cho môi trường sản xuất.

## 6. Đối chiếu nhãn

- Số hộp ghép được: 95
- IoU trung bình và trung vị: 0.889754 0.905189
- Mức đồng thuận lớp: 96.8421%
- Số hộp phía bạn không ghép được: 8
- Số hộp phía đối chiếu không ghép được: 9
- Một điểm khác biệt cụ thể: Một vật thể ở xa có thể bị bỏ sót hoặc có sự khác biệt về bounding box giữa hai bộ nhãn. Đây là điểm cần kiểm tra lại dựa trên ảnh gốc và quy tắc gán nhãn.
- Quy tắc hoặc hành động sửa phát sinh: Rà soát lại các vật thể ở xa, bảo đảm mỗi phương tiện được gán một bounding box riêng và hộp bám sát phần nhìn thấy của vật thể. Nếu không đủ bằng chứng để quyết định thì chuyển `review_state` sang `needs_review` thay vì đoán.
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?
    Vì hai người có thể cùng áp dụng một cách hiểu sai đối với cùng một vật thể. Mức đồng thuận cao chỉ cho thấy khả năng tái lập quy tắc giữa hai người, không chứng minh rằng tất cả nhãn đều đúng.

## 7. Kiểm tra kho GitHub cá nhân

- [ ] Có phiếu quy tắc với ba tình huống mơ hồ.
- [ ] Có kết quả kiểm hai gói xuất.
- [ ] Có thông tin lần huấn luyện và ảnh dự đoán.
- [ ] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [ ] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [ ] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach: 
Minh chứng mạnh nhất là kết quả đối chiếu nhãn: 95 hộp ghép được, IoU trung bình 0.889754, IoU trung vị 0.905189 và mức đồng thuận lớp 96.8421%. Kết quả này cho thấy hai bộ nhãn có mức độ tương đồng cao và giúp phát hiện các hộp không ghép được để kiểm tra lại.


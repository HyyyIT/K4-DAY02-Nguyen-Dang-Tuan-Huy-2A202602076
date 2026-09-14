# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Nguyễn Đăng Tuấn Huy<br>
**MSSV:** 2A202602076<br>
**Hình thức:** Cá Nhân<br>
**Mã cặp:** SOLO

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

- Ảnh và mã vật thể: `drive_038`, xe dài trong ảnh.
- Dấu hiệu nhìn thấy: Thân xe dài, có nhiều cửa sổ; không có dạng thân hộp nhỏ kín.
- Quy tắc áp dụng: Gán `bus` khi thấy thân xe khách dài với nhiều cửa sổ; không gán `van` cho thân xe có đặc điểm xe buýt.
- Quyết định: `bus`.
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Đánh dấu `needs_review`, ghi lý do và hỏi Lab Coach; không đoán lớp chỉ dựa vào IoU.

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: `drive_022`, xe có thùng phía sau cabin.
- Dấu hiệu nhìn thấy: Cabin và thùng hàng tách rõ, có phần chở hàng phía sau.
- Quy tắc áp dụng: Gán `truck` khi thấy thùng, ben hoặc sàn hàng rõ; không gán `van` hoặc `car` khi khoang hàng tách biệt như xe tải.
- Quyết định: `truck`.
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Đánh dấu `needs_review`, kiểm tra lại ảnh ở mức 100% và xin hỗ trợ trước khi sửa lớp.

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: `drive_008`, đốm nhỏ ở xa (mã hộp cụ thể chưa được ghi trong nhật ký).
- Dấu hiệu nhìn thấy khi phóng 100%: Chỉ thấy một vật thể rất nhỏ và mờ; không đủ rõ cửa sổ hoặc thùng hàng để phân lớp chắc chắn.
- Giá trị `visibility`: `unclear`
- Giá trị `boundary`: `inside`
- Trạng thái `review_state`: `needs_review`
- Lý do: Phóng 100% vẫn không đủ bằng chứng về đặc điểm lớp; không được đoán lớp từ hình dạng mơ hồ.

## 6. Xác nhận tự kiểm tra

- [x] Đã rà đủ bốn ảnh.
- [x] Đã kiểm vật thể thiếu và trùng.
- [x] Đã kiểm lớp và hình học từng hộp.
- [x] Mỗi hộp có đủ ba thuộc tính.
- [x] Đã xử lý mọi hộp `needs_review`.
- [x] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
- [ ] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
- [x] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu.
- [x] Số vật thể thực tế: 91 hộp đã gán trên bốn ảnh — tính từ 48 hộp ghép được và 43 hộp phía tôi không ghép được; 40–60 chỉ là mục tiêu khối lượng, không phải điểm cắt.

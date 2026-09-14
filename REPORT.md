# Báo cáo — Ngày 2: phát hiện vật thể

**Họ và tên:** Nguyễn Đăng Tuấn Huy<br>
**MSSV:** 2A202602076<br>
**Hình thức:** Cá Nhân<br>
**Mã cặp:** SOLO

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: f7d99888f21440fb0374d84962b93213bd8c14e665d093cc8d37f4c61b71ed33
- Bốn mã ảnh: drive_008, drive_022, drive_033, drive_038
- Số vật thể thực tế: Số hộp bạn đã gán trên 4 ảnh (mục tiêu 40–60, không phải điểm cắt)
- Mã SHA-256 của gói YOLO của bạn:4d5c5638a7c801a4e797e897ad7c32533eeca4a4eb915199436b0032e8f5c078
- Mã SHA-256 của gói CVAT gốc của bạn:81ab0b4ececce53890b53f07bfdc5bcd95d1427c9057bb365e5f21d9c3219a45
- Nguồn đối chiếu: bạn cùng cặp hoặc bộ tham chiếu do người hướng dẫn thực hành cấp:Bộ nhãn tham chiếu do Lab Coach cấp (vì làm cá nhân)
- Mã SHA-256 của gói đối chiếu:c8bbc767d8bb9a29f4ca5abf0c3516e5c2af94c58143a980b0148cfe0b500d2b
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu:
2026-09-14 11:14
Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu:
Tôi gán nhãn đủ 4 ảnh, tự kiểm (lớp, hộp, thuộc tính), xuất YOLO + CVAT và ghi SHA-256 trước khi mở bộ tham chiếu. Nguồn đối chiếu chỉ dùng sau khi bản của tôi đã khóa; không copy nhãn, không dùng lại ZIP của chính mình.

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| drive_022 sedan làn phải   | Car | Xe ô tô con nhỏ có 4 cửa |sedan và không có thùng hàng  |
| drive_038 xe dài    | Bus | Thân xe dài nhiều cửa sổ | Gán bus khi thấy thân xe khách dài nhiều cửa sổ  |
| drive_022 xe có thùng sau cabin    | Truck | Cabin thùng hàng tách rõ | Truck khi có thùng ben hoặc sàn hàng  |
| drive_008 xe hộp nhỏ kín   | van | Thân hộp kín ngắn hơn bus. | Van thân hộp nhỏ kín không thân bus không thùng kiểu tải  |
Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:
Lớp ta sẽ biết nó là loại xe nào. Thuộc tính không đổi loại xe
## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| drive_022, một hộp bao hai ô tô liền nhau | phạm vi | Rà trùng: một hộp chứa nhiều phương tiện | Tách 2 hộp mỗi xe một hộp, không gộp |

- Số hộp `needs_review` trước và sau khi kiểm:Trước 5 sau 6
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ:
drive_008, đốm nhỏ xa, phóng 100% vẫn không thấy cửa sổ.

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`:
- Tên lớp và tọa độ điểm ảnh `xyxy`: Row : [1, 0.941875, 0.458359, 0.078437, 0.106406]
lớp=1 (truck) | tâm=(0.9419, 0.4584) | kích thước=(0.0784, 0.1064)
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?
Sai phạm vi (một dòng cho hai xe), sai hình học (box lỏng). Format ≠ ground truth đúng quy tắc.

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện:
- Mã ảnh thẩm định:
- Mô tả một dự đoán trong `detect_result.jpg`:
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào?
- Minh chứng nào có thể bác bỏ nhận định của bạn?
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?

## 6. Đối chiếu nhãn

- Số hộp ghép được: 48 cặp hộp.
- IoU trung bình và trung vị: IoU trung bình `0.859765`, trung vị `0.883327`.
- Mức đồng thuận lớp: `34/48 = 70.8333%` cặp ghép có cùng lớp.
- Số hộp phía bạn không ghép được: 43 hộp.
- Số hộp phía đối chiếu không ghép được: 2 hộp.
- Một điểm khác biệt cụ thể: Ở `drive_022`, hộp phía tôi số 1 và hộp đối chiếu số 5 có IoU `0.901661` nhưng lớp lần lượt là `truck` và `bus`, nên hình học khớp tốt nhưng phân lớp không đồng thuận.
- Quy tắc hoặc hành động sửa phát sinh: Rà lại 14 cặp không đồng thuận lớp bằng dấu hiệu nhìn thấy của xe; đặc biệt phân biệt `bus`, `truck` và `van` theo thân xe, cửa sổ và thùng hàng. Chỉ sửa lớp sau khi kiểm tra ảnh, không suy ra lớp từ IoU.
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng? Vì mức đồng thuận chỉ đo sự giống nhau giữa hai bộ nhãn, không chứng minh cả hai đều đúng theo thực tế. Ngoài 14 cặp khác lớp, vẫn có thể cùng mắc một lỗi, bỏ sót cùng một vật thể, hoặc vẽ hộp sai nhưng vẫn có IoU đủ để ghép. Ngưỡng `0.01` trong phép tính chỉ là ngưỡng ghép kỹ thuật, không phải ngưỡng đạt chất lượng.

## 7. Kiểm tra kho GitHub cá nhân

- [x] Có phiếu quy tắc với ba tình huống mơ hồ.
- [x] Có kết quả kiểm hai gói xuất.
- [x] Có thông tin lần huấn luyện và ảnh dự đoán.
- [x] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [x] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [x] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach: 
    - Hai gói xuất YOLO và CVAT đã được khóa, ghi SHA-256 trước khi đối chiếu, cùng bảng comparison_iou.csv và comparison_summary.json ghi rõ 48 cặp ghép, IoU trung bình 0.859765, trung vị 0.883327 và mức đồng thuận lớp 70.8333%. Các số liệu này cho thấy quá trình đối chiếu có thể kiểm tra lại và không thay thế bài làm độc lập ban đầu.

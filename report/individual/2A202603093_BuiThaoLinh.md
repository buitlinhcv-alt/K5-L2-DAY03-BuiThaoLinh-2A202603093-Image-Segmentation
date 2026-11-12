# Báo cáo thực hành Day 3 — Image Segmentation

- **Mã học viên theo lớp:** 2A202603093
- **Ngày / CVAT local:** 11–12/11/2026 — CVAT Local (Docker, localhost:8080)
- **Công cụ đã dùng:** CVAT Local, Polygon. Không sử dụng SAM.

## 1. Bài đã nộp

Đã thực hiện gán nhãn và lưu toàn bộ 14 ảnh thuộc 9 task. Các file ZIP đã được kiểm tra bằng `scripts/inspect_submissions.py`, kết quả 9/9 task báo `[OK]`.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
|---|---|---:|---:|
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | **14 / 14** | **100** |

## 2. Một quyết định trước khi dùng gợi ý

- **Ảnh, vị trí và object Medium đầu tiên tự vẽ:** [CẦN XÁC NHẬN: ảnh số mấy, người phụ nữ mặc áo dài ở giữa ảnh có phải object đầu tiên không?]
- **Class và quy tắc tôi dùng để chọn biên:** Nếu là người phụ nữ mặc áo dài, chọn class `person`, dùng Polygon vẽ sát đường viền cơ thể và trang phục nhìn thấy; không lấy thêm vùng nền hoặc phương tiện phía sau.
- **Nếu dùng gợi ý sau đó:** Không dùng công cụ gợi ý tự động. [XÁC NHẬN]
- **Quyết định gán nhãn:** Mỗi người được tạo thành một object riêng để phân biệt các instance, kể cả khi nhiều người cùng có nhãn `person`.

## 3. Một lỗi tôi tìm thấy và sửa

Task/ảnh/vùng: Đã rà soát các task sau khi hoàn thành gán nhãn.

Lỗi thuộc loại: Chưa phát hiện lỗi cụ thể cần sửa.

Bằng chứng tôi nhìn thấy: Kiểm tra trực quan trên CVAT chưa phát hiện sai sót rõ ràng.

Quy tắc và hành động sửa: Đã kiểm tra lại các Polygon, nhãn đối tượng và vùng được gán. Chưa thực hiện chỉnh sửa vì chưa xác định được lỗi.

Sau sửa đã Save và export lại chưa? Đã Save và export các task; không có lần sửa lỗi cụ thể để xác nhận.

Kết quả kiểm tra: Chạy py scripts/inspect_submissions.py --dir submissions, cả 9 task đều báo [OK]. Chưa có điểm chấm chất lượng mask.
## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
|---|---|---|---|
| medium_instance — xe bị người che khuất [xác nhận ảnh/vị trí] | Vẽ các phần nhìn thấy thành nhiều object hay giữ cùng một instance? | Một chiếc xe chỉ là một instance dù bị che khuất. Không được tính phần bị che thành xe khác. | Cần đảm bảo các phần nhìn thấy của cùng một xe được biểu diễn đúng một instance. |
| cp2_slice — hai xe sát nhau [xác nhận vị trí] | Gộp hai xe cùng loại thành một Polygon hay tách riêng? | Instance Segmentation yêu cầu mỗi phương tiện là một object độc lập. | Tách riêng từng xe, kể cả khi chúng sát nhau hoặc cùng class. |
| cp4_curb — ranh giới mặt đường và vỉa hè [xác nhận vị trí] | Tính phần bó vỉa vào `road` hay `sidewalk`? | Dựa vào ranh giới bó vỉa và chức năng từng vùng, tránh tô chồng lấn. | Tách vùng `road` và `sidewalk` theo ranh giới nhìn thấy trong ảnh. |

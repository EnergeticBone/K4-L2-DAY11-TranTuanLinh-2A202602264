# Escalation ticket

## Ticket 1

- **Frame:** `adasind_102750.jpg`
- **Ảnh chụp:** `submission/screenshots/adasind_102750_cvat_rework.png`
- **Expected impact:** Nếu không chuẩn hóa phân định giữa Mini-Truck và ThreeWheeler chở hàng, toàn bộ chiến dịch gán nhãn 50.000 frame SVM sẽ chịu tỷ lệ nhầm lẫn nhãn lên tới 15–20% ở zone `mid` và `edge`. Điều này làm sai lệch ground truth kiểm thử ADAS, khiến hệ thống nhận diện xe tải bị giảm độ nhạy phát hiện phương tiện vận tải nhẹ ở rìa camera.
- **Owner:** `guideline`
- **Recommendation:** Phê duyệt và áp dụng ngay Guideline Patch **R04b** vào bộ quy tắc chính thức `v1.1.0`. Tổ chức một buổi hiệu chuẩn ngắn (calibration clinic) cho toàn bộ đội ngũ annotator với 10 ca mẫu phân biệt xe tải nhỏ (mini-truck/pickup) và xe ba bánh kéo thùng.

# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 6 | 1 | 1 | 1 | 3 | SPURIOUS (1) |
| mid | 6 | 0 | 0 | 4 | 8 | — |
| edge | 6 | 2 | 1 | 4 | 1 | MISSING (1) |

## Nhận xét

- **Zone người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên:**
  - **Người (L)** gãy nhiều nhất ở zone **`edge`** với 2 ca bỏ sót (MISSING) và 1 ca thừa/sai lệch (SPURIOUS) trên tổng số 6 reference. Zone `center` cũng có 1 ca MISSING và 1 ca SPURIOUS. Ngược lại, ở zone `mid`, L thực hiện rất tốt với 0 missing và 0 spurious. Lỗi chính của L là bỏ sót đối tượng nhỏ ở rìa ảnh (`MISSING`) và nhầm lẫn phân loại phương tiện (`WRONG_CLASS`).
  - **Model (M)** gãy nặng nhất ở zone **`mid`** và **`edge`**: tại `mid`, Model bỏ sót 4 ca (`LR_noM`) và vẽ thừa tới 8 ca ảo giác (`M_only`), tổng cộng 12 lỗi; tại `edge`, Model bỏ sót 4 ca (`LR_noM` + `R_only`) và thừa 1 ca. Ở zone `center`, Model hoạt động tương đối tốt hơn nhưng vẫn thừa 3 ca do bắt nhầm bối cảnh.

- **Giả thuyết vì sao và giới hạn của slice ba frame:**
  - *Với Model (M):* Mô hình YOLO26m được huấn luyện chủ yếu trên ảnh phối cảnh phẳng (rectilinear pinhole). Khi kiểm thử trên ảnh fisheye mắt cá có quang sai lớn, hiện tượng lệch miền dữ liệu (domain shift) xảy ra nghiêm trọng: độ cong hình học ở $r/R \ge 0.35$ (`mid`) và $r/R \ge 0.6$ (`edge`) khiến đặc trưng biên dạng xe bị biến dạng mạnh làm model bỏ sót (`LR_noM`). Đồng thời, các vệt phản chiếu, bóng râm và texture mặt đường bị model nhận nhầm thành phương tiện (`M_only`).
  - *Với Người (L):* Ở vùng `edge`, mật độ điểm ảnh giảm và vật thể bị nén dẹt sát viền kính khiến người gán nhãn dễ bỏ sót đối tượng nhỏ (như `Bike R5` trong `adasind_082170.jpg`). Ngoài ra, góc nhìn fisheye méo cũng làm khó phân biệt hình thái thùng xe tải nhỏ với xe ba bánh (`ThreeWheeler` vs `Truck` trong `adasind_102750.jpg` theo R04).
  - *Giới hạn của slice 3 frame:* Tập dữ liệu chỉ gồm 3 frame (18 đối tượng reference chia đều 6 box/zone). Đây là cỡ mẫu quá nhỏ (small sample size) trong bài thực hành lab, không đại diện cho toàn bộ 50.000 frame hay các điều kiện thời tiết, ánh sáng, góc camera khác nhau (rear, left, right). Các số liệu trên mang tính chẩn đoán định tính để định hướng rule và kế hoạch gán nhãn, không dùng làm kết luận độ chính xác của hệ thống sản xuất thật.

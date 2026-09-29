# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | ATTRIBUTE | 1 |
| center | B2 | MISSING | 3 |
| center | B2 | SPURIOUS | 5 |
| center | C0 | MISSING | 1 |
| center | C0 | SPURIOUS | 2 |
| edge | B2 | ATTRIBUTE | 4 |
| edge | B2 | IGNORE_SCOPE | 2 |
| edge | B2 | MISSING | 5 |
| edge | B2 | SPURIOUS | 2 |
| edge | B2 | WRONG_CLASS | 1 |
| edge | C0 | WRONG_CLASS | 1 |
| mid | B2 | ATTRIBUTE | 2 |
| mid | B2 | MISSING | 5 |
| mid | B2 | SPURIOUS | 8 |
| mid | C0 | MISSING | 1 |
| mid | C0 | SPURIOUS | 1 |

## Top defects
- SPURIOUS: 18 (ví dụ frame adasind_019560.jpg)
- MISSING: 15 (ví dụ frame adasind_019560.jpg)
- ATTRIBUTE: 7 (ví dụ frame adasind_069450.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:**
  - Lỗi nổi bật nhất của con người (L) là sai phân loại `WRONG_CLASS` tại frame `adasind_102750.jpg` (`L4+R2`), khi annotator gán nhãn `ThreeWheeler` nhưng Reference và Model đều gán `Truck`. Nguyên nhân là `E1_annotator_error` (nhầm lẫn hình thái) kết hợp `E2_guideline_gap`: phương tiện là xe tải nhỏ chở hàng (mini-truck/pickup chở hàng địa phương), đầu xe ngắn nằm sát mép rìa bị méo quang học fisheye khiến người gán nhãn nhìn thoáng qua tưởng xe ba bánh (auto-rickshaw), trong khi thùng chở hàng và trục bánh sau thể hiện rõ đây là xe tải theo định nghĩa R04.
  - Về phía toàn bảng: Hai lỗi chiếm số lượng áp đảo là `SPURIOUS` (18 ca) và `MISSING` (15 ca), tập trung dày đặc ở zone `mid` và `edge`. Nguyên nhân chủ đạo là `E4_model_domain`: mô hình YOLO26m đóng băng được huấn luyện trên ảnh phối cảnh chuẩn (rectilinear pinhole), khi suy luận trên ảnh fisheye 360° với biến dạng cong radial lớn ($r/R \ge 0.35$) đã không phát hiện được đối tượng in-scope (`MISSING`) và nhận nhầm bóng cây, vết sơn, texture mặt đường thành vật thể (`SPURIOUS`).

- **Cách sửa và ai nhận việc (`owner`):**
  - Ca `L4+R2` (`WRONG_CLASS`): `owner = annotator`. Người gán nhãn trực tiếp mở task CVAT ở vòng P5 Rework, sửa class của box `L4` từ `ThreeWheeler` thành `Truck` và xuất bản khóa mới `annotations-v2.xml`.
  - Khoảng trống quy tắc phân định xe tải nhỏ: `owner = guideline`. Đội ngũ quản lý guideline bổ sung tiêu chí nhận diện trực quan cho mini-truck/xe tải thùng vào Rule R04 trong `20_guideline_patch.md`.
  - Lỗi lệch miền của mô hình AI: `owner = ai_team`. Đội ngũ AI ghi nhận backlog để huấn luyện lại mô hình với dữ liệu fisheye augmentation hoặc sử dụng mạng nơ-ron hỗ trợ biến dạng góc rộng.

- **Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule):**
  - Ảnh minh chứng: `submission/screenshots/adasind_102750_cvat_rework.png` thể hiện rõ xe tải màu tím ở mép trái ngoài cùng (IoU đạt 0.873 so với Reference).
  - Dòng findings liên quan: Dòng 19 (`r1_craft,B2-edge,adasind_102750.jpg,L4+R2,na,WRONG_CLASS`) và các dòng chẩn đoán 38-39 (`r3_diag,B2-edge,adasind_102750.jpg,L4,L_only,SPURIOUS` và `R2+M2,RM_noL,MISSING`).
  - Quy tắc vi phạm: **Rule R04** (Ánh xạ 6 class — xe bán tải nhỏ hoặc xe tải chuyên chở hàng phải gán nhãn `Truck`, không gán `ThreeWheeler`).

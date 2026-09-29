# QA review · B2-edge

Mã khóa: 2F90-6D77

Cold-review fallback: đã chờ hơn 5 phút nhưng không có export khóa B2-center của Ha trong workspace; tự soát lại bản khóa B2-edge theo luật, không mở reference/model.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_069450.jpg | L4 Bike | R05 | Cờ CVAT occluded=1 nhưng attribute occluded=false; box đồng thời truncated=true ở biên trái. Kiểm tra ảnh xem có vật khác che Bike không; cắt ở biên không tự tính là occluded. |
| adasind_082170.jpg | L1 Bike | R05 | Cờ CVAT occluded=1 nhưng attribute occluded=false. Đối chiếu ảnh gốc trong cụm giao thông để xác định có vật khác che Bike không rồi đồng bộ attribute. |
| adasind_082170.jpg | L2 Bike | R05 | Cờ CVAT occluded=1 nhưng attribute occluded=false; box truncated=true ở mép trái. Phân biệt phần bị cắt khỏi ảnh với phần bị vật khác che trước khi đặt attribute. |
| adasind_082170.jpg | L4 Bike | R05 | Cờ CVAT occluded=1 nhưng attribute occluded=false. Box chồng vùng với L5; kiểm tra ảnh xem có che khuất thực tế không rồi đồng bộ attribute. |
| adasind_082170.jpg | L5 Bike | R05 | Cờ CVAT occluded=1 nhưng attribute occluded=false. Box chồng vùng với L4; kiểm tra ảnh xem có che khuất thực tế không rồi đồng bộ attribute. |
| adasind_082170.jpg | L6 Bike | R05 | Cờ CVAT occluded=1 nhưng attribute occluded=false. Đối chiếu ảnh gốc trong cụm giao thông để xác định có vật khác che Bike không rồi đồng bộ attribute. |
| adasind_082170.jpg | L7 Bike | R05 | Cờ CVAT occluded=1 nhưng attribute occluded=false. Đối chiếu ảnh gốc trong cụm giao thông để xác định có vật khác che Bike không rồi đồng bộ attribute. |

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.

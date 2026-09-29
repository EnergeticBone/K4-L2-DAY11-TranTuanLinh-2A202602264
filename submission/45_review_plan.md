# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `B2-edge` / `adasind_102750.jpg` | 4 ca (1 `WRONG_CLASS` ThreeWheeler vs Truck, 1 `MISSING` Truck trung tâm, 1 `SPURIOUS` out-of-scope, 1 `RM_noL`) | Có xung đột phân loại class nghiêm trọng giữa annotator và Reference (IoU 0.873) cùng ca bỏ sót xe tải ở khu vực trung tâm; đây là các lỗi P1 in-scope có rủi ro an toàn cao nhất trong toàn bộ slice. | Ảnh chụp màn hình CVAT so sánh phân loại Truck vs ThreeWheeler (`submission/screenshots/adasind_102750_cvat_rework.png`), ma trận IoU và trích dẫn Rule R04. |
| `B2-edge` / `adasind_082170.jpg` | 8 ca (1 `MISSING` Bike ở vùng rìa edge, 7 ca `ATTRIBUTE` occluded vs truncated) | Tập trung mật độ xe hai bánh (Bike) đông đúc ở vùng méo cong fisheye tiếp xúc vành kính; xuất hiện hiện tượng bỏ sót xe máy nhỏ sát rìa (R5 MISSING) và lệch cờ thuộc tính hệ thống với attribute thực tế theo R05. | Bảng đối chiếu thuộc tính R05 trong `qa_review.md`, ảnh zoom chi tiết xe máy R5 sát vành kính (`submission/screenshots/adasind_082170_edge_bike.jpg`) và tọa độ r/R > 0.6. |

Giới hạn của kết luận từ ba frame ADASIND: Tập dữ liệu chỉ gồm 3 frame trên một camera fisheye đơn lẻ (front-facing) trong điều kiện ban ngày khô ráo tại một bối cảnh đường phố Ấn Độ. Kích thước mẫu quá nhỏ (18 đối tượng reference) không có ý nghĩa thống kê để đại diện cho toàn bộ phân phối giao thông thực tế (thiếu ban đêm, trời mưa, đường cao tốc, hầm tối), và đặc biệt không thể đại diện cho đặc tính quang học của 3 camera còn lại (rear và 2 camera hông bên gương).

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
- **Cách soát độ phủ và khử trùng lặp:** Áp dụng kỹ thuật lấy mẫu phân tầng (stratified sampling) kết hợp ràng buộc giãn cách thời gian (temporal subsampling — tối thiểu giãn cách 3–5 giây hoặc cách nhau ≥30 frame giữa các mẫu lấy từ cùng một video clip) để loại trừ hoàn toàn hiện tượng tự tương quan (autocorrelation). Điều này ngăn chặn việc đếm các frame liên tiếp trong cùng một cảnh dừng đèn đỏ hoặc kẹt xe thành nhiều ca lỗi độc lập. Phân tầng bao phủ đầy đủ 4 camera × 2 mức độ (normal/hard) với các điều kiện môi trường đa dạng (ngày, đêm, ngược sáng, ống kính mờ bẩn do thời tiết).
- **Vì sao kế hoạch chỉ giúp tìm ca cần soi mà chưa đo được tỷ lệ lỗi:** Lấy mẫu có chủ đích tập trung vào ca khó (oversampling hard cases: 85/200 frame hard chiếm tới 42.5%) là phương pháp "tìm kiếm lỗi có định hướng" (targeted stress testing) nhằm tối đa hóa việc phát hiện các ca biên nguy hiểm (edge cases) và khoảng trống quy tắc. Do phân bố mẫu không phản ánh đúng tần suất xuất hiện tự nhiên của các tình huống trong 50.000 frame vận hành thật (nơi các cảnh đường thoáng bình thường chiếm hơn 90%), tỷ lệ lỗi đo được trên 200 frame này sẽ bị lệch có chủ ý và không thể dùng làm ước lượng không chệch (unbiased estimator) cho tỷ lệ lỗi tổng thể của hệ thống.

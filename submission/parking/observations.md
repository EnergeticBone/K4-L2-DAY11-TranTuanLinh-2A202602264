# Quan sát vạch ô đỗ

- **Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):**
  1. Vạch sơn trắng phân chia ô đỗ ở tiền cảnh bên trái bãi đỗ (đoạn polyline phân định ranh giới giữa hai ô đỗ chéo từ tọa độ `(58.45, 521.03)` đến `(48.57, 570.84)`).
  2. Vạch sơn trắng phân chia hai ô đỗ liền kề ở tiền cảnh trung tâm (đoạn polyline kéo dài từ `(278.29, 516.91)` đến `(422.39, 553.96)`).
  Cả hai vạch này đều có vai trò trực tiếp tạo ranh giới phân cách vật lý giữa các vị trí đỗ xe riêng biệt.

- **Một vạch/dấu sơn hoặc biên không vẽ, và vì sao:**
  Dải vạch sơn dài nằm ngang ở ranh giới giữa dãy ô đỗ và lối xe chạy chính xuyên suốt bãi đỗ không được vẽ làm `parking_line`. Lý do: theo quy chuẩn [docs/11-parking-lines-vi.md](docs/11-parking-lines-vi.md), vạch này có vai trò phân định lối xe chạy/làn lưu thông nội bộ (driving aisle line), không có chức năng tạo ranh giới chia ô đỗ riêng lẻ. Tương tự, gờ mép bó vỉa hè (curb) cũng bị loại trừ.

- **Polygon `free_space` dừng ở đâu; có phần bị che nào không:**
  Polygon `free_space` bao phủ phần mặt đường nhựa trống nhìn thấy được của lối xe chạy chính ở tiền cảnh bãi đỗ. Ranh giới polygon dừng lại chính xác tại mép tiếp xúc mặt đất của lốp xe ô tô đang đỗ, dừng trước bó vỉa hè và các vật cản tĩnh. Vùng mặt đường bị thân xe ô tô đỗ che khuất tầm nhìn hoặc khuất sau bóng râm lớn hoàn toàn không bị vẽ xuyên qua.

- **Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”):**
  không có

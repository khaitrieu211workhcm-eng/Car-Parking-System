# Car Parking System - Bãi giữ xe tự động dùng Arduino

Mô hình bãi giữ xe ô tô tự động: cảm biến hồng ngoại (IR) phát hiện xe ra vào, servo điều khiển thanh cản, màn hình LCD hiển thị lời chào và số chỗ còn trống. Khi bãi đã đầy, thanh cản không mở và LCD thông báo cho chủ xe.

> Đồ án môn **Nhập môn Kỹ thuật (ĐTVT)** - HK1, năm học 2024-2025
> Khoa Điện tử - Viễn thông, Trường Đại học Khoa học Tự nhiên, ĐHQG TP.HCM
> Giảng viên hướng dẫn: PGS.TS. Lê Đức Hùng



---

## 1. Giới thiệu

Theo số liệu Cục Đăng kiểm Việt Nam (giữa năm 2023) được trích trong báo cáo, cả nước có khoảng 5 triệu ô tô đang lưu hành, trong đó hơn 3 triệu xe du lịch dưới 9 chỗ. Nhu cầu bãi giữ xe vì vậy ngày càng lớn, nhất là ở các đô thị lớn, trong khi nhiều bãi vẫn vận hành theo cách truyền thống.

Nhóm xây dựng một mô hình thu nhỏ để áp dụng điều khiển tự động vào bãi giữ xe, đồng thời làm quen với thiết kế mạch, lập trình Arduino và mô phỏng mạch trên Tinkercad.

**Mục tiêu của đồ án**

- Mô hình có kích thước phù hợp, hoạt động ổn định.
- Dùng linh kiện giá rẻ, lắp ráp nhanh.
- Nắm nguyên lý thiết kế mạch đơn giản và các công cụ mô phỏng.
- Rèn kỹ năng làm việc nhóm và quản lý dự án.

## 2. Tính năng

- Phát hiện xe **đi vào** và **đi ra** bằng 2 cảm biến IR.
- Tự động **mở/đóng thanh cản** bằng servo.
- Đếm và hiển thị **số chỗ còn trống** (mô hình có 4 chỗ) trên LCD 16x2.
- Khi **bãi đầy**, không mở thanh cản và hiển thị thông báo cho chủ xe.

## 3. Nguyên lý hoạt động

| Tình huống | Điều kiện | Hành động | LCD |
|---|---|---|---|
| Chờ | Không có xe | Thanh cản đóng | `XIN CHAO !` / `Con Trong: N` |
| Xe vào | IR1 phát hiện xe, còn chỗ trống | Thanh cản mở, số chỗ trống giảm 1 | `XIN CHAO !` / `Con Trong: N-1` |
| Xe ra | IR2 phát hiện xe | Thanh cản mở, số chỗ trống tăng 1 | `XIN CHAO !` / `Con Trong: N+1` |
| Bãi đầy | IR1 phát hiện xe, hết chỗ | Thanh cản **không** mở | `XIN LOI` / `BAI GIU XE DAY!` (3 giây) |

Chương trình dùng hai biến cờ `flag1`, `flag2` để theo dõi thứ tự xe đi qua hai cảm biến:

1. Xe chạm cảm biến nào trước thì hệ thống xác định đó là chiều vào hay ra, mở thanh cản (sau khoảng 1,5 giây) và cập nhật số chỗ trống.
2. Khi xe đã đi qua cả hai cảm biến (`flag1 = 1` và `flag2 = 1`), thanh cản đóng lại sau khoảng 1 giây và hai cờ được đặt về 0 để sẵn sàng cho lần tiếp theo.
3. Ở cuối mỗi vòng lặp, LCD cập nhật lời chào và số chỗ còn trống.



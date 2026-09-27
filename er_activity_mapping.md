# Phân tích Thiết kế Cơ sở Dữ liệu Hệ thống AutoRide

## 1. Giải thích về ràng buộc cột `damage_fee`
Cột `damage_fee` trong bảng `rentals` bắt buộc phải cấu hình thuộc tính `NOT NULL` và có giá trị mặc định là `0.00`. Điều này giúp hệ thống hoạt động toàn vẹn, phân biệt rõ ràng giữa một chuyến đi an toàn không phát sinh phí (`damage_fee = 0`) với trường hợp dữ liệu bị bỏ sót hoặc lỗi hệ thống (`NULL`).

## 2. Giải quyết các "khoảng trống dữ liệu" tài chính và nghiệp vụ
- **Quản lý dòng tiền:** Bổ sung cột `security_deposit` (tiền cọc) để ghi nhận nghĩa vụ tài chính ban đầu và `late_fee` (phí phạt trễ) xử lý tình huống quá hạn, tránh thất thoát ngân sách.
- **Trạng thái vòng đời hợp đồng:** Sử dụng kiểu dữ liệu `ENUM('pending', 'active', 'completed', 'cancelled')` giúp chuẩn hóa các bước của quy trình thuê xe, ngăn chặn cập nhật sai trạng thái logic.
- **Quy trình kiểm tra an toàn:** Bảng `inspections` lưu lại bằng chứng sau khi nhận xe, làm căn cứ minh bạch để tính toán số tiền phạt hư hại và hoàn trả tiền cọc cho khách hàng một cách chính xác.

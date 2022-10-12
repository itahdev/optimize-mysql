### Tối ưu các câu lệnh select
Các câu lệnh select thực hiện tất cả các câu lệnh tra cứu trong CSDL nên là ưu tiên hàng đầu để tối ưu.

#### Những điều cần xem xét khi tối ưu:

- Có cần thêm index hay không?
- Giảm thiểu số lần query toàn bộ table. Đặc biệt với các bảng lớn.
- Tìm hiểu tuning techniques, indexing techniques và các tham số cấu hình dành riêng cho công cụ lưu trữ cho mỗi bảng.
- Chú ý đến câu lệnh EXPLAIN
- Điều chỉnh kích thước memory của các phần dành cho caching. Các truy vấn lặp đi lặp lại sẽ nhanh hơn vì được lấy từ cache.
- Ngay cả các truy vẫn nhanh bằng cách sử dụng cache, vẫn cần optimize để chúng sử dụng ít bộ nhớ cache hơn. Thì có thể lưu nhiều vào cache hơn.
- Giải quyết các vấn đề về locking. Vì vấn đề này có thể ảnh hưởng đến tốc độ query bới nhiều phiên có thể truy cập vào bảng cùng 1 lúc.

### Các câu lệnh where

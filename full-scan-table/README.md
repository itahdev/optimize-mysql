FULL SCAN TABLE
===========================

## Cách nhận biết

Khi sử dụng `EXPLAIN`, kết quả trả về cột type là `ALL`

## Nguyên nhân

`SELECT *` hoặc select số lượng lớn cột trong bảng. Trong trường hợp này, việc quét toàn bộ `index` và tìm kiếm các hàng để tìm các cột không có trong `index` có thể tốn kém hơn so với quét toàn bộ bảng và sắp xếp kết quả. Nếu vậy, trình tối ưu hóa có thể không sử dụng `index`. Nếu `SELECT *` chỉ chọn các cột `index`, lúc này `index` được sử dụng và tránh sắp xếp.

Bảng quá nhỏ nên việc quét bảng sẽ nhanh hơn việc sử dụng `index`. Thướng xảy ra đối với bảng có ít hơn 10 hàng và độ dài hàng ngắn.

Không xác định rõ ràng điều kiện trong mệnh đề `ON` hoặc `WHERE` đối với các cột được đánh `index`.

So sánh các cột được đánh `index` với các giá trị không đổi (constant) và MYSQL đã tính toán (dựa trên index tree) rằng các hằng số bao phủ một phần quá lớn của bảng và quá trình quét bảng sẽ nhanh hơn.

Sử dụng khóa có [cardinality](https://en.wikipedia.org/wiki/Cardinality_(SQL_statements)) thấp (nhiều hàng khớp với giá trị khóa) và so sánh khóa này với [một cột khác](https://dba.stackexchange.com/questions/300841/what-does-through-another-column-mean-in-the-mysql-docs). Trong trường hợp này, MySQL giả định rằng bằng cách sử dụng khóa có thể yêu cầu nhiều lần tra cứu khóa và quá trình quét bảng sẽ nhanh hơn.

## Tránh quét toàn bộ bảng

Sử dụng `ANALYZE TABLE tbl_name` để cập nhật các [key distribution](https://dev.mysql.com/doc/refman/5.6/en/analyze-table.html#:~:text=Key%20Distribution%20Analysis,-If%20the%20table&text=MySQL%20uses%20the%20stored%20key,specific%20table%20within%20a%20query.) cho bảng đã quét. 

Sử dụng `FORCE INDEX` cho bảng được quét để nói với MySQL rằng quét bảng rất tốn kém so với việc sử dụng chỉ mục đã cho:
```mysql
SELECT * FROM t1, t2 FORCE INDEX (index_for_column) WHERE t1.col_name=t2.col_name;
```
Xem thêm về [Index Hints](https://dev.mysql.com/doc/refman/8.0/en/index-hints.html).

Bắt đầu mysqld với tùy chọn `--max-seeks-for-key = 1000` hoặc sử dụng `SET max_seeks_for_key = 1000` để yêu cầu trình tối ưu hóa giả định rằng không có quá trình quét khóa nào gây ra nhiều hơn 1.000 lần tìm kiếm khóa.

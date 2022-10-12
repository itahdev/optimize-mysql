OPTIMIZATION AND INDEXES
===========================

### [How MySQL Uses Indexes](usage)

> Note:
>
> Index, Index, Index. Cái gì quan trọng nhắc lại 3 lần. Chú ý cách sử dụng `index` với các lệnh `JOIN ON`, `WHERE`
> , `ORDER BY`, `GROUP BY`, ...
>
> Tìm hiểu thêm về B-tree và Hash index.
>
> MYSQL sẽ lựa chọn sử dụng chỉ mục chọn lọc nhất.
>
> Multiple-column index chỉ sử dụng index khi câu query có chứa chỉ mục bên trái ngoài cùng. `ORDERBY DESC` để bao giờ
> test thử đã rồi update lại ...
>

### [Primary Key Optimization](https://dev.mysql.com/doc/refman/8.0/en/primary-key-optimization.html)

> Note:
>
> `Primary Key` không được phép NULL.
>

### [SPATIAL Index Optimization](https://dev.mysql.com/doc/refman/8.0/en/spatial-index-optimization.html)

> Note:
>
> Đọc mấy cái chỉ mục không gian này đau đầu lắm. Để sau ....
>

### [Foreign Key Optimization](https://dev.mysql.com/doc/refman/8.0/en/foreign-key-optimization.html)

> Note:
>
> Đọc thêm chuẩn hóa dữ liệu: 1NF, 2NF, 3NF.
>

### [Column Indexes](https://dev.mysql.com/doc/refman/8.0/en/column-indexes.html)

> Note:
>
> ...
>

### [Multiple-Column Indexes](https://dev.mysql.com/doc/refman/8.0/en/multiple-column-indexes.html)

> Note:
>
> Một index có thể gồm 16 cột
>
> If the table has a multiple-column index, any leftmost prefix of the index can
> be used by the optimizer to look up rows. For example, if you have a three-column
> index on (col1, col2, col3), you have indexed search capabilities on (col1), (col1, col2), and (col1, col2, col3).
>

### [Comparison of B-Tree and Hash Indexes](https://dev.mysql.com/doc/refman/8.0/en/index-btree-hash.html)

> Note:
>
> **_B-tree_**
>
> B-tree index có thể được sử dụng để so sánh cột trong các biểu thức sử dụng =, >, >=, <, <=, hoặc BETWEEN.
>
> Đối với LIKE: Tránh sử dụng câu truy vấn với '%' phía trước vì index không được dùng.
>
> Đối với mệnh đề WHERE: Index sẽ được sử dụng khi nó xuất hiện trong mệnh đề WHERE. Nhiều index sẽ đươc sử dụng thông
> qua AND.
>
> Trường hợp đặc biết: Đôi khi index không được sử dụng, khi trình tối ưu của MYSQL
> ước tính rằng sử dụng index sẽ truy cập lượng lớn hàng trong bảng.
>
> **_Hash_**
>
> Chỉ được sử dụng để so sánh đối với các toán tử = hoặc <> một cách nhanh chóng.
>
> Không thể tối ưu hóa toán tử ORDER BY bằng việc sử dụng HASH index bởi vì nó không thể tìm kiếm được phần từ tiếp theo
> trong Order.
>
> Toàn bộ nội dung của Key được sử dụng để tìm kiếm giá trị records, khác với BTREE một phần của node cũng có thể được
> sử dụng để tìm kiếm.
>
> Nhanh hơn B-tree.
>
> **_Chú ý_**
>
> Việc chọn index theo kiểu BTREE hay HASH ngoài yếu tố về mục đích sử dụng index thì nó còn phụ thuộc vào mức độ hỗ trợ
> của Storage Engine. Ví dụ MyISAM, InnoDB hay Archive chỉ hỗ trợ Btree, trong khi Memory lại hỗ trợ cho cả 2.
> 

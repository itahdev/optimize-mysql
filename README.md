# Overview

### [Full scan table](./full-scan-table/README.md)
> Note:
>
> Index, Index, Index.  Cái gì quan trọng nhắc lại 3 lần. Chú ý cách sử dụng `index` với các lệnh `JOIN ON`, `WHERE`, `ORDER BY`, `GROUP BY`, ...
>
> Khi trình tối ưu hóa của MYSQL tính toán rằng việc tìm kiếm và sử dụng `index` quá tốn kém thì MYSQL sẽ tự động chuyển qua quét toàn bộ bảng thay vì sử dụng index.
>
> Nên sử dụng lệnh `EXPLAIN ` để phân tích câu query.
>
> Hạn chế `SELECT *` với bảng có nhiều cột.
>
> Bảng dưới 10 hàng không cần quan tâm `index`.
>
### [Optimization and Indexes](indexes)
> Note: 
> 
> [How MySQL Uses Indexes](indexes#how-mysql-uses-indexes)
> 
> [Primary Key Optimization](indexes#primary-key-optimization)
> 
> [SPATIAL Index Optimization](indexes#spatial-index-optimization)
> 
> [Foreign Key Optimization](indexes#foreign-key-optimization)
> 
> [Column Indexes](indexes#column-indexes)
> 
> [Multiple-Column Indexes](indexes#multiple-column-indexes)
> 
> [Comparison of B-Tree and Hash Indexes](indexes#comparison-of-b-tree-and-hash-indexes)
> 

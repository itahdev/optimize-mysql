OPTIMIZATION AND INDEXES
===========================

## Index là gì?
Chỉ mục (Index) là bảng tra cứu đặc biệt mà Database Search Engine có thể sử dụng để tăng nhanh thời gian và hiệu suất thu thập dữ liệu. Hiểu đơn giản, một chỉ mục là một con trỏ tới dữ liệu trong một bảng. 

Một chỉ mục trong một Database là tương tự như một chỉ mục trong Mục lục của cuốn sách.

## Cách Mysql sử dụng index
Các chỉ mục được sử dụng để tìm các hàng có giá trị cột cụ thể một cách nhanh chóng. Không có chỉ mục, MySQL phải bắt đầu với hàng đầu tiên và sau đó đọc qua toàn bộ bảng để tìm các hàng có liên quan. Bảng càng lớn, chi phí này càng nhiều. Nếu bảng có một chỉ mục cho các cột được đề cập, MySQL có thể nhanh chóng xác định vị trí để tìm kiếm ở giữa tệp dữ liệu mà không phải xem tất cả dữ liệu. Điều này nhanh hơn nhiều so với đọc mỗi hàng tuần tự.

Hầu hết các chỉ mục MySQL (PRIMARY KEY, UNIQUE, INDEX và FULLTEXT) sử dụng `B-tree`.

Các chỉ mục trên các loại dữ liệu không gian sử dụng `R-tree`.

MEMORY tables hỗ trợ `hash indexs`

Innodb sử dụng [inverted lists](https://en.wikipedia.org/wiki/Inverted_index) cho các chỉ mục FullText.

## Các trường hợp sử dụng - không sử dụng index

- Tìm kiếm sử dụng mệnh đề `WHERE`.

- Để loại bỏ các hàng khỏi việc xem xét. Nếu có sự lựa chọn giữa nhiều chỉ mục, MYSQL thường sử dụng chỉ mục tìm số hàng nhỏ nhất (chỉ mục chọn lọc nhất).

- Nếu bảng có chỉ mục chứa nhiều cột được đánh `index` (multiple-column index), MYSQL chỉ sử dụng index để tìm kiếm khi tổ hợp chỉ mục có chứa chỉ mục đầu tiên (ngoài cùng bên trái). 

  Ví dụ: nếu bạn có tổ hợp chỉ mục ba cột (col1, col2, col3), MYSQL sẽ sử dụng `index` khi tìm kiếm trên (col1), (col1, col2) và (col1, col2, col3). 

  Tóm lại, trong các mệnh đề tìm kiếm bắt buộc phải sử dụng col1 thì `index` mới được sử dụng để tìm kiếm, còn col2 và col3 có hay không, không quan trọng!

- Để có hiệu quả nhất khi JOIN bảng thì các cột trong điều kiện ON phải được đánh index, đồng thời phải cùng type và size. 

  VARCHAR và CHAR được coi là giống nhau nếu chúng được khai báo là có cùng kích thước. Ví dụ, VARCHAR (10) và CHAR (10) có cùng kích thước, nhưng VARCHAR (10) và CHAR (15) thì không.

  Để so sánh chuỗi, các cột nên cùng bộ kí tự. Ví dụ, so sánh cột `utf8mb4` với cột `latin1` thì `index` sẽ không được sử dụng.

- Để tìm giá trị MIN() hoặc MAX() cho một cột được đánh `index`.
  ```mysql
  SELECT MIN(key_part2),MAX(key_part2)
  FROM tbl_name WHERE key_part1=10;
  ```
- Để SORT hoặc GROUP. `Index` được sử dụng nếu việc sort hoặc group được thực hiện trên key ngoài cùng bên trái. Nếu là DESC, khóa sẽ được đọc theo thứ tự ngược lại.

- Nếu truy vấn chỉ sử dụng bao gồm các `index`, các giá trị có thể được truy xuất từ cây chỉ mục để có tốc độ cao hơn.
  ```mysql
  SELECT key_part3 FROM tbl_name
  WHERE key_part1=1
  ```

Các chỉ mục ít ảnh hưởng đến các truy vấn trên các bảng nhỏ hoặc các bảng lớn, trong đó các truy vấn báo cáo xử lý hầu hết hoặc tất cả các hàng.

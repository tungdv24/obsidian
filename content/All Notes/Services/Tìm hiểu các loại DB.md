2025-03-27 16:54
Tags: #services 

| Relational Database                                                   | No Relational Database                                                                    |
| --------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| -Organizing them into tables with rows and columns.                   | -Document-oriented, key-value, column-family and graph.                                   |
| -Making changes to the schema can be complex                          | -Offers flexible schema.                                                                  |
| -Uses Structured Query Language (SQL)                                 | -Uses query languages based on the data models.                                           |
| -Best suited for apps with complex relationships and structured data. | -Best suited for applications with large volumes of unstructured or semi-structured data. |

|               |                                                                          |                                                                            |                                                            |                                                                                 |
| ------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Database      | MySQL                                                                    | Postgre SQL                                                                | MongoDB                                                    | Oracle database                                                                 |
| Data Model    | Relational database (with row and columns)                               | Relational database (with row and columns)                                 | No SQL database(Document-oriented)                         | Relational database                                                             |
| Use Cases     | Good for read-heavy workloads; supports indexing and caching mechanisms. | Applications requiring complex queries, GIS, large-scale data warehousing  | Real-time analytics, content management, IoT applications. | Enterprise applications, large-scale data warehousing, mission-critical systems |
| Scalability   | Vertical scaling                                                         | Support for horizontal scaling                                             | Horizontal scaling is inherent                             | Vertical and horizontal; advanced clustering                                    |
| Performance   | Good for read-heavy workloads; supports indexing and caching mechanisms. | Optimized for complex queries; supports advanced indexing and concurrency. | Designed for high write throughput and horizontal scaling. | High performance in transaction-heavy environment                               |
| Compatibility | Wide language support, integration                                       | Standard SQL, rich extensions                                              | Integrates well with various stacks                        | Broad language and platform support                                             |
## Ưu điểm và nhược điểm của các loại DB

### **MySQL**

**Ưu điểm:**

1. **Dễ sử dụng:** MySQL rất dễ cài đặt và cấu hình, phù hợp cho người mới bắt đầu.
2. **Hiệu năng cao:** MySQL có thể xử lý một lượng lớn dữ liệu với hiệu suất cao.
3. **Hỗ trợ nhiều ngôn ngữ lập trình:** MySQL hỗ trợ nhiều ngôn ngữ như PHP, Python, Java, C++, v.v.
4. **Bảo mật:** MySQL cung cấp nhiều tính năng bảo mật như xác thực người dùng, mã hóa dữ liệu, và kiểm soát truy cập.

**Nhược điểm:**

1. **Hạn chế về tính năng:** So với PostgreSQL, MySQL có ít tính năng hơn trong việc hỗ trợ các yêu cầu phức tạp.
2. **Giấy phép sử dụng:** Một số tính năng cao cấp của MySQL yêu cầu giấy phép thương mại từ Oracle.

### **MariaDB (Dùng chủ yếu hiện tại để cài các ứng dụng: nextcloud, zabbix, uptime kuma, …)**

**Ưu điểm:**

1. **Mã nguồn mở hoàn toàn:** MariaDB là một fork của MySQL và hoàn toàn miễn phí và mã nguồn mở.
2. **Tính tương thích:** MariaDB hoàn toàn tương thích với MySQL, cho phép di chuyển dễ dàng giữa hai hệ thống.
3. **Hiệu năng cải thiện:** MariaDB thường có hiệu suất cao hơn và thêm nhiều tính năng so với MySQL.
4. **Bảo mật tốt hơn:** MariaDB cải tiến về bảo mật so với MySQL.

**Nhược điểm:**

1. **Ít phổ biến hơn MySQL:** Mặc dù MariaDB đang ngày càng phổ biến, nhưng vẫn ít phổ biến hơn MySQL trong một số môi trường.
2. **Hỗ trợ thương mại hạn chế:** MariaDB không có sự hỗ trợ thương mại mạnh mẽ như MySQL từ Oracle.

### **PostgreSQL**

**Ưu điểm:**

1. **Tính năng mạnh mẽ:** PostgreSQL hỗ trợ nhiều tính năng nâng cao như xử lý giao dịch, lưu trữ dữ liệu không cấu trúc, và quản lý các kiểu dữ liệu phức tạp.
2. **Khả năng mở rộng:** PostgreSQL có khả năng mở rộng tốt với các công cụ như table partitioning và replication.
3. **Bảo mật cao:** PostgreSQL cung cấp các tính năng bảo mật tiên tiến như Row-Level Security, mã hóa dữ liệu và kiểm soát truy cập mạnh mẽ.
4. **Tuân thủ tiêu chuẩn:** PostgreSQL tuân thủ nhiều tiêu chuẩn SQL, giúp dễ dàng di chuyển ứng dụng giữa các hệ quản trị cơ sở dữ liệu khác nhau.

**Nhược điểm:**

1. **Phức tạp hơn:** PostgreSQL có thể phức tạp hơn để cài đặt và quản lý so với MySQL hoặc MariaDB, đặc biệt đối với người mới.
2. **Tài liệu và cộng đồng:** Mặc dù PostgreSQL có một cộng đồng mạnh mẽ, nhưng có thể khó tìm thấy tài liệu và hỗ trợ so với MySQL.

# References

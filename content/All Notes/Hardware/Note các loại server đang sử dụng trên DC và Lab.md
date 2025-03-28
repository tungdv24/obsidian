# Note các loại server đang sử dụng trên DC và Lab
28-03-2025
Tags: #hardware 


## 1. Các loại server
Hiện server Dell thường có các loại 1U, 2U và 3U, nhưng chủ yếu các server thường sử dụng là 1U và 2U 
Trong đó 1U thì có PowerEdge R620 và 2U thì là PowerEdge R730xd hoặc PowerEdge R720xd
Các loại RAM thường dùng có các loại DDR3 và DDR4 với dung lượng là 16 hoặc 32GB
Về các dòng CPU sẽ có nhiều dòng xeon với tốc độ (GHz) khác nhau
## 2. Các loại SSD và HDD
Về SSD chủ yếu sử dụng SAMSUNG MZ7LH960HAJR (960GB) với khả năng ghi tối đa là 1500GB và số giờ chạy tối đa lên tới 2 triệu giờ. Tuy vậy chủ yếu enterprise recommend ổ đĩa chỉ nên sử dụng trong khoảng 5 năm
Để check tuổi thọ của ổ đĩa, chủ yếu để đọc các thông số ta dùng smartmonctl (có hỗ trợ cả vCenter)
## 3. Về RAID
Thông thường để cấu hình RAID ấn Ctrl + R
Các dòng card RAID hiện nay đã bỏ đi RAID 0. Tuy vậy RAID 0 cho phép khả năng đọc ghi tốt hơn vì không cần phải đọc qua card RAID. Để cấu hình RAID 0 cần có card RAID H310 mini
Giải thích về các cấu hình RAID phổ biến:
- RAID 0: Dung lượng được giữ nguyên (nếu có 2 ổ mỗi ổ 2TB thì tổng dung lượng vẫn là 2TB)
- RAID 1: Dung lượng sẽ bị chia đôi (nếu một ổ hỏng thì data sẽ không bị mất). Nếu có 3 ổ 1TB thì tổng dung lượng vẫn là 1TB nhưng có thể chịu lỗi 2 ổ
- RAID 5 sẽ có dung lượng nhỏ hơn một ổ cứng (ví dụ: sử dụng 5 ổ cứng RAID 5 sẽ có dung lượng tương đương với 4 ổ cứng)
- RAID 6 sẽ có dung lượng nhỏ hơn hai ổ cứng (ví dụ: sử dụng 5 ổ cứng RAID 6 sẽ có dung lượng tương đương với 3 ổ cứng).
- RAID 10 chỉ có thể được tạo ra khi sử dụng số lượng ổ cứng chẵn và tối thiểu là bốn ổ cứng. Dung lượng sử dụng của RAID 10 bằng một nửa tổng dung lượng của các ổ cứng sử dụng (ví dụ: sử dụng 10 ổ cứng RAID 10 sẽ có dung lượng tương đương với 5 ổ cứng).
Ngoài ra còn có khái niệm về hotspare
- Global Hot Spare: có thể thay thế bất cứ đĩa nào bị lỗi
- Dedicated Hot Spare: chỉ có thể thay thế phần RAID được chỉ định
## 4. Về iDRAC
iDRAC chủ yếu để quản lý server trực tiếp
Nếu server có license iDRAC enterprise thì sẽ có cổng riêng và có quyền truy cập vào console
Còn nếu server có license iDRAC thấp hơn thì sẽ không có console để thao tác và phải sử dụng một cổng mạng để làm kết nối iDRAC

## 5. Về hệ điều hành
Về storage sử dụng chủ yếu để host cho các bên qua nextcloud và có thể sử dụng các hệ điều hành như ubuntu hoặc centos
Khi đó sử dụng và cấu hình config RAID 5 để đảm bảo không bị lỗi đĩa


## 6. Về dây mạng
Có hai loại dây mạng quang chủ yếu sử dụng trên DC đó là
## References


# Set up IIS on Window
29-03-2025
Tags: #window 

**Mục đích chính để có thể host một folder trên một trang http và có thể download files trên server windows**

1. Turn on Windows Features và tick chọn IIS services
2. Sau đó log vào IIS services, xoá web mặc định và tạo một web site mới
3. Cấp quyền directory browsing
![[Ảnh màn hình 2025-03-29 lúc 10.46.17.png]]
4. Cấp quyền read cho everyone trong folder mà mình share
![[Ảnh màn hình 2025-03-29 lúc 10.45.30.png]]
5. Setting trỏ vào folder mà mình muốn chia sẻ
![[Ảnh màn hình 2025-03-29 lúc 10.47.32.png]]
6. Để có thể download hoặc wget các file trong folder làm theo các bước sau
![[Ảnh màn hình 2025-03-29 lúc 10.49.26.png]]
Add MIME type sau đó nhập đuôi của file và add `application/octet-stream` để file đấy có thể được download
![[Ảnh màn hình 2025-03-29 lúc 10.50.19.png]]
Sau khi cấp quyền sẽ có thể tải được tệp về máy

**Đánh giá: Về cầu trúc site khá tương tự như apache trên Linux nhưng thao tác đơn giản hơn trên interface Windows**
![[Ảnh màn hình 2025-03-29 lúc 10.50.53.png]]
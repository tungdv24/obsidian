# Cách sử dụng web vsphere
25-04-2025
Tags: #services 

# Login và Main Page

- Login vào bằng tài khoản được admin cấp
  ![[Ảnh màn hình 2025-04-25 lúc 14.34.23.png]]!
- Giao diện trang chính
  ![[Ảnh màn hình 2025-04-25 lúc 14.35.56.png]]
- Thông tin người dùng và một số service khác:
  ![[Ảnh màn hình 2025-04-25 lúc 14.56.55.png]]
-  Click vào cluster để xem các vms của user
   ![[Ảnh màn hình 2025-04-25 lúc 14.37.01.png]]
- Giao diện để xem các thông tin liên quan đến vms
  ![[Ảnh màn hình 2025-04-25 lúc 14.38.02.png]]
- Một số phím để interact với vms
	- Start: Bắt đầu máy ảo
	- Stop: Dừng máy ảo
	- Reboot: Reboot máy ảo
	- Take Snapshot (khi nhấn sẽ tạo ra một bản snapshot với tên user và ngày giờ đã tạo). User có thể quản lí và revert snapshot nếu cần. Sau khi tạo snapshot, nhấn refresh để view các grafana panel
	  
	  ![[Ảnh màn hình 2025-04-25 lúc 14.49.00.png]]
	- Delete: Xoá máy ảo (một khi đã xoá sẽ không thể quay lại)

- Người dùng có thể xem các thông tin chi tiết ở cột details

  ![[Ảnh màn hình 2025-04-25 lúc 14.54.58.png]]

# Clone VMs

- Giao diện trang clone
  ![[Ảnh màn hình 2025-04-25 lúc 15.03.55.png]]
- Một số thông tin như lượng resource còn lại trên các host và resource pools mà user có
- Bảng khai báo thông tin VMs
   ![[Ảnh màn hình 2025-04-25 lúc 15.10.32.png]]
- Dưới đây là khai báo ví dụ nếu một user muốn tạo vms và các lưu ý cần có khi tạo vm
	- Khi khai báo tên vm hãy đặt tên theo cấu trúc:
	  user-VM (tungdv-Grafana, lapbd-Amalinux,...)
	- Hãy chọn đúng template của user đó, click vào ô nhập source vms sẽ hiện ra một số template. 
	- Note: Chỉ khi clone vm từ đúng template của user mới có thể quản lí và ssh được
      ![[Ảnh màn hình 2025-04-25 lúc 15.17.07.png]]
      - Khai báo chính xác số lượng vms và số IPs (nếu số lương vm là 3 thí sẽ cần 3 IP)
      ![[Ảnh màn hình 2025-04-25 lúc 15.18.21.png]]![[Ảnh màn hình 2025-04-25 lúc 15.19.02.png]]
	  - Để tránh trùng lặp IP trong hệ thống phong lab, click vào IP Dashboard, nhập user name sẽ hiện ra các IP của mình
	  ![[Ảnh màn hình 2025-04-28 lúc 08.59.25.png]]
	- Sau khi nhập tên user được cấp sẽ có một số IP thuộc về user và một số IP chưa on. Sử dụng IP báo down để cấp cho vm được clone. 
	 ![[Ảnh màn hình 2025-04-28 lúc 09.01.24.png]]
	- Sau khi điền đầy đủ thông tin click vào Add VM sẽ thấy VM được add thêm vào cột bên phải
	 ![[Ảnh màn hình 2025-04-28 lúc 09.08.03.png]]
	 - Có thể điền thêm thông tin và Add VM tiếp để thêm vào hàng chờ
	 - Sau khi khai báo đủ số lượng VM cần thiết ấn vào nút Finalize và check lại các thông tin đã khai báo
	   ![[Ảnh màn hình 2025-04-28 lúc 09.09.37.png]]
	 - Nếu các thông tin đều chính xác (không có IP nào bị trùng), nhấn run để bắt đầu
	 ![[Ảnh màn hình 2025-04-28 lúc 09.10.20.png]]
	- Khi nhấn run sẽ hiện ra console logs, đợi đến khi có thông báo deployment complete mới được rời khỏi hoặc reload trang web
     ![[Ảnh màn hình 2025-04-28 lúc 09.13.07.png]]
     - Sau đó có thể về trang Main Page để xem VM mới được tạo
     
# VM Logs
![[Ảnh màn hình 2025-04-28 lúc 09.05.46.png]]
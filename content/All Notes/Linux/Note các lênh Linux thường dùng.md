# Note các lênh Linux thường dùng
28-03-2025
Tags: #linux 

- Các dòng lệnh quản lí user, group:
	- adduser tungdv : tạo một user tên tungdv
	- groupadd admin : tạo một group tên admin
	- usermod -aG (group) (user) : add user vào group
	- cat /etc/group : list các group đang có
	- gpasswd --delete (user) (group) : xoá một user ra khỏi group
	- userdel (user) : xoá một user
	- groupdel (group) : xoá một group
	- chown (user)
	- chown **:**(group) folder: trao quyền folder cho một group
	- groups: show user đang trong group nào
	- stat -c "%a" folder: hiện numeric quyền của folder
- Các dòng lệnh quản lí gói
	- apt-get update : update package repository
	- apt-get check : check các gói bị lỗi
	- apt-get upgrade: upgrade tất các các package đã tải
	- apt-get upgrade (package) : upgrade một package cụ thể
	- apt-get dist-upgrade: upgrade repo
	- apt-get install (name) : install package
	- apt-get install (name1) (name2) 
	- apt-get remove (name) :  xoá package
	- apt-get purge (name) : xoá package và các file config liên quan
	- apt-get clean : removes everything except the local file from **/var/cache/apt/archives/** and **/var/cache/apt/archives/partial/**)
- Các dòng quản lí thư mục:
	- chgrp (group) (file) : change group owner của một file
	- chown (user) (file) : change user của một file
	- chmod xxx (file) :
		- Ý nghĩa của các số:
			- 0: no access
			- 1: execute
			- 2: write
			- 4: read
		- Ý nghĩa các vị trí trong số:
			- Vị trí 1 là của chủ file
			- Vị trí 2 là của group
			- Vị trí 3 là những người còn lại
- Lệnh tìm kiếm trong linux:
	- find (dir) -name "\*.txt" : tìm trong thư mục các file có định dạng
	- locate -i : tìm qua db (sudo updatedb)
	- grep : chủ yếu tìm word
	- tail -n 5 (file) : hiển thị 5 dòng cuối
	- head -n 5 (file) : hiển thị 5 dòng đầu
- Lệnh check storage:
	- df : hiện thị dung lượng còn trống
	- fdisk -l : bảng phân vùng của các ổ đĩa
	- lsblk : liệt kê các block device đang được sử dụng
	- du -h : liệt kê file đang sử dụng bao nhiêu dung lượng
	- echo "- - -" | tee /sys/class/scsi_host/host*/scan : lệnh scan lại các disk trên ubuntu

- Lệnh quản lí file: (các lệnh đã biết: pwd, cd, ls, cat, cp, mv, mkdir, rmdir, touch)
	- stat (file) : Show các thông tin về file
	- find (dir) -name "\*.txt" : tìm trong thư mục các file có định dạng

- Lệnh về networking (các lệnh đã biết: ip a, ping, traceroute,)
	- ifconfig -a : show IP (require net-tools)
	- ifconfig eth0 192.168.56.5 netmask 255.255.255.0 : gắn IP cho eth0
	- ifconfig up eth0
	- ifconfig down eth0
	- mtr google.com : ping và traceroute kết hợp
	- sudo ss -tulpn | grep LISTEN : check port listening
- Lệnh chuyển data từ 2 máy ảo:
	- rsync -avz -e "ssh -p 2112" file ubuntu@192.168.10.147:(dest )

- Một số lệnh thao tác trên postgresql
	- \q : quit
	- \c (DB) : chọn DB để log vào
	- \l+ : list các DB và dung lượng
	- SELECT pg_size_pretty(pg_database_size('(db name)')); : check dung lượng của một db
	- pg_restore -U postgres -d large_db rcv/large_db_dump_cluster.sql : restore một db bằng user postgre
	- pg_dump -U post

# References
- https://systemhow.com/c/linux-command
- https://www.nexcess.net/help/what-is-chmod/
- https://systemhow.com/linux-command/10-command-linux-it-duoc-biet-den-phan-3-438

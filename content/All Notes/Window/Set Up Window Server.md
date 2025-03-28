2025-03-05 16:09
Tags: #window 

Yêu cầu:
- Setup vm-tools 
- Change port remote 10089
- Update firewall allow ip office 
- Add user + gen passwd

1. VMTools: Install trực tiếp trên ESXi
2. Change port remote 10089 + Firewall allow IP văn phòng
	- Window + R
	- **regedit**
	- **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp**
	- Tìm kiếm key **PortNumber** trong thư mục **RDP-Tcp** rồi nhấn đúp chuột vào nó
	- Chuyển thành port 10089
	- Restart
	- Allow firewall:
		- Window + R
		- **wf.msc**
		- Inbounds rule + New Rule
		- Allow IP văn phòng
		- Save
	- Test: IP:10089


# References
- https://quantrimang.com/cong-nghe/thay-doi-cong-listening-remote-desktop-connection-66451
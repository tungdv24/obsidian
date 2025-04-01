# Ansible x AWX
01-04-2025
Tags: #services 

## [Ansible Git](https://github.com/tungdv24/Ansible)
# Cấu trúc trên Git Ansible
![[Pasted image 20250401093338.png]]
### Trong đó:
- File yml để chạy ansible
- File hosts để chứa các host IP
# Cách sử dụng AWX Tower
- Mục template để gắn file yml trên git và chạy

![[Pasted image 20250401093146.png]]
- Mục project dùng để gắn link git

![[Pasted image 20250401093208.png]]
- Mục inventory dùng để store các host

![[Pasted image 20250401093234.png]]
![[Pasted image 20250401093243.png]]
 - Inventory có thể nhập được từ link git được add
 
![[Pasted image 20250401093254.png]]
- Mục credential dùng để store ssh private key vào server
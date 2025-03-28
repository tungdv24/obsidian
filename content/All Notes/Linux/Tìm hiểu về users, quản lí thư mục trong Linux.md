# Tìm hiểu về users, quản lí thư mục trong Linux
28-03-2025
Tags: #linux 

## Cách đổi chủ, quyền của một folder

- chown (user) folder: change user owner của một folder
- chown :(group) folder : change group owner của một folder
- chmod 777 folder
- #### Symbolic method
```plaintext
chmod WhoWhatWhich file | directory
```

	user, group, other, all = u,g,o,a
	add, remove, set exact =  +, -, =


## Tìm hiểu về các số chmod
```plaintext
chmod (7)777 file | directory
```
### Trong đó:
Số đầu tiên dùng để set một số quyền như
	setuid = 4
	setgid = 2
	sticky = 1
	nochanges = 0
Các số còn lại tượng trung lần lượt cho: **owner**, **group** và **other users**
	 r (read) = 4
	 w (write) = 2
	 x (execute) = 1
	 nopermission = 0
### Giải thích thêm về **SUID**, **SGID** và **sticky bit**
#### Sticky bit
Cho phép chỉ có người chủ sở hữu file có quyền đổi tên hoặc xoá file 
# References
- https://linuxize.com/post/chmod-command-in-linux/#:~:text=Give%20the%20file's%20owner%20read,given%20directory%3A%20chmod%201777%20dirname
- https://vietdata.com.vn/chmod-va-sticky-bits-tren-linux/
- https://www.redhat.com/en/blog/suid-sgid-sticky-bit

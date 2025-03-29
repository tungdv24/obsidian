# Test Backup trên Postgresql
29-03-2025
Tags: #services 

## TH1: 1 DB + Dump vào Server cũ

## TH2: 1 DB + Dump vào Server mới
## TH3: Dựng DB theo Cluster với 3 node sync live


**ENV: Lab + 1GB network + chung một dải IP + Data sql 5GB**

### Cấu hình: 

**Server DB: 2 CPU 2GB RAM

**Server Backup: 2 CPU 4GB RAM**

**Quy trình back up: dump ra sau đó copy file sang một server khác sau đó drop table và tìm các khôi phục. Tính thời gian từ lúc drop table cho đến khi khôi phục lại data.**

### TH1: 1 DB + Dump vào Server cũ

Một DB được tạo với tổng dung lượng là 5GB

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeucXbP4_wngaf7A3xHoWiEbSfh8Ps9I-6IYfprSQkvyCtnwTf-bdmAxjHDYXO3RC7EAmk1tGK65x2LhdaYYcCs3wA1nUqOt1FsYDFEy29PUUtl3J04t4kE-hus-_0znr2cZKDY?key=aS7ieBQUyZ7ZSsb-YSRyUkYB)

Sau khi sử dụng pg_dump ta sẽ được một file có tổng dung lượng là 1.8GB

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcnd3MCE06aAwBslyC-EzcITcr0xbs-2BinyeH7-NuoJ7wpUB0fft0uQ245FyoCYw7EwPiEgt6nDTTXrfCD9PUJXYeCdOcZotWMS_ZbypTnvGPAxko5GrfZnfRNv-TZR9PNxvAu9g?key=aS7ieBQUyZ7ZSsb-YSRyUkYB)

Sau đó sync to back-up server

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcvZCIki1yvalQso7clm53nOU0vg8kDWxKccP-WYfBwzYMscZTD4CWqUoZSa5uKCiitpg1QmzN_g36WLohNShx5Q0HY87vcpxuaF3dYXAc1ey4eZzyel1McmE6viROfRg-tMxwp1g?key=aS7ieBQUyZ7ZSsb-YSRyUkYB)

Sau đó ta sẽ drop table ở server cũ
Sau đó copy lại db từ server backup sang server vừa bị drop

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe0GwK6VLMF6fKDu_h-sUEDzPLBDB-dp5Lt77rP1YbdRDdfaGOYhnkkqOpCj7KmPQE4rcN_QbT5D-3ue43Ec5qS0lPIV5YdQbuCzCaHNrsowp8Fsd46CS6AnvbP8Uzhv0pAhIsYRg?key=aS7ieBQUyZ7ZSsb-YSRyUkYB)
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcX8WQBN242nqQAA4oN2cIrOIDQeMtxHvLNpbwzmx4d23a5t8rcE4UZAwFkxKSeRZDvWL-UAm8mA6E_amI_uqK1K3ChCY9vZ_izdLsZ9OKE469oN1jIUwDcrwPvhD9QsezpP40T?key=aS7ieBQUyZ7ZSsb-YSRyUkYB)**
#### Tổng thời gian off và khôi phục : 5 mins

### TH2: 1 DB + Dump vào Server mới
Tạo một server mới
Cài đặt DB only (postgresql)
Tạo bảng có cấu trúc giống như cũ

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfH_OA2fNOWprIT0aqUimD1a27AO5ggYJaFyvYnngezcn3cfD9FAicWs4FVQwa7FXbfjOKtLbLeiBjOkQya04AfSF89cvX3_JFm2VxoCmtPlYYqk89xSgFAE9VeUjBpj7rmaX7btA?key=aS7ieBQUyZ7ZSsb-YSRyUkYB)
Sync data từ backup sang

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfWeCtUb5ZnmdFYN6JArYVGKW57bGF50mzd4d_sbk4WJfLFygwlyls1EV8M0ArVyDhNyaflB7Y8amXD9a9f-3t3hI3NeJyIoNGMe3qRA75itjA4r61oO4yn1-n8y0d5r76J1Yru1Q?key=aS7ieBQUyZ7ZSsb-YSRyUkYB)
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcTOwKJzL_LbXAJzUGw43It7r4xOylDejZOu5RA_9jduwjDNJLrv9H3wsB_bMcL4oVfnp8EXSBHl8oT-f4h1SvOffFnOvmYLWLGKwvhQE20dtLtkjjNmNIOKRw8Z_lTHsh8PLwk?key=aS7ieBQUyZ7ZSsb-YSRyUkYB)

#### Tổng thời gian dựng lại host và import DB : 10 mins

### TH3: Dựng DB theo Cluster với 3 node sync live
### Cấu hình
**Server DB (3 nodes): 2 CPU 2GB RAM
Server Backup: 2 CPU 4GB RAM

Sau khi nén db 6G được file sql 2GB sau đó sync qua backup server

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfgQLEJCAD3thHZrss1Vokauvw9eQevgc7zOdG_TGszZwKbyRAq_8DQjle5GJMgtMeZkcH1cCiDwIoN_NXBCQcTp2U4jvobiaNwtw_OuZbMGJ5pjShPsWHxsEoag9LYXJsJPw3cfQ?key=aS7ieBQUyZ7ZSsb-YSRyUkYB)

Drop table ở server chính
Sau đó sync file backup từ server back up đến node chính
Sau đó sử dụng pg_restorre để khôi phục lại

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdDOcrXd2_eqzcf3jLiG6UK-j3VtnVGCVRPFwwdfAz7LIBuHP4KLMCr9muAGOkcPtWhYHsbKaGmbr84_V654RgAMq3FEq_NLLjCqcIj0sGYkIZ8MHAM8pUWuNGyLC_1TGpODjOGPQ?key=aS7ieBQUyZ7ZSsb-YSRyUkYB)
#### Tổng thời gian để cả 3 server có cùng lượng data backup: 13 mins

**Note: Test chỉ apply với mỗi database chưa có kết nối với services. Trong thực tế, thời gian để service chạy lại có thể lâu hơn so với chỉ restore lại back up sql data.**


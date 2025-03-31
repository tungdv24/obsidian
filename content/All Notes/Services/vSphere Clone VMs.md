# vSphere Clone VMs
28-03-2025
Tags: #services 
## [Link git](https://github.com/tungdv24/vsphere-clone-vms)
## Giải thích code

Code được sử dụng trong mục đích tạo dựng hàng loạt VMs trong cùng một thời điểm trên vSphere. VMs sẽ được clone từ một template có sẵn trong host. VMs bắt buộc phải là Linux và sẽ được cấu hình hostname, IP theo thông tin khai báo.
## Cách sử dụng code 

- Đổi username password đến vSphere trong file vsphere_credentials.json
- Đổi VM Templates trong file templates/index.html line 53 
```html
<div class="mb-3">

	<label for="SourceVM" class="form-label">Source VM</label>

	<select class="form-select" id="SourceVM" name="SourceVM" required>

		<option value="Z-Template-Ubuntu22">Z-Template-Ubuntu22</option>

		<option value="Z-Template-Cent-9">Z-Template-Cent-9</option>

		<option value="tungdv-Pool-Temp">tungdv-Pool-Temp</option>

</select>
```
- Edit login users trong file users.json
### Chạy local
```console
git clone https://github.com/tungdv24/vsphere-clone-vms.git
apt install python3
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python3 app.py
```
### Chạy trên Docker
```console
docker build -t vsphere:latest .
docker run -v /app:/usr/share/app vsphere:latest
```

### Truy cập vào IP:5000 và login bằng user đã cấu hình trong users.json

## Một số lưu ý khi sử dụng

- Khi cấu hình từ 2 vms trở lên IP cần được cung cấp vào form đầy đủ. VD: (192.168.10.10,192.168.10.20)
- Với cấu hình VMs trong Resource Pools, Resource Pools sẽ được lấy từ VMs được Cloned nên hãy move VMs được clone vào trong Resource Pools nếu muốn

### Dự kiến update trong thời gian tới
- Versioning: Update thêm versioning giúp lưu trữ lịch sử VMs đã tạo
- Turn Off,On hoặc xoá các VMs đã tạo
- Support thêm các phần ảo hoá khác như Proxmox, Openstack
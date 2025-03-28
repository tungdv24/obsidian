2025-02-19 10:15
Tags: #virtualization 

# Proxmox
## 1. Giới thiệu về Proxmox

 **Proxmox** là giải pháp ảo hóa máy chủ mã nguồn mở, hỗ trợ hai công nghệ ảo hóa **OpenVZ** và **KVM**, cho phép triển khai và quản lý nhiều máy ảo trên một máy chủ vật lý. Được phát triển bởi **Proxmox Server Solutions**, nền tảng này cung cấp giao diện web dễ sử dụng để người quản trị có thể giám sát và điều khiển hệ thống. **Proxmox** hoàn toàn miễn phí và được phát hành dưới giấy phép GNU General Public License, giúp tăng hiệu quả sử dụng tài nguyên và linh hoạt trong việc triển khai các hệ điều hành khác nhau trên cùng một máy chủ.

Một số tính năng nổi bật của **Proxmox** có thể kể đến như
	- **Công nghệ ảo hóa KVM**
	    KVM là một giải pháp ảo hóa toàn diện cho các hệ điều hành khách, cho phép chạy nhiều hệ điều hành độc lập trên cùng một máy chủ vật lý. KVM chuyển đổi nhân Linux thành một hypervisor, hỗ trợ chạy các máy ảo với phần cứng ảo hóa, giúp tối ưu hóa hiệu suất và khả năng mở rộng. Các máy ảo sử dụng KVM có thể cài đặt bất kỳ hệ điều hành nào, bao gồm Linux, Windows, hoặc các hệ điều hành khác, và được cấp phát tài nguyên riêng biệt như CPU, RAM và ổ đĩa.
	- **Giao diện web quản lý đơn giản**
		![[Anh_man_hinh_2025-02-12_luc_09.06.34.png]]
	- **Hỗ trợ phần cứng lớn**
		Proxmox, được xây dựng trên hệ điều hành Linux, tận dụng sự ổn định và linh hoạt của Linux để cung cấp giao diện web dễ sử dụng. Với các công cụ quản lý tài nguyên tích hợp sẵn giúp tối ưu hiệu suất hệ thống, làm cho Proxmox trở thành giải pháp ảo hóa vừa mạnh mẽ, vừa dễ thao tác cho nguời dùng.
	- **Có thể mở rộng đến 32 node**

**Proxmox** sở hữu các công cụ quản lí đa dụng đó là **Web-based UI** và **Command Line Interface (CLI)**
- **Web-based UI**![[Anh_man_hinh_2025-02-12_luc_09.06.34.png]]
- **CLI** ![[Anh_man_hinh_2025-02-12_luc_09.07.47.png]]
Ngoài ra **Proxmox** cũng sở hữu tính năng cho phép quản lý truy cập cluster từ bất kì node nào với cùng một interface. Điều này giảm thiểu việc yêu cầu phải có một server hoặc máy ảo quản trị riêng biệt như vSphere

Một số tính năng nổi bật của **Proxmox**
- **Live migration**
Tương tự như vSphere có vMotion, **Proxmox** cũng sở hữu tính năng **Live Migration**. Dưới đây là bảng so sánh giữa hai tính năng quan trọng trong việc vận  hành hệ thống
- **HA Cluster**
**Proxmox** cũng sở hữu tính năng **HA (High Availability)** cho phép tự động chuyển máy ảo từ các node gặp vấn đề sang node hoạt động bình thường. Dưới đây là bảng so sánh HA Cluster Giữa VSphere Và Proxmox

| Tiêu chí                         | vMotion (vSphere)                                                                        | Live Migration (Proxmox)                                                     |
| -------------------------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| 1. Cách hoạt động                | Hỗ trợ Live Migration mà không cần shared storage, tối ưu với vSphere DRS.               | Cần shared storage để tối ưu, nếu không phải sao chép toàn bộ đĩa ảo.        |
| 2. Hiệu suất và yêu cầu hệ thống | Tối ưu hóa với memory compression và page tracing, giảm downtime.                        | Hiệu suất phụ thuộc vào băng thông mạng và tốc độ lưu trữ.                   |
| 3. Tích hợp                      | Tích hợp sâu với vCenter, hỗ trợ cross-vCenter migration. Có tự động cân bằng tài nguyên | Hoạt động tốt trong Proxmox Cluster nhưng thiếu tự động cân bằng tài nguyên. |

| Tiêu chí                             | vSphere HA                                                                               | Proxmox HA                                                                                |
| ------------------------------------ | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| 1. Cơ chế hoạt động                  | Tích hợp với vCenter, sử dụng heartbeats để phát hiện lỗi máy chủ và tự động restart VM. | Sử dụng Proxmox HA Manager và watchdog để giám sát node, kích hoạt failover khi có sự cố. |
| 2. Khả năng tự động khôi phục máy ảo | Tự động phát hiện máy chủ lỗi và khởi động lại VM trên host khác trong cluster.          | Tự động chuyển VM/container sang node khác khi phát hiện lỗi.                             |
| 3. Cấu hình và quản lý               | Dễ dàng cấu hình thông qua vCenter, có giao diện trực quan và tích hợp với DRS.          | Cấu hình qua giao diện web hoặc CLI, phức tạp hơn so với vSphere.                         |
| 4. Yêu cầu phần cứng                 | Yêu cầu shared storage, vCenter Server và các license tương ứng.                         | Không bắt buộc shared storage nhưng được khuyến nghị để tối ưu hiệu suất.                 |

**Proxmox HA** không bắt buộc yêu cầu share storage vì có thể sử dụng CephFS và GlusterFS thay thế. Cơ chế hoạt động của cách này là sao chép và phân phối dữ liệu trên nhiều node trong cluster, giúp đảm bảo rằng nếu một node bị lỗi, dữ liệu vẫn có thể được truy cập từ các node khác mà không cần có một kho lưu trữ trung tâm.

Ngoài ra, **Proxmox** cũng sở hữu cơ chế sao chép dữ liệu giữa các node như ZFS Replication. **Proxmox** có thể thiết lập ZFS replication định kì và đảm bảo dữ liệu được sao chép từ node này sang node khác. Khi một node bị lỗi, **Proxmox** có thể khởi động lại máy ảo từ bản sao lưu gần nhất trên một node khác.

Tuy vậy, **Proxmox** vẫn được khuyến nghị sử dụng shared storage vì một số đặc điểm:
- **Giảm thời gian downtime**: Khi có shared storage, các VM không cần sao chép lại toàn bộ dữ liệu từ node bị lỗi sang node mới, giúp quá trình failover diễn ra nhanh hơn.
- **Hạn chế mất dữ liệu**: Khi không có shared storage, việc đồng bộ dữ liệu giữa các node phải thông qua các phương pháp như replication, có thể làm tăng độ trễ và nguy cơ mất dữ liệu nếu không cấu hình đúng.
- **Cải thiện hiệu suất hệ thống**: Shared storage giúp các node trong cluster truy cập nhanh vào dữ liệu mà không cần thực hiện quá trình đồng bộ nặng nề qua mạng.
## 2. Giới thiệu thành phần trong Proxmox

Về **Network**, Proxmox sử dụng **Linux Bridge**, **VLAN**, và **SDN (Software-Defined Networking)** để quản lý mạng ảo hóa.
- **Linux Bridge**: Hoạt động như một switch ảo, kết nối các VM và container với nhau hoặc với mạng vật lý.
- **VLAN (802.1Q)**: Cho phép phân tách mạng ảo trên cùng một kết nối vật lý, giúp quản lý và bảo mật tốt hơn.
- **SDN (Software-Defined Networking)**: Được hỗ trợ trong các phiên bản mới, giúp quản lý mạng phức tạp trong môi trường lớn.
Người dùng có thể tự tạo thêm các linux bridge để cấu hình cho máy ảo hoặc cho cụm Ceph có đường mạng riêng biệt để đảm bảo hiệu suất.

Về **Storage**, có rất nhiều mô hình lưu trữ trên **Proxmox**
- Một số các loại **network storage** có thể kể đến như:
	- NFS Share
	- iSCSI
    - NFS       
    - SMB/CIFS
	- Ceph RBD
    - GlusterFS
    - CephFS
- Một số loại **local storage** như: LVM Group, Directory và ZFS
- **Software-Defined Storage** (SDS) và **Ceph**

**Ceph** là một hệ thống tệp và lưu trữ đối tượng phân tán nguồn mở được thiết kế để cung cấp hiệu suất, độ tin cậy và khả năng mở rộng cao
**Ceph** là một trong những giải pháp SDS mạnh mẽ nhất, cung cấp khả năng lưu trữ phân tán, tự động mở rộng, và đảm bảo tính sẵn sàng cao. Ceph được tích hợp trực tiếp trong Proxmox VE, giúp triển khai lưu trữ phân tán dễ dàng và hiệu quả cho các hệ thống ảo hóa.

**Ceph** hoạt động dựa trên 3 thành phần chính
- **OSD (Object Storage Daemon)**: Quản lý dữ liệu và thực hiện các tác vụ đọc/ghi. Mỗi OSD tương ứng với một ổ đĩa vật lý hoặc một phân vùng.
- **MON (Monitor Daemon)**: Theo dõi trạng thái của cluster, duy trì thông tin cấu hình và điều phối hoạt động của các OSD.
- **MGR (Manager Daemon)**: Cung cấp các chức năng giám sát và báo cáo về hiệu suất, sức khỏe của cluster Ceph.
Một số **lợi ích** khi tích hợp **Ceph** vào trong **Proxmox**
- **Back-up dữ liệu**: Ceph tự động sao chép dữ liệu giữa các OSD, đảm bảo tính toàn vẹn và khả năng chịu lỗi (nếu một OSD hoặc một node bị lỗi, dữ liệu vẫn có thể được phục hồi từ các OSD khác).
- **Giao diện tích hợp trong Proxmox:** Giao diện quản lý trực quan giúp dễ dàng tạo và quản lý cluster Ceph mà không cần cài đặt thủ công phức tạp.
- **Hiệu suất cao, dễ mở rộng**: Hệ thống có thể mở rộng bằng cách thêm các OSD hoặc node mới mà không ảnh hưởng đến hoạt động hiện tại.
- **Đa dạng phương thức lưu trữ:**
    - **Ceph RBD (RADOS Block Device)**: Dùng để lưu trữ ổ đĩa ảo cho máy ảo Proxmox.
    - **CephFS (Ceph File System)**: Dùng để chia sẻ dữ liệu qua giao thức file system.
    - **Ceph Object Storage (S3/Swift)**: Hỗ trợ lưu trữ đối tượng tương tự như Amazon S3.

## 3 . Cách cài đặt Proxmox

**Bước 1**: Tải xuống Proxmox VE

**Bước 2**: Khi tải xuống hoàn tất, tạo một file image có thể được ghi vào đĩa DVD hoặc USB boot, khởi động hệ thống của bạn bằng tệp này. Khi bạn khởi động hệ thống của bạn từ Proxmox ISO, sẽ hiện ra một giao diện, lúc này, hãy chọn “Cài đặt Proxmox VE” từ trên menu này và nhấn Enter.

**Bước 3**: Đọc vào chấp nhận thỏa thuận cấp phép.

**Bước 4**: Đây là bước quan trọng nhất - chọn đĩa cứng - nơi Proxmox sẽ được cài đặt. Quá trình cài đặt sẽ định dạng đĩa mà hệ điều hành mới sẽ được cài đặt vào.

**Note**:
Một số lưu ý trong việc cấu hình ổ cứng cho Ceph. Việc cấu hình RAID khi dựng cụm Ceph là không cần thiết. Ceph có tính năng tự replication để tạo bản sao trên các node để đảm bảo dữ liệu. Ngoài ra sử dụng RAID có thể làm chậm khả năng đọc ghi của ổ đĩa trong khi Ceph yêu cầu tốc độ đọc ghi cao. Khi sử dụng RAID, Ceph không thể quản lý từng ổ đĩa riêng biệt, khiến việc xử lý lỗi kém hiệu quả hơn.

Cách tối ưu ổ đĩa cho Ceph cho server là:
- Cấu hình riêng một ổ sda cho Ceph cài OS, tránh ảnh hưởng đến hiệu suất lưu trữ Ceph
- Thay vì tổng hợp thành một ổ đĩa lớn, hãy cấu hình đĩa đơn (sdb, sdc ,sdd) cho Ceph. Điều này giúp Ceph quản lý trực tiếp từng ổ đĩa, tối ưu hiệu suất và khả năng chịu lỗi. 

**Bước 5**: Chọn vị trí địa lý, múi giờ và bố cục bàn phím.
    
**Bước 6**: Điền Email và mật khẩu quản trị viên cài đặt mới của bạn.
    
**Bước 7**: Trước tiên, hãy đảm bảo rằng bạn cung cấp cấu hình mạng chính xác tại bước này, bạn sẽ dùng nó để truy cập vào giao diện web của bản cài đặt Proxmox.

**Bước 8**: Quá trình cài đặt bắt đầu.

**Bước 9**: Khởi động lại hệ thống sau khi hoàn tất cài đặt.

## 4. Tổng kết

Proxmox là một **giải pháp ảo hóa toàn diện, linh hoạt, dễ sử dụng và tiết kiệm chi phí** khi so sánh với các nền tảng thương mại như VMware vSphere. Với các tính năng như **hỗ trợ ảo hóa KVM/LXC, tích hợp Ceph SDS, Live Migration và HA Cluster**, Proxmox phù hợp với đa dạng môi trường triển khai—từ người dùng cá nhân, doanh nghiệp vừa và nhỏ cho đến các hệ thống quy mô lớn.

Tuy nhiên, **đối với những hệ thống cần tính năng tự động hóa mạnh mẽ và giao diện quản lý đơn giản**, VMware vSphere vẫn là lựa chọn phù hợp hơn. Mặt khác, nếu **bạn cần một nền tảng ảo hóa mã nguồn mở, tiết kiệm chi phí và có khả năng tùy biến cao**, Proxmox chính là giải pháp đáng cân nhắc.


## References
- https://tinhte.vn/thread/tim-hieu-ve-nen-tang-ao-hoa-voi-proxmox.3684573/
- https://maychusaigon.vn/proxmox-la-gi/
- https://cloudviet.com.vn/proxmox-la-gi-va-huong-dan-cai-dat-proxmox-ve/
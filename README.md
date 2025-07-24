# Thiết kế hệ thống mạng cho doanh nghiệp

## Khảo sát hiện trạng  
Công ty có hợp đồng triển khai mạng cho Viện Giáo Dục Quốc Tế HUFLIT cụ thể như sau:  
+ Nhân sự: 400 sinh viên, 30 giảng viên, 20 nhân viên marketing và giáo vụ, 5 quản giáo cao cấp bao gồm giám đốc chương trình và quản lí đào tạo, 3 nhân viên quản trị mạng.
+ Thiết bị: 60 máy tính cho phòng Lab, 35 máy tính cho nhân viên, 3 máy in, 3 Server.
+ Tòa nhà: gồm 3 tầng, máy tính và máy in đặt tầng trệt, ngoại trừ phòng thực hành IT: 1 phòng ở tầng 1, 1 phòng khác tầng 2 và tầng 3
+ Thiết bị mạng: 5 Switch Layer 2, 1 Switch Layer 3, 1 Firewall Fortinet, 3 Access Point.

## Sơ đồ hệ thống
### Sơ đồ logic
![Sơ đồ logic](image/Sodologic.png)
### Sơ đồ vật lý
![Sơ đồ Vật Lý](image/SodoVatly.png)
## Bảng phân hoạch IP
### 🗂️ Thông tin mạng theo tầng và phòng

| Tầng  | Tên phòng   | Số lượng máy | Dãy IP        | Subnet Mask       | Gateway        | VLAN |
|-------|-------------|--------------|---------------|--------------------|----------------|------|
| Trệt  | Server      | 3            | 192.168.1.0    | 255.255.255.248    | 192.168.1.1    | 2    |
|       | Giảng viên  | 30           | 192.168.2.0    | 255.255.255.224    | 192.168.2.1    | 3    |
|       | Marketing   | 20           | 192.168.3.0    | 255.255.255.224    | 192.168.3.1    | 4    |
|       | Quản lí     | 5            | 192.168.4.0    | 255.255.255.248    | 192.168.4.1    | 5    |
| 1     | Lab         | 20           | 192.168.5.0    | 255.255.255.192    | 192.168.5.1    | 6    |
| 2     | Lab         | 20           | 192.168.5.0    | 255.255.255.192    | 192.168.5.1    | 6    |
| 3     | Lab         | 20           | 192.168.5.0    | 255.255.255.192    | 192.168.5.1    | 6    |  
  
Mạng nối giữa Switch và Firewall: 192.168.200.0/24  
Mạng nối giữa Firewall và Router: 192.168.190.0/24  
  
## Cấu hình các dịch vụ
### Cấu hình DHCP
Switch Layer 3  
Vai trò quan trọng trong việc điều tiết hệ thống mạng nội bộ.  
Sử dụng VTP (VLAN Trunking Protocol cho phép các VLAN được Trunk qua các Switch Layer 2 nhờ vào các port tương ứng)  
![Config SWL3](image/configSWL3.png)  
Cổng E0/0 nối với Firewall  
![e0/0](image/e00_to_FW.png)  
Các cổng từ E0/1-3 và E1/0-1 cấu hình trunking các VLAN sang Switch Layer 2  
![Trunk config](image/SWL2_1.png)  
![Trunk config](image/SWL2_2.png)  
Cấu hình các VLAN (các VLAN sẽ được cấp DHCP nhờ vào địa chỉ 192.168.1.2 – địa chỉ của DHCP Server)  
![Vlans config](image/VLANs_config.png)  
IP Routing  
![IP routing](image/IP_Routing.png)  
Switch Layer 2  
Các Switch Layer 2 được tiến hành cấu hình tương tự nhau để gán các port access vào từng VLAN tương ứng.  
Nhận các VLAN từ Switch Layer 3 dựa vào VTP  
![](image/SWL2_3.png)  
![](image/SWL2_4.png)  
![](image/SWL2_5.png)  
![](image/SWL2_6.png)  
![](image/SWL2_7.png)  
Cổng nối với Switch Layer 3 sẽ là mode trunking để nhiều VLAN có thể đi qua  
![](image/SWL2_8.png)  
![](image/SWL2_9.png)  
![](image/SWL2_10.png)  
![](image/SWL2_11.png)  
![](image/SWL2_12.png)  
Các cổng nối với End-Device sẽ là mode access và access vào từng VLAN tương ứng.  
![](image/modeaccess_1.png)  
![](image/modeaccess_2.png)  
![](image/modeaccess_3.png)  
![](image/modeaccess_4.png)  
![](image/modeaccess_5.png)  
Cấu hình trên Server DHCP  
*Không tạo scope cho VLAN 2  
Nhập tên scope  
![](image/DHCP_Server_1.png)  
Nhập dãy IP sẽ cấp phát  
![](image/DHCP_Server_2.png)  
Nhập dãy IP không cấp phát, ngoại trừ  
![](image/DHCP_Server_3.png)  
Nhập thời gian được sử dụng IP  
![](image/DHCP_Server_4.png)  
Nhập vào Gateway  
![](image/DHCP_Server_5.png)  
Nhập dns
![](image/DHCP_Server_6.png)  
Chọn Yes, sau đó bấm Next để kích hoạt scope  
![](image/DHCP_Server_7.png)  
Làm với các scope tương tự  
![](image/DHCP_Server_8.png)  
  
### Cấu hình Routing
E0/0 sử dụng IP DHCP do ISP cấp  
![](image/Router_ISP.png)  
E0/1 nối với mạng nội bộ  
![](image/Router_noibo.png)  
NAT theo port  
![](image/Router_NATPORT.png)  
ACL  
![](image/Router_ACL.png)  
IP Routing  
![](image/Router_IPROUTING.png)  
  
### Cấu hình Firewall


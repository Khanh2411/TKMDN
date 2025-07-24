# Thiết kế hệ thống mạng cho doanh nghiệp

## Khảo sát hiện trạng  
Công ty có hợp đồng triển khai mạng cho Viện Giáo Dục Quốc Tế HUFLIT cụ thể như sau:  
+ Nhân sự: 400 sinh viên, 30 giảng viên, 20 nhân viên marketing và giáo vụ, 5 quản giáo cao cấp bao gồm giám đốc chương trình và quản lí đào tạo, 3 nhân viên quản trị mạng.
+ Thiết bị: 60 máy tính cho phòng Lab, 35 máy tính cho nhân viên, 3 máy in, 3 Server.
+ Tòa nhà: gồm 3 tầng, máy tính và máy in đặt tầng trệt, ngoại trừ phòng thực hành IT: 1 phòng ở tầng 1, 1 phòng khác tầng 2 và tầng 3
+ Thiết bị mạng: 5 Switch Layer 2, 1 Switch Layer 3, 1 Firewall Fortinet, 3 Access Point.

## Sơ đồ hệ thống
### Sơ đồ logic

### Sơ đồ vật lý

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
  
## Cấu hình các dịch vụ


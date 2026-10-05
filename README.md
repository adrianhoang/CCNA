[Host Name]
[PassWord Console]
    [Gỡ bỏ PassWord Console]
[PassWord Enable]
    [Gỡ bỏ PassWord Enable]
[Mã hóa các PassWord]
    [Bỏ mã hóa các PassWord]
[IP Address]
[Loopback Address]
[CDP]
    [NO CDP để bảo mật]
[LLDP]
[SSH Key RSA]
    [telnet | ssh]
[Backup - Running or Startup-config]
[Backup - Flash Memory]
[Routing]
    [Route]
    [Default Route]
    [AD]
    [IP SLA]
[Trace Route]
    [Tracert]
    [Traceroute]
[MAC]
[VLAN]
    [NO VLAN]
    [TRUNK]
    [VTP]
    [Access Port]
    [Spanning-tree portfast]
[Router on a Stick (ROAS) - Định tuyến giữa nhiều VLAN với nhau - Chỉ một cổng vật lý, nhưng bên Router chia thành nhiều sub-interface.]
[Switch Layer 3 SVI]
[DHCP]
    [IP Helper Address]
[STP (Spanning Tree Protocol)]
    [STP - Chỉnh Priority - Chỉnh Cost]
    [VTP - STP(Root Primary/Root Secondary)]
    [VTP - STP(Root Primary/Root Secondary) - ROAS]
[EtherChannel]
    [Gỡ bỏ EtherChannel]
    [LACP]
    [PAgP - Độc quyền của Cisco]
[HSRP]
    [Shutdown R1 -> R2 chiếm quyền làm Active]
    [Track]
[Port Security]
    [TH1: PC1 kết nối thành công với Port]
    [TH2: PC1 kết nối không thành công với Port]
    [TH3: Router kết nối thành công với Port]
    [TH4: Router kết nối không thành công với Port]
[BPDU Gurad]
[DHCP Snooping]
[DTP - VLAN Hooping]
[IP Source Guard]
[DAI (Dymanic ARP Inspection)]
[SPAN (Switch Port Analyzer)]
[AAA]
    [Bật cơ chế xác thực AAA]
    [Telnet]
    [Authorization (Phân quyền)]
    [Accounting (Ghi nhận và giám sát)]
[Static Router - Giao thức định tuyến tĩnh]
    [Static Router - AD (Administrative Distance)]
    [Static Router - AD - IP SLA]
[Dynamic Router - RIP(120) - NAT - ACL]
    [CẤU HÌNH NÂNG CAO CHO RIPv2]
[Dynamic Router - OSPF(110)]
    [Dynamic Router - OSPF(110) - DR - BDR]
    [Dynamic Router - OSPF(110) - Chỉnh Cost]
[ACL (Access Control List) - STANDARD ACL]
    [ACL - EXTENDED ACL]
    [ACL - NAMED ACL]
[NAT (Network Address Translation) - STATIC NAT]
    [NAT & ACL - NAT OVERLOAD/PAT]
    [NAT & ACL - TROUBLESHOOTING & KIỂM TRA LỖI]
[BÀI LAB TỔNG HỢP: OSPF & ACL & NAT & TELNET]

========================================================[Host Name]
Router> enable
Router# config terminal
Router(config)# hostname R1
R1(config)# 
========================================================[PassWord Console]
Router# config terminal
Router(config)# line console 0 - PassWord đầu tiên để vô Router (Hầu hết Router chỉ có 1 cổng)
Router(config-line)# password cisco
Router(config-line)# login
Router(config-line)# exit
========================================================[Gỡ bỏ PassWord Console]
Router(config)# line console 0
Router(config-line)# no password
Router(config-line)# exit
========================================================[PassWord Enable]
Router# config terminal
Router(config)# enable password admin
========================================================[Gỡ bỏ PassWord Enable]
Router(config)# no enable password
Router(config)# exit
========================================================[Mã hóa các PassWord]
Router(config)# service password-encryption
========================================================[Bỏ mã hóa các PassWord]
Router(config)# no service password-encryption
========================================================[IP Address]
Router> enable
Router# config terminal
Router(config)# interface f0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# end
Router# copy running-config startup-config
Router# show running-config
Router# show ip interface brief     - Show tất cả các cổng trên thiết bị
* Lưu ý: 
    - running-config được lưu trong bộ nhớ RAM của Router, khi mất điện or bảo trị thì các cài đặt sẽ mất hết
    - Do đó khi cài đặt xong chúng ta sẽ copy nó sang NVRAM khi mất điện sẽ không bị mất cấu hình
========================================================[Loopback Address]
R3(config)# interface loopback 0
R3(config-if)# ip address 192.168.5.1 255.255.255.0
R3(config-if)# exit
========================================================[CDP]
Switch# show cdp neighbors          - Biết được các thông tin của các thiết bị khi được kết nối mạng
Switch# show cdp neighbors detaile
========================================================[NO CDP để bảo mật]
Switch(config)# interface f0/1      - tắt cdp ở 1 port
Switch(config-if)# no cdp enable
Switch(config-if)# exit
--------------------------------------------------------
Switch(config)# no cdp run  - tắt cdp ở toàn bộ các port
========================================================[LLDP]
Switch# show lldp neighbors
========================================================[SSH Key RSA]
Router(config)# hostname R1
R1(config)# ip domain-name local.com
R1(config)# crypto key generate rsa
========================================================[telnet | ssh]
Router# show ip ssh         - kiểm tra Router đang hỗ trợ ssh version nào
Router# show ssh            - kiểm tra xem có thiết bị nào đang ssh tới Router không
Router# show users          - kiểm tra xem ip của thiết bị đang ssh tới Router
Router# clear line [line]   - [line] là giá trị định danh cho session telnet, để ngắt kết nối
--------------------------------------------------C1 sử dụng Telnet
R1# config terminal
R1(config)# line vty 0 4
R1(config-line)# password abc
R1(config-line)# transport input telnet
R1(config-line)# login
R1(config)# enable password admin
R2# telnet 192.168.3.1
PassWord: [...]
R1> 
--------------------------------------------------C2 sử dụng SSH - Lưu ý phải cấu hình SSH Key RSA
R1# config terminal
R1(config)# username user1 password abc
R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# login local
R1(config)# enable password admin
R2# ssh -l user1 192.168.3.1
PassWord: [...]
R1> 
========================================================[Backup - Running or Startup-config]
R1# copy running-config tftp:           - copy running-config của R1 ra ngoài Server
----------------------------------------
R1# copy tftp: running-config           - lấy file running-config từ Server chuyển về R1
----------------------------------------
R1# write memory                        - copy running-config sang startup-config (C1)
----------------------------------------
R1# copy running-config startup-config  - copy running-config sang startup-config (C2)
R1# copy startup-config tftp:           - copy startup-config của R1 ra ngoài Server
R1# copy tftp: startup-config           - lấy file startup-config từ Server chuyển về R1
========================================================[Backup - Flash Memory]
R1# show flash                          - Hệ điều hành của Router được lưu trong Flash Memory
R1# copy flash: tftp:                   - copy flash của Router ra ngoài Server
R1# copy tftp: flash:                   - lấy file flash của Server chuyển về Router
===================================================================[Routing]
R1# show ip router                          ---- Xác định nơi để chuyển tiếp các gói tin
R1# show running-config | include ip route  ---- show các route đã thực hiện
-------------------------------------------------------------------[Route]
R1(config)# ip route [destination_network] [subnet_mask] [output_interface]
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.1
-------------------------------------------------------------------[Default Route]
R2(config)# ip route 0.0.0.0 0.0.0.0 192.168.2.2    - Lớp mạng R2 không biết rõ cụ thể mạng R1 là gì
-------------------------------------------------------------------[AD]
R1(config)# ip route [destination_network] [subnet_mask] [output_interface] [AD]
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.1 5
-------------------------------------------------------------------[IP SLA]
R1(config)# ip sla 1                                            - IP SLA có số hiệu là 1
R1(config-ip-sla)# icmp-echo 192.168.12.2 source-interface f0/0 - thực hiện ping đến địa chỉ 192.168.12.2 sử dụng 
                                                                  source là IP trên cổng f0/0

===================================================================[Trace Route]
-------------------------------------------------------------------[Tracert]
C:\PC1> tracert 192.168.1.2
-------------------------------------------------------------------[Traceroute]
R1# traceroute 192.168.1.2
===================================================================[MAC]
Switch> show mac address-table
===================================================================[VLAN]
SW1(config)# vlan 2                              - Tạo VLAN 2
SW1(config-vlan)# name "IT"                      - Tên của VLAN 2 là IT
SW1(config)# int range f0/5-6                    - Đi vô port 5 và 6
SW1(config-if-range)# switchport access VLAN 2   - Port 5 và 6 thuộc VLAN 2
SW1# show vlan brief                             - Hiển thị VLAN và các port của VLAN
-------------------------------------------------------------------[NO VLAN]
SW1(config)# vlan 3
SW1(config-vlan)# name "HR"
SW1(config)# int range f0/7
SW1(config-if-range)# switchport access VLAN 3
SW1(config)# no vlan 3                           - Xóa VLAN 3 đi
Lưu ý: VLAN 3 có port là f0/7. Khi chúng ra xóa VLAN 3 đi thì port 7 trở thành port mồ côi. Và khi tạo lại VLAN 3 thì port 7 tự động quay lại VLAN 3.
-------------------------------------------------------------------[TRUNK]
SW1(config)# interface f0/1                           - Port này đang kết nối với Switch khác
SW1(config-if)# switchport trunk encapsulation dot1q
SW1(config-if)# switchport mode trunk
SW1# show interface trunk                             - Hiển thị các interface nào đang đóng vai trò TRUNK
-------------------------------------------------------------------[VTP]
SW1(config)# vtp mode server            - Mode server: tạo, xóa, sửa VLAN
SW2(config)# vtp mode client            - Mode client: sẽ có các VLAN từ server. Không làm được gì VLAN cả
SW3(config)# vtp mode transparent       - Mode transparent: tạo, xóa, sửa VLAN, nhưng ko gửi VLAN đi SW khác
SW1-2-3(config)# vtp domain cisco
SW1-2-3(config)# vtp password abc
SW1-2-3# show vtp status
SW1-2-3# show vtp password
SW1-2-3# show vlan brief
-------------------------------------------------------------------[Access Port]
SW1(config)# interface f0/9
SW1(config-if)# switchport mode access      - Port dành cho thiết bị cuối (PC)
SW1(config-if)# switchport access vlan 2    - Port thuộc VLAN 2
-------------------------------------------------------------------[Spanning-tree portfast]
SW1(config)# interface f0/9
SW1(config-if)# switchport mode access      - Cổng dành cho thiết bị cuối (PC)
SW1(config-if)# switchport access vlan 2    - Port thuộc VLAN 2
SW1(config-if)# spanning-tree portfast      - Giúp Forwarding đi nhanh hơn
===================================================================[Router on a Stick (ROAS)]
SWserver(config)# interface f0/3                              - Cổng mà Switch nối với Route
SWserver(config-if)# switchport mode trunk
Router(config)# interface f0/0                                - Cổng mà Router nối với Switch
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# interface f0/0.10                             - Tạo một Sub Interface cho VLAN 10
Router(config-subif)# encapsulation dot1Q 10                  - Liên kết Sub Interface với VLAN 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0   - Tạo IP cho Port f0/0.10
Router(config-subif)# no shutdown
Router(config)# interface f0/0.20                             - Tạo một Sub Interface cho VLAN 20
Router(config-subif)# encapsulation dot1Q 20                  - Liên kết Sub Interface với VLAN 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0   - Tạo IP cho Port f0/0.20
Router(config-subif)# no shutdown
Router(config)# interface f0/0.1                              - Tạo một Sub Interface cho VLAN 1
Router(config-subif)# encapsulation dot1Q 1 native            - Đặt mặc định cho VLAN 1 là native VLAN 
Router(config-subif)# ip address 10.0.0.1 255.0.0.0           - Tạo IP cho Port f0/0.1
Router(config-subif)# no shutdown

* Lưu ý: 
    - Khi mà có 3 SW: Client - Server - Transparent thì Transparent nối với Server phải có các VLAN giống nhau
    - Khi gửi dữ liệu qua mạng khác nhau như thì PC phải có Default Gateway
===================================================================[Switch Layer 3 SVI]
PC> 172.16.2.2/24 172.16.2.1                         - Đặt IP cho PC và trỏ về IP Interface VLAN 2 trên SW 
Switch(config)# hostname DS1
DS1(config)# int vlan 2                              - Đặt IP cho VLAN 2
DS1(config-if)# ip address 172.16.2.1 255.255.255.0
DS1(config-if)# no shutdown
DS1(config)# int f0/1                                - Access Port f0/1 cho VLAN 2
DS1(config-if)# switchport mode access
DS1(config-if)# switchport access VLAN 2
DS1(config)# int vlan 3                              - Đặt IP cho VLAN 3
DS1(config-if)# ip address 172.16.3.1 255.255.255.0
DS1(config-if)# no shutdown
DS1(config)# int f0/2                                - Access Port f0/2 cho VLAN 3
DS1(config-if)# switchport mode access
DS1(config-if)# switchport access VLAN 3
PC> ping 172.16.2.1                                  - Lúc này PC đã ping đc tới IP của VLAN 2 trên SW
DS1(config)# ip routing                              - Bật chức năng định tuyến trên switch layer 3
-------------------------------------------------------------------[C1: Assign IP SW connect Router]
DS1(config)# int f0/3                                - Port connect with Router
DS1(config-if)# no switchport                        - Apply Port f0/3 from layer 2 to layer 3
DS1(config-if)# ip address 192.168.1.2 255.255.255.0 - Assign IP Address for f0/3
DS1(config-if)# no shutdown
-------------------------------------------------------------------[C2: Assign IP VLAN 1]
DS1(config)# int f0/3
DS1(config-if)# switchport                           - Apply Port f0/3 from layer 3 to layer 2
DS1(config-if)# switchport mode access
DS1(config-if)# switchport access vlan 1
DS1(config)# int vlan 1
DS1(config-if)# ip address 192.168.1.2 255.255.255.0
DS1(config-if)# no shutdown
-------------------------------------------------------------------[Chọn cách làm ở trên xong làm tiếp]
DS1(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.1    - To allow VLAN 2 to connect to the Internet
Router(config)# hostname R1
R1(config)# int f0/0                                 - Port connect with Switch
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config)# ip route 172.16.2.0 255.255.255.0 192.168.1.2   - Routing to network vlan 2
R1(config)# ip route 172.16.3.0 255.255.255.0 192.168.1.2   - Routing to network vlan 3
===================================================================[DHCP]
RouterDHCP(config)# service dhcp                                    - Bật dịch vụ DHCP
RouterDHCP(config)# ip dhcp excluded-address 172.16.2.1 172.16.2.2  - Không cấp phát 2 IP này
RouterDHCP(config)# ip dhcp pool LAN1                               - Định nghĩa Pool tên là LAN1 cho LAN 1
RouterDHCP(dhcp-config)# network 172.16.2.0 255.255.255.0           - Đặt địa chỉ mạng
RouterDHCP(dhcp-config)# default-router 172.16.2.1                  - Trỏ về IP của RouterDHCP
RouterDHCP(dhcp-config)# dns-server 8.8.8.8
PC1> ipconfig /renew                                                
RouterDHCP# show ip dhcp binding                                    - Kiểm tra DHCP đã cấp IP nào
-------------------------------------------------------------------[IP Helper Address - Chưa làm đc]
RouterDHCP(config)# ip dhcp excluded-address 172.16.1.1             - Không cấp phát IP này
RouterDHCP(config)# ip dhcp pool LAN2
RouterDHCP(dhcp-config)# network 172.16.1.0 255.255.255.0
RouterDHCP(dhcp-config)# default-router 172.16.1.1
RouterDHCP(dhcp-config)# dns-server 8.8.8.8
R1(config)# interface f0/1                              - Port R1 nối với PCX thuộc LAN 2
R1(config-if)# ip helper-address 172.16.12.2            - Trỏ đến RouterDHCP sử dụng địa chỉ IP nào cũng được
PCX> ipconfig /renew                                    - PCX của R1 xin IP của RouterDHCP
-------------------------------------------------------------------[Cấu hình IP theo MAC - Chưa làm đc]
RouterDHCP(config)# ip dhcp server pool serverA
RouterDHCP(dhcp-config)# client-identifier 0060.7071.A49E
RouterDHCP(dhcp-config)# host 172.16.2.99 255.255.255.0
RouterDHCP(dhcp-config)# default-router 172.16.2.1
RouterDHCP(dhcp-config)# dns-server 8.8.8.8
===================================================================[STP (Spanning Tree Protocol)]
**** ĐIỀU KHIỂN BẦU CHỌN ROOT BRIDGE BẰNG PRIORITY ****
SW1(config)# spanning-tree vlan 10 priority 4096        - Ép SW1 làm Root Bridge cho VLAN 10 (Hạ Priority xuống thấp)
SW1(config)# spanning-tree vlan 20 priority 28672       - Đặt SW1 làm Root phụ cho VLAN 20
SW2(config)# spanning-tree vlan 20 priority 4096        - Ép SW2 làm Root Bridge cho VLAN 20 (Hạ Priority xuống thấp)
SW2(config)# spanning-tree vlan 10 priority 28672       - Đặt SW2 làm Root phụ cho VLAN 10
**** ĐIỀU KHIỂN CỔNG CHẶN (BLOCK PORT) BẰNG PATH COST ****
SW2(config)# interface f0/1                             - Vào cổng f0/1 của SW2 (Đường nối thẳng sang SW1)
SW2(config-if)# spanning-tree vlan 10 cost 100          - Tăng Cost VLAN 10 lên 100 để ép cổng f0/1 của SW2 thành cổng Block (X)
**** TỐI ƯU HÓA CỔNG KẾT NỐI USER (MÁY TÍNH CON) ****
Switch(config)# interface f0/10                         - Vào cổng kết nối trực tiếp với máy tính của người dùng (Host)
Switch(config-if)# spanning-tree portfast               - Bật tính năng PortFast giúp cổng mở ngay lập tức, không đợi 30 giây
**** LỆNH KIỂM TRA (CHẾ ĐỘ PRIVILEGE EXEC) ****
Switch# show spanning-tree vlan 10                      - Kiểm tra chi tiết trạng thái các cổng (Root, Desg, Altn) của VLAN 10
Switch# show spanning-tree vlan 20                      - Kiểm tra chi tiết trạng thái các cổng (Root, Desg, Altn) của VLAN 20
-------------------------------------------------------------------[STP - Chỉnh Priority - Chỉnh Cost]
LAB: 3 SW 1-2-3: kết nối với nhau, SW1 nối với R1, PC1 nối với SW2
SW1(config)# spanning-tree vlan 1 priority 4096          - SW 1 làm Root Bridge
SW1(config)# int range f0/2, f0/3
SW1(config-if)# switchport mode trunk
SW3(config)# int range f0/1, f0/2
SW3(config-if)# switchport mode trunk
SW2(config)# spanning-tree vlan 1 priority 61440         - SW 2 làm Alternated Port (Port bị khóa)
SW2(config)# int range f0/2, f0/3
SW2(config-if)# switchport mode trunk
SW2# show spanning-tree                                  - Kiểm tra Cost của các cổng f0/2 và f0/3
SW2(config)# int f0/3                                    - Để cổng f0/3 làm Blocking Port
SW2(config-if)# spanning-tree vlan 1 cost 200            - Chỉnh Cost cao hơn cổng f0/2
SW2# show spanning-tree vlan 1
-------------------------------------------------------------------[VTP - STP(Root Primary/Root Secondary)]
* LAB: 3 SW 1-2-3 kết nối với nhau
* Yêu cầu: - SW1 là server (tạo VLAN 10, 20), SW2 transparent (tạo VLAN 10, 20), SW3 là client
           - SW1 là Primary cho VLAN 10, là Secondary cho VLAN 20
           - SW3 là Primary cho VLAN 20, là Secondary cho VLAN 10
SW1-2-3(config)# interface range f0/1 - 2           - Vào cấu hình các cổng kết nối giữa các Switch
SW1-2-3(config-if-range)# switchport mode trunk     - Chuyển các cổng kết nối thành đường trung kế (Trunk)
**** CẤU HÌNH TRÊN SW1 (SERVER) ****
SW1(config)# vtp domain CiscoNet                    - Đặt tên miền VTP chung cho hệ thống là CiscoNet
SW1(config)# vtp password 123456                    - Đặt mật khẩu đồng bộ bảo mật cho nhóm VTP
SW1(config)# vtp mode server                        - Cấu hình SW1 làm Server (được quyền tạo/xóa VLAN)
SW1(config)# vlan 10                                - Tạo VLAN 10 trên hệ thống mạng
SW1(config-vlan)# vlan 20                           - Tạo VLAN 20 trên hệ thống mạng
**** CẤU HÌNH TRÊN SW2 (TRANSPARENT) ****
SW2(config)# vtp domain CiscoNet                    - Đặt tên miền VTP trùng với SW1
SW2(config)# vtp password 123456                    - Đặt mật khẩu VTP trùng với SW1
SW2(config)# vtp mode transparent                   - Cấu hình SW2 làm Transparent (chỉ chuyển tiếp, không nhận đồng bộ)
SW2(config)# vlan 10                                - Tạo VLAN 10 thủ công (vì chế độ Transparent không tự đồng bộ)
SW2(config-vlan)# vlan 20                           - Tạo VLAN 20 thủ công (vì chế độ Transparent không tự đồng bộ)
**** CẤU HÌNH TRÊN SW3 (CLIENT) ****      
SW3(config)# vtp domain CiscoNet                    - Đặt tên miền VTP trùng với SW1
SW3(config)# vtp password 123456                    - Đặt mật khẩu VTP trùng với SW1
SW3(config)# vtp mode client                        - Cấu hình SW3 làm Client (tự động nhận VLAN từ SW1)
**** CẤU HÌNH [STP (Root Primary / Secondary)] ****
SW1(config)# spanning-tree vlan 10 root primary         - Ép SW1 làm Root chính cho VLAN 10 (Tự hạ Priority xuống thấp)
SW1(config)# spanning-tree vlan 20 root secondary       - Đặt SW1 làm Root dự phòng cho VLAN 20
SW3(config)# spanning-tree vlan 20 root primary         - Ép SW3 làm Root chính cho VLAN 20 (Tự hạ Priority xuống thấp)
SW3(config)# spanning-tree vlan 10 root secondary       - Đặt SW3 làm Root dự phòng cho VLAN 10
Switch# show spanning-tree vlan 10                      - Kiểm tra xem SW1 đã thành Root của VLAN 10 chưa
Switch# show spanning-tree vlan 20                      - Kiểm tra xem SW3 đã thành Root của VLAN 20 chưa
-------------------------------------------------------------------[VTP - STP(Root Primary/Root Secondary) - ROAS]
* LAB: 3 SW 1-2-3 kết nối với nhau, SW1 kết nối với 1 Router, các PC có thể ping được nhau
* Yêu cầu:  - SW1 là server (tạo VLAN 10, 20), SW2 client, SW3 là client
            - SW1 là Primary cho VLAN 10, là Secondary cho VLAN 20
            - SW2 là Primary cho VLAN 20, là Secondary cho VLAN 10
            - Router thực hiện định tuyến cho VLAN 1, 10, 20
                + E0/0 VLAN 1 172.16.11.1/24
                + E0/0.10 VLAN 10 172.16.10.1/24
                + E0/0.20 VLAN 20 172.16.20.1/24
SW1-2-3(config)# interface range f0/1 - 2               - Vào cấu hình các cổng kết nối giữa các Switch
SW1-2-3(config-if-range)# switchport mode trunk         - Chuyển các cổng liên kết Switch thành đường Trunk
**** CẤU HÌNH TRÊN SW1 (SERVER) ****
SW1(config)# interface f0/24                            - Vào cổng f0/24 trên SW1 (Cổng kết nối lên Router)
SW1(config-if)# switchport mode trunk                   - Bắt buộc phải bật Trunking cho cổng nối lên Router
SW1(config-if)# exit
SW1(config)# vtp domain CiscoNet                        - Đặt tên miền VTP chung cho hệ thống là CiscoNet
SW1(config)# vtp password 123456                        - Đặt mật khẩu đồng bộ bảo mật cho nhóm VTP
SW1(config)# vtp mode server                            - Cấu hình SW1 làm Server để quản lý cơ sở dữ liệu VLAN
SW1(config)# vlan 10                                    - Tạo VLAN 10 trên hệ thống mạng
SW1(config-vlan)# vlan 20                               - Tạo VLAN 20 trên hệ thống mạng
**** CẤU HÌNH TRÊN SW2-3 (CLIENT) ****
SW2-3(config)# vtp domain CiscoNet                      - Đặt tên miền VTP trùng với SW1
SW2-3(config)# vtp password 123456                      - Đặt mật khẩu VTP trùng với SW1
SW2-3(config)# vtp mode client                          - Cấu hình SW2-3 làm Client để tự động đồng bộ VLAN 10, 20
**** CẤU HÌNH [STP (Root Primary / Secondary)]
SW1(config)# spanning-tree vlan 10 root primary         - Ép SW1 làm Root chính cho VLAN 10
SW1(config)# spanning-tree vlan 20 root secondary       - Đặt SW1 làm Root dự phòng cho VLAN 20
SW2(config)# spanning-tree vlan 20 root primary         - Ép SW2 làm Root chính cho VLAN 20
SW2(config)# spanning-tree vlan 10 root secondary       - Đặt SW2 làm Root dự phòng cho VLAN 10
**** LỆNH KIỂM TRA TRẠNG THÁI (CHẾ ĐỘ PRIVILEGE EXEC) ****
Switch# show spanning-tree vlan 10                      - Kiểm tra xem SW1 đã lên làm Root của VLAN 10 chưa
Switch# show spanning-tree vlan 20                      - Kiểm tra xem SW2 đã lên làm Root của VLAN 20 chưa

**** CẤU HÌNH[ROAS (Router-on-a-stick)]
Router(config)# interface ethernet 0/0                  - Vào cổng vật lý chính kết nối xuống SW1
Router(config-if)# no shutdown                          - Bật cổng vật lý chính (Cực kỳ quan trọng)
**** CẤU HÌNH CHO VLAN 1 (Cổng chính đóng vai trò Native VLAN) ****
Router(config-if)# ip address 172.16.11.1 255.255.255.0   - Cấu hình IP Gateway trực tiếp trên cổng chính cho VLAN 1
Router(config-if)# exit
**** CẤU HÌNH SUB-INTERFACE CHO VLAN 10 ****
Router(config)# interface ethernet 0/0.10                       - Tạo Sub-interface .10 cho VLAN 10
Router(config-subif)# encapsulation dot1Q 10                    - Đóng gói theo chuẩn dot1Q mã hóa cho VLAN 10
Router(config-subif)# ip address 172.16.10.1 255.255.255.0      - Cấu hình IP Default Gateway cho người dùng VLAN 10
Router(config-subif)# exit
**** CẤU HÌNH SUB-INTERFACE CHO VLAN 20 ****
Router(config)# interface ethernet 0/0.20                       - Tạo Sub-interface .20 cho VLAN 20
Router(config-subif)# encapsulation dot1Q 20                    - Đóng gói theo chuẩn dot1Q mã hóa cho VLAN 20
Router(config-subif)# ip address 172.16.20.1 255.255.255.0      - Cấu hình IP Default Gateway cho người dùng VLAN 20
Router(config-subif)# exit
**** LỆNH KIỂM TRA TRẠNG THÁI (CHẾ ĐỘ PRIVILEGE EXEC) ****
Router# show ip interface brief                                 - Kiểm tra trạng thái Up/Up và IP của các Sub-interface

**** GIẢ ĐỊNH THIẾT KẾ CỔNG NỐI VỚI PC: ****
* Cổng f0/10 trên các Switch: Dùng để nối vào PC thuộc VLAN 10
* Cổng f0/20 trên các Switch: Dùng để nối vào PC thuộc VLAN 20
SW1(config)# interface f0/10                    - Vào cổng f0/10 kết nối với PC 1 (VLAN 10)
SW1(config-if)# switchport mode access          - Cấu hình cổng chạy ở chế độ Access (kết nối thiết bị cuối)
SW1(config-if)# switchport access vlan 10       - Gán cổng này vào quản lý thuộc cấu trúc VLAN 10
SW1(config-if)# spanning-tree portfast          - Tối ưu hóa: Bật tính năng PortFast giúp PC có mạng ngay lập tức
SW1(config-if)# exit
**** CẤU HÌNH TRÊN SWITCH 2 (SW2) ****
SW2(config)# interface f0/20                    - Vào cổng f0/20 kết nối với PC 2 (VLAN 20)
SW2(config-if)# switchport mode access
SW2(config-if)# switchport access vlan 20       - Gán cổng này vào quản lý thuộc cấu trúc VLAN 20
SW2(config-if)# spanning-tree portfast
SW2(config-if)# exit
**** CẤU HÌNH TRÊN SWITCH 3 (SW3) ****
(Lặp lại cấu hình tương tự cho các cổng kết nối PC trên SW3 nếu có máy tính cắm vào)
SW3(config)# interface f0/10
SW3(config-if)# switchport mode access
SW3(config-if)# switchport access vlan 10
SW3(config-if)# spanning-tree portfast
SW3(config-if)# exit
**** LỆNH KIỂM TRA TRẠNG THÁI (CHẾ ĐỘ PRIVILEGE EXEC) ****
Switch# show vlan brief                         - Kiểm tra xem cổng f0/10, f0/20 đã hiện đúng cột VLAN 10, 20 chưa
===================================================================[EtherChannel]
SW1-2(config)# interface range f0/1, f0/2
SW1-2(config-if-range)# shutdown
SW1(config-if-range)# channel-group 1 mode on                  - Gom 2 Port 1-2 thành Po1
SW2(config-if-range)# channel-group 2 mode on                  - Gom 2 Port 1-2 thành Po2
SW1-2(config-if-range)# switchport trunk encapsulation dot1q
SW1-2(config-if-range)# switchport mode trunk
SW1-2(config-if-range)# no shutdown
SW1-2# show etherchannel summay                                  - Hiển thị thông tin Po2
SW1-2# show interface trunk
-------------------------------------------------------------------[Gỡ bỏ EtherChannel - LACP - PAgP]
SW1-2(config)# default interface f0/1
SW1-2(config)# default interface f0/2
SW1(config)# no interface port-channel 1
SW2(config)# no interface port-channel 2
-------------------------------------------------------------------[LACP]
SWA(config)# interface range f0/1, f0/2
SWA(config-if-range)# shutdown
SWA(config-if-range)# switchport mode trunk
SWA(config-if-range)# channel-protocol lacp
SWA(config-if-range)# channel-group 1 mode active         - Mode active chủ động gửi bản tin
SWA(config-if-range)# no shutdown
SWB(config)# interface range f0/1-2
SWB(config-if-range)# shutdown
SWB(config-if-range)# switchport mode trunk
SWB(config-if-range)# channel-protocol lacp
SWB(config-if-range)# channel-group 2 mode passive        - Mode passive đợi sw láng giềng gửi bản tin
SWB(config-if-range)# no shutdown
-------------------------------------------------------------------[PAgP - Độc quyền của Cisco]
SWC(config)# interface range f0/1, f0/2
SWC(config-if-range)# shutdown
SWC(config-if-range)# switchport mode trunk
SWC(config-if-range)# channel-protocol pagp
SWC(config-if-range)# channel-group 1 mode desirable     - Mode desirable chủ động gửi bản tin
SWC(config-if-range)# no shutdown
SWD(config)# interface range f0/1-2
SWD(config-if-range)# shutdown
SWD(config-if-range)# switchport mode trunk
SWD(config-if-range)# channel-protocol pagp
SWD(config-if-range)# channel-group 2 mode auto          - Mode auto đợi sw láng giềng gửi bản tin
SWD(config-if-range)# no shutdown
===================================================================[HSRP]
PC1> 172.16.0.4/24 172.16.0.3
PC2> 172.16.0.5/24 172.16.0.3
R1(config)# int e0/0
R1(config-if)# ip address 172.16.0.1 255.255.255.0      - Đặt địa chỉ IP cho R1
R1(config-if)# no shutdown
R1(config-if)# standby 1 ip 172.16.0.3                  - Tạo HSRP Group 1
R1(config-if)# standby 1 priority 20                    - Đặt priority cao hơn để làm Active
R1(config-if)# standby 1 preempt                        - Giành lại quyền khi R1 hoạt động trở lại
R2(config)# int e0/0
R2(config-if)# ip address 172.16.0.2 255.255.255.0      - Đặt địa chỉ IP cho R2
R2(config-if)# no shutdown
R2(config-if)# standby 1 ip 172.16.0.3                  - Tạo HSRP Group 1
R2(config-if)# standby 1 priority 10                    - Đặt priority thấp hơn để làm Standby
R2(config-if)# standby 1 preempt                        - Giành lại quyền khi R2 hoạt động trở lại
R1-2# show standby brief                                - Kiểm tra trạng thái của Router
PC1-2# ping 172.16.0.3                                  - Ping thành công đến R1
R1-2(config)# int e0/0
R1-2(config)# standby version 2                         - Cấu hình Virtual MAC v2 [0000.0C9F.F**XXX]
PC1-2# ping 172.16.0.3                                  - Ping thành công đến R1
PC1-2# arp -a                                           - Xem IP 172.16.0.3 đang ánh xạ tới HSRP
-------------------------------------------------------------------[Shutdown R1 -> R2 chiếm quyền làm Active]
R1(config)# int e0/0
R1(config-if)# shutdown
R2# show standby brief                                  - R2 chuyển từ Standby -> Active
-------------------------------------------------------------------[Track]
R1(config)# int e0/1
R1(config-if)# shutdown                             - cổng ra internet của R1 tắt
R1(config)# track 9 int e0/1 line-protocol          - Tạo Track số 9
R1(config)# int e0/0
R1(config-if)# standby 1 track 9 decrement 15       - HSRP Group 1 theo dõi Track 9, trừ đi 15 priority của R1
R1# show standby brief                              - Lúc này priority của R1 còn 5
R1(config)# int e0/1
R1(config-if)# no shutdown                          - bật lại cổng ra internet
R1# show standby brief                              - Lúc này priority của R1 là 20. Giành lại Active
===================================================================[Port Security]
Devices: PC1-2 or Switch or Router
PC1> 192.169.1.1/24 192.168.1.254
PC2> 192.169.1.2/24 192.168.1.254
Switch(config)# int vlan 1
Switch(config-if)# ip address 192.168.1.251 255.255.255.0
Switch(config-if)# no shutdown
Switch(config)# int range f0/0, f0/1, f0/2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 1
Switch(config-if-range)# spanning-tree portfast
Router(config)# int f0/0
Router(config-if)# ip address 192.168.1.254 255.255.255.0
Router(config-if)# no shutdown
-------------------------------------------------------------------[TH1: PC1 kết nối thành công với Port]
Switch(config)# int f0/2                                               - Port kết nối với PC1
Switch(config-if)# switchport port-security                            - Bật tính năng Port Security
Switch(config-if)# switchport port-security maximun 1                  - Chỉ 1 thiết bị(MAC address) được kết nối
Switch(config-if)# shutdown                                            - Shutdown Port trước khi cấu hình
Switch(config-if)# switchport port-security mac-address 0001.C79A.B58C - Cho phép MAC của PC1 được kết nối
Switch(config-if)# switchport port-security violation shutdown         - Mode (Shutdown) port khi khác MAC address
Switch(config-if)# no shutdown                                         - Bật lại Port
Switch# show port-security interface f0/2                              - Hiển thị cấu hình Port Security
PC1> ping 192.168.1.254                                                - Ping thành công đến Router
-------------------------------------------------------------------[TH2: PC1 kết nối không thành công với Port]
Switch(config)# int f0/2                                                  - Port kết nối với PC1
Switch(config-if)# no switchport port-security mac-address 0001.C79A.B58C - xóa cấu hình cũ
Switch(config-if)# switchport port-security mac-address 0001.C79A.B58D    - Cho phép MAC address được kết nối
PC1> ping 192.168.1.254                                                   - Ping không thành công đến Router
Switch#                                            - Hiển thị cảnh báo về kết nối Port và Shutdown Port
Switch# show interface f0/2 status                                        - Kiểm tra trạng thái của Port
-------------------------------------------------------------------[TH3: Router kết nối thành công với Port]
Switch(config)# int f0/1                                            - Port kết nối với Router
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximun 1
Switch(config-if)# switchport port-security mac-address sticky      - Địa chỉ MAC đầu tiên kết nối với Port
Switch(config-if)# switchport port-security violation restrict      - Mode (restrict)
Switch# show port-security address                                  - Kiểm tra MAC của Router có trong Port
Router# ping 192.168.1.1                                            - Ping thành công đến PC1
-------------------------------------------------------------------[TH4: Router kết nối không thành công với Port]
Router(config)# interface f0/0
Router(config-if)# mac-address 0000.aaaa.1001           - Thiết lập MAC address khác
Router# show interface f0/0                             - Kiểm tra MAC đã được cập nhật chưa
Router# ping 192.168.1.1                                - Ping không thành công đến PC1
Switch#                                                 - Hiển thị cảnh báo về kết nối Port và hủy gói tin
===================================================================[BPDU Gurad]

===================================================================[DHCP Snooping]

===================================================================[DTP - VLAN Hooping]

===================================================================[IP Source Guard]

===================================================================[DAI (Dymanic ARP Inspection)]

===================================================================[SPAN (Switch Port Analyzer)]

===================================================================[AAA]
Devices: PC1-2 or Switch or Router
PC1> 192.169.1.1/24 192.168.1.254
PC2> 192.169.1.2/24 192.168.1.254
Switch(config)# int vlan 1
Switch(config-if)# ip address 192.168.1.251 255.255.255.0
Switch(config-if)# no shutdown
Switch(config)# int range f0/0, f0/1, f0/2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 1
Switch(config-if-range)# spanning-tree portfast
Router(config)# int f0/0
Router(config-if)# ip address 192.168.1.254 255.255.255.0
Router(config-if)# no shutdown
Router(config)# hostname R1
R1(config)# enable password 123456
-------------------------------------------------------------------[Bật cơ chế xác thực AAA]
R1(config)# aaa new-model                                       - Bật cơ chế xác thực AAA
R1(config)# aaa authentication login Local_AAA local            - Sử dụng username và password khai báo cục bộ
R1(config)# username admin privilege 15 password cisco 123      - Account có đặc quyền cao nhất là 15
R1(config)# username subadmin privilege 1 password ccna 123     - Account có đặc thấp nhất là 1
R1(config)# line vty 0 4                                        - Cho phép 5 người kết nối
R1(config-line)# login authentication Local_AAA
-------------------------------------------------------------------[Telnet]
Switch> ping 192.168.1.254              - ping được đến Router
Switch> telnet 192.168.1.254            - Telnet được đến Router
Username:admin                          - Sử dụng account có đặc quyền cao nhất
Password:
R1>
-------------------------------------------------------------------[Authorization (Phân quyền)]
R1(config)# privilege exec level 8 show running-config     - level 8 chỉ được show running-config
R1(config)# privilege exec level 8 configure terminal      - level 8 được vào configure terminal
R1(config)# privilege configure level 8 interface          - level 8 được vào các interface
R1(config)# privilege interface level 8 ip address         - level 8 cấu hình được IP
R1(config)# privilege interface level 8 no shutdown        - level 8 cấu hình được no shutdown
R1(config)# enable secret level 8 ccna123                  - Đặt mật khẩu cho level 8
Switch> telnet 192.168.1.254
Username: subadmin                                         - Sử dụng account có đặc quyền thấp nhất
Password:
R1> enable 8
Password:
R1# show privilege                                         - Current privilege level is 8
-------------------------------------------------------------------[Accounting (Ghi nhận và giám sát)]
R1(config)# archive                         - Bật cơ chế Accounting
R1(config-archive)# log config
R1(config-archive-log-cfg)# logging enable
R1# show archive log config all
===================================================================[Static Router - Giao thức định tuyến tĩnh]
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.1      - Mặc định giá trị AD = 1
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.4.1      - Mặc định giá trị AD = 1
-------------------------------------------------------------------[Static Router - AD (Administrative Distance)]
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.1 5     - Giá trị AD càng thấp độ ưu tiên càng cao
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.4.1 10
* Tại một thời điểm: Bảng định tuyến (Routing Table) chỉ lưu duy nhất tuyến đường chính có AD thấp hơn (AD = 5). Tuyến đường dự phòng (AD = 10) bị ẩn đi và lưu trong bộ nhớ cấu hình (Running-config).
-------------------------------------------------------------------[Static Router - AD - IP SLA]
R1(config)# ip sla 1                                        - Tạo SLA số thứ tự là 1
R1(config-ip-sla)# icmp-echo 192.168.2.1                    - Gửi gói tin đến IP 192.168.2.1
R1(config-ip-sla-echo)# frequency 5                         - Chu kỳ 5 giây một lần
R1(config)# ip sla schedule 1 life forever start-time now   - Bật cho tiến trình này chạy ngay lập tức 
                                                                và không bao giờ hết hạn
R1(config)# track 10 ip sla 1 reachability          - Tạo Track số 10. 
                                                    - Nếu IP SLA 1 ping thông, Track 10 sẽ ở trạng thái Up
                                                    - Nếu ping thất bại, Track 10 sẽ chuyển sang Down
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.1 5 track 10   - Áp dụng Track
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.4.1 10
===================================================================[Dynamic Router - RIP(120) - NAT - ACL]
* Devices: 3 Router kết nối với 1 Switch
* R1: G0/0 172.16.1.1/24 - Loopback1 192.168.1.1/26
* R2: G0/0 172.16.1.2/24 - Loopback1 192.168.1.65/26
* R3: G0/0 172.16.1.3/24 - Loopback0 10.3.0.1/24 - Loopback1 10.3.1.1/24 - Loopback2 10.3.2.1/24
R1-2-3(config)# router rip                                - Kích hoạt giao thức định tuyến RIP
R1-2-3(config-route)# version 2                           - Sử dụng RIP phiên bản 2 (Hỗ trợ chia Subnet/Classless)
R1-2-3(config-route)# no auto-summary                     - Tắt tính năng tự động tóm tắt mạng về Classful
R1(config-route)# network 172.16.0.0                      - Quảng bá dải mạng kết nối chung giữa các Router
R1(config-route)# network 192.168.1.0                     - Quảng bá dải mạng Loopback 1 (192.168.1.0/26)
R2(config-route)# network 172.16.0.0                      - Quảng bá dải mạng kết nối chung giữa các Router
R2(config-route)# network 192.168.1.0                     - Quảng bá dải mạng Loopback 1 (192.168.1.64/26 - Gõ mạng gốc lớp C)
R3(config-route)# network 172.16.0.0                      - Quảng bá dải mạng kết nối chung giữa các Router
R3(config-route)# network 10.0.0.0                        - Quảng bá toàn bộ các dải Loopback 0, 1, 2 (Mạng gốc lớp A)
R1-2-3# show ip route rip                                 - Kiểm tra xem các Router đã học được mạng của nhau chưa (Ký hiệu chữ R)
**** [CẤU HÌNH NAT & ACL RA INTERNET TRÊN R2] ****
R2(config)# interface gigabitEthernet 0/1                 - Vào cổng G0/1 (Cổng kết nối ra Modem/Internet)
R2(config-if)# ip address dhcp                            - Cấu hình nhận IP tự động từ nhà mạng (ISP)
R2(config-if)# ip nat outside                             - Đánh dấu đây là cổng hướng ra ngoài Internet
R2(config-if)# no shutdown
R2(config-if)# exit
R2(config)# interface gigabitEthernet 0/0                 - Vào cổng G0/0 (Cổng kết nối vùng mạng nội bộ LAN)
R2(config-if)# ip nat inside                              - Đánh dấu đây là cổng hướng vào trong mạng nội bộ
R2(config-if)# exit
R2(config)# access-list 1 permit 172.16.1.0 0.0.0.255     - (Tối ưu bảo mật) Chỉ cho phép dải mạng nội bộ này được NAT
R2(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R2(config)# access-list 1 permit 10.3.0.0 0.0.255.255
*(Nếu muốn cho phép tất cả mọi dải mạng, gõ: R2(config)# access-list 1 permit any)*
R2(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload  - Kích hoạt NAT Overload (PAT) qua cổng G0/1
**** [CẤP ĐƯỜNG ĐI INTERNET CHO R1 VÀ R3] ****
R2(config)# router rip
R2(config-router)# default-information originate           - Tự động phát dải Default Route (0.0.0.0/0) qua RIP cho R1 và R3
R1-3# show ip route                                        - Sẽ thấy xuất hiện dòng đường đi cuối: R* 0.0.0.0/0 [120/1]...
R1-3# ping 8.8.8.8                                         - Thử nghiệm ping ra một IP ngoài Internet (Ví dụ DNS Google)
R2# show ip nat translations                               - Xem bảng chuyển đổi địa chỉ NAT từ IP nội bộ sang IP công cộng
-------------------------------------------------------------------[CẤU HÌNH NÂNG CAO CHO RIPv2]
1. CẤU HÌNH CỔNG THỤ ĐỘNG (Passive-interface) - Bắt buộc cho cổng LAN/Loopback
- Mặc định, RIP sẽ liên tục gửi gói tin cập nhật định tuyến (mỗi 30 giây) ra TẤT CẢ các cổng tham gia lệnh network. 
- Việc gửi gói tin RIP ra cổng nối với PC (LAN) hoặc cổng Loopback là vô ích, làm tốn băng thông và tạo cơ hội cho hacker gửi gói tin RIP giả mạo vào Router.
--- CẤU HÌNH TRÊN R1 ---
R1(config)# router rip
R1(config-router)# passive-interface Loopback1          - Chặn không cho RIP gửi gói tin cập nhật ra cổng Loopback1
--- CẤU HÌNH TRÊN R3 ---
R3(config)# router rip
R3(config-router)# passive-interface Loopback0          - Chặn gửi gói tin RIP ra Loopback0
R3(config-router)# passive-interface Loopback1          - Chặn gửi gói tin RIP ra Loopback1
R3(config-router)# passive-interface Loopback2          - Chặn gửi gói tin RIP ra Loopback2
2. XÁC THỰC BẢO MẬT (RIP Authentication)
- Mặc định, RIP không có bảo mật. Nếu ai đó cắm một Router lạ vào mạng và bật RIP trùng tên dải mạng, Router của bạn sẽ tự động học các tuyến đường bậy bạ đó. 
- Để an toàn, các Router kết nối trực tiếp với nhau cần phải "khớp" mật khẩu mã hóa MD5 thì mới chịu học định tuyến của nhau.
--- CẤU HÌNH TRÊN R1 (Cổng G0/0 nối sang R2, R3) ---
R1(config)# key chain RIP_KEY                           - Tạo một chuỗi chìa khóa tên là RIP_KEY
R1(config-keychain)# key 1                              - Tạo mã chìa khóa số 1
R1(config-keychain-key)# key-string ChuanCisco123       - Đặt mật khẩu là ChuanCisco123
R1(config-keychain-key)# exit
R1(config-keychain)# exit
R1(config)# interface gigabitEthernet 0/0               - Vào cổng vật lý kết nối chung
R1(config-if)# ip rip authentication mode md5           - Bật chế độ mã hóa mật khẩu bảo mật MD5
R1(config-if)# ip rip authentication key-chain RIP_KEY  - Áp dụng chuỗi khóa vừa tạo vào cổng này
--- CẤU HÌNH TRÊN R2 VÀ R3 ---
(Bạn thực hiện tạo key chain trùng tên/mật khẩu và áp dụng vào cổng G0/0 tương tự như trên R1)
===================================================================[Dynamic Router - OSPF(110)]
* Devices: Router 1: nối với PC 1(G0/1: 172.16.1.1/24), nối với Switch 1 (G0/0: 192.168.0.1/24)
           Router 2: nối với PC 2(G0/1: 172.16.2.1/24), nối với Switch 1 (G0/0: 192.168.0.2/24)
           Router 3: nối với PC 3(G0/1: 172.16.3.1/24), nối với Switch 1 (G0/0: 192.168.0.3/24)
**** CÁCH 1: DÙNG LỆNH NETWORK ****
R1-2-3(config)# router ospf 1
R1-2-3(config-router)# network 192.168.0.0 0.0.0.255 area 0     - Bật OSPF trên cổng G0/0 (Nối vào Switch 1)
R1(config-router)# router-id 0.0.0.1                            - Định danh định tuyến của R1 là 0.0.0.1
R2(config-router)# router-id 0.0.0.2                            - Định danh định tuyến của R2 là 0.0.0.2
R3(config-router)# router-id 0.0.0.3                            - Định danh định tuyến của R3 là 0.0.0.3
R1(config-router)# network 172.16.1.0 0.0.0.255 area 0          - Bật OSPF trên cổng G0/1 (Mạng PC 1)
R2(config-router)# network 172.16.2.0 0.0.0.255 area 0          - Bật OSPF trên cổng G0/1 (Mạng PC 2)
R3(config-router)# network 172.16.3.0 0.0.0.255 area 0          - Bật OSPF trên cổng G0/1 (Mạng PC 3)
**** CÁCH 2: VÀO THẲNG INTERFACE ****
*(Cách này chuyên nghiệp và ít lỗi hơn cách 1, không cần gõ lệnh network trong router ospf)*
R1-2-3(config)# interface gigabitEthernet 0/0       - Khi cả 3 đầu cổng cùng bật, OSPF mới thiết lập láng giềng thành công
R1-2-3(config-if)# ip ospf 1 area 0                 - Kích hoạt trực tiếp OSPF 1 vùng 0 ngay trên cổng này
R1-2-3(config)# interface gigabitEthernet 0/1
R1-2-3(config-if)# ip ospf 1 area 0
R1-2-3# show ip ospf interface brief                - Thấy được cổng (G0/0 - G0/1) đã tham gia định tuyến OSPF
R1-2-3# show ip ospf neighbor                       - Kiểm tra xem trạng thái đã lên FULL với các Router khác chưa
R1-2-3# show ip route ospf                          - Hiển thị bảng định tuyến, các mạng học từ OSPF sẽ có chữ O ở đầu
R2# ping 172.16.1.1                                 - ping thành công
R2# ping 172.16.1.1 source 172.16.2.1               - ping thành công
-------------------------------------------------------------------[Dynamic Router - OSPF(110) - DR - BDR - DROTHER]
* Mặc định: Router nào có Router-ID lớn nhất sẽ làm DR (R3 làm DR, R2 làm BDR, R1 làm DROTHER).
* Yêu cầu: R1-DROTHER, R2-BDR, R3-DR, dựa trên thông số Priority (Mặc định priority = 1).
R1-2-3# show ip ospf interface G0/0                 - Lúc này thấy R1 là DR, R2 là BDR, R3 là DROTHER
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip ospf priority 0                   - Priority = 0 nghĩa là từ bỏ quyền ứng cử, luôn luôn là DROTHER
R3(config)# interface gigabitEthernet 0/0
R3(config-if)# ip ospf priority 30                  - Tăng Priority lớn nhất mạng để giành quyền làm DR
R1-2-3# clear ip ospf process                       - RESET TIẾN TRÌNH TRÊN CẢ 3 ROUTER ĐỂ BẦU CHỌN LẠI
*(Hệ thống hỏi [no]: gõ Y và ấn Enter để đồng ý)*
-------------------------------------------------------------------[Dynamic Router - OSPF(110) - Chỉnh Cost]
* Chỉ sử dụng khi R1 đến R3 có 2 đường kết nối, ta sẽ chọn đường tối ưu hơn để chỉnh Cost
* Nguyên tắc OSPF: Ưu tiên đường đi có tổng Cost thấp nhất (Metric = Cost cổng ra).
* Cổng GigabitEthernet mặc định có Cost = 1.
R1# show ip route ospf                      - Thấy Metric của mạng R2 là [110/2] (1 của cổng ra R1 + 1 của cổng ra R2)
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip ospf cost 20              - Tăng cấu hình Cost cổng này lên 20
R1# show ip route ospf                      - Lúc này Metric tăng lên thành [110/21] vì Cost gốc đã thay đổi
===================================================================[ACL (Access Control List) - STANDARD ACL]
* STANDARD ACL (Chỉ lọc dựa vào địa chỉ IP NGUỒN - Số hiệu từ 1 đến 99)
* Kịch bản: Chặn không cho máy PC1 (192.168.1.50) đi vào vùng mạng Kế toán (10.0.0.0/24). Các máy khác đi bình thường.
Router(config)# access-list 10 deny host 192.168.1.50   - Cấm đích danh địa chỉ IP của máy PC1
Router(config)# access-list 10 permit any               - Lệnh sống còn! Nếu không có lệnh này, mọi IP khác đều bị cấm hết sạch
* Áp dụng vào cổng (Đặt gần ĐÍCH - Tức là cổng ra hướng vào mạng Kế toán trên Router):
Router(config)# interface gigabitEthernet 0/2
Router(config-if)# ip access-group 10 out               - Lọc dữ liệu theo hướng đi ra khỏi cổng để vào mạng Kế toán
-------------------------------------------------------------------[ACL - EXTENDED ACL]
* EXTENDED ACL (Lọc chi tiết: IP Nguồn, IP Đích, Loại giao thức TCP/UDP, Số hiệu Port - Số hiệu từ 100 đến 199)
* Kịch bản: Cấm toàn bộ mạng LAN (192.168.1.0/24) không được truy cập Web (Port 80, 443) của Server (10.0.0.100), nhưng vẫn cho phép Ping thoải mái.
Router(config)# access-list 100 deny tcp 192.168.1.0 0.0.0.255 host 10.0.0.100 eq 80   - Cấm giao thức HTTP (web thường)
Router(config)# access-list 100 deny tcp 192.168.1.0 0.0.0.255 host 10.0.0.100 eq 443  - Cấm giao thức HTTPS (web bảo mật)
Router(config)# access-list 100 permit ip any any      - Cho phép tất cả các loại dữ liệu khác (như ping, file) chạy bình thường
* Áp dụng vào cổng (Đặt gần NGUỒN - Tức là cổng tiếp nhận dữ liệu từ vùng mạng LAN đi vào Router):
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip access-group 100 in              - Lọc dữ liệu ngay khi nó vừa đi vào cổng Router từ mạng LAN
-------------------------------------------------------------------[ACL - NAMED ACL]
* NAMED ACL (ACL đặt bằng tên chữ thay vì số - Giúp dễ quản lý và chỉnh sửa xóa dòng lẻ được)
Router(config)# ip access-list extended CHAN_WEB_NHAN_VIEN - Tạo một bảng ACL mở rộng đặt tên dạng chữ dễ nhớ
Router(config-ext-nacl)# deny tcp any host 8.8.8.8 eq www  - Cấm mọi máy truy cập trang web có IP 8.8.8.8
Router(config-ext-nacl)# permit ip any any                 - Cho phép tất cả các luồng mạng còn lại
Router(config-ext-nacl)# exit
Router# show access-lists                                   - Hiển thị danh sách các bảng ACL đang có và số lượt gói tin bị chặn
===================================================================[NAT (Network Address Translation) - STATIC NAT]
* STATIC NAT (Ánh xạ tĩnh 1-1: Dùng public Server nội bộ cho Internet bên ngoài truy cập vào)
* Kịch bản: Ánh xạ Web Server nội bộ (IP Private: 192.168.1.100) ra một IP Public cố định (203.162.1.10) do ISP cấp.
Router(config)# ip nat inside source static 192.168.1.100 203.162.1.10   - Khai báo ánh xạ cứng 1 Private sang 1 Public
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside                                         - Chỉ định cổng này quay mặt vào mạng LAN nội bộ
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside                                        - Chỉ định cổng này quay mặt ra ngoài Internet (IS
-------------------------------------------------------------------[NAT & ACL - NAT OVERLOAD/PAT]
* NAT OVERLOAD / PAT (Ánh xạ nhiều IP Private dùng chung 1 IP Public của cổng WAN - Phân biệt bằng số hiệu Port)
* Kịch bản: Cho phép toàn bộ mạng LAN nội bộ (192.168.1.0/24) đi ra Internet qua IP của cổng kết nối trực tiếp với ISP (int G0/1)
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255   - Dùng Standard ACL để chọn dải IP nội bộ được phép NAT
Router(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside                         - Cổng cắm vào Switch của mạng nội bộ LAN
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside                        - Cổng kết nối trực tiếp với nhà mạng ISP
-------------------------------------------------------------------[NAT & ACL - TROUBLESHOOTING & KIỂM TRA LỖI]
* Các lệnh kiểm tra trạng thái hoạt động trên chế độ Mode Privilege (Router#)
Router# show ip nat translations              - Hiển thị bảng tra cứu dịch mã IP đang chạy trong thời gian thực
Router# show ip nat statistics                - Hiển thị các thông số cấu hình và số lượng gói tin đã được NAT
Router# clear ip nat translation *            - Lệnh xóa sạch bảng NAT hiện tại để ép Router dịch lại từ đầu
Router# show access-lists                     - Xem chi tiết các dòng ACL đang hoạt động và số lượng gói tin bị chặn (match)
===================================================================[BÀI LAB TỔNG HỢP: OSPF & ACL & NAT & TELNET]
* KỊCH BẢN HỆ THỐNG MẠNG:
  - Doanh nghiệp có 2 vùng mạng: Vùng Quản trị (192.168.10.0/24) và Vùng Nhân viên (192.168.20.0/24).
  - Kết nối định tuyến nội bộ giữa các Router Core bằng giao thức OSPF (Area 0).
  - Router Biên (Gateway) kết nối ra Internet với ISP qua cổng GigabitEthernet 0/1 (IP Public: 200.0.0.3/24).
* YÊU CẦU CẤU HÌNH BẢO MẬT & ĐỊNH TUYẾN:
  1. OSPF: Cấu hình OSPF chạy trong mạng nội bộ để các vùng nhìn thấy nhau.
  2. NAT Overload: Cho phép vùng Nhân viên và Quản trị đi Internet thông qua IP của cổng G0/1.
  3. Extended ACL: Ngăn vùng Nhân viên (192.168.20.0/24) không được truy cập vào Vùng Quản trị (192.168.10.0/24).
  4. Telnet & ACL: Chỉ cho phép duy nhất máy Admin (192.168.10.50) được Telnet vào cấu hình Router. Cấm tất cả các IP khác.
-------------------------------------------------------------------[PHẦN 1: CẤU HÌNH ĐỊNH TUYẾN DÒNG OSPF]
* Cấu hình trên Router biên để thiết lập định tuyến với các mạng bên trong:
Router(config)# router ospf 1                                   - Khởi chạy tiến trình OSPF với Process ID là 1
Router(config-router)# router-id 1.1.1.1                        - Đặt định danh thủ công cho Router là 1.1.1.1
Router(config-router)# network 192.168.10.0 0.0.0.255 area 0    - Quảng bá dải mạng Quản trị vào vùng Trung tâm Area 0
Router(config-router)# network 192.168.20.0 0.0.0.255 area 0    - Quảng bá dải mạng Nhân viên vào vùng Trung tâm Area 0
Router(config-router)# exit
-------------------------------------------------------------------[PHẦN 2: CẤU HÌNH EXTENDED ACL - CẤM NHÂN VIÊN SANG QUẢN TRỊ]
* Sử dụng Extended ACL (Đặt gần NGUỒN - Cổng tiếp nhận lưu lượng từ mạng Nhân viên đi vào Router):
Router(config)# access-list 120 deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255 - Cấm mạng Nhân viên đi sang mạng Quản trị
Router(config)# access-list 120 permit ip any any        - Cho phép Nhân viên đi ra các hướng khác (Internet)
Router(config)# interface gigabitEthernet 0/2            - Cổng nối với vùng mạng Nhân viên
Router(config-if)# ip access-group 120 in                - Chặn ngay lập tức khi gói tin từ mạng Nhân viên gửi lên
-------------------------------------------------------------------[PHẦN 3: CẤU HÌNH NAT OVERLOAD (PAT) RA INTERNET]
* Cho phép cả 2 mạng LAN nội bộ dịch mã IP Public đi ra ngoài mạng Internet qua cổng G0/1 kết nối ISP:
Router(config)# access-list 1 permit 192.168.0.0 0.0.255.255            - Tạo Standard ACL gom toàn bộ dải IP Private 192.168.x.x
Router(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload
Router(config)# interface gigabitEthernet 0/0                             - Cổng kết nối xuống Switch mạng Quản trị
Router(config-if)# ip nat inside
Router(config)# interface gigabitEthernet 0/2                             - Cổng kết nối xuống Switch mạng Nhân viên
Router(config-if)# ip nat inside
Router(config)# interface gigabitEthernet 0/1                             - Cổng kết nối trực tiếp ra ngoài nhà mạng ISP
Router(config-if)# ip nat outside
Router(config)# exit
-------------------------------------------------------------------[PHẦN 4: BẢO MẬT TELNET BẰNG STANDARD ACL]
* Bước 1: Tạo Standard ACL chỉ định đích danh IP của máy Admin được phép truy cập:
Router(config)# access-list 5 permit host 192.168.10.50  - Chỉ cho duy nhất máy Admin qua cổng lọc
* Lưu ý: Mặc định cuối ACL 5 có lệnh ẩn 'deny any', nên các IP khác kết nối Telnet sẽ bị Router từ chối ngay lập tức.
* Bước 2: Bật tính năng Telnet trên các đường ảo (Line VTY) và áp dụng ACL kiểm soát đầu vào:
Router(config)# line vty 0 4                             - Mở 5 phiên kết nối từ xa cùng lúc (từ phiên số 0 đến 4)
Router(config-line)# password Admin@123                  - Đặt mật khẩu đăng nhập Telnet cho Router
Router(config-line)# login                               - Yêu cầu xác thực mật khẩu khi có người kết nối
Router(config-line)# access-class 5 in                   - Áp dụng ACL số 5 để lọc các IP muốn đăng nhập vào
Router(config-line)# exit




[Host Name]
[PassWord Console]
    [Gỡ bỏ PassWord Console]
[PassWord Enable]
    [Gỡ bỏ PassWord Enable]
[Mã hóa các PassWord]
    [Bỏ mã hóa các PassWord]
[IP Address]
[Loopback Address]
[CDP]
    [NO CDP để bảo mật]
[LLDP]
[SSH Key RSA]
    [telnet | ssh]
[Backup - Running or Startup-config]
[Backup - Flash Memory]
[Routing]
    [Route]
    [Default Route]
    [AD]
    [IP SLA]
[Trace Route]
    [Tracert]
    [Traceroute]
[MAC]
[VLAN]
    [NO VLAN]
    [TRUNK]
    [VTP]
    [Access Port]
    [Spanning-tree portfast]
[Router on a Stick (ROAS) - Định tuyến giữa nhiều VLAN với nhau - Chỉ một cổng vật lý, nhưng bên Router chia thành nhiều sub-interface.]
[Switch Layer 3 SVI]
[DHCP]
    [IP Helper Address]
[STP (Spanning Tree Protocol)]
    [STP - Chỉnh Priority - Chỉnh Cost]
    [VTP - STP(Root Primary/Root Secondary)]
    [VTP - STP(Root Primary/Root Secondary) - ROAS]
[EtherChannel]
    [Gỡ bỏ EtherChannel]
    [LACP]
    [PAgP - Độc quyền của Cisco]
[HSRP]
    [Shutdown R1 -> R2 chiếm quyền làm Active]
    [Track]
[Port Security]
    [TH1: PC1 kết nối thành công với Port]
    [TH2: PC1 kết nối không thành công với Port]
    [TH3: Router kết nối thành công với Port]
    [TH4: Router kết nối không thành công với Port]
[BPDU Gurad]
[DHCP Snooping]
[DTP - VLAN Hooping]
[IP Source Guard]
[DAI (Dymanic ARP Inspection)]
[SPAN (Switch Port Analyzer)]
[AAA]
    [Bật cơ chế xác thực AAA]
    [Telnet]
    [Authorization (Phân quyền)]
    [Accounting (Ghi nhận và giám sát)]
[Static Router - Giao thức định tuyến tĩnh]
    [Static Router - AD (Administrative Distance)]
    [Static Router - AD - IP SLA]
[Dynamic Router - RIP(120) - NAT - ACL]
    [CẤU HÌNH NÂNG CAO CHO RIPv2]
[Dynamic Router - OSPF(110)]
    [Dynamic Router - OSPF(110) - DR - BDR]
    [Dynamic Router - OSPF(110) - Chỉnh Cost]
[ACL (Access Control List) - STANDARD ACL]
    [ACL - EXTENDED ACL]
    [ACL - NAMED ACL]
[NAT (Network Address Translation) - STATIC NAT]
    [NAT & ACL - NAT OVERLOAD/PAT]
    [NAT & ACL - TROUBLESHOOTING & KIỂM TRA LỖI]
[BÀI LAB TỔNG HỢP: OSPF & ACL & NAT & TELNET]

========================================================[Host Name]
Router> enable
Router# config terminal
Router(config)# hostname R1
R1(config)# 
========================================================[PassWord Console]
Router# config terminal
Router(config)# line console 0 - PassWord đầu tiên để vô Router (Hầu hết Router chỉ có 1 cổng)
Router(config-line)# password cisco
Router(config-line)# login
Router(config-line)# exit
========================================================[Gỡ bỏ PassWord Console]
Router(config)# line console 0
Router(config-line)# no password
Router(config-line)# exit
========================================================[PassWord Enable]
Router# config terminal
Router(config)# enable password admin
========================================================[Gỡ bỏ PassWord Enable]
Router(config)# no enable password
Router(config)# exit
========================================================[Mã hóa các PassWord]
Router(config)# service password-encryption
========================================================[Bỏ mã hóa các PassWord]
Router(config)# no service password-encryption
========================================================[IP Address]
Router> enable
Router# config terminal
Router(config)# interface f0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config-if)# end
Router# copy running-config startup-config
Router# show running-config
Router# show ip interface brief     - Show tất cả các cổng trên thiết bị
* Lưu ý: 
    - running-config được lưu trong bộ nhớ RAM của Router, khi mất điện or bảo trị thì các cài đặt sẽ mất hết
    - Do đó khi cài đặt xong chúng ta sẽ copy nó sang NVRAM khi mất điện sẽ không bị mất cấu hình
========================================================[Loopback Address]
R3(config)# interface loopback 0
R3(config-if)# ip address 192.168.5.1 255.255.255.0
R3(config-if)# exit
========================================================[CDP]
Switch# show cdp neighbors          - Biết được các thông tin của các thiết bị khi được kết nối mạng
Switch# show cdp neighbors detaile
========================================================[NO CDP để bảo mật]
Switch(config)# interface f0/1      - tắt cdp ở 1 port
Switch(config-if)# no cdp enable
Switch(config-if)# exit
--------------------------------------------------------
Switch(config)# no cdp run  - tắt cdp ở toàn bộ các port
========================================================[LLDP]
Switch# show lldp neighbors
========================================================[SSH Key RSA]
Router(config)# hostname R1
R1(config)# ip domain-name local.com
R1(config)# crypto key generate rsa
========================================================[telnet | ssh]
Router# show ip ssh         - kiểm tra Router đang hỗ trợ ssh version nào
Router# show ssh            - kiểm tra xem có thiết bị nào đang ssh tới Router không
Router# show users          - kiểm tra xem ip của thiết bị đang ssh tới Router
Router# clear line [line]   - [line] là giá trị định danh cho session telnet, để ngắt kết nối
--------------------------------------------------C1 sử dụng Telnet
R1# config terminal
R1(config)# line vty 0 4
R1(config-line)# password abc
R1(config-line)# transport input telnet
R1(config-line)# login
R1(config)# enable password admin
R2# telnet 192.168.3.1
PassWord: [...]
R1> 
--------------------------------------------------C2 sử dụng SSH - Lưu ý phải cấu hình SSH Key RSA
R1# config terminal
R1(config)# username user1 password abc
R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# login local
R1(config)# enable password admin
R2# ssh -l user1 192.168.3.1
PassWord: [...]
R1> 
========================================================[Backup - Running or Startup-config]
R1# copy running-config tftp:           - copy running-config của R1 ra ngoài Server
----------------------------------------
R1# copy tftp: running-config           - lấy file running-config từ Server chuyển về R1
----------------------------------------
R1# write memory                        - copy running-config sang startup-config (C1)
----------------------------------------
R1# copy running-config startup-config  - copy running-config sang startup-config (C2)
R1# copy startup-config tftp:           - copy startup-config của R1 ra ngoài Server
R1# copy tftp: startup-config           - lấy file startup-config từ Server chuyển về R1
========================================================[Backup - Flash Memory]
R1# show flash                          - Hệ điều hành của Router được lưu trong Flash Memory
R1# copy flash: tftp:                   - copy flash của Router ra ngoài Server
R1# copy tftp: flash:                   - lấy file flash của Server chuyển về Router
===================================================================[Routing]
R1# show ip router                          ---- Xác định nơi để chuyển tiếp các gói tin
R1# show running-config | include ip route  ---- show các route đã thực hiện
-------------------------------------------------------------------[Route]
R1(config)# ip route [destination_network] [subnet_mask] [output_interface]
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.1
-------------------------------------------------------------------[Default Route]
R2(config)# ip route 0.0.0.0 0.0.0.0 192.168.2.2    - Lớp mạng R2 không biết rõ cụ thể mạng R1 là gì
-------------------------------------------------------------------[AD]
R1(config)# ip route [destination_network] [subnet_mask] [output_interface] [AD]
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.1 5
-------------------------------------------------------------------[IP SLA]
R1(config)# ip sla 1                                            - IP SLA có số hiệu là 1
R1(config-ip-sla)# icmp-echo 192.168.12.2 source-interface f0/0 - thực hiện ping đến địa chỉ 192.168.12.2 sử dụng 
                                                                  source là IP trên cổng f0/0

===================================================================[Trace Route]
-------------------------------------------------------------------[Tracert]
C:\PC1> tracert 192.168.1.2
-------------------------------------------------------------------[Traceroute]
R1# traceroute 192.168.1.2
===================================================================[MAC]
Switch> show mac address-table
===================================================================[VLAN]
SW1(config)# vlan 2                              - Tạo VLAN 2
SW1(config-vlan)# name "IT"                      - Tên của VLAN 2 là IT
SW1(config)# int range f0/5-6                    - Đi vô port 5 và 6
SW1(config-if-range)# switchport access VLAN 2   - Port 5 và 6 thuộc VLAN 2
SW1# show vlan brief                             - Hiển thị VLAN và các port của VLAN
-------------------------------------------------------------------[NO VLAN]
SW1(config)# vlan 3
SW1(config-vlan)# name "HR"
SW1(config)# int range f0/7
SW1(config-if-range)# switchport access VLAN 3
SW1(config)# no vlan 3                           - Xóa VLAN 3 đi
Lưu ý: VLAN 3 có port là f0/7. Khi chúng ra xóa VLAN 3 đi thì port 7 trở thành port mồ côi. Và khi tạo lại VLAN 3 thì port 7 tự động quay lại VLAN 3.
-------------------------------------------------------------------[TRUNK]
SW1(config)# interface f0/1                           - Port này đang kết nối với Switch khác
SW1(config-if)# switchport trunk encapsulation dot1q
SW1(config-if)# switchport mode trunk
SW1# show interface trunk                             - Hiển thị các interface nào đang đóng vai trò TRUNK
-------------------------------------------------------------------[VTP]
SW1(config)# vtp mode server            - Mode server: tạo, xóa, sửa VLAN
SW2(config)# vtp mode client            - Mode client: sẽ có các VLAN từ server. Không làm được gì VLAN cả
SW3(config)# vtp mode transparent       - Mode transparent: tạo, xóa, sửa VLAN, nhưng ko gửi VLAN đi SW khác
SW1-2-3(config)# vtp domain cisco
SW1-2-3(config)# vtp password abc
SW1-2-3# show vtp status
SW1-2-3# show vtp password
SW1-2-3# show vlan brief
-------------------------------------------------------------------[Access Port]
SW1(config)# interface f0/9
SW1(config-if)# switchport mode access      - Port dành cho thiết bị cuối (PC)
SW1(config-if)# switchport access vlan 2    - Port thuộc VLAN 2
-------------------------------------------------------------------[Spanning-tree portfast]
SW1(config)# interface f0/9
SW1(config-if)# switchport mode access      - Cổng dành cho thiết bị cuối (PC)
SW1(config-if)# switchport access vlan 2    - Port thuộc VLAN 2
SW1(config-if)# spanning-tree portfast      - Giúp Forwarding đi nhanh hơn
===================================================================[Router on a Stick (ROAS)]
SWserver(config)# interface f0/3                              - Cổng mà Switch nối với Route
SWserver(config-if)# switchport mode trunk
Router(config)# interface f0/0                                - Cổng mà Router nối với Switch
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# interface f0/0.10                             - Tạo một Sub Interface cho VLAN 10
Router(config-subif)# encapsulation dot1Q 10                  - Liên kết Sub Interface với VLAN 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0   - Tạo IP cho Port f0/0.10
Router(config-subif)# no shutdown
Router(config)# interface f0/0.20                             - Tạo một Sub Interface cho VLAN 20
Router(config-subif)# encapsulation dot1Q 20                  - Liên kết Sub Interface với VLAN 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0   - Tạo IP cho Port f0/0.20
Router(config-subif)# no shutdown
Router(config)# interface f0/0.1                              - Tạo một Sub Interface cho VLAN 1
Router(config-subif)# encapsulation dot1Q 1 native            - Đặt mặc định cho VLAN 1 là native VLAN 
Router(config-subif)# ip address 10.0.0.1 255.0.0.0           - Tạo IP cho Port f0/0.1
Router(config-subif)# no shutdown

* Lưu ý: 
    - Khi mà có 3 SW: Client - Server - Transparent thì Transparent nối với Server phải có các VLAN giống nhau
    - Khi gửi dữ liệu qua mạng khác nhau như thì PC phải có Default Gateway
===================================================================[Switch Layer 3 SVI]
PC> 172.16.2.2/24 172.16.2.1                         - Đặt IP cho PC và trỏ về IP Interface VLAN 2 trên SW 
Switch(config)# hostname DS1
DS1(config)# int vlan 2                              - Đặt IP cho VLAN 2
DS1(config-if)# ip address 172.16.2.1 255.255.255.0
DS1(config-if)# no shutdown
DS1(config)# int f0/1                                - Access Port f0/1 cho VLAN 2
DS1(config-if)# switchport mode access
DS1(config-if)# switchport access VLAN 2
DS1(config)# int vlan 3                              - Đặt IP cho VLAN 3
DS1(config-if)# ip address 172.16.3.1 255.255.255.0
DS1(config-if)# no shutdown
DS1(config)# int f0/2                                - Access Port f0/2 cho VLAN 3
DS1(config-if)# switchport mode access
DS1(config-if)# switchport access VLAN 3
PC> ping 172.16.2.1                                  - Lúc này PC đã ping đc tới IP của VLAN 2 trên SW
DS1(config)# ip routing                              - Bật chức năng định tuyến trên switch layer 3
-------------------------------------------------------------------[C1: Assign IP SW connect Router]
DS1(config)# int f0/3                                - Port connect with Router
DS1(config-if)# no switchport                        - Apply Port f0/3 from layer 2 to layer 3
DS1(config-if)# ip address 192.168.1.2 255.255.255.0 - Assign IP Address for f0/3
DS1(config-if)# no shutdown
-------------------------------------------------------------------[C2: Assign IP VLAN 1]
DS1(config)# int f0/3
DS1(config-if)# switchport                           - Apply Port f0/3 from layer 3 to layer 2
DS1(config-if)# switchport mode access
DS1(config-if)# switchport access vlan 1
DS1(config)# int vlan 1
DS1(config-if)# ip address 192.168.1.2 255.255.255.0
DS1(config-if)# no shutdown
-------------------------------------------------------------------[Chọn cách làm ở trên xong làm tiếp]
DS1(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.1    - To allow VLAN 2 to connect to the Internet
Router(config)# hostname R1
R1(config)# int f0/0                                 - Port connect with Switch
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
R1(config)# ip route 172.16.2.0 255.255.255.0 192.168.1.2   - Routing to network vlan 2
R1(config)# ip route 172.16.3.0 255.255.255.0 192.168.1.2   - Routing to network vlan 3
===================================================================[DHCP]
RouterDHCP(config)# service dhcp                                    - Bật dịch vụ DHCP
RouterDHCP(config)# ip dhcp excluded-address 172.16.2.1 172.16.2.2  - Không cấp phát 2 IP này
RouterDHCP(config)# ip dhcp pool LAN1                               - Định nghĩa Pool tên là LAN1 cho LAN 1
RouterDHCP(dhcp-config)# network 172.16.2.0 255.255.255.0           - Đặt địa chỉ mạng
RouterDHCP(dhcp-config)# default-router 172.16.2.1                  - Trỏ về IP của RouterDHCP
RouterDHCP(dhcp-config)# dns-server 8.8.8.8
PC1> ipconfig /renew                                                
RouterDHCP# show ip dhcp binding                                    - Kiểm tra DHCP đã cấp IP nào
-------------------------------------------------------------------[IP Helper Address - Chưa làm đc]
RouterDHCP(config)# ip dhcp excluded-address 172.16.1.1             - Không cấp phát IP này
RouterDHCP(config)# ip dhcp pool LAN2
RouterDHCP(dhcp-config)# network 172.16.1.0 255.255.255.0
RouterDHCP(dhcp-config)# default-router 172.16.1.1
RouterDHCP(dhcp-config)# dns-server 8.8.8.8
R1(config)# interface f0/1                              - Port R1 nối với PCX thuộc LAN 2
R1(config-if)# ip helper-address 172.16.12.2            - Trỏ đến RouterDHCP sử dụng địa chỉ IP nào cũng được
PCX> ipconfig /renew                                    - PCX của R1 xin IP của RouterDHCP
-------------------------------------------------------------------[Cấu hình IP theo MAC - Chưa làm đc]
RouterDHCP(config)# ip dhcp server pool serverA
RouterDHCP(dhcp-config)# client-identifier 0060.7071.A49E
RouterDHCP(dhcp-config)# host 172.16.2.99 255.255.255.0
RouterDHCP(dhcp-config)# default-router 172.16.2.1
RouterDHCP(dhcp-config)# dns-server 8.8.8.8
===================================================================[STP (Spanning Tree Protocol)]
**** ĐIỀU KHIỂN BẦU CHỌN ROOT BRIDGE BẰNG PRIORITY ****
SW1(config)# spanning-tree vlan 10 priority 4096        - Ép SW1 làm Root Bridge cho VLAN 10 (Hạ Priority xuống thấp)
SW1(config)# spanning-tree vlan 20 priority 28672       - Đặt SW1 làm Root phụ cho VLAN 20
SW2(config)# spanning-tree vlan 20 priority 4096        - Ép SW2 làm Root Bridge cho VLAN 20 (Hạ Priority xuống thấp)
SW2(config)# spanning-tree vlan 10 priority 28672       - Đặt SW2 làm Root phụ cho VLAN 10
**** ĐIỀU KHIỂN CỔNG CHẶN (BLOCK PORT) BẰNG PATH COST ****
SW2(config)# interface f0/1                             - Vào cổng f0/1 của SW2 (Đường nối thẳng sang SW1)
SW2(config-if)# spanning-tree vlan 10 cost 100          - Tăng Cost VLAN 10 lên 100 để ép cổng f0/1 của SW2 thành cổng Block (X)
**** TỐI ƯU HÓA CỔNG KẾT NỐI USER (MÁY TÍNH CON) ****
Switch(config)# interface f0/10                         - Vào cổng kết nối trực tiếp với máy tính của người dùng (Host)
Switch(config-if)# spanning-tree portfast               - Bật tính năng PortFast giúp cổng mở ngay lập tức, không đợi 30 giây
**** LỆNH KIỂM TRA (CHẾ ĐỘ PRIVILEGE EXEC) ****
Switch# show spanning-tree vlan 10                      - Kiểm tra chi tiết trạng thái các cổng (Root, Desg, Altn) của VLAN 10
Switch# show spanning-tree vlan 20                      - Kiểm tra chi tiết trạng thái các cổng (Root, Desg, Altn) của VLAN 20
-------------------------------------------------------------------[STP - Chỉnh Priority - Chỉnh Cost]
LAB: 3 SW 1-2-3: kết nối với nhau, SW1 nối với R1, PC1 nối với SW2
SW1(config)# spanning-tree vlan 1 priority 4096          - SW 1 làm Root Bridge
SW1(config)# int range f0/2, f0/3
SW1(config-if)# switchport mode trunk
SW3(config)# int range f0/1, f0/2
SW3(config-if)# switchport mode trunk
SW2(config)# spanning-tree vlan 1 priority 61440         - SW 2 làm Alternated Port (Port bị khóa)
SW2(config)# int range f0/2, f0/3
SW2(config-if)# switchport mode trunk
SW2# show spanning-tree                                  - Kiểm tra Cost của các cổng f0/2 và f0/3
SW2(config)# int f0/3                                    - Để cổng f0/3 làm Blocking Port
SW2(config-if)# spanning-tree vlan 1 cost 200            - Chỉnh Cost cao hơn cổng f0/2
SW2# show spanning-tree vlan 1
-------------------------------------------------------------------[VTP - STP(Root Primary/Root Secondary)]
* LAB: 3 SW 1-2-3 kết nối với nhau
* Yêu cầu: - SW1 là server (tạo VLAN 10, 20), SW2 transparent (tạo VLAN 10, 20), SW3 là client
           - SW1 là Primary cho VLAN 10, là Secondary cho VLAN 20
           - SW3 là Primary cho VLAN 20, là Secondary cho VLAN 10
SW1-2-3(config)# interface range f0/1 - 2           - Vào cấu hình các cổng kết nối giữa các Switch
SW1-2-3(config-if-range)# switchport mode trunk     - Chuyển các cổng kết nối thành đường trung kế (Trunk)
**** CẤU HÌNH TRÊN SW1 (SERVER) ****
SW1(config)# vtp domain CiscoNet                    - Đặt tên miền VTP chung cho hệ thống là CiscoNet
SW1(config)# vtp password 123456                    - Đặt mật khẩu đồng bộ bảo mật cho nhóm VTP
SW1(config)# vtp mode server                        - Cấu hình SW1 làm Server (được quyền tạo/xóa VLAN)
SW1(config)# vlan 10                                - Tạo VLAN 10 trên hệ thống mạng
SW1(config-vlan)# vlan 20                           - Tạo VLAN 20 trên hệ thống mạng
**** CẤU HÌNH TRÊN SW2 (TRANSPARENT) ****
SW2(config)# vtp domain CiscoNet                    - Đặt tên miền VTP trùng với SW1
SW2(config)# vtp password 123456                    - Đặt mật khẩu VTP trùng với SW1
SW2(config)# vtp mode transparent                   - Cấu hình SW2 làm Transparent (chỉ chuyển tiếp, không nhận đồng bộ)
SW2(config)# vlan 10                                - Tạo VLAN 10 thủ công (vì chế độ Transparent không tự đồng bộ)
SW2(config-vlan)# vlan 20                           - Tạo VLAN 20 thủ công (vì chế độ Transparent không tự đồng bộ)
**** CẤU HÌNH TRÊN SW3 (CLIENT) ****      
SW3(config)# vtp domain CiscoNet                    - Đặt tên miền VTP trùng với SW1
SW3(config)# vtp password 123456                    - Đặt mật khẩu VTP trùng với SW1
SW3(config)# vtp mode client                        - Cấu hình SW3 làm Client (tự động nhận VLAN từ SW1)
**** CẤU HÌNH [STP (Root Primary / Secondary)] ****
SW1(config)# spanning-tree vlan 10 root primary         - Ép SW1 làm Root chính cho VLAN 10 (Tự hạ Priority xuống thấp)
SW1(config)# spanning-tree vlan 20 root secondary       - Đặt SW1 làm Root dự phòng cho VLAN 20
SW3(config)# spanning-tree vlan 20 root primary         - Ép SW3 làm Root chính cho VLAN 20 (Tự hạ Priority xuống thấp)
SW3(config)# spanning-tree vlan 10 root secondary       - Đặt SW3 làm Root dự phòng cho VLAN 10
Switch# show spanning-tree vlan 10                      - Kiểm tra xem SW1 đã thành Root của VLAN 10 chưa
Switch# show spanning-tree vlan 20                      - Kiểm tra xem SW3 đã thành Root của VLAN 20 chưa
-------------------------------------------------------------------[VTP - STP(Root Primary/Root Secondary) - ROAS]
* LAB: 3 SW 1-2-3 kết nối với nhau, SW1 kết nối với 1 Router, các PC có thể ping được nhau
* Yêu cầu:  - SW1 là server (tạo VLAN 10, 20), SW2 client, SW3 là client
            - SW1 là Primary cho VLAN 10, là Secondary cho VLAN 20
            - SW2 là Primary cho VLAN 20, là Secondary cho VLAN 10
            - Router thực hiện định tuyến cho VLAN 1, 10, 20
                + E0/0 VLAN 1 172.16.11.1/24
                + E0/0.10 VLAN 10 172.16.10.1/24
                + E0/0.20 VLAN 20 172.16.20.1/24
SW1-2-3(config)# interface range f0/1 - 2               - Vào cấu hình các cổng kết nối giữa các Switch
SW1-2-3(config-if-range)# switchport mode trunk         - Chuyển các cổng liên kết Switch thành đường Trunk
**** CẤU HÌNH TRÊN SW1 (SERVER) ****
SW1(config)# interface f0/24                            - Vào cổng f0/24 trên SW1 (Cổng kết nối lên Router)
SW1(config-if)# switchport mode trunk                   - Bắt buộc phải bật Trunking cho cổng nối lên Router
SW1(config-if)# exit
SW1(config)# vtp domain CiscoNet                        - Đặt tên miền VTP chung cho hệ thống là CiscoNet
SW1(config)# vtp password 123456                        - Đặt mật khẩu đồng bộ bảo mật cho nhóm VTP
SW1(config)# vtp mode server                            - Cấu hình SW1 làm Server để quản lý cơ sở dữ liệu VLAN
SW1(config)# vlan 10                                    - Tạo VLAN 10 trên hệ thống mạng
SW1(config-vlan)# vlan 20                               - Tạo VLAN 20 trên hệ thống mạng
**** CẤU HÌNH TRÊN SW2-3 (CLIENT) ****
SW2-3(config)# vtp domain CiscoNet                      - Đặt tên miền VTP trùng với SW1
SW2-3(config)# vtp password 123456                      - Đặt mật khẩu VTP trùng với SW1
SW2-3(config)# vtp mode client                          - Cấu hình SW2-3 làm Client để tự động đồng bộ VLAN 10, 20
**** CẤU HÌNH [STP (Root Primary / Secondary)]
SW1(config)# spanning-tree vlan 10 root primary         - Ép SW1 làm Root chính cho VLAN 10
SW1(config)# spanning-tree vlan 20 root secondary       - Đặt SW1 làm Root dự phòng cho VLAN 20
SW2(config)# spanning-tree vlan 20 root primary         - Ép SW2 làm Root chính cho VLAN 20
SW2(config)# spanning-tree vlan 10 root secondary       - Đặt SW2 làm Root dự phòng cho VLAN 10
**** LỆNH KIỂM TRA TRẠNG THÁI (CHẾ ĐỘ PRIVILEGE EXEC) ****
Switch# show spanning-tree vlan 10                      - Kiểm tra xem SW1 đã lên làm Root của VLAN 10 chưa
Switch# show spanning-tree vlan 20                      - Kiểm tra xem SW2 đã lên làm Root của VLAN 20 chưa

**** CẤU HÌNH[ROAS (Router-on-a-stick)]
Router(config)# interface ethernet 0/0                  - Vào cổng vật lý chính kết nối xuống SW1
Router(config-if)# no shutdown                          - Bật cổng vật lý chính (Cực kỳ quan trọng)
**** CẤU HÌNH CHO VLAN 1 (Cổng chính đóng vai trò Native VLAN) ****
Router(config-if)# ip address 172.16.11.1 255.255.255.0   - Cấu hình IP Gateway trực tiếp trên cổng chính cho VLAN 1
Router(config-if)# exit
**** CẤU HÌNH SUB-INTERFACE CHO VLAN 10 ****
Router(config)# interface ethernet 0/0.10                       - Tạo Sub-interface .10 cho VLAN 10
Router(config-subif)# encapsulation dot1Q 10                    - Đóng gói theo chuẩn dot1Q mã hóa cho VLAN 10
Router(config-subif)# ip address 172.16.10.1 255.255.255.0      - Cấu hình IP Default Gateway cho người dùng VLAN 10
Router(config-subif)# exit
**** CẤU HÌNH SUB-INTERFACE CHO VLAN 20 ****
Router(config)# interface ethernet 0/0.20                       - Tạo Sub-interface .20 cho VLAN 20
Router(config-subif)# encapsulation dot1Q 20                    - Đóng gói theo chuẩn dot1Q mã hóa cho VLAN 20
Router(config-subif)# ip address 172.16.20.1 255.255.255.0      - Cấu hình IP Default Gateway cho người dùng VLAN 20
Router(config-subif)# exit
**** LỆNH KIỂM TRA TRẠNG THÁI (CHẾ ĐỘ PRIVILEGE EXEC) ****
Router# show ip interface brief                                 - Kiểm tra trạng thái Up/Up và IP của các Sub-interface

**** GIẢ ĐỊNH THIẾT KẾ CỔNG NỐI VỚI PC: ****
* Cổng f0/10 trên các Switch: Dùng để nối vào PC thuộc VLAN 10
* Cổng f0/20 trên các Switch: Dùng để nối vào PC thuộc VLAN 20
SW1(config)# interface f0/10                    - Vào cổng f0/10 kết nối với PC 1 (VLAN 10)
SW1(config-if)# switchport mode access          - Cấu hình cổng chạy ở chế độ Access (kết nối thiết bị cuối)
SW1(config-if)# switchport access vlan 10       - Gán cổng này vào quản lý thuộc cấu trúc VLAN 10
SW1(config-if)# spanning-tree portfast          - Tối ưu hóa: Bật tính năng PortFast giúp PC có mạng ngay lập tức
SW1(config-if)# exit
**** CẤU HÌNH TRÊN SWITCH 2 (SW2) ****
SW2(config)# interface f0/20                    - Vào cổng f0/20 kết nối với PC 2 (VLAN 20)
SW2(config-if)# switchport mode access
SW2(config-if)# switchport access vlan 20       - Gán cổng này vào quản lý thuộc cấu trúc VLAN 20
SW2(config-if)# spanning-tree portfast
SW2(config-if)# exit
**** CẤU HÌNH TRÊN SWITCH 3 (SW3) ****
(Lặp lại cấu hình tương tự cho các cổng kết nối PC trên SW3 nếu có máy tính cắm vào)
SW3(config)# interface f0/10
SW3(config-if)# switchport mode access
SW3(config-if)# switchport access vlan 10
SW3(config-if)# spanning-tree portfast
SW3(config-if)# exit
**** LỆNH KIỂM TRA TRẠNG THÁI (CHẾ ĐỘ PRIVILEGE EXEC) ****
Switch# show vlan brief                         - Kiểm tra xem cổng f0/10, f0/20 đã hiện đúng cột VLAN 10, 20 chưa
===================================================================[EtherChannel]
SW1-2(config)# interface range f0/1, f0/2
SW1-2(config-if-range)# shutdown
SW1(config-if-range)# channel-group 1 mode on                  - Gom 2 Port 1-2 thành Po1
SW2(config-if-range)# channel-group 2 mode on                  - Gom 2 Port 1-2 thành Po2
SW1-2(config-if-range)# switchport trunk encapsulation dot1q
SW1-2(config-if-range)# switchport mode trunk
SW1-2(config-if-range)# no shutdown
SW1-2# show etherchannel summay                                  - Hiển thị thông tin Po2
SW1-2# show interface trunk
-------------------------------------------------------------------[Gỡ bỏ EtherChannel - LACP - PAgP]
SW1-2(config)# default interface f0/1
SW1-2(config)# default interface f0/2
SW1(config)# no interface port-channel 1
SW2(config)# no interface port-channel 2
-------------------------------------------------------------------[LACP]
SWA(config)# interface range f0/1, f0/2
SWA(config-if-range)# shutdown
SWA(config-if-range)# switchport mode trunk
SWA(config-if-range)# channel-protocol lacp
SWA(config-if-range)# channel-group 1 mode active         - Mode active chủ động gửi bản tin
SWA(config-if-range)# no shutdown
SWB(config)# interface range f0/1-2
SWB(config-if-range)# shutdown
SWB(config-if-range)# switchport mode trunk
SWB(config-if-range)# channel-protocol lacp
SWB(config-if-range)# channel-group 2 mode passive        - Mode passive đợi sw láng giềng gửi bản tin
SWB(config-if-range)# no shutdown
-------------------------------------------------------------------[PAgP - Độc quyền của Cisco]
SWC(config)# interface range f0/1, f0/2
SWC(config-if-range)# shutdown
SWC(config-if-range)# switchport mode trunk
SWC(config-if-range)# channel-protocol pagp
SWC(config-if-range)# channel-group 1 mode desirable     - Mode desirable chủ động gửi bản tin
SWC(config-if-range)# no shutdown
SWD(config)# interface range f0/1-2
SWD(config-if-range)# shutdown
SWD(config-if-range)# switchport mode trunk
SWD(config-if-range)# channel-protocol pagp
SWD(config-if-range)# channel-group 2 mode auto          - Mode auto đợi sw láng giềng gửi bản tin
SWD(config-if-range)# no shutdown
===================================================================[HSRP]
PC1> 172.16.0.4/24 172.16.0.3
PC2> 172.16.0.5/24 172.16.0.3
R1(config)# int e0/0
R1(config-if)# ip address 172.16.0.1 255.255.255.0      - Đặt địa chỉ IP cho R1
R1(config-if)# no shutdown
R1(config-if)# standby 1 ip 172.16.0.3                  - Tạo HSRP Group 1
R1(config-if)# standby 1 priority 20                    - Đặt priority cao hơn để làm Active
R1(config-if)# standby 1 preempt                        - Giành lại quyền khi R1 hoạt động trở lại
R2(config)# int e0/0
R2(config-if)# ip address 172.16.0.2 255.255.255.0      - Đặt địa chỉ IP cho R2
R2(config-if)# no shutdown
R2(config-if)# standby 1 ip 172.16.0.3                  - Tạo HSRP Group 1
R2(config-if)# standby 1 priority 10                    - Đặt priority thấp hơn để làm Standby
R2(config-if)# standby 1 preempt                        - Giành lại quyền khi R2 hoạt động trở lại
R1-2# show standby brief                                - Kiểm tra trạng thái của Router
PC1-2# ping 172.16.0.3                                  - Ping thành công đến R1
R1-2(config)# int e0/0
R1-2(config)# standby version 2                         - Cấu hình Virtual MAC v2 [0000.0C9F.F**XXX]
PC1-2# ping 172.16.0.3                                  - Ping thành công đến R1
PC1-2# arp -a                                           - Xem IP 172.16.0.3 đang ánh xạ tới HSRP
-------------------------------------------------------------------[Shutdown R1 -> R2 chiếm quyền làm Active]
R1(config)# int e0/0
R1(config-if)# shutdown
R2# show standby brief                                  - R2 chuyển từ Standby -> Active
-------------------------------------------------------------------[Track]
R1(config)# int e0/1
R1(config-if)# shutdown                             - cổng ra internet của R1 tắt
R1(config)# track 9 int e0/1 line-protocol          - Tạo Track số 9
R1(config)# int e0/0
R1(config-if)# standby 1 track 9 decrement 15       - HSRP Group 1 theo dõi Track 9, trừ đi 15 priority của R1
R1# show standby brief                              - Lúc này priority của R1 còn 5
R1(config)# int e0/1
R1(config-if)# no shutdown                          - bật lại cổng ra internet
R1# show standby brief                              - Lúc này priority của R1 là 20. Giành lại Active
===================================================================[Port Security]
Devices: PC1-2 or Switch or Router
PC1> 192.169.1.1/24 192.168.1.254
PC2> 192.169.1.2/24 192.168.1.254
Switch(config)# int vlan 1
Switch(config-if)# ip address 192.168.1.251 255.255.255.0
Switch(config-if)# no shutdown
Switch(config)# int range f0/0, f0/1, f0/2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 1
Switch(config-if-range)# spanning-tree portfast
Router(config)# int f0/0
Router(config-if)# ip address 192.168.1.254 255.255.255.0
Router(config-if)# no shutdown
-------------------------------------------------------------------[TH1: PC1 kết nối thành công với Port]
Switch(config)# int f0/2                                               - Port kết nối với PC1
Switch(config-if)# switchport port-security                            - Bật tính năng Port Security
Switch(config-if)# switchport port-security maximun 1                  - Chỉ 1 thiết bị(MAC address) được kết nối
Switch(config-if)# shutdown                                            - Shutdown Port trước khi cấu hình
Switch(config-if)# switchport port-security mac-address 0001.C79A.B58C - Cho phép MAC của PC1 được kết nối
Switch(config-if)# switchport port-security violation shutdown         - Mode (Shutdown) port khi khác MAC address
Switch(config-if)# no shutdown                                         - Bật lại Port
Switch# show port-security interface f0/2                              - Hiển thị cấu hình Port Security
PC1> ping 192.168.1.254                                                - Ping thành công đến Router
-------------------------------------------------------------------[TH2: PC1 kết nối không thành công với Port]
Switch(config)# int f0/2                                                  - Port kết nối với PC1
Switch(config-if)# no switchport port-security mac-address 0001.C79A.B58C - xóa cấu hình cũ
Switch(config-if)# switchport port-security mac-address 0001.C79A.B58D    - Cho phép MAC address được kết nối
PC1> ping 192.168.1.254                                                   - Ping không thành công đến Router
Switch#                                            - Hiển thị cảnh báo về kết nối Port và Shutdown Port
Switch# show interface f0/2 status                                        - Kiểm tra trạng thái của Port
-------------------------------------------------------------------[TH3: Router kết nối thành công với Port]
Switch(config)# int f0/1                                            - Port kết nối với Router
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximun 1
Switch(config-if)# switchport port-security mac-address sticky      - Địa chỉ MAC đầu tiên kết nối với Port
Switch(config-if)# switchport port-security violation restrict      - Mode (restrict)
Switch# show port-security address                                  - Kiểm tra MAC của Router có trong Port
Router# ping 192.168.1.1                                            - Ping thành công đến PC1
-------------------------------------------------------------------[TH4: Router kết nối không thành công với Port]
Router(config)# interface f0/0
Router(config-if)# mac-address 0000.aaaa.1001           - Thiết lập MAC address khác
Router# show interface f0/0                             - Kiểm tra MAC đã được cập nhật chưa
Router# ping 192.168.1.1                                - Ping không thành công đến PC1
Switch#                                                 - Hiển thị cảnh báo về kết nối Port và hủy gói tin
===================================================================[BPDU Gurad]

===================================================================[DHCP Snooping]

===================================================================[DTP - VLAN Hooping]

===================================================================[IP Source Guard]

===================================================================[DAI (Dymanic ARP Inspection)]

===================================================================[SPAN (Switch Port Analyzer)]

===================================================================[AAA]
Devices: PC1-2 or Switch or Router
PC1> 192.169.1.1/24 192.168.1.254
PC2> 192.169.1.2/24 192.168.1.254
Switch(config)# int vlan 1
Switch(config-if)# ip address 192.168.1.251 255.255.255.0
Switch(config-if)# no shutdown
Switch(config)# int range f0/0, f0/1, f0/2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 1
Switch(config-if-range)# spanning-tree portfast
Router(config)# int f0/0
Router(config-if)# ip address 192.168.1.254 255.255.255.0
Router(config-if)# no shutdown
Router(config)# hostname R1
R1(config)# enable password 123456
-------------------------------------------------------------------[Bật cơ chế xác thực AAA]
R1(config)# aaa new-model                                       - Bật cơ chế xác thực AAA
R1(config)# aaa authentication login Local_AAA local            - Sử dụng username và password khai báo cục bộ
R1(config)# username admin privilege 15 password cisco 123      - Account có đặc quyền cao nhất là 15
R1(config)# username subadmin privilege 1 password ccna 123     - Account có đặc thấp nhất là 1
R1(config)# line vty 0 4                                        - Cho phép 5 người kết nối
R1(config-line)# login authentication Local_AAA
-------------------------------------------------------------------[Telnet]
Switch> ping 192.168.1.254              - ping được đến Router
Switch> telnet 192.168.1.254            - Telnet được đến Router
Username:admin                          - Sử dụng account có đặc quyền cao nhất
Password:
R1>
-------------------------------------------------------------------[Authorization (Phân quyền)]
R1(config)# privilege exec level 8 show running-config     - level 8 chỉ được show running-config
R1(config)# privilege exec level 8 configure terminal      - level 8 được vào configure terminal
R1(config)# privilege configure level 8 interface          - level 8 được vào các interface
R1(config)# privilege interface level 8 ip address         - level 8 cấu hình được IP
R1(config)# privilege interface level 8 no shutdown        - level 8 cấu hình được no shutdown
R1(config)# enable secret level 8 ccna123                  - Đặt mật khẩu cho level 8
Switch> telnet 192.168.1.254
Username: subadmin                                         - Sử dụng account có đặc quyền thấp nhất
Password:
R1> enable 8
Password:
R1# show privilege                                         - Current privilege level is 8
-------------------------------------------------------------------[Accounting (Ghi nhận và giám sát)]
R1(config)# archive                         - Bật cơ chế Accounting
R1(config-archive)# log config
R1(config-archive-log-cfg)# logging enable
R1# show archive log config all
===================================================================[Static Router - Giao thức định tuyến tĩnh]
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.1      - Mặc định giá trị AD = 1
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.4.1      - Mặc định giá trị AD = 1
-------------------------------------------------------------------[Static Router - AD (Administrative Distance)]
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.1 5     - Giá trị AD càng thấp độ ưu tiên càng cao
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.4.1 10
* Tại một thời điểm: Bảng định tuyến (Routing Table) chỉ lưu duy nhất tuyến đường chính có AD thấp hơn (AD = 5). Tuyến đường dự phòng (AD = 10) bị ẩn đi và lưu trong bộ nhớ cấu hình (Running-config).
-------------------------------------------------------------------[Static Router - AD - IP SLA]
R1(config)# ip sla 1                                        - Tạo SLA số thứ tự là 1
R1(config-ip-sla)# icmp-echo 192.168.2.1                    - Gửi gói tin đến IP 192.168.2.1
R1(config-ip-sla-echo)# frequency 5                         - Chu kỳ 5 giây một lần
R1(config)# ip sla schedule 1 life forever start-time now   - Bật cho tiến trình này chạy ngay lập tức 
                                                                và không bao giờ hết hạn
R1(config)# track 10 ip sla 1 reachability          - Tạo Track số 10. 
                                                    - Nếu IP SLA 1 ping thông, Track 10 sẽ ở trạng thái Up
                                                    - Nếu ping thất bại, Track 10 sẽ chuyển sang Down
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.2.1 5 track 10   - Áp dụng Track
R1(config)# ip route 192.168.3.0 255.255.255.0 192.168.4.1 10
===================================================================[Dynamic Router - RIP(120) - NAT - ACL]
* Devices: 3 Router kết nối với 1 Switch
* R1: G0/0 172.16.1.1/24 - Loopback1 192.168.1.1/26
* R2: G0/0 172.16.1.2/24 - Loopback1 192.168.1.65/26
* R3: G0/0 172.16.1.3/24 - Loopback0 10.3.0.1/24 - Loopback1 10.3.1.1/24 - Loopback2 10.3.2.1/24
R1-2-3(config)# router rip                                - Kích hoạt giao thức định tuyến RIP
R1-2-3(config-route)# version 2                           - Sử dụng RIP phiên bản 2 (Hỗ trợ chia Subnet/Classless)
R1-2-3(config-route)# no auto-summary                     - Tắt tính năng tự động tóm tắt mạng về Classful
R1(config-route)# network 172.16.0.0                      - Quảng bá dải mạng kết nối chung giữa các Router
R1(config-route)# network 192.168.1.0                     - Quảng bá dải mạng Loopback 1 (192.168.1.0/26)
R2(config-route)# network 172.16.0.0                      - Quảng bá dải mạng kết nối chung giữa các Router
R2(config-route)# network 192.168.1.0                     - Quảng bá dải mạng Loopback 1 (192.168.1.64/26 - Gõ mạng gốc lớp C)
R3(config-route)# network 172.16.0.0                      - Quảng bá dải mạng kết nối chung giữa các Router
R3(config-route)# network 10.0.0.0                        - Quảng bá toàn bộ các dải Loopback 0, 1, 2 (Mạng gốc lớp A)
R1-2-3# show ip route rip                                 - Kiểm tra xem các Router đã học được mạng của nhau chưa (Ký hiệu chữ R)
**** [CẤU HÌNH NAT & ACL RA INTERNET TRÊN R2] ****
R2(config)# interface gigabitEthernet 0/1                 - Vào cổng G0/1 (Cổng kết nối ra Modem/Internet)
R2(config-if)# ip address dhcp                            - Cấu hình nhận IP tự động từ nhà mạng (ISP)
R2(config-if)# ip nat outside                             - Đánh dấu đây là cổng hướng ra ngoài Internet
R2(config-if)# no shutdown
R2(config-if)# exit
R2(config)# interface gigabitEthernet 0/0                 - Vào cổng G0/0 (Cổng kết nối vùng mạng nội bộ LAN)
R2(config-if)# ip nat inside                              - Đánh dấu đây là cổng hướng vào trong mạng nội bộ
R2(config-if)# exit
R2(config)# access-list 1 permit 172.16.1.0 0.0.0.255     - (Tối ưu bảo mật) Chỉ cho phép dải mạng nội bộ này được NAT
R2(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R2(config)# access-list 1 permit 10.3.0.0 0.0.255.255
*(Nếu muốn cho phép tất cả mọi dải mạng, gõ: R2(config)# access-list 1 permit any)*
R2(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload  - Kích hoạt NAT Overload (PAT) qua cổng G0/1
**** [CẤP ĐƯỜNG ĐI INTERNET CHO R1 VÀ R3] ****
R2(config)# router rip
R2(config-router)# default-information originate           - Tự động phát dải Default Route (0.0.0.0/0) qua RIP cho R1 và R3
R1-3# show ip route                                        - Sẽ thấy xuất hiện dòng đường đi cuối: R* 0.0.0.0/0 [120/1]...
R1-3# ping 8.8.8.8                                         - Thử nghiệm ping ra một IP ngoài Internet (Ví dụ DNS Google)
R2# show ip nat translations                               - Xem bảng chuyển đổi địa chỉ NAT từ IP nội bộ sang IP công cộng
-------------------------------------------------------------------[CẤU HÌNH NÂNG CAO CHO RIPv2]
1. CẤU HÌNH CỔNG THỤ ĐỘNG (Passive-interface) - Bắt buộc cho cổng LAN/Loopback
- Mặc định, RIP sẽ liên tục gửi gói tin cập nhật định tuyến (mỗi 30 giây) ra TẤT CẢ các cổng tham gia lệnh network. 
- Việc gửi gói tin RIP ra cổng nối với PC (LAN) hoặc cổng Loopback là vô ích, làm tốn băng thông và tạo cơ hội cho hacker gửi gói tin RIP giả mạo vào Router.
--- CẤU HÌNH TRÊN R1 ---
R1(config)# router rip
R1(config-router)# passive-interface Loopback1          - Chặn không cho RIP gửi gói tin cập nhật ra cổng Loopback1
--- CẤU HÌNH TRÊN R3 ---
R3(config)# router rip
R3(config-router)# passive-interface Loopback0          - Chặn gửi gói tin RIP ra Loopback0
R3(config-router)# passive-interface Loopback1          - Chặn gửi gói tin RIP ra Loopback1
R3(config-router)# passive-interface Loopback2          - Chặn gửi gói tin RIP ra Loopback2
2. XÁC THỰC BẢO MẬT (RIP Authentication)
- Mặc định, RIP không có bảo mật. Nếu ai đó cắm một Router lạ vào mạng và bật RIP trùng tên dải mạng, Router của bạn sẽ tự động học các tuyến đường bậy bạ đó. 
- Để an toàn, các Router kết nối trực tiếp với nhau cần phải "khớp" mật khẩu mã hóa MD5 thì mới chịu học định tuyến của nhau.
--- CẤU HÌNH TRÊN R1 (Cổng G0/0 nối sang R2, R3) ---
R1(config)# key chain RIP_KEY                           - Tạo một chuỗi chìa khóa tên là RIP_KEY
R1(config-keychain)# key 1                              - Tạo mã chìa khóa số 1
R1(config-keychain-key)# key-string ChuanCisco123       - Đặt mật khẩu là ChuanCisco123
R1(config-keychain-key)# exit
R1(config-keychain)# exit
R1(config)# interface gigabitEthernet 0/0               - Vào cổng vật lý kết nối chung
R1(config-if)# ip rip authentication mode md5           - Bật chế độ mã hóa mật khẩu bảo mật MD5
R1(config-if)# ip rip authentication key-chain RIP_KEY  - Áp dụng chuỗi khóa vừa tạo vào cổng này
--- CẤU HÌNH TRÊN R2 VÀ R3 ---
(Bạn thực hiện tạo key chain trùng tên/mật khẩu và áp dụng vào cổng G0/0 tương tự như trên R1)
===================================================================[Dynamic Router - OSPF(110)]
* Devices: Router 1: nối với PC 1(G0/1: 172.16.1.1/24), nối với Switch 1 (G0/0: 192.168.0.1/24)
           Router 2: nối với PC 2(G0/1: 172.16.2.1/24), nối với Switch 1 (G0/0: 192.168.0.2/24)
           Router 3: nối với PC 3(G0/1: 172.16.3.1/24), nối với Switch 1 (G0/0: 192.168.0.3/24)
**** CÁCH 1: DÙNG LỆNH NETWORK ****
R1-2-3(config)# router ospf 1
R1-2-3(config-router)# network 192.168.0.0 0.0.0.255 area 0     - Bật OSPF trên cổng G0/0 (Nối vào Switch 1)
R1(config-router)# router-id 0.0.0.1                            - Định danh định tuyến của R1 là 0.0.0.1
R2(config-router)# router-id 0.0.0.2                            - Định danh định tuyến của R2 là 0.0.0.2
R3(config-router)# router-id 0.0.0.3                            - Định danh định tuyến của R3 là 0.0.0.3
R1(config-router)# network 172.16.1.0 0.0.0.255 area 0          - Bật OSPF trên cổng G0/1 (Mạng PC 1)
R2(config-router)# network 172.16.2.0 0.0.0.255 area 0          - Bật OSPF trên cổng G0/1 (Mạng PC 2)
R3(config-router)# network 172.16.3.0 0.0.0.255 area 0          - Bật OSPF trên cổng G0/1 (Mạng PC 3)
**** CÁCH 2: VÀO THẲNG INTERFACE ****
*(Cách này chuyên nghiệp và ít lỗi hơn cách 1, không cần gõ lệnh network trong router ospf)*
R1-2-3(config)# interface gigabitEthernet 0/0       - Khi cả 3 đầu cổng cùng bật, OSPF mới thiết lập láng giềng thành công
R1-2-3(config-if)# ip ospf 1 area 0                 - Kích hoạt trực tiếp OSPF 1 vùng 0 ngay trên cổng này
R1-2-3(config)# interface gigabitEthernet 0/1
R1-2-3(config-if)# ip ospf 1 area 0
R1-2-3# show ip ospf interface brief                - Thấy được cổng (G0/0 - G0/1) đã tham gia định tuyến OSPF
R1-2-3# show ip ospf neighbor                       - Kiểm tra xem trạng thái đã lên FULL với các Router khác chưa
R1-2-3# show ip route ospf                          - Hiển thị bảng định tuyến, các mạng học từ OSPF sẽ có chữ O ở đầu
R2# ping 172.16.1.1                                 - ping thành công
R2# ping 172.16.1.1 source 172.16.2.1               - ping thành công
-------------------------------------------------------------------[Dynamic Router - OSPF(110) - DR - BDR - DROTHER]
* Mặc định: Router nào có Router-ID lớn nhất sẽ làm DR (R3 làm DR, R2 làm BDR, R1 làm DROTHER).
* Yêu cầu: R1-DROTHER, R2-BDR, R3-DR, dựa trên thông số Priority (Mặc định priority = 1).
R1-2-3# show ip ospf interface G0/0                 - Lúc này thấy R1 là DR, R2 là BDR, R3 là DROTHER
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip ospf priority 0                   - Priority = 0 nghĩa là từ bỏ quyền ứng cử, luôn luôn là DROTHER
R3(config)# interface gigabitEthernet 0/0
R3(config-if)# ip ospf priority 30                  - Tăng Priority lớn nhất mạng để giành quyền làm DR
R1-2-3# clear ip ospf process                       - RESET TIẾN TRÌNH TRÊN CẢ 3 ROUTER ĐỂ BẦU CHỌN LẠI
*(Hệ thống hỏi [no]: gõ Y và ấn Enter để đồng ý)*
-------------------------------------------------------------------[Dynamic Router - OSPF(110) - Chỉnh Cost]
* Chỉ sử dụng khi R1 đến R3 có 2 đường kết nối, ta sẽ chọn đường tối ưu hơn để chỉnh Cost
* Nguyên tắc OSPF: Ưu tiên đường đi có tổng Cost thấp nhất (Metric = Cost cổng ra).
* Cổng GigabitEthernet mặc định có Cost = 1.
R1# show ip route ospf                      - Thấy Metric của mạng R2 là [110/2] (1 của cổng ra R1 + 1 của cổng ra R2)
R1(config)# interface gigabitEthernet 0/0
R1(config-if)# ip ospf cost 20              - Tăng cấu hình Cost cổng này lên 20
R1# show ip route ospf                      - Lúc này Metric tăng lên thành [110/21] vì Cost gốc đã thay đổi
===================================================================[ACL (Access Control List) - STANDARD ACL]
* STANDARD ACL (Chỉ lọc dựa vào địa chỉ IP NGUỒN - Số hiệu từ 1 đến 99)
* Kịch bản: Chặn không cho máy PC1 (192.168.1.50) đi vào vùng mạng Kế toán (10.0.0.0/24). Các máy khác đi bình thường.
Router(config)# access-list 10 deny host 192.168.1.50   - Cấm đích danh địa chỉ IP của máy PC1
Router(config)# access-list 10 permit any               - Lệnh sống còn! Nếu không có lệnh này, mọi IP khác đều bị cấm hết sạch
* Áp dụng vào cổng (Đặt gần ĐÍCH - Tức là cổng ra hướng vào mạng Kế toán trên Router):
Router(config)# interface gigabitEthernet 0/2
Router(config-if)# ip access-group 10 out               - Lọc dữ liệu theo hướng đi ra khỏi cổng để vào mạng Kế toán
-------------------------------------------------------------------[ACL - EXTENDED ACL]
* EXTENDED ACL (Lọc chi tiết: IP Nguồn, IP Đích, Loại giao thức TCP/UDP, Số hiệu Port - Số hiệu từ 100 đến 199)
* Kịch bản: Cấm toàn bộ mạng LAN (192.168.1.0/24) không được truy cập Web (Port 80, 443) của Server (10.0.0.100), nhưng vẫn cho phép Ping thoải mái.
Router(config)# access-list 100 deny tcp 192.168.1.0 0.0.0.255 host 10.0.0.100 eq 80   - Cấm giao thức HTTP (web thường)
Router(config)# access-list 100 deny tcp 192.168.1.0 0.0.0.255 host 10.0.0.100 eq 443  - Cấm giao thức HTTPS (web bảo mật)
Router(config)# access-list 100 permit ip any any      - Cho phép tất cả các loại dữ liệu khác (như ping, file) chạy bình thường
* Áp dụng vào cổng (Đặt gần NGUỒN - Tức là cổng tiếp nhận dữ liệu từ vùng mạng LAN đi vào Router):
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip access-group 100 in              - Lọc dữ liệu ngay khi nó vừa đi vào cổng Router từ mạng LAN
-------------------------------------------------------------------[ACL - NAMED ACL]
* NAMED ACL (ACL đặt bằng tên chữ thay vì số - Giúp dễ quản lý và chỉnh sửa xóa dòng lẻ được)
Router(config)# ip access-list extended CHAN_WEB_NHAN_VIEN - Tạo một bảng ACL mở rộng đặt tên dạng chữ dễ nhớ
Router(config-ext-nacl)# deny tcp any host 8.8.8.8 eq www  - Cấm mọi máy truy cập trang web có IP 8.8.8.8
Router(config-ext-nacl)# permit ip any any                 - Cho phép tất cả các luồng mạng còn lại
Router(config-ext-nacl)# exit
Router# show access-lists                                   - Hiển thị danh sách các bảng ACL đang có và số lượt gói tin bị chặn
===================================================================[NAT (Network Address Translation) - STATIC NAT]
* STATIC NAT (Ánh xạ tĩnh 1-1: Dùng public Server nội bộ cho Internet bên ngoài truy cập vào)
* Kịch bản: Ánh xạ Web Server nội bộ (IP Private: 192.168.1.100) ra một IP Public cố định (203.162.1.10) do ISP cấp.
Router(config)# ip nat inside source static 192.168.1.100 203.162.1.10   - Khai báo ánh xạ cứng 1 Private sang 1 Public
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside                                         - Chỉ định cổng này quay mặt vào mạng LAN nội bộ
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside                                        - Chỉ định cổng này quay mặt ra ngoài Internet (IS
-------------------------------------------------------------------[NAT & ACL - NAT OVERLOAD/PAT]
* NAT OVERLOAD / PAT (Ánh xạ nhiều IP Private dùng chung 1 IP Public của cổng WAN - Phân biệt bằng số hiệu Port)
* Kịch bản: Cho phép toàn bộ mạng LAN nội bộ (192.168.1.0/24) đi ra Internet qua IP của cổng kết nối trực tiếp với ISP (int G0/1)
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255   - Dùng Standard ACL để chọn dải IP nội bộ được phép NAT
Router(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside                         - Cổng cắm vào Switch của mạng nội bộ LAN
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside                        - Cổng kết nối trực tiếp với nhà mạng ISP
-------------------------------------------------------------------[NAT & ACL - TROUBLESHOOTING & KIỂM TRA LỖI]
* Các lệnh kiểm tra trạng thái hoạt động trên chế độ Mode Privilege (Router#)
Router# show ip nat translations              - Hiển thị bảng tra cứu dịch mã IP đang chạy trong thời gian thực
Router# show ip nat statistics                - Hiển thị các thông số cấu hình và số lượng gói tin đã được NAT
Router# clear ip nat translation *            - Lệnh xóa sạch bảng NAT hiện tại để ép Router dịch lại từ đầu
Router# show access-lists                     - Xem chi tiết các dòng ACL đang hoạt động và số lượng gói tin bị chặn (match)
===================================================================[BÀI LAB TỔNG HỢP: OSPF & ACL & NAT & TELNET]
* KỊCH BẢN HỆ THỐNG MẠNG:
  - Doanh nghiệp có 2 vùng mạng: Vùng Quản trị (192.168.10.0/24) và Vùng Nhân viên (192.168.20.0/24).
  - Kết nối định tuyến nội bộ giữa các Router Core bằng giao thức OSPF (Area 0).
  - Router Biên (Gateway) kết nối ra Internet với ISP qua cổng GigabitEthernet 0/1 (IP Public: 200.0.0.3/24).
* YÊU CẦU CẤU HÌNH BẢO MẬT & ĐỊNH TUYẾN:
  1. OSPF: Cấu hình OSPF chạy trong mạng nội bộ để các vùng nhìn thấy nhau.
  2. NAT Overload: Cho phép vùng Nhân viên và Quản trị đi Internet thông qua IP của cổng G0/1.
  3. Extended ACL: Ngăn vùng Nhân viên (192.168.20.0/24) không được truy cập vào Vùng Quản trị (192.168.10.0/24).
  4. Telnet & ACL: Chỉ cho phép duy nhất máy Admin (192.168.10.50) được Telnet vào cấu hình Router. Cấm tất cả các IP khác.
-------------------------------------------------------------------[PHẦN 1: CẤU HÌNH ĐỊNH TUYẾN DÒNG OSPF]
* Cấu hình trên Router biên để thiết lập định tuyến với các mạng bên trong:
Router(config)# router ospf 1                                   - Khởi chạy tiến trình OSPF với Process ID là 1
Router(config-router)# router-id 1.1.1.1                        - Đặt định danh thủ công cho Router là 1.1.1.1
Router(config-router)# network 192.168.10.0 0.0.0.255 area 0    - Quảng bá dải mạng Quản trị vào vùng Trung tâm Area 0
Router(config-router)# network 192.168.20.0 0.0.0.255 area 0    - Quảng bá dải mạng Nhân viên vào vùng Trung tâm Area 0
Router(config-router)# exit
-------------------------------------------------------------------[PHẦN 2: CẤU HÌNH EXTENDED ACL - CẤM NHÂN VIÊN SANG QUẢN TRỊ]
* Sử dụng Extended ACL (Đặt gần NGUỒN - Cổng tiếp nhận lưu lượng từ mạng Nhân viên đi vào Router):
Router(config)# access-list 120 deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255 - Cấm mạng Nhân viên đi sang mạng Quản trị
Router(config)# access-list 120 permit ip any any        - Cho phép Nhân viên đi ra các hướng khác (Internet)
Router(config)# interface gigabitEthernet 0/2            - Cổng nối với vùng mạng Nhân viên
Router(config-if)# ip access-group 120 in                - Chặn ngay lập tức khi gói tin từ mạng Nhân viên gửi lên
-------------------------------------------------------------------[PHẦN 3: CẤU HÌNH NAT OVERLOAD (PAT) RA INTERNET]
* Cho phép cả 2 mạng LAN nội bộ dịch mã IP Public đi ra ngoài mạng Internet qua cổng G0/1 kết nối ISP:
Router(config)# access-list 1 permit 192.168.0.0 0.0.255.255            - Tạo Standard ACL gom toàn bộ dải IP Private 192.168.x.x
Router(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload
Router(config)# interface gigabitEthernet 0/0                             - Cổng kết nối xuống Switch mạng Quản trị
Router(config-if)# ip nat inside
Router(config)# interface gigabitEthernet 0/2                             - Cổng kết nối xuống Switch mạng Nhân viên
Router(config-if)# ip nat inside
Router(config)# interface gigabitEthernet 0/1                             - Cổng kết nối trực tiếp ra ngoài nhà mạng ISP
Router(config-if)# ip nat outside
Router(config)# exit
-------------------------------------------------------------------[PHẦN 4: BẢO MẬT TELNET BẰNG STANDARD ACL]
* Bước 1: Tạo Standard ACL chỉ định đích danh IP của máy Admin được phép truy cập:
Router(config)# access-list 5 permit host 192.168.10.50  - Chỉ cho duy nhất máy Admin qua cổng lọc
* Lưu ý: Mặc định cuối ACL 5 có lệnh ẩn 'deny any', nên các IP khác kết nối Telnet sẽ bị Router từ chối ngay lập tức.
* Bước 2: Bật tính năng Telnet trên các đường ảo (Line VTY) và áp dụng ACL kiểm soát đầu vào:
Router(config)# line vty 0 4                             - Mở 5 phiên kết nối từ xa cùng lúc (từ phiên số 0 đến 4)
Router(config-line)# password Admin@123                  - Đặt mật khẩu đăng nhập Telnet cho Router
Router(config-line)# login                               - Yêu cầu xác thực mật khẩu khi có người kết nối
Router(config-line)# access-class 5 in                   - Áp dụng ACL số 5 để lọc các IP muốn đăng nhập vào
Router(config-line)# exit





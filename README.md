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
[Dynamic Router - Giao thức định tuyến động RIP]

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
R1(config-if)# ip helper-address 172.16.12.2            - Trở đến RouterDHCP sử dụng địa chỉ IP nào cũng được
PCX> ipconfig /renew                                    - PCX của R1 xin IP của RouterDHCP
-------------------------------------------------------------------[Cấu hình IP theo MAC - Chưa làm đc]
RouterDHCP(config)# ip dhcp server pool serverA
RouterDHCP(dhcp-config)# client-identifier 0060.7071.A49E
RouterDHCP(dhcp-config)# host 172.16.2.99 255.255.255.0
RouterDHCP(dhcp-config)# default-router 172.16.2.1
RouterDHCP(dhcp-config)# dns-server 8.8.8.8
===================================================================[STP (Spanning Tree Protocol)]
Switch(config)# spanning-tree priority 32768            - Can thiệp vào giá trị Priority của SW
Switch(config-if)# spanning-tree cost 20                - Can thiệp vào giá trị Path Cost của Port
Switch(config-if)# spanning-tree portfast               - Chuyển trạng thái Forwarding
SW# show spanning-tree                                  - Kiểm tra Cost của các cổng
SW1(config)# spanning-tree vlan 1 priority 20480        - Can thiệp vào giá trị Priority của VLAN 1
SW1(config)# spanning-tree vlan 2 priority 32768        - Can thiệp vào giá trị Priority của VLAN 2
SW1(config-if)# spanning-tree vlan 1 cost 10            - Can thiệp vào giá trị Path Cost của VLAN 1
SW1(config-if)# spanning-tree vlan 2 cost 20            - Can thiệp vào giá trị Path Cost của VLAN 2
SW1# show spanning-tree vlan 1                          - Để biết được thiết bị nào Root Bridge
-------------------------------------------------------------------[STP]
LAB: 3 SW 1-2-3: kết nối với nhau, SW1 nối với R1, PC1 nối với SW2
SW1(config)# spanning-tree vlan 1 priority 4096          - SW 1 làm Root Bridge
SW1(config)# int range f0/2, f0/3
SW1(config-if)# switchport mode trunk
SW3(config)# int range f0/1, f0/2
SW3(config-if)# switchport mode trunk
SW2(config)# spanning-tree vlan 1 priority 61440         - SW 2 làm Alternated Port (Port bị khóa)
SW2(config)# int range f0/2, f0/3
SW2(config-if)# switchport mode trunk
-------------------------------------------------------------------[STP - VLAN]

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
===================================================================[Dynamic Router - RIP(120)]
R1# show ip route rip           - Hiển thị







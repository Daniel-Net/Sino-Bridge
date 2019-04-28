H3C BFD MAD检测方式的IRF典型配置  
一．组网需求  
   由于网络规模迅速扩大，当前中心交换机（Device A）转发能力已经不能满足需求，现需要在保护现有投资的基础上将网络转发能力提高一倍，并要求网络易管理、易维护。  
二．BFD检测机制  
BFD（Bidirectional Forwarding Detection，双向转发检测）协议用于快速检测、监控网络中链路或者IP路由的转发连通状况，保证邻居之间能够快速检测到通信故障。BFD MAD就是利用BFD技术来实现MAD快速检测。  
采用BFD MAD检测时，需要在IRF的成员设备之间搭建BFD检测链路，该链路可以在成员设备间直接连接，也可以通过其他设备进行透传。另外，还要创建一个VLAN接口作为BFD MAD检测VLAN，并为每台成员设备配置不同的MAD IP地址，与成员设备编号进行绑定，用于成员设备间BFD检测及分裂后的竞选。  
  
  
三.配置步骤  
(1)  配置设备编号  
# Device A保留缺省编号为1，不需要进行配置。  
# 在Device B上将设备的成员编号修改为2。  
system-view   
[DeviceB] irf member 1 renumber 2   
Warning: Renumbering the switch number may result in configuration change or loss. Continue? [Y/N]:y   
[DeviceB](2)  将两台设备断电后，按图1-14所示连接IRF链路，然后将两台设备上电。  
# 在Device A上创建设备的IRF端口2，与物理端口Ten-GigabitEthernet1/1/2绑定，并保存配置。  
system-view  
[DeviceA] interface ten-gigabitethernet 1/1/2   
[DeviceA-Ten-GigabitEthernet1/1/2] shutdown   
[DeviceA] irf-port 1/2   
[DeviceA-irf-port1/2] port group interface ten-gigabitethernet 1/1/2   
[DeviceA-irf-port1/2] quit   
[DeviceA] interface ten-gigabitethernet 1/1/2   
[DeviceA-Ten-GigabitEthernet1/1/2] undo shutdown   
[DeviceA-Ten-GigabitEthernet1/1/2] save   
# 在Device B上创建设备的IRF端口1，与物理端口Ten-GigabitEthernet2/1/1绑定，并保存配置。  
system-view   
[DeviceB] interface ten-gigabitethernet 2/1/1   
[DeviceB-Ten-GigabitEthernet2/1/1] shutdown   
[DeviceB] irf-port 2/1   
[DeviceB-irf-port2/1] port group interface ten-gigabitethernet 2/1/1   
[DeviceB-irf-port2/1] quit   
[DeviceB] interface ten-gigabitethernet 2/1/1   
[DeviceB-Ten-GigabitEthernet2/1/1] undo shutdown   
[DeviceB-Ten-GigabitEthernet2/1/1] save   
# 激活DeviceA的IRF端口配置。  
[DeviceA-Ten-GigabitEthernet1/1/2] quit   
[DeviceA] irf-port-configuration active   
# 激活DeviceB的IRF端口配置。  
[DeviceB-Ten-GigabitEthernet2/1/1] quit   
[DeviceB] irf-port-configuration active  
  
(3)  两台设备间将会进行Master竞选，竞选失败的一方将自动重启，重启完成后，IRF形成，系统名称统一为DeviceA。  
  
(4)  配置BFD MAD检测  
# 创建VLAN 3，并将Device A上的端口GigabitEthernet1/0/1和Device B上的端口GigabitEthernet2/0/1加入VLAN中。  
system-view   
[DeviceA] vlan 3   
[DeviceA-vlan3] port gigabitethernet 1/0/1 gigabitethernet 2/0/1   
[DeviceA-vlan3] quit   
# 创建VLAN接口3，并配置MAD IP地址。  
[DeviceA] interface vlan-interface 3   
[DeviceA-Vlan-interface3] mad bfd enable   
[DeviceA-Vlan-interface3] mad ip address 192.168.2.1 24 member 1   
[DeviceA-Vlan-interface3] mad ip address 192.168.2.2 24 member 2   
[DeviceA-Vlan-interface3] quit   
# 按图1-14所示连接BFD MAD链路。  
# 因为BFD MAD和生成树功能互斥，所以在GigabitEthernet1/0/1和GigabitEthernet2/0/1上关闭生成树协议。  
[Sysname] interface gigabitethernet 1/0/1   
[Sysname-gigabitethernet1/0/1] undo stp enable   
[Sysname-gigabitethernet1/0/1] quit   
[Sysname] interface gigabitethernet 2/0/1   
[Sysname-gigabitethernet2/0/1] undo stp enable  
  
四．注意  
用于BFD MAD检测的接口以及BFD MAD检测链路上的端口必须为BFD MAD功能专用，不能传输业务数据，也不能配置包括ARP、LACP在内的所有的二层或三层协议应用。  
如果网络中存在多个IRF，在配置BFD MAD时，各IRF必须使用不同的VLAN作为BFD MAD检测专用VLAN。  
  

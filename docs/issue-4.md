## 一．dhcp-snooping的作用以及抵御哪些攻击？说明攻击原理和抵御方式？  
### dhcp-snooping的定义：   
DHCP Snooping是一种DHCP安全特性，可以过滤不信任的DHCP消息并建立和维护一个DHCP Snooping绑定表。该绑定表包括MAC地址、IP地址、租约时间、绑定类型、VLAN ID、接口信息。DHCP Snooping的作用就如同在DHCP Client和DHCP Server之间建立一道防火墙。  
  
### dhcp交互过程  
#### 发现阶段  
即DHCP客户端寻找DHCP服务器的阶段。客户端通过UDP知名端口68以广播方式发送DHCPDISCOVER报文，只有DHCP服务器才会进行响应。  
#### 提供阶段  
即DHCP服务器提供IP地址的阶段。DHCP服务器接收到客户端的DHCPDISCOVER报文后，从IP地址池中挑选一个尚未分配的IP地址准备分配给客户端，通过UDP知名端口67向该客户端发送包含待分配的IP地址、租期和其他配置信息（如网关地址、DNS服务器地址等）的DHCPOFFER报文。  
#### 选择阶段  
即DHCP客户端选择IP地址的阶段。如果有多台DHCP服务器向该客户端发来DHCPOFFER报文，客户端只接受第一个收到的DHCPOFFER报文，然后以广播方式向各DHCP服务器回应DHCPREQUEST报文，该DHCPREQUEST报文中包含向所选定的DHCP服务器请求IP地址的内容。  
#### 确认阶段  
即DHCP服务器确认所提供IP地址的阶段。当DHCP服务器收到DHCP客户端回答的DHCPREQUEST报文后：如果是客户端选定的服务器，则该服务器根据DHCPREQUEST报文中携带的MAC地址来查找有没有相应的租约记录。若果没有找到相应的租约记录，则向客户端发送一个nak报文，告知没有可分配的ip地址。  
dhcp decline 当客户端发现服务器给它的ip地址发生冲突时会通过发送此报文来通知服务器，并且重新向服务器申请地址  
dhcp release 客户端主动释放服务器分配的ip地址，当服务器收到后，可将这个ip地址分配给其他客户端  
dhcp inform  客户端获取地址后，如果需要向服务器获取更详细的配置信息 如网关dns等，就发送此报文。  
  
## 介绍DHCP Snooping的实现原理，常见的dhcp攻击有哪些？  
  
### 1.DHCP Server仿冒者攻击  
由于DHCP请求报文以广播形式发送，所以DHCP Server仿冒者可以侦听到此报文。DHCP Server仿冒者回应给DHCP Client仿冒信息，如错误的网关地址、错误的DNS服务器、错误的IP等，达到DoS（Deny of Service）的目的。如图1所示。  
图1 DHCP Server仿冒者攻击示意图   
   
为防止DHCP Server仿冒者攻击，可使用DHCP Snooping的“信任（Trusted）/不信任（Untrusted）”工作模式。  
把某个物理接口或者VLAN的接口设置为“信任（Trusted）”或者“不信任（Untrusted）”。凡是从“不信任（Untrusted）”接口上收到的DHCP Reply（Offer、ACK、NAK）报文直接丢弃，这样可以隔离DHCP Server仿冒者攻击。如图2所示。  
  
  
### 2.中间人攻击  
首先，中间人向客户端发送带有自己的MAC地址和服务器IP地址的报文，让客户端学到中间人的IP和MAC，达到仿冒DHCP Server的目的。达到目的后，客户端发到DHCP服务器的报文都会经过中间人；然后，中间人向服务器发送带有自己MAC和客户端IP的报文，让服务器学到中间人的IP和MAC，达到仿冒客户端的目的，如图3所示。  
中间人完成服务器和客户端的数据交换。在服务器看来，所有的报文都是来自或者发往客户端；在客户端看来，所有的报文也都是来自或者发往服务器端。但实际上这些报文都是经过了中间人的“二手”信息。  
图3 中间人攻击示意图   
   
### 3.IP/MAC Spoofing攻击  
攻击者向服务器发送带有合法用户IP和MAC的报文，令服务器误以为已经学到这个合法用户的IP和MAC，但真正的合法用户不能从服务器获得服务。如图4所示。  
图4 IP/MAC Spoofing攻击示意图   
   
为隔离中间人攻击与IP/MAC Spoofing攻击，可使用DHCP Snooping绑定表工作模式。  
当接口接收到ARP或者IP报文，使用ARP或者IP报文中的“源IP+源MAC”匹配DHCP Snooping绑定表。如果匹配就进行转发，如果不匹配就丢弃。如图5所示。  
DHCP Snooping绑定表中的表项分为两种：  
通过命令行配置的静态表项，只能通过命令行删除。  
通过DHCP Snooping功能自动学习到的动态表项，并根据DHCP租期进行老化。  
DHCP Snooping绑定表中的动态表项是根据DHCP server回的ACK报文自动生成的。根据网络设备层次不同，动态表项的建立过程也不同：  
在二层设备上  
如果使能了Option82选项，二层设备通过侦听DHCP报文，在DHCP请求报文中插入Option82，DHCP Server会在ACK报文中携带Option82选项。二层设备分析Option82选项即可确定DHCP Reply报文回应给哪个接口，从而建立对应的DHCP Snooping绑定表项。  
如果未使能Option82选项，二层设备根据MAC地址表来识别不同的接口信息。  
在三层设备上  
对于配置为“不信任”的接口，设备通过侦听DHCP Reply消息，获取DHCP Server分配给用户的IP地址、用户MAC地址、报文通过的接口等信息，建立不信任侧的IP与MAC绑定表项。该表项与分配给用户的IP地址具有同样的租期，租约到期或者用户释放该IP地址则自动删除该表项。  
  
对于配置静态IP的用户，由于没有通过DHCP请求而获得IP，所以，没有对应的DHCP Snooping绑定表项，该用户发出的ARP、IP报文会被丢弃，从而防止该用户非法使用网络。只能通过配置静态DHCP Snooping绑定表来允许静态IP用户访问网络。  
对于盗用其他合法用户IP地址的用户，由于同样不是自己通过DHCP请求而获得IP的，IP对应的DHCP Snooping绑定表项中的MAC以及接口与盗用者的不一致，盗用者发出的ARP、IP报文会被丢弃，从而防止该盗用者非法使用网络。  
图5 应用IP与MAC绑定表示意图   
   
  
### 4.改变CHADDR值的DoS攻击  
如果攻击者改变的不是数据帧头部的源MAC，而是改变DHCP报文中的CHADDR（Client Hardware Address）值来不断申请IP地址，如图6所示，而设备仅根据数据帧头部的源MAC来判断该报文是否合法，那么“MAC地址限制”方案不能起作用。  
图6 改变CHADDR的DOS攻击   
   
这时，可以使用DHCP Snooping检查DHCP请求报文中CHADDR字段的功能。如果该字段跟数据帧头部的源MAC相匹配，便转发报文；否则，丢弃报文。  
Option82  
Option82报文格式  
Option82是DHCP报文的一个特殊字段，供DHCP Relay设置，用来标识DHCP Request报文的发送路径。  
客户端发送的DHCP请求报文，经过DHCP Relay时，DHCP Relay为DHCP Request报文填充Option82字段，DHCP Server收到含有Option82字段的DHCP请求报文时，在DHCP Relay报文中原样添加该字段返回给DHCP Relay，DHCP Relay根据relay报文中携带的Option82字段确定发送给那个接口。  
  

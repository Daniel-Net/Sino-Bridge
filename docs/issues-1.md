## 华为IPsec VPN一端穿越NAT配置
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/IPsec%20VPN.png)
AR1配置:
ipsec proposal 10  
ike peer 10 v1  
 exchange-mode aggressive  
 pre-shared-key simple 123456  
 nat traversal  
ipsec policy-template 10 10  
 ike-peer 10  
 proposal 10  
ipsec policy 1 10 isakmp template 10  
interface GigabitEthernet0/0/0  
 ip address 1.1.1.1 255.255.255.252   
 ipsec policy 1  

AR3配置  
acl number 2000    
 rule 10 permit  
interface GigabitEthernet0/0/1  
 ip address 2.2.2.1 255.255.255.252   
 nat outbound 2000  

AR4配置  
acl number 3000    
 rule 5 permit ip source 172.16.1.0 0.0.0.255 destination 192.168.1.0 0.0.0.255   
ipsec proposal 20  
ike peer 20 v1  
 exchange-mode aggressive  
 pre-shared-key simple 123456  
 nat traversal  
 remote-address 1.1.1.1  
ipsec policy 2 10 isakmp  
 security acl 3000  
 ike-peer 20  
 proposal 20  
interface GigabitEthernet0/0/0  
 ip address 10.0.0.2 255.255.255.0   
 ipsec policy 2  

1.	NAT设备上需要把AR4的互联端口也进行nat转换，在AR1看来是1.1.1.1/30和2.2.2.1/30是互为对端，在AR4看来，是10.0.0.2/24和1.1.1.1/30互为对端，并且AR1上需要使用策略模版进行配置，野蛮模式，开启nat穿越，AR1上不需要定义感兴趣流量。IKE v2版本也可以，配置中没有野蛮模式。
2.	也可以使用另一种方法实现，两个出口路由器可以配置VPN，在定义出口nat转换时，把两端互通的地址即感兴趣流量在nat转换列表里剔除，也就是感兴趣流量在经过路由器时，不进行nat转换。


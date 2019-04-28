## 思科7206路由器替换遇到的问题    
    
### 一、  前期准备    
将现网运行的路由器思科7206的软件版本、板卡信息、配置文件、设备状态等信息进行备份。将配置导入到将要替换的7206路由器中，抓取新配置的路由器配置，使用对比软件进行配置对比，确认无误后保存路由器的配置，并将备机关机。    
### 二、	问题描述和解决办法    
#### 1.设备替换前遇到的问题    
在1月26日进行测试替换，为了确保当晚的替换能够顺利完成，在1月25日晚对两台设备进行了加电测试和配置比较，带有qos配置的设备在加载ios前报了如下错误。报错信息如下：    
`%QOS Policy-maps configuration is not supported`  
因为路由器在启动过程加载ios之前报错，初步判断是ios问题，下载7206路由器npe-g2引擎支持的较新版本的两个版本的ios进行升级。（c7200p-adventerprisek9-mz.152-4.M11.bin）（c7200p-spservicesk9-mz.152-4.M11.bin）升级后修改引导，重新启动设备加载新升级的ios仍然报错，将系统加载的ios恢复至初始的ios。并将报错信息在思科官网进行查询查询。    
qos在设备上可以配置并且正常调用在子接口下。在设备上使用show policy-map interface x/x    

![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-7/7-1.png)

对于重启后加载ios过程中的报错信息进行了查询，思科官网给出的信息：    
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-7/7-2.png)  

 #### 2.解决方法：（来源于思科官网）    
 ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-7/7-3.png)    
`（翻译软件翻译）此行上方的消息来自引导加载程序。如果消息出现在“sys-6-boot_messages:此行上方的消息来自引导加载程序。”它们是由启动映像在分析配置时生成的。这通常是因为启动映像不支持某个功能。加载“最终”IOS后，这不应影响路由器。`  
    
##### 2.2、设备替换中遇到的问题和解决办法     
###### 1.遇到的问题：     
按照替换文档中的操作步骤将第一台城域网路由器上架，连接好线缆，开机启动后，部分广域网子接口物理协议都是up，并且eigrp邻居能够正常建立，但是ping不通互联的支行广域网口地址。 （使用GNS3模拟器模拟生产环境）    
分行端城域网路由器的接口状态：  

![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-7/7-4.png)
     
分行端城域网路由器的eigrp邻居关系： 

![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-7/7-5.png)
     
但是不能ping通对端的直连地址：   
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-7/7-6.png)
     
###### 2.解决办法：    
将原有的路由器还原后恢复。经过分析，部分路由器配置了ip和mac绑定信息。更换城域网路由器的子接口mac地址变化，路由器绑定的mac地址和新更换的路由器子接口mac地址不一致，是导致不通的原因。登录到有绑定广域网口ip地址的路由器上，使用show run | i arp来查看设备绑定的分行侧城域网路由器互联的子接口的ip和mac地址对应关系。    
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-7/7-7.png)     
在路由器上将绑定关系解除。   
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-7/7-8.png)
     
更换设备后再一次进行ping验证，能够正常ping通支行的互联地址。    
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-7/7-9.png)

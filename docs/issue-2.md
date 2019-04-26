## 思科WLC与AP MAC绑定
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-1/IPsec%20VPN.png)
我写这篇经验分享，主要不是说明这个具体的技术，更多的是遇到问题的一种处理过程；问题一定会有，关键是遇到问题的处理方法。我的做事原则是，遇到问题自己先努力去解决，实在没有办法了，再寻求帮助。
最近在一个客户那进行技术支持，涉及到思科无线控制器WLC相关问题，思科WLC需要与思科AP进行MAC绑定。对于做过的人来说，这个问题很简单，没做的话就得花点时间去研究了。
这个问题如果用华三AC去实现就很简单了，华三的AC就是通过AP的MAC地址或者序列号去识别AP的。由于思科WLC接触的比较少，而且页面都是英文，所以处理起来有些头疼的。遇到一些不会的问题很正常，就看你怎么去解决了。
首先，熟悉WLC的页面，根据各个选项及复选框字面意思去理解。花了半个小时去熟悉WLC，生产环境，不敢瞎搞。百度或者谷歌吧，搜索一些关于这个相关的问题，资料不多，反馈的大多也是WLC如何去实现客户端MAC地址的绑定，没有WLC与AP绑定的相关问题。看了一些资料，介绍AP与WLC注册过程，这个注册过程默认是不需要进行验证的。最后登陆思科官网，思科官网资料又多又乱还都是英文，随随便便一个技术文档就几百页，根本不知从何下手啊。折腾了一上午也没有解决，还好客户不是很着急。最后只能联系思科400客服了。问题反馈给思科客服，等待反馈，下午思科客服给了我反馈，然后附上思科英文资料，我把英文资料熟读了2遍，很简单的一个问题，按照文档，下班实施变更，将问题解决，客户很满意。
一个客户对一个工程师是否认可，从很多方面都能体现出来，首先是你的态度，一个认真负责的态度，这个态度要比你的专业技能更可贵。其次，你的专业技能，本职技能，有什么好说的呢。最后，要有一颗再学习的心，IT这行可谓“干到老，学到老”。
如下为思科官网的具体操作过程，文档很详细，我就不录视频了。
具体链接如下：
https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html

##Contents
[Introduction](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc14)(https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc14)(https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc0)
[Prerequisites](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc1)
[Requirements](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc2)
[Components Used](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc3)
[Conventions](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc4)
[Lightweight Access Point (AP) Authorization](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc5)
[Configure](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc6)
[Configuration using the Internal Authorization List on the WLC](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc7)
[Verify](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc8)
[AP Authorization Against an AAA Server](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc9)
[Configure the Cisco ISE to Authorize APs](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc10)
[Configure the WLC as an AAA Client on the Cisco ISE](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc11)
[Add the AP MAC Address to the User Database on the Cisco ISE](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc12)
[Define a Policy Set](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc13)
[Verify](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc14)  
[Troubleshoot](https://www.cisco.com/c/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config.html#anc15)
##Introduction
Security is always a concern nowadays and making sure that only legitimate Access Points (APs) connect to your Wireless LAN Controllers (WLCs) can be needed.
This document explains how to configure WLC to authorize the APs based on the MAC address of the APs.
##Prerequisites
##Requirements
Ensure that you meet these requirements before you attempt this configuration:
•    Basic knowledge of how to configure a Cisco Identity Services Engine (ISE).
•    Knowledge of the configuration of Cisco APs and Cisco WLCs.
•    Knowledge of Cisco Unified Wireless Security Solutions.
##Components Used
The information in this document is based on these software and hardware versions:
•    
•    WLCs running AireOS 8.8.111.0 Software.
•    Wave1 APs: 1700/2700/3700 and 3500 (1600/2600/3600 are still supported but AireOS support ends on version 8.5.x).
•    Wave2 APs: 1800/2800/3800/4800, 1540 and 1560. 
•    ISE version 2.3.0.298.
The information in this document was created from the devices in a specific lab environment. All of the devices used in this document started with a cleared (default) configuration. If your network is live, make sure that you understand the potential impact of any command.
##Conventions
Refer to [Cisco Technical Tips Conventions](https://www.cisco.com/en/US/tech/tk801/tk36/technologies_tech_note09186a0080121ac5.shtml) for more information on document conventions.
##Lightweight Access Point (AP) Authorization
During the AP registration process, the APs and WLCs mutually authenticate using X.509 certificates.
The X.509 certificates are burned into protected flash on both the access point (AP) and WLC at the factory by Cisco.
On the AP, factory installed certificates are called manufacturing installed certificates (MIC). All Cisco APs manufactured after July 18, 2005 have MICs.
In addition to this mutual authentication that occurs during the registration process, the WLCs can also restrict the APs that register with them based on the MAC address of the AP.
The lack of a strong password by the use of the AP's MAC address should not be an issue because the controller uses MIC to authenticate the AP before authorizing the AP through the RADIUS server. The use of MIC provides strong authentication.
AP authorization can be performed in two ways:
•    Using the Internal Authorization list on the WLC.
•    Using the MAC address database on an AAA server.
The behaviors of the APs differ based on the certificate used:
•    APs with SSCs—The WLC will only use the Internal Authorization list and will not forward a request to a RADIUS server for these APs.
•    APs with MICs—WLC can use either the Internal Authorization list configured on the WLC or use a RADIUS server to authorize the APs.
This document discusses AP authorization using both the Internal Authorization list and the AAA server.
##Configure
##Configuration using the Internal Authorization List on the WLC
On the WLC, use the AP authorization list to restrict APs based on their MAC address. The AP authorization list is available under Security > AP Policies in the WLC GUI.
This example shows how to add the AP with MAC address 4c:77:6d:9e:61:62.
1.    From the WLC controller GUI, click Security > AP Policies.and the AP Policies page appears.
2.    Click the Add button on the right hand side of the screen.

![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-00.jpeg)
 
3.    Under Add AP to Authorization List, enter the AP MAC address (NOT the AP Radio mac address). Then, choose the certificate type and click Add.
In this example, a AP with MIC certificate is added.
Note: For APs with SSCs, choose SSC under Certificate Type.

![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-01.jpeg)
 
The AP is added to the AP authorization list and is listed under AP Authorization List.
4.    Under Policy Configuration, check the box for Authorize MIC APs against auth-list or AAA.
When this parameter is selected, the WLC checks the local authorization list first. If the AP's MAC is not present, it checks the RADIUS server.
 
![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-02.jpeg)
 
##Verify
In order to verify this configuration, you need to connect the AP with MAC address 4c:77:6d:9e:61:62 to the network and monitor. Use the debug capwap events/errors enable and debug aaa all enable commands to perform this.
This output shows the debugs when the AP MAC address is not present in the AP authorization list:
Note: Some of the lines in the output have been moved to the second line due to space constraints.
##(Cisco Controller) >debug capwap events enable
##(Cisco Controller) >debug capwap errors enable
##(Cisco Controller) >debug aaa all enable

*spamApTask4: Feb 27 10:15:25.592: 70:69:5a:51:4e:c0 Join Request from 192.168.79.151:5256

*spamApTask4: Feb 27 10:15:25.592: 70:69:5a:51:4e:c0 Unable to get Ap mode in Join request

*spamApTask4: Feb 27 10:15:25.592: 70:69:5a:51:4e:c0 Allocate database entry for AP 192.168.79.151:5256, already allocated index 277

*spamApTask4: Feb 27 10:15:25.592: 70:69:5a:51:4e:c0 AP Allocate request at index 277 (reserved)
*spamApTask4: Feb 27 10:15:25.593: 24:7e:12:19:41:ef Deleting AP entry 192.168.79.151:5256 from temporary database.
*spamApTask4: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 AP group received default-group is found in ap group configured in wlc.

*spamApTask4: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Dropping request or response packet to AP :192.168.79.151 (5256) by Controller: 10.48.71.20 (5246), message Capwap_wtp_event_response, state Capwap_no_state

*spamApTask4: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 In AAA state 'Idle' for AP 70:69:5a:51:4e:c0
*spamApTask4: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Join Request failed!

*spamApTask4: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 State machine handler: Failed to process msg type = 3 state = 0 from 192.168.79.151:5256

*aaaQueueReader: Feb 27 10:15:25.593: Unable to find requested user entry for 4c776d9e6162
*aaaQueueReader: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Normal Response code for AAA Authentication : -9
*aaaQueueReader: Feb 27 10:15:25.593: ReProcessAuthentication previous proto 8, next proto 40000001
*aaaQueueReader: Feb 27 10:15:25.593: AuthenticationRequest: 0x7f01b4083638

*aaaQueueReader: Feb 27 10:15:25.593: Callback.....................................0xd6cef02166

*aaaQueueReader: Feb 27 10:15:25.593: protocolType.................................0x40000001

*aaaQueueReader: Feb 27 10:15:25.593: proxyState...................................70:69:5A:51:4E:C0-00:00

*aaaQueueReader: Feb 27 10:15:25.593: Packet contains 9 AVPs:

*aaaQueueReader: Feb 27 10:15:25.593: AVP[01] User-Name................................4c776d9e6162 (12 bytes)

*aaaQueueReader: Feb 27 10:15:25.593: AVP[02] Called-Station-Id........................70-69-5a-51-4e-c0 (17 bytes)

*aaaQueueReader: Feb 27 10:15:25.593: AVP[03] Calling-Station-Id.......................4c-77-6d-9e-61-62 (17 bytes)

*aaaQueueReader: Feb 27 10:15:25.593: AVP[04] Nas-Port.................................0x00000001 (1) (4 bytes)

*aaaQueueReader: Feb 27 10:15:25.593: AVP[05] Nas-Ip-Address...........................0x0a304714 (170936084) (4 bytes)

*aaaQueueReader: Feb 27 10:15:25.593: AVP[06] NAS-Identifier...........................0x6e6f (28271) (2 bytes)

*aaaQueueReader: Feb 27 10:15:25.593: AVP[07] User-Password............................[...]

*aaaQueueReader: Feb 27 10:15:25.593: AVP[08] Service-Type.............................0x0000000a (10) (4 bytes)

*aaaQueueReader: Feb 27 10:15:25.593: AVP[09] Message-Authenticator....................DATA (16 bytes)

*aaaQueueReader: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Error Response code for AAA Authentication : -7
*aaaQueueReader: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Returning AAA Error 'No Server' (-7) for mobile 70:69:5a:51:4e:c0 serverIdx 0
*aaaQueueReader: Feb 27 10:15:25.593: AuthorizationResponse: 0x7f017adf5770


*aaaQueueReader: Feb 27 10:15:25.593: RadiusIndexSet(0), Index(0)
*aaaQueueReader: Feb 27 10:15:25.593: resultCode...................................-7

*aaaQueueReader: Feb 27 10:15:25.593: protocolUsed.................................0xffffffff

*aaaQueueReader: Feb 27 10:15:25.593: proxyState...................................70:69:5A:51:4E:C0-00:00

*aaaQueueReader: Feb 27 10:15:25.593: Packet contains 0 AVPs:

*aaaQueueReader: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 User entry not found in the Local FileDB for the client.

*spamApTask0: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Join Version: = 134770432

*spamApTask0: Feb 27 10:15:25.593: 00:00:00:00:00:00 apType = 54 apModel: AIR-AP4800-E-K

*spamApTask0: Feb 27 10:15:25.593: 00:00:00:00:00:00 apType: Ox36 bundleApImageVer: 8.8.111.0
*spamApTask0: Feb 27 10:15:25.593: 00:00:00:00:00:00 version:8 release:8 maint:111 build:0
*spamApTask0: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Join resp: CAPWAP Maximum Msg element len = 79

*spamApTask0: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Join Failure Response sent to 0.0.0.0:5256

*spamApTask0: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Radius Authentication failed. Closing dtls Connection.
*spamApTask0: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Disconnecting DTLS Capwap-Ctrl session 0xd6f0724fd8 for AP (192:168:79:151/5256). Notify(true)
*spamApTask0: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 CAPWAP State: Dtls tear down

*spamApTask0: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 acDtlsPlumbControlPlaneKeys: lrad:192.168.79.151(5256) mwar:10.48.71.20(5246)

*spamApTask0: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 DTLS keys for Control Plane deleted successfully for AP 192.168.79.151

*spamApTask4: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 DTLS connection closed event receivedserver (10.48.71.20/5246) client (192.168.79.151/5256)
*spamApTask4: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Entry exists for AP (192.168.79.151/5256)
*spamApTask0: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 AP Delete request 
*spamApTask4: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 AP Delete request 
*spamApTask4: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 Unable to find AP 70:69:5a:51:4e:c0
*spamApTask4: Feb 27 10:15:25.593: 70:69:5a:51:4e:c0 No AP entry exist in temporary database for 192.168.79.151:5256
This output show the debugs when the LAP's MAC address is added to the AP authorization list:
Note: Some of the lines in the output have been moved to the second line due to space constraints.
(Cisco Controller) >debug capwap events enable
(Cisco Controller) >debug capwap errors enable
(Cisco Controller) >debug aaa all enable

*spamApTask4: Feb 27 09:50:25.393: 70:69:5a:51:4e:c0 Join Request from 192.168.79.151:5256

*spamApTask4: Feb 27 09:50:25.393: 70:69:5a:51:4e:c0 using already alloced index 274
*spamApTask4: Feb 27 09:50:25.393: 70:69:5a:51:4e:c0 Unable to get Ap mode in Join request

*spamApTask4: Feb 27 09:50:25.393: 70:69:5a:51:4e:c0 Allocate database entry for AP 192.168.79.151:5256, already allocated index 274

*spamApTask4: Feb 27 09:50:25.393: 70:69:5a:51:4e:c0 AP Allocate request at index 274 (reserved)
*spamApTask4: Feb 27 09:50:25.393: 24:7e:12:19:41:ef Deleting AP entry 192.168.79.151:5256 from temporary database.
*spamApTask4: Feb 27 09:50:25.393: 70:69:5a:51:4e:c0 AP group received default-group is found in ap group configured in wlc.

*spamApTask4: Feb 27 09:50:25.393: 70:69:5a:51:4e:c0 Dropping request or response packet to AP :192.168.79.151 (5256) by Controller: 10.48.71.20 (5246), message Capwap_wtp_event_response, state Capwap_no_state

*spamApTask4: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 Message type Capwap_wtp_event_response is not allowed to send in state Capwap_no_state for AP 192.168.79.151

*spamApTask4: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 In AAA state 'Idle' for AP 70:69:5a:51:4e:c0
*spamApTask4: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 Join Request failed!


*aaaQueueReader: Feb 27 09:50:25.394: User 4c776d9e6162 authenticated
*aaaQueueReader: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 Normal Response code for AAA Authentication : 0
*aaaQueueReader: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 Returning AAA Success for mobile 70:69:5a:51:4e:c0
*aaaQueueReader: Feb 27 09:50:25.394: AuthorizationResponse: 0x7f0288a66408


*aaaQueueReader: Feb 27 09:50:25.394: structureSize................................194

*aaaQueueReader: Feb 27 09:50:25.394: resultCode...................................0

*aaaQueueReader: Feb 27 09:50:25.394: proxyState...................................70:69:5A:51:4E:C0-00:00

*aaaQueueReader: Feb 27 09:50:25.394: Packet contains 2 AVPs:

*aaaQueueReader: Feb 27 09:50:25.394: AVP[01] Service-Type.............................0x00000065 (101) (4 bytes)

*aaaQueueReader: Feb 27 09:50:25.394: AVP[02] Airespace / WLAN-Identifier..............0x00000000 (0) (4 bytes)

*aaaQueueReader: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 User authentication Success with File DB on WLAN ID :0 
*spamApTask0: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 Join Version: = 134770432

*spamApTask0: Feb 27 09:50:25.394: 00:00:00:00:00:00 apType = 54 apModel: AIR-AP4800-E-K

*spamApTask0: Feb 27 09:50:25.394: 00:00:00:00:00:00 apType: Ox36 bundleApImageVer: 8.8.111.0
*spamApTask0: Feb 27 09:50:25.394: 00:00:00:00:00:00 version:8 release:8 maint:111 build:0
*spamApTask0: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 Join resp: CAPWAP Maximum Msg element len = 79

*spamApTask0: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 Join Response sent to 0.0.0.0:5256

*spamApTask0: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 CAPWAP State: Join

*spamApTask0: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 capwap_ac_platform.c:2095 - Operation State 0 ===> 4
*spamApTask0: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 Capwap State Change Event (Reg) from capwap_ac_platform.c 2136

*apfReceiveTask: Feb 27 09:50:25.394: 70:69:5a:51:4e:c0 Register LWAPP event for AP 70:69:5a:51:4e:c0 slot 0
##AP Authorization Against an AAA Server
You can also configure WLCs to use RADIUS servers to authorize APs using MICs. The WLC uses a AP's MAC address as both the username and password when sending the information to a RADIUS server. For example, if the MAC address of the AP is 4c:77:6d:9e:61:62, both the username and password used by the controller to authorize the AP will be that mac address using the defined delimeter.
This example shows how to configure the WLCs to authorize APs using the Cisco ISE.
1.    From the WLC controller GUI, click Security > AP Policies. The AP Policies page appears.  
2.    Under Policy Configuration, check the box for Authorize MIC APs against auth-list or AAA.
When this parameter is selected, the WLC checks the local authorization list first. If the AP's MAC is not present, it checks the RADIUS server.

![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-03.jpeg)
 
3.    Go to Security > RADIUS Authentication from the controller GUI to display the RADIUS Authentication Servers page. In this page we can define the MAC Delimiter. The WLC will get the AP Mac address and send it to the Radius Server using the delimiter defnied here. this is important so that the username matches what is configured in the Radius server. In this example we will use No Delimeter so that the username will be 4c776d9e6162. 

![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-04.jpeg)

4.    Then, click New in order to define a RADIUS server.

![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-05.jpeg)
 
5.    Define the RADIUS server parameters on the RADIUS Authentication Servers > New page. These parameters include the RADIUS Server IP Address, Shared Secret, Port Number, and Server Status. When done, click Apply. This example uses the Cisco ISE as the RADIUS server with IP address 10.48.39.128.
##Configure the Cisco ISE to Authorize APs
In order to enable the Cisco ISE to authorize APs, you need to complete these steps:
1.    Configure the WLC as an AAA Client on the Cisco ISE.
2.    Add the AP MAC Addresses to the User Database on the Cisco ISE.
Configure the WLC as an AAA Client on the Cisco ISE
1.    Go to Administration > Network Resources > Network Devices > Add. The New Network Device page appears.  
2.    On this page, define the WLC Name, Management Interface IP Address and Radius Authentications Settings like Shared Secret.

![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-06.jpeg) 
 
3.    Click Submit.
Add the AP MAC Address to the User Database on the Cisco ISE
1.    Go to Administration > Identity Management. Here we need to make sure the password policy allows the usage of the username as password and the policy must also allow the usage of the mac address characters whitout the need for different types of characters. Go to Settings > User Authentication Settings > Password Policy: 
 
![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-07.jpeg)
  
2.    Then go to Identities > Users and click Add. When the User Setup page appears, define the username and password for this AP as shown. Tip: use the Description field to enter the password to later be easy to know what was defined as password.
The password should also be the AP's MAC address. In this example, it is 4c776d9e6162.
![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-08.jpeg) 
3.    Click Submit. 
Define a Policy Set
1.    We need to define a Policy Set to match the authentication request coming from the WLC. First we build a Condition going to Policy > Policy Elements > Conditions, and creating a new condition to match the WLC location, in this example, "LAB_WLC" and Radius:Service-Type Equals Call Check which is used for Mac authentication. Here the condition is named "AP_Auth".   
![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-09.jpeg)  
2.    Click Save.
3.    Then create a new Allowed Protocols Service for the AP authenticaton. Make sure you select only "Allow PAP/ASCII": 
![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-10.jpeg)   
4.     Select the previously created Service in the Allowed Protocols/Server Sequence. Expand the View and under Authentication Policy > Use > Internal Users so that ISE searched the internal DB for the username/password of the AP. 
![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-11.jpeg)
![](https://www.cisco.com/c/dam/en/us/support/docs/wireless/4400-series-wireless-lan-controllers/98848-lap-auth-uwn-config-12.jpeg) 
5.    Click Save.
Verify
In order to verify this configuration, you need to connect the AP with MAC address 4c:77:6d:9e:61:62 to the network and monitor. Use the debug capwap events/errors enable and debug aaa all enable commands in order to perform this.
As seen from the debugs, the WLC passed on the AP MAC address to the RADIUS server 10.48.39.128, and the server has successfully authenticated the AP. The AP then registers with the controller.
Note: Some of the lines in the output have been moved to the second line due to space constraints.
*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Join Request from 192.168.79.151:5248

*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 using already alloced index 437
*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Unable to get Ap mode in Join request

*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Allocate database entry for AP 192.168.79.151:5248, already allocated index 437

*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 AP Allocate request at index 437 (reserved)
*spamApTask4: Feb 27 14:58:07.566: 24:7e:12:19:41:ef Deleting AP entry 192.168.79.151:5248 from temporary database.
*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 AP group received default-group is found in ap group configured in wlc.

*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Dropping request or response packet to AP :192.168.79.151 (5248) by Controller: 10.48.71.20 (5246), message Capwap_wtp_event_response, state Capwap_no_state

*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Message type Capwap_wtp_event_response is not allowed to send in state Capwap_no_state for AP 192.168.79.151

*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 In AAA state 'Idle' for AP 70:69:5a:51:4e:c0
*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Join Request failed!

*spamApTask4: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 State machine handler: Failed to process msg type = 3 state = 0 from 192.168.79.151:5248

*spamApTask4: Feb 27 14:58:07.566: 24:7e:12:19:41:ef Failed to parse CAPWAP packet from 192.168.79.151:5248

*aaaQueueReader: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Normal Response code for AAA Authentication : -9
*aaaQueueReader: Feb 27 14:58:07.566: ReProcessAuthentication previous proto 8, next proto 40000001
*aaaQueueReader: Feb 27 14:58:07.566: AuthenticationRequest: 0x7f01b404f0f8


*aaaQueueReader: Feb 27 14:58:07.566: Callback.....................................0xd6cef02166

*aaaQueueReader: Feb 27 14:58:07.566: protocolType.................................0x40000001

*aaaQueueReader: Feb 27 14:58:07.566: proxyState...................................70:69:5A:51:4E:C0-00:00

*aaaQueueReader: Feb 27 14:58:07.566: Packet contains 9 AVPs:

*aaaQueueReader: Feb 27 14:58:07.566: AVP[02] Called-Station-Id........................70:69:5a:51:4e:c0 (17 bytes)

*aaaQueueReader: Feb 27 14:58:07.566: AVP[03] Calling-Station-Id.......................4c:77:6d:9e:61:62 (17 bytes)

*aaaQueueReader: Feb 27 14:58:07.566: AVP[04] Nas-Port.................................0x00000001 (1) (4 bytes)

*aaaQueueReader: Feb 27 14:58:07.566: AVP[05] Nas-Ip-Address...........................0x0a304714 (170936084) (4 bytes)

*aaaQueueReader: Feb 27 14:58:07.566: AVP[06] NAS-Identifier...........................0x6e6f (28271) (2 bytes)

*aaaQueueReader: Feb 27 14:58:07.566: AVP[08] Service-Type.............................0x0000000a (10) (4 bytes)

*aaaQueueReader: Feb 27 14:58:07.566: AVP[09] Message-Authenticator....................DATA (16 bytes)

*aaaQueueReader: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 radiusServerFallbackPassiveStateUpdate: RADIUS server is ready 10.48.39.128 port 1812 index 1 active 1
*aaaQueueReader: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 NAI-Realm not enabled on Wlan, radius servers will be selected as usual
*aaaQueueReader: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Found the radius server : 10.48.39.128 from the global server list
*aaaQueueReader: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Send Radius Auth Request with pktId:185 into qid:0 of server at index:1
*aaaQueueReader: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Sending the packet to v4 host 10.48.39.128:1812 of length 130
*aaaQueueReader: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 Successful transmission of Authentication Packet (pktId 185) to 10.48.39.128:1812 from server queue 0, proxy state 70:69:5a:51:4e:c0-00:00
*aaaQueueReader: Feb 27 14:58:07.566: 00000000: 01 b9 00 82 d9 c2 ef 27 f1 bb e4 9f a8 88 5a 6d .......'......Zm
*aaaQueueReader: Feb 27 14:58:07.566: 00000010: 4b 38 1a a6 01 0e 34 63 37 37 36 64 39 65 36 31 K8....4c776d9e61
*aaaQueueReader: Feb 27 14:58:07.566: 00000020: 36 32 1e 13 37 30 3a 36 39 3a 35 61 3a 35 31 3a 62..70:69:5a:51:
*aaaQueueReader: Feb 27 14:58:07.566: 00000030: 34 65 3a 63 30 1f 13 34 63 3a 37 37 3a 36 64 3a 4e:c0..4c:77:6d:
*aaaQueueReader: Feb 27 14:58:07.566: 00000040: 39 65 3a 36 31 3a 36 32 05 06 00 00 00 01 04 06 9e:61:62........
*aaaQueueReader: Feb 27 14:58:07.566: 00000050: 0a 30 47 14 20 04 6e 6f 02 12 54 46 96 61 2a 38 .0G...no..TF.a*8
*aaaQueueReader: Feb 27 14:58:07.566: 00000060: 5a 57 22 5b 41 c8 13 61 97 6c 06 06 00 00 00 0a ZW"[A..a.l......
*aaaQueueReader: Feb 27 14:58:07.566: 00000080: 15 f9 ..
*aaaQueueReader: Feb 27 14:58:07.566: 70:69:5a:51:4e:c0 User entry not found in the Local FileDB for the client.
*radiusTransportThread: Feb 27 14:58:07.587: Vendor Specif Radius Attribute(code=26, avp_len=28, vId=9)
*radiusTransportThread: Feb 27 14:58:07.588: 70:69:5a:51:4e:c0 *** Counted VSA 150994944 AVP of length 28, code 1 atrlen 22)
*radiusTransportThread: Feb 27 14:58:07.588: Vendor Specif Radius Attribute(code=26, avp_len=28, vId=9)
*radiusTransportThread: Feb 27 14:58:07.588: 70:69:5a:51:4e:c0 AVP: VendorId: 9, vendorType: 1, vendorLen: 22

*radiusTransportThread: Feb 27 14:58:07.588: 00000000: 70 72 6f 66 69 6c 65 2d 6e 61 6d 65 3d 55 6e 6b profile-name=Unk
*radiusTransportThread: Feb 27 14:58:07.588: 00000010: 6e 6f 77 6e nown
*radiusTransportThread: Feb 27 14:58:07.588: 70:69:5a:51:4e:c0 Processed VSA 9, type 1, raw bytes 22, copied 0 bytes
*radiusTransportThread: Feb 27 14:58:07.588: 70:69:5a:51:4e:c0 Access-Accept received from RADIUS server 10.48.39.128 (qid:0) with port:1812, pktId:185
*radiusTransportThread: Feb 27 14:58:07.588: RadiusIndexSet(1), Index(1)
*radiusTransportThread: Feb 27 14:58:07.588: structureSize................................432

*radiusTransportThread: Feb 27 14:58:07.588: protocolUsed.................................0x00000001

*radiusTransportThread: Feb 27 14:58:07.588: proxyState...................................70:69:5A:51:4E:C0-00:00

*radiusTransportThread: Feb 27 14:58:07.588: Packet contains 4 AVPs:

*radiusTransportThread: Feb 27 14:58:07.588: AVP[01] User-Name................................4c776d9e6162 (12 bytes)

*radiusTransportThread: Feb 27 14:58:07.588: AVP[02] State....................................ReauthSession:0a302780bNEx79SKIFosJ2ioAmIYNOiRe2iDSY3drcFsHuYpChs (65 bytes)

*radiusTransportThread: Feb 27 14:58:07.588: AVP[03] Class....................................DATA (83 bytes)

*radiusTransportThread: Feb 27 14:58:07.588: AVP[04] Message-Authenticator....................DATA (16 bytes)

*spamApTask0: Feb 27 14:58:07.588: 70:69:5a:51:4e:c0 Join Version: = 134770432

*spamApTask0: Feb 27 14:58:07.588: 00:00:00:00:00:00 apType = 54 apModel: AIR-AP4800-E-K

*spamApTask0: Feb 27 14:58:07.588: 00:00:00:00:00:00 apType: Ox36 bundleApImageVer: 8.8.111.0
*spamApTask0: Feb 27 14:58:07.588: 00:00:00:00:00:00 version:8 release:8 maint:111 build:0
*spamApTask0: Feb 27 14:58:07.588: 70:69:5a:51:4e:c0 Join resp: CAPWAP Maximum Msg element len = 79

*spamApTask0: Feb 27 14:58:07.588: 70:69:5a:51:4e:c0 Join Response sent to 0.0.0.0:5248

*spamApTask0: Feb 27 14:58:07.588: 70:69:5a:51:4e:c0 CAPWAP State: Join






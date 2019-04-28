## N7K交换机- Module:7报警问题的原因说明
### 一、6月27日报警短信内容说明
第7块板卡（板卡型号N7K-F248XT-25E）环回测试失败，受影响接口是第22接口。通过查看配置和巡检信息确认第22接口物理状态为disabled,接口配置为空，确认接口没在使用中，业务不受影响。
报警短信内容为：'新监控系统：xx交换机-备', 'x.x.x.x', '设备 xx交换机-备] Module:7 Test:PortLoopback failed 10 consecutive times. Faulty module: affected ports:22 Error:Test Failed Could not identify the Faulty Device''。
### 二、报警短信反馈问题分析
此次报警短信反映未使用接口中的某一接口会受到板卡检测失败影响，7槽22口是非业务端口，接口处在disable状态。通过开思科技术CASE，思科工程师给出原因为：是交换机软件版本BUG导致的；软件版本在6.1（X），或更早期的软件版本都可能导致F2/F2E类型板卡接口环回测试失败；该问题不会影响到端口及板卡的正常使用，如果想要规避此问题则需要升级软件版本，已知的受影响版本号为6.1(2)、6.1(3)、6.2(1.59)，已知的修复版本号为6.2(2)、6.1(5)。
### 三、思科工程师针对这一问题的反馈邮件原文
Thanks for your information.
PortLoopback3
15 minutes	active	Verifies connectivity through every port that is administratively down on every module in the system.
https://www.cisco.com/c/en/us/td/docs/switches/datacenter/sw/5_x/nx-os/system_management/configuration/guide/sm_nx_os_cg/sm_11gold.html
 
After checking the log, I found that the device seems hit the bug:
CSCug13653
Port Loopback test failed for f2 card
Description
Symptom:
DIAG_PORT_LB-2-PORTLOOPBACK_TEST_FAIL Module:X Test:PortLoopback failed 10 consecutive times. Faulty module: affected ports:46 Error:Test Failed, Could not identify the Faulty Device

Conditions:
Issue only seen on F2/F2E line card.
Switch running 6.1(x), specifically earlier versions in this train.

Workaround(s):
None.
This issue/behavior is not going to impact regular traffic when the port is brought up for regular traffic.
 
Known Affected Releases:	(3)
6.1(2)
6.1(3)
6.2(1.59)
Known Fixed Releases:	(4)
6.2(2)
6.1(5)
 
https://bst.cloudapps.cisco.com/bugsearch/bug/CSCug13653/?reffering_site=dumpcr
 




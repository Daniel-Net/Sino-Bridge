## 思科WLC重启后licenses需重新导入
设备型号：cisco 5508 版本：7.4.121.0
该型号设备重启后设备支持AP数量Licenses需重新导入，导入方式如下：

登陆设备执行以下操作：
选择 MANAGEMENT > Software Activation > Commands 
进入该页后，依次进行license 添加、保存 操作。
Install License  

1.在 Action 下拉选项中, 选择 Install License. 添加license文件对话框将出现。
2.在File Name to Install 栏中填入以下信息, 填写license 路径使用tftp方式。 tftp://X.X.X.X/*.lic  //依次导入证书cisco lincese 
3.点击 Install License. 将会提示证书文件导入成功。
4.如果EULA对话框出现，点击Accept.
5.重启控制器。 进入COMMANDS > Reboot 选择 Save and Reboot / Reboot without Save

--------------------------------------------------------------------------------
add an AP-count license

选择 MANAGEMENT > Software Activation > Licenses

To add an AP-count license for Cisco controllers follow these steps: 

1.进入conut在license conunt 选项栏中。
2.在下拉菜单中选择Add 
3.点击设置Count. 
4.当出现(EULA) 时, 点击 Accept. 
--------------------------------------------------------------------------------
切换设备lincense

1.选择 MANAGEMENT > Software Activation > Licenses 
2.选择 base-ap-count type(*) conut(500) 选项
3.下拉Priority 选择high 然后点击Set Priority
4.当EULA出现时，阅读项目并且同意然后点击Accept.
5.当提示重启控制器时点击OK
6.待重启后进行验证工作。进入COMMANDS > Reboot 选择 Save and Reboot / Reboot without Save

设备lincenses	切换过程中可能需要多次重启设备。

点击Save Configuration 保存设备配置。

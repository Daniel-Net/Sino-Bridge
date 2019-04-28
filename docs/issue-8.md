## DNS基础  
　　DNS 是计算机域名系统 (Domain Name System 或Domain Name Service) 的缩写，域名服务器是进行域名(domain name)和与之相对应的IP地址 (IP address)转换的服务器。DNS中保存了一张域名(domain name)和与之相对应的IP地址 (IP address)的表，以解析消息的域名。 域名是Internet上某一台计算机或计算机组的名称，用于在数据传输时标识计算机的电子方位（有时也指地理位置）。域名是由一串用点分隔的名字组成的，通常包含组织名，而且始终包括两到三个字母的后缀，以指明组织的类型或该域所在的国家或地  
　　基本工作流程  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-1.png)
  
　　   
## 相关概念  
　　1、域名  
　　　　域名（Domain Name），是由一串用点分隔的名字组成的Internet上某一台计算机或计算机组的名称，用于在数据传输时标识计算机的电子方位（有时也指地理位置，地理上的域名，指代有行政自主权的一个地方区域）。  
　　　　注意域名对大小写不敏感  
　  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-2.png)　　　   
　　2、FQDN  
　　　　FQDN全称为Fully Qualified Domain Name，即完全合格域名。FQDN由两个部分组成：主机名和域名。因为DNS是逐级管理的，所以在不同的层级中主机名与域名也是不同的；以www.google.com为例，在第二层中，.com就是域名，google就是主机名，而到了第三层中，.google.com就成了域名，www就成了主机名。  
　　　　注意：主机名与域名并不是依据"."来划分的，主机名中也可以包含"."号的，主要还是要根据域名的注册情况来划分。  
　　3、正向解析  
　　　　从FQDN转换为IP地址称为正向解析。  
　　4、反向解析   
　　　　从IP地址转换为FQDN称为反向解析。  
　　5、区域  
　　　　正向解析或反向解析中，每个域的记录就是一个区域。  
## DNS的解析库  
　　DNS的主要作用是进行主机名的解析。解析：根据用户提供一种名称，去查询解析库，以得到另一种名称。 正向解析与反向解析使用不同的解析库。  
　　资源记录：rr(resource record)，有类型的概念；用于此记录解析的属性。  
•	A：Address地址 IPv4  
•	AAAA：Address地址 IPv6  
•	NS：Name Server域名服务器  
•	SOA：Start of Authority授权状态  
•	MX：Mail Exchanger邮件交换  
•	CNAME：Canonical Name规范名  
•	PTR：Pointer指针  
## DNS的查询过程  
　　DNS采用两种查询机制：递归（Recursive Query）和迭代（Iterative Query）。  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-3.png)　　 
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-4.png)  
　　实际应用中，即使用递归查询，又使用迭代查询  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-5.png)  　　   
## DNS的查询顺序  
　　　1、本地hosts文件  
　　　2、本地DNS缓存  
　　　3、本地DNS服务器  
　　　4、发起迭代查询  
## DNS使用的端口号  
　　DNS协议使用udp/tcp的53端口提供服务，客户端向DNS服务发起请求时，使用udp的53端口；DNS服务器间进行区域传送的时候使用TCP的53端口。  
## DNS服务器类型  
　　1、主DNS服务器  
　　　　为客户端提供域名解析的主要区域，主DNS服务器宕机，会启用从DNS服务器提供服务。  
　　2、从DNS服务器  
　　　　主服务器DNS长期无应答，从服务器也会停止提供服务。  
　　　　主从区域之间的同步采用周期性检查+通知的机制，从服务器周期性的检查主服务器上的记录情况，一旦发现修改就会同步，另外主服务器上如果有数据被修改了，会立即通知从服务器更新记录。  
　　3、缓存服务器  
　　　　服务器本身不提供解析区域，只提供非权威应答。  
　　4、转发服务器  
　　　　当DNS服务器的解析区域（包括缓存）中无法为当前的请求提供权威应答时，将请求转发至其它的DNS服务器，此时本地DNS服务器就是转发服务器。  
## BIND简介  
　　现在使用最为广泛的DNS服务器软件是BIND（Berkeley Internet Name Domain），最早有伯克利大学的一名学生编写，现在最新的版本是9，有ISC（Internet Systems Consortium）编写和维护。  
　　BIND支持先今绝大多数的操作系统（Linux，UNIX，Mac，Windows）  
　　BIND服务的名称称之为named  
　　DNS默认使用UDP、TCP协议，使用端口为53（domain），953（mdc，远程控制使用）  
## BIND安装  
　　本例使用的环境是CentOS 7.0的Linux操作系统（非CentOS 7.0系统，安装会有所区别），所以直接采用命令：yum install -y bind bind-chroot bind-utils  
　　其中bind-chroot和bind-utils是bind的相关包。  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-6.png)  　
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-7.png)  
　　   
## BIND配置  
　　1、BIND配置文件保存在两个位置：  
　　　　/etc/named.conf　　- BIND服务主配置文件  
　　　　/var/named/　　　　- zone文件（域的dns信息）  
　　如果安装了bind-chroot（其中chroot是 change root 的缩写），BIND会被封装到一个伪根目录内，配置文件的位置变为：  
　　　　/var/named/chroot/etc/named.conf 　　- BIND服务主配置文件  
　　　　/var/named/chroot/var/named/　　　　- zone文件  
　　chroot是通过相关文件封装在一个伪根目录内，已达到安全防护的目的，一旦程序被攻破，将只能访问伪根目录内的内容，而不是真实的根目录  
　　2、BIND安装好之后不会有预制的配置文件，但是在BIND的文档文件夹内（/usr/share/doc/bind-9.9.4），BIND为我们提供了配置文件模板，我们可以直接拷贝过来：  
　　　　cp -r /usr/share/doc/bind-9.9.4/sample/etc/* /var/named/chroot/etc/  
　　　　cp -r /usr/share/doc/bind-9.9.4/sample/var/* /var/named/chroot/var/  
　　3、配置BIND服务的主配置文件（/var/named/chroot/etc/named.conf），命令：vim /var/named/chroot/etc/named.conf；  
　　　　内容很多使用简单配置，删除文件中logging以下的全部内容，以及option中的部分内容，得到如下配置  
```   
/*  
 Sample named.conf BIND DNS server 'named' configuration file  
 for the Red Hat BIND distribution.  
  
 See the BIND Administrator's Reference Manual (ARM) for details about the  
 configuration located in /usr/share/doc/bind-{version}/Bv9ARM.html  
*/  
options  
{  
        // Put files that named is allowed to write in the data/ directory:  
        directory               "/var/named";           // "Working" directory
          
        // listen-on port 53     { any; };  
        listen-on port 53       { 127.0.0.1; };  
  
        // listen-on-v6 port 53  { any; };  
        listen-on-v6 port 53    { ::1; };  
}; 
```
   
 　4、在主配置文件（/var/named/chroot/etc/named.conf ）中加入，zone参数  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-8.png)     
　　　　   
　　5、新建example.net.zone文件，example.net的域名解析文件，zone文件放在/var/named/chroot/var/named/下，zone文件可以已/var/named/chroot/var/named/named.localhost为模板。  
　　　　命令：cp /var/named/chroot/var/named/named.localhost /var/named/chroot/var/named/example.net.zone  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-9.png)  　　　　   
　　　　文件example.net.zone的内容如下：  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-10.png)  　　　　   
　　6、禁用bind默认方式启动，改用bind-chroot方式启动。命令如下：  
　　　　[root@iZ2806l73p6Z named]# /usr/libexec/setup-named-chroot.sh /var/named/chroot on  
　　　　[root@iZ2806l73p6Z named]# systemctl stop named  
　　　　[root@iZ2806l73p6Z named]# systemctl disable named  
　　　　[root@iZ2806l73p6Z named]# systemctl start named-chroot  
　　　　[root@iZ2806l73p6Z named]# systemctl enable named-chroot  
　　　　[root@iZ2806l73p6Z named]#  
　　　　图：
      ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-11.png)  
　　　　   
　　　　注意：如果是CentOS 6.5的系统，这个步骤回有所区别，直接使用默认的service named start 启动服务，bind就直接运行在chroot包中，如下图：  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-12.png)  　　　　   
　　7、查看是否启动，命令：ps -ef|grep named  
　  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-13.png)  　　　   
　　8、测试DNS服务，本例在本机上测试，也可在其他电脑上测试，修改DNS服务的ip地址即可（命令：vim /etc/resolv.conf ），然后使用命令dig（命令：dig www.example.net）测试  
    ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-14.png)  
　　　　   
　　　内容如下：  
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-15.png)  
　　　　   
　　　测试结果：  
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-16.png)  　　　　   
　　　注：非本机测试需要修改主配置文件named.conf，允许任何ip访问，然后重启服务器  
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-17.png)  
![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-18.png)  
DNS服务-BIND从服务器、缓存服务器及转发服务器配置  
## 环境  
　　操作系统：CentOS 6.5  
　　DNS软件：bind  
## BIND从服务器  
　　从服务器就是在bind的主配置文件中添加从域example.net的配置信息即可3  
　　1、配置文件位置  
　　　　/var/named/chroot/etc/named.conf  
　　2、在主配置文件中添加一行域的zone定义：  
　　　　zone "example.net" {  
　　　　　　　type slave;  
        　　　　  masters { 120.27.99.64; };  
        　　　　  file "slaves/example.net.zone";  
　　　　}  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-19.png)      
　　　　   
　　3、由于bind是以named用户来运行的，所以要给存放zone文件的文件夹（/var/named/chroot/var/named/slaves）授权：  
　　　　未授权前：  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-20.png)  
　　　　   
　　　授权：  
  ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-21.png)    
　　　　   
　　4、启动bind服务，service named start，在存放zone文件夹（/var/named/chroot/var/named/slaves）中查看，已经把example.net.zone文件下下来了  
   ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-22.png)  
　　　　   
　　5、修改dns服务器ip地址，测试dig www.example.net  
    ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-23.png)  
　　　　   
## 缓存服务器及转发服务器  
　　一个DNS服务器可以即不是某个域的master服务器，也不是某个域的slave服务器，一个服务器可以不包含任务域的配置信息，它将接手到所有DNS查询进行递归解析，将解析结果返回给查询客户端，并且将查询结果缓存下来，这样的DNS服务器称之为caching name server。  
　　通常一个局域网中配置缓存服务器是为了加速网络访问。  
　　也可以为缓存服务器配置一个上游DNS服务器地址，缓存服务器可以给客户提供一个上游DNS服务器的地址，我们可以通过 以下设置完成：  
　　　　在主配置文件中的option中加入： forwarders { 192.168.0.168;};  
　　还可以通过一下选项让服务器转发所有DNS查询到forwarders服务器：  
　　　　在主配置文件中的option中加入： forward only;  
      ![](https://github.com/Daniel-Net/Sino-Bridge/blob/master/image/issue-8/8-24.png)  
　　   
  

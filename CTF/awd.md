
### 在使用`VMware`启动AWD靶机后，可使用`ssh`连接

```shell
ssh root@192.168.1.253
```

```
123456
```

### 在该靶机上有`systemd`服务用于开机启动`Docker`中的容器
### `awd` 服务

## 启动

```shell
systemctl start awd.docker.service
```
## 停止

```shell
systemctl stop awd.docker.service
```

## 开启开机自启

```shell
systemctl enable awd.docker.service
```

### 关闭开机自启

```shell
systemctl disable awd.docker.service
```


### 该服务默认启动4个队伍，若需查看队伍的端口映射情况可使用

```shell
sudo docker ps
```
### 添加`-a`选项可查看处于停止状态下的容器

```shell
sudo docker ps -a
```

```
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE              COMMAND                  CREATED        STATUS          PORTS                                                                                              NAMES
cc9c8d2da099   web_14.04:latest   "bash /var/www/html/…"   8 months ago   Up 25 seconds   22/tcp, 5000/tcp, 8080/tcp, 0.0.0.0:8080->80/tcp, :::8080->80/tcp                                  flag_server
76fbc36d580e   web_14.04:latest   "bash /var/www/html/…"   8 months ago   Up 18 seconds   22/tcp, 80/tcp, 5000/tcp, 8080/tcp                                                                 check_server
a4e76ae55e3f   web_14.04          "bash /var/www/html/…"   8 months ago   Up 9 seconds    5000/tcp, 8080/tcp, 0.0.0.0:2204->22/tcp, :::2204->22/tcp, 0.0.0.0:8804->80/tcp, :::8804->80/tcp   team4
55e8ee4e5a7a   web_14.04          "bash /var/www/html/…"   8 months ago   Up 11 seconds   5000/tcp, 8080/tcp, 0.0.0.0:2203->22/tcp, :::2203->22/tcp, 0.0.0.0:8803->80/tcp, :::8803->80/tcp   team3
38bbc96938a0   web_14.04          "bash /var/www/html/…"   8 months ago   Up 12 seconds   5000/tcp, 8080/tcp, 0.0.0.0:2202->22/tcp, :::2202->22/tcp, 0.0.0.0:8802->80/tcp, :::8802->80/tcp   team2
a949c04ad1d8   web_14.04          "bash /var/www/html/…"   8 months ago   Up 13 seconds   5000/tcp, 8080/tcp, 0.0.0.0:2201->22/tcp, :::2201->22/tcp, 0.0.0.0:8801->80/tcp, :::8801->80/tcp   team1
[root@localhost ~]#
```

### 如何访问？
### 以team1 举例

```
0.0.0.0:2201->22/tcp
```

### 这代表将容器的`22`端口映射为`2201` 
### `22`端口是`ssh`
### 该靶机地址是`192.168.1.253`
### 那么我们可以使用

```shell
ssh root@192.168.1.253 -p 2201
```
### 来访问team1
### 其中root代表用户名，如果是其他的，请自行替换

```
0.0.0.0:8801->80/tcp
```

### 同理，这个容器开放`80`端口映射到了`8801`端口，可以直接在浏览器打开

```URl
http://192.168.1.253:8801
```

根据[此项目](https://github.com/AppSo/shadowsocks_install/blob/master/docker/shadowsocks-libev/alpine/Dockerfile)文件修改, 主要为了支持客户端, 而不是服务端

#### 拉取镜像
```
docker pull SYFH/shadowsocks-libev-local
```   

创建 config 文件
比如在目录 /etc/shadowsocks-libev 下创建 config.json，完整路径也就是 /etc/shadowsocks-libev/config.json
范例内容如下：

```
{
    "server":"0.0.0.0",
    "server_port":9000,
    "password":"password",
    "timeout":300,
    "method":"aes-256-gcm",
    "fast_open":false,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp"
}
```

如果你想同时开启 simple-obfs，那么配置文件范例如下：

```
{
    "server":"0.0.0.0",
    "server_port":9000,
    "password":"password",
    "timeout":300,
    "method":"aes-256-gcm",
    "fast_open":false,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp",
    "plugin":"obfs-server",
    "plugin_opts":"obfs=http"
}
```


#### 启动容器
在上面这个范例里，定义的端口是 9000，那么在启动容器时就需要将 9000 端口映射到宿主机的对外端口上。
启动命令：

```
docker run -d -p 9000:9000 -p 9000:9000/udp --name ss-libev -v /etc/shadowsocks-libev:/etc/shadowsocks-libev SYFH/shadowsocks-libev-local
```

docker run：开始运行一个容器。
-d 参数：容器以后台运行并输出容器 ID。
-p 参数：容器的 9000 端口映射到本机的 9000 端口。默认是映射 TCP，当需要映射 UDP 时，那就再追加一次 UDP 的映射。冒号前面是宿主机端口，冒号后面是容器端口，可以写成一致，也可以不一致。
–name 参数：给容器分配一个识别符，方便将来的启动，停止，删除等操作。
-v 参数：挂载卷（volume），冒号前面是宿主机的路径，冒号后面是容器的路径，可以写成一致，也可以不一致。
SYFH/shadowsocks-libev-local：这是拉取回来的镜像路径。

####查看容器
利用如下命令可以查看所有已创建的 Docker 容器并显示容器的大小等信息：

```
docker ps -as
```
####停止容器
利用如下命令可以停止正在运行中的容器：

```
docker stop $name
```
此处的 $name 就是在启动容器那一步定义的容器的识别符，比如范例的 ss-libev

####启动容器
利用如下命令可以启动已经停止的容器：

```
docker start $name
```
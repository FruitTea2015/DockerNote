# docker容器(Ubuntu系统)安装shadowsocks客户端

1. 首先安装shadowsocks-libev和privoxy（安装privoxy的时候会报错说fetch失败之类的，解决方法是再执行一次apt-get update , 然后再执行 apt-get install privoxy）

```
# apt-get update
# apt-get install shadowsocks-libev
# apt-get install privoxy
```

2. 编辑shadowsocks配置文件

```
# vim /etc/shadowsocks-libev/config.json
{
"server":"1.2.3.4",
"server_port":23333,
"local_address":"0.0.0.0",
"local_port":1080,
"password":"mypassword",
"timeout":60,
"method":"chacha20-ietf-poly1305"
}
```

        说明："server":"1.2.3.4"表示安装了shadowsocks服务器端的远程服务器的IP地址，下面的server_port也是同理；"local_address":"0.0.0.0"表示监听代理本地所有IP地址，"local_port":1080一般不用改。password填写ss服务器的密码，methed按照服务器设置的加密方式填写。

3. ss-local 启动shadowsocks客户端

```
# ss-local
2019-02-15 10:08:43 INFO: initializing ciphers... chacha20-ietf-poly1305
2019-02-15 10:08:43 INFO: listening at 127.0.0.1:1080
2019-02-15 10:08:43 INFO: running from root user
```

确认客户端启动没有问题后用 nohup ss-local & 命令让客户端在后台运行；ps -ef 命令显示所有进程，可以在里面看到ss-local的进程号；如果要杀死进程，使用命令 kill -9 <进程号PID>，比如要杀死PID为7709的进程，则使用命令 kill -9 7709

1). 编辑privoxy配置文件 /etc/privoxy/config
2). 找到listen-address这一行（大约在第781行），将其修改为listen-address 127.0.0.1:8118，其中8118就是你需要的http代理的端口。
        注意：实际操作中发现 listen-address 127.0.0.1:8118 这一行下面还有一行，内容是listen-address [::1]:8118 ，经过测试，这一行必须要注释掉（即在这一行最左边写上#），否则会报错

3). 再找到forward-socks5t这一行（大约在第1388行），取消注释，将这一行修改为 forward-socks5t / 127.0.0.1:1080 . 1080是socks代理的端口。
4). 重启服务
5). service privoxy start #默认自启动
6). 给系统设置http代理

使用命令 vim ~/.bashrc 对该文件进行编辑（如果没有vim命令，可以先apt-get update , 然后apt-get install vim）。按键盘上的 i 键进入编辑（Insert）模式，在最下面写三行语句：

```
export ALL_PROXY="socks5://0.0.0.0:1080"
export http_proxy="http://0.0.0.0:8118"
export https_proxy="https://0.0.0.0:8118"
```

```
source ~/.bashrc    注释：重启环境变量，使得刚刚的修改生效wget google.com      注释：测试能否连接google,若能连接，则说明ss客户端配置成功
```


 到此为止，docker容器（Ubuntu系统）应该已经可以连接外网，后面的暂时不用看。

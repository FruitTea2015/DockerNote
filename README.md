# DockerNote
Some Notes of using Docker in China mainland.

由于网络原因，截至2020年11月，在中国大陆访问google等网站需要使用代理或VPN等途径。如果docker中的程序需要连接google或某些其他网站服务器下载数据集，则需要在docker中设置代理。
《docker容器(Ubuntu系统)安装shadowsocks客户端》这片文章是我=在已有shadowsocks服务器账号的基础上，为docker容器（container）安装ss客户端的过程。以后可以直接照次教程配置。

《Docker学习之SSH连接docker容器》和《使用pycharm专业版远程调试docker容器的程序》是遇到了这样的问题：在GitHub下载的代码有时候是docker镜像形式，而dockers镜像运行为容器以后，只有命令行界面，调试和修改相当不方便，所以采用宿主机pycharm+ssh的方式远程调试和修改docker容器里的app代码。

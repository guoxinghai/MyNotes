# 远程登录Linux系统

**说明：**

* Linux服务器是小组共享的
* 正式上线的项目是运行在公网的
* 程序员需要远程登录到Linux进行项目管理或者开发
* 远程登录客户端有：Xshell，Xftp。其它远程工具大同小异
* 需要Linux开启sshd服务(22号端口)

**远程登录 Linux-Xshell5**

Xshell5的关键配置

新建会话（设置）

* 名称——自己随便写

* 协议——SSH
* 主机——xxx.xxx.xxx.xxx(这个是你要连接的Linux的IP使用`ifconfig`来查看)
* 端口——22


# `ls -l`信息解释

例：`-rwxrw-r-- 1 root root 1213 feb 2 09:39 abc`

* `-rwxrw-r--`介绍（0-9位）

  * 第0位确定文件类型
    * `-`：常规文件【重点】
    * `d`：目录文件【重点】
    * `b`：快(block)设备，如硬盘
    * `c`：字符(character)设备文件，如键盘，鼠标
    * `|`：链接文件，如软链接文件
    * `p`：pipe即命名管道文件
    * `s`：socket即套接字文件

  * 第1-3位确定所有者拥有该文件的权限
    * `r`：可读
    * `w`：可写
    * `x`：可运行
    * `-`：无

  * 第4-6位确定所属组拥有该文件的权限

    **同1-3位**

  * 第7-9位确定其他用户拥有该文件的权限

    **同1-3位**

* 1 介绍

  * 如果该文件是个目录

    表示该目录包含的文件个数

  * 如果不是目录

    表示该文件的硬连接个数(包括自己)

* root表示该文件所有者
* root表示该文件所属组
* 1213表示该文件大小(默认以字节为单位)
* feb 2 09:39表示文件最后更改时间
* abc文件名


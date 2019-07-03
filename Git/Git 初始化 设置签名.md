# Git 初始化 设置签名

* 本地库初始化

  命令：`git init`

* 设置签名

  * 形式
    * 用户名：tom
    * Email地址：goodmorning@qq.com

  * 作用

    区分不同开发人员的身份

  * 辨析

    本地库设置的签名和代码托管中心的账号密码没有关系

  * 命令

    * 项目级别/仓库级别

      `git config  user.name UserName`

      `git config user.email UserEmail`

    * 系统用户级别

      `git config --global user.name UserName`

      `git config --global user.email UserEmail`

    * 级别优先级
      * 就近原则：项目级别优先于系统用户
      * 如果只有系统用户级别就以系统用户级别的签名为准
      * 不允许二者都没有设置

    
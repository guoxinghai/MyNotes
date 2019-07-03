# pull 出错

> ​	当我们在github新建了一个远程库，如果这个远程库初始化了一个`readme.md`文件，我们无法直接push到远程库，需要先pull一下远程库。
>
> 但是第一次pull会报一个错误`refusing to merge unrelated histories`



**问题原因：**

出现这个问题的最主要原因还是在于本地仓库和远程仓库实际上是独立的两个仓库。`git pull` 是命令 `git fetch`和`git merge`的结合，**git默认拒绝两个独立仓库的合并**。假如之前是直接clone的方式在本地建立起远程github仓库的克隆本地仓库就不会有这问题了。

**问题解决：**

使用命令：`git pull origin master --allow-unrelated-histories`可解决无法合并的问题


[TortoiseGit入门教程（个人用 可能存在问题）](https://www.cnblogs.com/zjl8455482/p/13857540.html)

先克隆主分支
再切换分支   git checkout 分支名
//下拉最新    git pull origin 分支名
//从
From ssh://192.168.0.250:29418/master
branch            BASE\_ZD16\_C228228 -> FETCH\_HEAD
Already up to date.  //已经更新好了

git branch   查看当前分支

`git log --pretty=oneline`  查看提交日志

git 删除服务器的最近一次提交记录
这种情况比较简单，主要操作分两步：
第一步：回滚上一次提交      `git reset --hard HEAD^`
第二步：强制提交本地代码    `git push origin master -f`

### git删除已push记录

**按顺序执行**


| 命令                                                                                      | 作用                                                                                                                                 |
| ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| git log                                                                                   | 查出历史提交记录                                                                                                                     |
| git reset commit-id                                                                       | 把提交记录回滚到上一次提交(不建议用git revert，因为用它不但不会删除你想删除的那条记录，还会有一条新的提交记录用来重置你的上次的修改) |
| git log                                                                                   | 确认是否删除错误的提交记录                                                                                                           |
| git status                                                                                | 发现代码变成未提交状态，重新add并commit正确的代码修改                                                                                |
| 不要pull远程代码(防止第2步白做)，直接用git push --force origin 分支名，强制push到远程分支 |                                                                                                                                      |

### 修复TortoiseGit文件夹和文件状态图标不显示问题

可以参考如下
[修复TortoiseGit文件夹和文件状态图标不显示问题\_51CTO博客\_tortoisegit文件夹没有图标](https://blog.51cto.com/u_15801765/5697108)

* 按`Win+R`键打开运行对话框，输入 regedit ，打开注册表；
* 找到 `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer`；
* 新建一个“`字符串值`”名称为 “`Max Cached Icons`” 值是 “`2000`”；
* `重启一下电脑`，看图标是否显示。
* 如果图标还不显示，看下一步。
  * \`修改注册表
    * 在弹出的注册表编辑器中找到`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers`这一项。
    * 找到后可以发现在该项下有很多个，而`Windows Explorer Shell `支持的 `Overlay Icon` 最多 15 个，Windows 自身使用了 4 个，只剩 11 个可扩展使用。
    * 编辑`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers`，修改`tortoise`相关的名称（如在名称前加一个空格前缀，并加数字）。目的是将其排列到前面。
    * 重启电脑，图标正常显示。

### 修复TortoiseGit查看单个文件提交记录时，却显示全部文件的提交记录

在这种情况下，我们查看`show log`,在界面左下角能发现， “显示整个工程”的选项，被勾选上了，取消即可！！！

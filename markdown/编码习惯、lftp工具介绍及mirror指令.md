#编码习惯、lftp工具介绍及mirror指令
## 1. 编码习惯
每个人的编码习惯都不同，计算机技术发展到现在，会出现各种各要的软件来解决纷杂的问题，而如何合理地选择最适合自己的工作所需工具也是入门必备。
### 1.1 代码编辑器
写linux程序我喜欢用mvim 来写程序，完全脱离鼠标、不输于`Source Insight`的代码结构浏览窗口、随意的分屏操作及自由配置等等都是我喜欢它的理由。
### 1.2 虚拟机
对于linux 程序来我说我感觉环境是最重要的，因为程序最后一般部署在RedHat 6上，所以我选择 Parallels Desktop 来作为我的虚拟机，选择它是因为在mac上，它做得确实很不错。
<!-- more -->
### 1.3 Coding
这样就够成了基本环境，在macvim 里编写代码，上传至虚拟机，在linux虚拟机里编译、测试，保持自身系统的纯净。下面就说到关键的文件传输工具。
### 1.4 SSH
SSH工具以前用SecureCRT，但在使用sftp过程中发现不能用`tab`自动完成，故选择 iTerm2 来作为ssh 终端了，用macports安装的openssh，其里面带的sftp带有tab 自动完成功能，用得不错。但待我升级系统10.8到10.9，将macports清理换成homebrew后发现竟然没有顺手的sftp工具了，安装 `with-readline`老是报错，如果解决，工程量又有点大，就在不经意间结识了lftp

## 2. lftp 介绍
本人不喜欢写博客的复制粘贴百科的，我就以我的使用情况来说，这是一个很强大的工具，一点小皮毛的工具就解决了我的很多问题。lftp 相当给力地支持sftp，而且支持tab自动补全，稍加配置就可以使自己的工具很高效。
### 2.1 lftp bookmark
在你使用lftp登录sftp后，可以使用bookmark记住此会话，具体使用办法参考
``` shell
help bookmark
```
这些bookmark会保存在`~/.local/share/lftp/bookmarks`文件里
往`~/.bash_profile`里添加下述代码，可以实现lftp tab补全bookmark
``` shell
complete -W "$(echo `cat ~/.local/share/lftp/bookmarks |cut -f 1`;)" lftp
```
### 2.2 lftp mirror
这是一个相当给力的功能，可以把服务器上的一个文件夹和本地的一个文件夹进行映射，使用方法照旧
``` shell
help mirror
```
本人常用的是
``` shell
mirror -R xxx xxx
```
同时mirror 命令支持正则来排除一些无需同步的文件夹及文件。 e.g:
``` shell
mirror --exclude logs/ --exclude reports/ --exclude-glob *.bak
```

## 3 总结
当然lftp 功能不止这些，这只是冰山一脚，我只把我常用的这些功能列出来。它的mirror功能可以提供同步备份，具体使用办法，网上比较多，我也就不多说了，此处重点是mirror 里的正则，网上资料少，希望给读者一些帮助吧。
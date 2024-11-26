+++
title = 'Mac安装三方软件失败,提示:无法检查恶意软件,已损坏移到废纸篓'
date = 2024-11-26T16:53:58+08:00
tags = ['mac']
+++
## 安装时提示已损坏
![已经损坏](record/breakdown.jpg)
运行如下命令
``` ssh
// 注意空格需要使用\ 进行转义
sudo xattr -d com.apple.quarantine /Applications/xxx.app
```
## 安装时提示无法检查恶意软件
![恶意软件](record/security.png)
直接在如图位置打开允许任意安装即可

如果觉得不是特别安全可以设置成 "appStore和已知的开发者"，

这样如果再次提示恶意软件，来这里会有一个允许选项，允许即可
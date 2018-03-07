---
title: "Sublime Text插件安装方法"
date: 2017-12-9T08:08:50-04:00
share: false
---

#### 1.通过 Package Control 进行在线安装
说在前面:Package Control 是一个用来进行在线安装插件的工具，在安装插件前需要先安装 Package Control。

###### 安装 Package Control
安装方法：通过快捷键 ``` Ctrl + ` ```打开  Console，在最底部把下面的代码复制粘贴过去后回车，然后稍微等待一段时间。

```
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
如果未能安装，或者报错可能是版本更新导致失效，此时可以进入官网复制粘贴官网提供代码。
 <https://packagecontrol.io/installation#st3> [传送门](https://packagecontrol.io/installation#st3)
安装成功后重启Sublime Text即可。
###### 通过 Package Control 安装 Sublime 插件
使用方法：使用 `Ctrl + Shift + P ` 调出面板，然后输入 `pci` ，选中  `Package Control: Install Package`，然后通过输入插件的名字找到插件并回车安装即可。



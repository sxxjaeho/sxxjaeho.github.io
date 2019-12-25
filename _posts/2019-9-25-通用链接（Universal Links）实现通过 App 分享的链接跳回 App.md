---
layout: post
title: '通用链接（Universal Links）实现通过 App 分享的链接跳回 App'
cover: https://raw.githubusercontent.com/sxxjaeho/assets/master/assets/images/images1712780-aceb0e8284347eb9.png
date: 2019-9-25
categories: Plugins
tags: iOS
---


>QQ互联将于2019年12月1日进行Universal Links校验，请开发者及时更新适配。

<https://wiki.connect.qq.com/qq互联将于2019年12月1日进行universal-links校验，请开发者及时更>

![图1 QQ互联 wiki](https://raw.githubusercontent.com/sxxjaeho/assets/master/assets/images/images1712780-aceb0e8284347eb9.png)

官方说明：
当支持通用链接时，用户可以点击网站链接重定向到已安装的应用（无需通过Safari浏览器），如果未安装应用则打开该网站链接。

### 配置前准备：

`配置前提：域名需要支持 HTTPS 连接`

### 开始配置：
1. 打开 <https://developer.apple.com>，登录苹果开发者账号，开启要配置的`APP IDs`的`Associated Domains`服务 __（开启服务后需要重新激活失效的Provisioning Profiles，并重新下载安装）__。
![图2 苹果开发者官网-Certificates, Identifiers & Profiles-Identifiers-开启服务](https://raw.githubusercontent.com/sxxjaeho/assets/master/assets/images/images1712780-6e6f462b941c0209.png)

2.项目配置支持通用链接的域名（支持添加多个域名）
格式：applinks:xxx.xxx.xx，例如：applinks:d.vibesix.cn。
![图3 Capabilities-Associated Domains-添加需要支持的域名](https://raw.githubusercontent.com/sxxjaeho/assets/master/assets/images/images1712780-1d34f842ecdae83b.png)

3.需要服务器支持，确认通用链接对应的App的身份，创建一个命名为`apple-app-site-association`文件（没有后缀名），并写入一下`JSON`格式数据，如下：

```swift
{
    "applinks": {
        "apps": [],
        "details": [
            {
                "appID": "<App ID Prefix>.<Bundle ID>",
                "paths": [ "/open-app.html/*"]
            }
        ]
    }
}
```

然后将`apple-app-site-association`文件拷贝到该域名服务器的`.well-known`文件夹下面。

__ps:`App ID Prefix`和`Bundle ID`可在`图1`蓝框部分获取到，`paths`是通用链接域名后面的路径，*代表任意路径__。

4.测试通用链接是否生效，进入<https://search.developer.apple.com/appsearch-validation-tool>
进行验证。
![图4 验证通用链接是否生效](https://raw.githubusercontent.com/sxxjaeho/assets/master/assets/images/images1712780-1de86141da9bfaad.png)
然而我试过n+1次，无法验证成功，并报错：

`Could not extract required information for application links. Learn how to implement the recommended Universal Links.`

`Error no apps associated with url`

果断放弃验证（后来发现这个过程可以忽略），然后试了网上的方法将通用链接粘贴到手机`备忘录`上，点击链接即可跳转到 App 里，如图：![图5 备忘录验证通用链接是否生效](https://raw.githubusercontent.com/sxxjaeho/assets/master/assets/images/images1712780-0634046aefda436f.gif)

5.进入应用，定位到指定功能模块。

```swift
- (BOOL)application:(UIApplication *)application continueUserActivity:(NSUserActivity *)userActivity restorationHandler:(void (^)(NSArray<id<UIUserActivityRestoring>> * _Nullable))restorationHandler {
    if ([[userActivity activityType] isEqualToString:NSUserActivityTypeBrowsingWeb]) {
        NSString *host = userActivity.webpageURL.host;
        if ([host isEqualToString:@"d.vibesix.cn"]) {
                // 跳转操作
        }
    }
    return YES;
}
```

****

### 实现效果：
![图6 手机安装通用链接指向的应用时](https://raw.githubusercontent.com/sxxjaeho/assets/master/assets/images/images1712780-9c955fc36bad853b.gif)

![图7 手机没有安装通用链接指向的应用时](https://raw.githubusercontent.com/sxxjaeho/assets/master/assets/images/images1712780-9400841bab7159cd.gif)

### 参考链接：
* [Apple-Support Universal Links](https://developer.apple.com/library/archive/documentation/General/Conceptual/AppSearch/UniversalLinks.html)
* [ios通用链接 UniversalLink在微信打开app](https://www.jianshu.com/p/8e8840dcd54d)
* [iOS Universal Links(通用链接)](https://yohunl.com/ios-universal-links-tong-yong-lian-jie/)
* [iOS 通用链接（UniversalLinks）+ 分享功能的一些看法](https://www.jianshu.com/p/8ae3576b12b0)
* [通用链接(Universal Links)实践笔记](https://www.jianshu.com/p/848158b70a57)


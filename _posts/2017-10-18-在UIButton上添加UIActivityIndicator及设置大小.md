---
title: "在UIButton上添加UIActivityIndicator及设置大小"
date: 2017-10-18T08:08:50-04:00
share: false
categories:
  - Objective-C
---


开发中会遇到点击一个按钮,按钮上就多一个系统的小菊花,来示操作正在进行,例如下面的加好友按钮

![](http://upload-images.jianshu.io/upload_images/1712780-8f2fac0aff3ee5a1.gif?imageMogr2/auto-orient/strip)

直接上代码

```
    //初始化按钮
    UIButton *addFriendButton = [UIButton emptyFrameView];
    addFriendButton.layer.masksToBounds = YES;
    addFriendButton.layer.cornerRadius = 4.f;
    addFriendButton.layer.borderWidth = 0.5f;
    addFriendButton.layer.borderColor = [[UIColor green1Color] CGColor];
    addFriendButton.titleLabel.font = [UIFont systemFontOfSize:12.1];
    [addFriendButton setBackgroundColor:[UIColor white1Color]];
    [addFriendButton setTitle:@"+好友" forState:UIControlStateNormal];
    [addFriendButton addTarget:self action:@selector(didClickAddFriend:) forControlEvents:UIControlEventTouchUpInside];
```

```
- (IBAction)didClickAddFriend:(id)sender{
    //将文字置空
    [self.addFriendButton setTitle:nil forState:UIControlStateNormal];
    //初始化系统小菊花
    UIActivityIndicatorView *addFriendActivityIndicator = [[UIActivityIndicatorView alloc] initWithFrame:self.addFriendButton.bounds];
    [addFriendActivityIndicator setUserInteractionEnabled:YES];
    [addFriendActivityIndicator setActivityIndicatorViewStyle:UIActivityIndicatorViewStyleWhite];
    //改变小菊花颜色
    [addFriendActivityIndicator setColor:[UIColor green1Color]];
    //下文去解释下面这两行行代码的作用
    //CGAffineTransform transform = CGAffineTransformMakeScale(.7f, .7f);
    //addFriendActivityIndicator.transform = transform;
    [self.addFriendButton addSubview:addFriendActivityIndicator];
    //小菊花开始转圈圈
    [addFriendActivityIndicator startAnimating];
    //用block实现加好友操作
    if ([self addFriendHandler]) {
        self.addFriendHandler();
    }
}
```

>UIActivityIndicatorView不能自定义大小,创建的UIActivityIndicatorView有三种style,这三种style有默认的大小,不能通过设置frame的方式来修改大小,那么上面代码中注释的那一行就起作用了
需求:需要把显示的UIActivityIndicatorView显示得比预定义的小,实现的方式是通过transform来修改显示UIActivityIndicatorView的显示大小

```
    CGAffineTransform transform = CGAffineTransformMakeScale(.7f, .7f);
    addFriendActivityIndicator.transform = transform;
```


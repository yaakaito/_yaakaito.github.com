---
layout: post
title: "registerNibを使うとさらによいらしいです"
date: 2013-02-11 02:51
comments: true
categories: [Objective-C, iOS, UIKit, InterfaceBuilder]
---

こんにちは！うきょーです！
[前の続きです](http://yaakaito.github.com/blog/2013/02/10/uinib-lost-jinsei/)

UINibで楽チンに`UITableViewCell`をxibで作れることは分かったけど、

```objective-c
UINib *nib = [UINib nibWithNibName:@"HogeCell" bundle:nil];
cell = [[nib instantiateWithOwner:nil options:nil] objectAtIndex:0];
```

このコードはぶっちゃけ微妙だよね。

で、iOS5~だと`registerNib`というものがあるらしく

```objective-c
- (void)viewDidLoad {
    [super viewDidLoad];
    
    [self.tableView registerNib:[UINib nibWithNibName:@"HogeCell" bundle:nil] forCellReuseIdentifier:kHogeCellReuseIdentifier];
}
```

こういう感じに書いておくと、さっきの生成部分が

```objective-c
cell = [[HogeCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:kBGHogeCellReuseIdentifier];
```

こういう感じにいける、こっちのがよさげ。

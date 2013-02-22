---
layout: post
title: "Stubbilinoというスタブライブラリがよさげです"
date: 2013-02-12 04:28
comments: true
categories: [Objective-C, Testing, Mock]
---

こんにちは、うきょーです。
[Stubbilino](https://github.com/robb/Stubbilino) というスタブライブラリをみけたのですが、結構良さげです。

```objective-c
SEHoge *hoge = [[SEHoge alloc] init];
NSObject<SBStub> *stub = [Stubbilino stubObject:hoge];
[stub stubMethod:@selector(intMethod) withBlock:^ {
    return 10;
}];

NSLog(@"%d", [hoge intMethod]);
```

みたいにselectorとBlocksを使って書きます。
このBlocksはオブジェクトとか気にせず返せるみたいで、

```objective-c
[stub stubMethod:@selector(stringMethod) withBlock:^(){
    return @"string";
}];
```

こういうのもいける、超便利。
OCMockとかだと`OCValue`がどうたらとか出てくるので、非常に楽チンに書ける。
Blocks使ってるので、ちょっと生成がめんどいオブジェクトとかも、押し込んでおけるのでよい。

あとはクラスメソッドのスタブも出来るみたいです。
こっちはまだ使ってないのでREADMEからの引用ですが、

```objective-c
// https://github.com/robb/Stubbilino/blob/master/README.md
Class<SBClassStub> uiimage = [Stubbilino stubClass:UIImage.class];

[uiimage stubMethod:@selector(imageNamed:)
          withBlock:^(NSString *name) { return myImage; }];
```

こういう感じで書けるみたいです。
クラスメソッドのスタブは往々にしてだるいという感じだったので、良い。

OCMockなんかは`NSProxy`を使った実装ですが、こっちは`objc/runtime.h`使っています。
なのでちょっとコード追うのはしんどいですが、読んでみるとよいと思います。


　

## 宣伝:2/23に東京でiOSカンファレンスを開催します！

「conferenceWithDevelopers」というタイトルです。


[公式サイト](http://conference-with-developers.info/)

[チケットをゲットはこちらから](http://peatix.com/event/9727)

無料です！

豪華スピーカーでお送りしますので、みなさん是非ご参加ください！LT発表者も募集してますよ！

<iframe frameborder="0" width="400" height="460" src="http://peatix.com/event/9727/share/widget?z=1&c=dark&t=1&a=1"></iframe>
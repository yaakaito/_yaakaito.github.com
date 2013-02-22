---
layout: post
title: "Objective-CでRoleやってみる"
date: 2013-01-31 01:38
comments: true
categories: [Objective-C]
---

こんにちは、うきょーです！
Objective-C書いててふと、Role使いたいなー出来たっけーと、思ったのでやってみた。

怪しいところもあるので、あんまり参考にならないかもですよと言っておく。

## 最終的にデータと振る舞いを合体させる
ってのができればいいと思うんだけど、Objective-Cならカテゴリで書いておけばよくね？と思ったり思わなかったり。
けどちょっと違うよねーってことで、今回はそういうのはやめとく。

で、実装としては`NSProxy`使えばそれっぽいものは割と簡単に出来た。

[ObjCRoleSample](https://github.com/yaakaito/ObjCRoleSample)


```objective-c
@interface Book : NSObject
@property (nonatomic, strong) NSString *title;
@property (nonatomic) NSUInteger price;
@end
```

こういうモデルに対して、

```objective-c
@implementation BookPurchase

- (void)purchase
{
    Book *this = (Book *)self.target;
    NSLog(@"Purchased! %@ %u", this.title, this.price);
}
@end
```

こんな感じの振る舞いを、

```objective-c
Book *book = [[Book alloc] init];
book.title = @"hoge";
book.price = 420;
 
Book<BookPurchase> *extended = [book roleExtended:BookPurchase.class];
 
[extended purchase];
```

こう合成できるようにしてみた。

`Book<BookPurchase>`が気に入らないと人は、ちょっと微妙な感じがするんだけど、`Book`の定義の方に書いちゃえば省略はできる。


実装としては基本的には`NSProxy`で自分にメッセージ送れなかったら流すようにしたものをベースクラスにしてあげて、`protocol`を使ってこんな感じでロールを定義する

```objective-c
@protocol BookPurchase <NSObject>
@optional
- (void)purchase;
@end

@interface BookPurchase : OCRole <BookPurchase>
@end
```

こうやると`BookPurchase`ロールと`Book`モデルで同じメッセージが送れるような(正確にはXCodeでコンパイル出来るような)感じになる。
見た目はあんまりよくない。し、あんまり実用的でもないような感じがする。マクロとかで書きやすくしてあげるとちょっとは変わるかなー。


## 宣伝:2/23に東京でiOSカンファレンスを開催します！

「conferenceWithDevelopers」というタイトルです。


[公式サイト](http://conference-with-developers.info/)

[チケットをゲットはこちらから](http://peatix.com/event/9727)

無料です！

豪華スピーカーでお送りしますので、みなさん是非ご参加ください！LT発表者も募集してますよ！

<iframe frameborder="0" width="400" height="473" src="http://peatix.com/event/9727/share/widget?z=1&c=dark&t=1&a=1"></iframe>
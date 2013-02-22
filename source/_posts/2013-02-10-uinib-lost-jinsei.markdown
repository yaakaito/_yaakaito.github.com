---
layout: post
title: "UINib知らずにCellが作れなくて人生半分くらい損した話"
date: 2013-02-10 16:32
comments: true
categories: [Objective-C, iOS, UIKit, InterfaceBuilder]
---

こんにちは！うきょーです！
僕は`UIView`とか`UITableViewCell`を継承してかっちょいいビューを作ろうとすると3分でやる気が消える人なのですが、
最近UINibというものを知りました、創作意欲を返してほしいと思います！！！！

### 追記しました
[続き](http://yaakaito.github.com/blog/2013/02/11/register-nib/)


みなさん`UITableViewCell`のサブクラス作りますよね。
そしてそのままコーディングしていくと非常にだるく、3秒でモチベーションがなくなるので、
Interface Builderを使いたい！と思うわけですが、`UITableViewCell`用のxibファイル作るのは30秒で心が折れる。
(というかいつも忘れるしXcodeのバージョンあがると分からなくなる)

## UINibを使う

おもむろにxibファイルを作ります。

{% img /images/uinib_1.png 320 %}

UIViewを消します。

{% img /images/uinib_2.png 320 %}

UITableViewCellをおきます。

{% img /images/uinib_3.png 320 %}

`Class`と`Reuse identifiter`をセットします。
FilesOwnerの`Class`はとりあえず`UIViewController`にしておきます。

あとはこれをdatasource側で

```objective-c
    HogeCell *cell = [tableView dequeueReusableCellWithIdentifier:kHogeCellReuseIdentifier];
    if (!cell) {
        UINib *nib = [UINib nibWithNibName:@"HogeCell" bundle:nil];
        cell = [[nib instantiateWithOwner:nil options:nil] objectAtIndex:0];
    }
    
    [self updateCell:cell indexPath:indexPath];
```

とかすれば使えた、泣いた。いままでのよくわからん！はなんだったのか。

## 細かいところはコードで制御したい

ざっくりとしたレイアウトとかを決めるのはxibで大分楽できるんですが、
もっと細かいところとか、同じようなのだし、まとめて処理してほしい(背景とか)ってときに、毎回xibいじるのはだるいですね。

まあなんだかんだでいろいろコードで弄りたいこととか、コードから追加したい要素とかもあると思うんですが、
これをビューコントローラーからやったりするとゴミみたいなコードになるので、あまりやりたくないわけです。

が、しかしこれは`willMoveToSuperview:`あたりをうまく使えば解決できることに気づいて

```objective-c
- (void)willMoveToSuperview:(UIView *)newSuperview {

	[super willMoveToSuperview:newSuperview];
    
	// 共通のスタイルとかコードでやりたい処理書く

}
```

ってやればあっさりできた、全俺が号泣した。

## 次にだるいのが

```objective-c
- (void)setHoge:(Hoge *)hoge {
    
    if (hoge != hoge) {
        _hoge = hoge;
        
        self.thumbnail.image = hoge.thumbnail;
        self.message.text = hoge.message;
    }
}
```

こういうの、あると思います。(updateCellとか)

Model-View-Binder使えばうまくいけそうですが、メジャーなの知らないので教えてください。

けど多分ないような気がするので作ります。




## 宣伝:2/23に東京でiOSカンファレンスを開催します！

「conferenceWithDevelopers」というタイトルです。


[公式サイト](http://conference-with-developers.info/)

[チケットをゲットはこちらから](http://peatix.com/event/9727)

無料です！

豪華スピーカーでお送りしますので、みなさん是非ご参加ください！LT発表者も募集してますよ！

<iframe frameborder="0" width="400" height="460" src="http://peatix.com/event/9727/share/widget?z=1&c=dark&t=1&a=1"></iframe>

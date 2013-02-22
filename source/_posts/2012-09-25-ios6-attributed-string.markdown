---
layout: post
title: "iOS6のNSAttributedStringで色とか付ける"
date: 2012-09-25 13:53
comments: true
categories: [iOS, iOS6, Objective-C]
---

こんにちは！うきょーです！
いままでは外部のライブラリを使わないとかなり面倒だった、UILabelとかに色付きの文字列の描画が、iOS6から標準サポートになりました！
便利なので使ってみましょう！！！！

## サンプル
適当に色を付ける文章を探します、今回はgithubから「yaakaito pushed to master at yaakaito/Specs」という文章を抜き出してきました。
このうち yaakaito と yaakaito/Specs を青色にしてみようと思います。

コードはいつも通りgithubにあげてあります。

* [AttributedStringExample](https://github.com/yaakaito/AttributedStringExample)

## NSAttributedStringを作る
実際のところ結構だるいのであんまり使ったことがない人も多いと思うのでまずはAttributedStringの作り方から。

作り方は大きく分けて二通りあって、

* 小さいNSAttributedStringを複数つくって最後に繋げる
* 最初に全文でNSAttributedStringを作ってRangeで指定する

という具合なのですが、多分前者の方が扱いやすいです。この記事では前者を使っています。

今回は色を付ける場所が二箇所＋プレーンなものが間に一つなので、全部で三つのNSAttributedStringを作って繋げます。

まず yaakaito の部分を作ります。

```objective-c
NSAttributedString *name = [[NSAttributedString alloc] initWithString:@"yaakaito"
                                                           attributes:@{NSForegroundColorAttributeName : [UIColor blueColor]}];
```

`NSString`と同じような要領で、`initWithString:`してあげて、二個目の引数に`attributes`を取ります。
この`attributes`は`NSDictionary`で、今回の例だと文字の色だけ指定しています。
他にも背景色や、フォント、アンダーラインを付けたり、影を付けたりとか、いろいろできるようです。
よく使いそうなのは `NSForegroundColorAttributedName`(文字の色) と `NSFontAttributeName`(フォント) あたりですかね。

せっかくなので次はフォントも指定してみましょう。 yaakaito/Specs の部分を作ります。

```objective-c
NSAttributedString *repository = [[NSAttributedString alloc] initWithString:@"yaakaito/Specs"
                                                                 attributes:@{ NSForegroundColorAttributeName : [UIColor blueColor],
                                                                               NSFontAttributeName : [UIFont boldSystemFontOfSize:16]} ];
```

最後に真ん中の部分を作って、連結します。

```objective-c
NSAttributedString *others = [[NSAttributedString alloc] initWithString:@" pushed to master at "];
        
NSMutableAttributedString *message = [[NSMutableAttributedString alloc] initWithAttributedString:name];
[message appendAttributedString:others];
[message appendAttributedString:repository];
```

これで、NSAttributedStringの完成です。

## UILabelに表示する

こっちは笑えるほど簡単で、いままでは `label.text` にNSStringを入れていたものを、 `label.attributedText` にNSAttributedStringを入れるだけです。

```objective-c
UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, 320, 320)];
[self.view addSubview:label];
label.attributedText = message;
```

こうするとさっき作ったNSAttributedStringが表示されます、便利ですね！！！！



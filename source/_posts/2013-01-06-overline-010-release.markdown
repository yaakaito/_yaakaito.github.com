---
layout: post
title: "Objective-C用ユーティリティOverlineを公開しました"
date: 2013-01-06 13:45
comments: true
categories: [Objective-C, Overline]
---

こんにちは！うきょーです。寒いですね。
Objective-C(主にiOS開発)向けのユーティリティライブラリを公開しました。
バージョンは0.1.0で、まだAPIはそこまでそろってません。

[Overline](https://github.com/yaakaito/Overline)

似たようなライブラリではunderscore.mとかBlocksKitが近いかなーと思います。
基本的な機能の拡張で、めんどくさいところを楽にする系のライブラリです。ひかえめです。

主に僕がだるいなーと思ったベースで追加しています。
なのでいわゆるmapもあれば、URLエンコードしてくれるメソッドもありますし、という感じ。あとはいつも忘れる系とか。

underscoreなんかと違うところは、underscoreとかってがんばってJSっぽく書こうとしてる感じが伝わってくるんですが、
僕はJavaScriptみたいに書くのが綺麗だなとか書きやすいとかまったく思ってないので、Objective-Cらしく書けるようにしてあります。
好みのレベルかなーくらいのショートハンドは用意しています。

## 使い方

Cocoapodsでやるのが楽です。

```ruby
pod 'Overline'
```

```
pod install
```

して

```objective-c
#import "Overline.h"
```

簡単ですね。


必要なところだけ使いたかったり、自分でプロジェクトに追加する場合は、`/Overline`の下から好きなファイルをプロジェクトに追加してください。
READMEに書いてある通りに分割してあるので、それなどを参考に。

## 出来る事

一覧はREADMEに書いてありますが、一例をだすと`map`とか

```objective-c
NSArray *mapped = [@[@1,@2,@3,@4,@5,@6] mappedArrayUsingBlock:^id(id obj, NSUInteger idx) {
    if ([obj integerValue] % 2 == 0) {
        return obj;
    }
    return nil;
}];
// @[@2,@4,@6]
```

こういう感じで正規表現でマッチングできたりとか (正規表現オブジェクトとしてちゃんと表記できないので、ちょっと微妙だけども)

```objective-c
[@"https?" testInString:urlString];
```

たまにしか使わないけど毎回引き出してくるのだるいーみたいな

```objective-c
[@"hoge" md5]; 
[@"hoge" stringByHashingSha256];
[@"YQ==" decodeBase64];
```

なんでだよと突っ込みたくなる`insertObjects:atIndexes`とか

```objective-c
[marray insertObjects:@[@4,@5,@6] atIndex:1];
```

[よく話題になったりはまったりするNSNull](http://stackoverflow.com/questions/2060741/does-objective-c-use-short-circuit-evaluation)をできるだけ意識せずに使えるように

```
NSDictionary *dic = @{
        @"null-key" : [NSNull null]
};
[[dic objectForKey:@"null-key"] objectForKey:@"fuck"]; // nil
```

できたりします。

### よろしくね！

[Overline](https://github.com/yaakaito/Overline)

---
layout: post
title: "CSSライクにUIKitをスタイルできるNUIがおもしろい！"
date: 2012-12-06 22:01
comments: true
categories: [Objective-C, iOS, Design] 
---

こんにちは！うきょーです！
[Objective-Cアドベントカレンダー2012](http://qiita.com/advent-calendar/2012/objective-c) 6日目の記事です。
AppCodeのことを書こうと思っていたのですが、今日 [NUI](https://github.com/tombenner/nui) というライブラリを見つけて、今僕の中でアツいので紹介しようと思います。
サンプルも書いていたのですが、アドベントにカレンダーできなさそうなので、今回は紹介だけです。

## NUIって何

NUIはCSSライクにUIKitのスタイルを指定できるライブラリです。
READMEからの引用ですが、こんな感じに定義することができます。

```objective-c
@primaryFontName: HelveticaNeue;
@secondaryFontName: HelveticaNeue-Light;
@primaryFontColor: #333333;
@primaryBackgroundColor: #E6E6E6;

Button {
    background-color: @primaryBackgroundColor;
    border-color: #A2A2A2;
    border-width: @primaryBorderWidth;
    font-color: @primaryFontColor;
    font-color-highlighted: #999999;
    font-name: @primaryFontName;
    font-size: 18;
    corner-radius: 7;
}
NavigationBar {
    background-tint-color: @primaryBackgroundColor;
    font-name: @secondaryFontName;
    font-size: 20;
    font-color: @primaryFontColor;
    text-shadow-color: #666666;
    text-shadow-offset: 1,1;
}
```

{% img /images/adv_nui.png 320 %}

([README](https://github.com/tombenner/nui/blob/master/README.md)より引用)

実際にCSSが書けるわけではなくて、あくまでUIKitに対応するプロパティをCSSライクに設定できるライブラリです。
昔CSSをそのまま使えたら幸せじゃね、と考えてみたことはあったのですが、さすがにしんどくて挫折しました。
そういう意味では NUI みたいな感じになっていても、十分良さそうに思えますね。

## 簡単な使い方

僕自身もまだちゃんと使えている訳ではないのですが、起動後にNUIを有効にしてあげて、

```objective-c
[NUIAppearance init];
```

で、NUIをセットアップします。

あとは適用したいUIクラスをNUIButtonなどのサブクラスで定義して、

```objective-c
- (void)initNUI {
    [super initNUI];
    self.nuiClass = @"Button:MyButton";
}
```

とかしてあげると、プロジェクトに含まれいる`NUIStyle.nss`を元にスタイリングしてくれるようです。
自信はそんなにないですが。

## 設定できるプロパティとか

[READMEのStyleClasses](https://github.com/tombenner/nui/blob/master/README.md#style-classes)に書いてありますが、一通りは設定できる様子。


## 楽しみですね

ログをみたらまだできたばかりのプロダクトでした。今後どうなるか楽しみですね！


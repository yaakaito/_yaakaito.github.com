---
layout: post
title: "KiwiとSpectaの比較"
date: 2012-10-22 00:48
comments: true
categories: [iOS, Objective-C, Testing, BDD, Kiwi, Specta]
---

こんにちは！うきょーです！
前まではiOSのテストには主にKiwiを使っていたのですが、最近Spectaが良い感じなので使っています。
結局のところ好みになってしまうのですが、簡単に比較というか感想を。(コードはそれぞれのREADMEみてください)
どちらもRSpecに代表されるBDDスタイルの記述ができます。

* [Kiwi](https://github.com/allending/Kiwi)
* [Specta](https://github.com/petejkim/specta)


## そもそもの違い

どちらも似た感じでテストを書くことができますが、そもそもとしてSpectaは自身がマッチャーなどは提供していません。
主にExpectaを使うことになると思いますが、他のものを使うことができます。
対してKiwiはモックから何からそろったフルセットのフレームワークです。

Spectaの方がモジュール単位に分割されているので、ライブラリとして見たときは扱い易いです。
ただ、好みのライブラリとか特にないって場合はKiwiを使った方がスムーズにいける印象です。

## 導入のしやすさ

SenTestingKitで動かすならどっちも同じくらい簡単にセットアップできます。
ただしSpectaは分割している分、importとdefineが多くなってすこし見た目が悪いです。

## アサーション

Sepcta使う最大の理由がほぼここにあって、Expectaががんばっているんですが、Objective-Cではありがちなプリミティブに対するラッパーが必要ないこと。
Kiwiの場合は`theValue`マクロが用意されていて、これを使わなきゃいけないのでちょっとだるい。

```
expect(1).to.equal(1); // Specta
[[theValue(1) should] equal:theValue(1)]; // Kiwi
```

あとはObjective-Cっぽく書くか、マクロで書くかの違いくらい。僕はKiwiっぽい記述の方が好きなんですが、どうしても`theValue`書きたくないでござる症候群が・・・。
マッチャーの豊富さはどちらも同じくらいです。beNonZeroとかが分かりやすく書けるのはちょっとKiwiの方がいいかな。
あとはBooleanがExpectaは`beTruthy` `beFalsy`ですが、Kiwiが`beTrue` `beFalse` なので気をつけましょう。

## モックとか

Kiwiは組み込みのモック、SpectaはOCMockやLRMockyが推奨されているようです。
メソッドをモックしたりとか基本的なところは一緒ですが、ちょっとずつ特徴があります。

Kiwiの組み込みとLRMockyは、メッセージエクスペクテーションとして`recieve`がちゃんと使えるのがよいところ。
OCMockはこのあたりがちょっとめんどくさくて、mockしてverifyしてね、という形式。notはない。

逆にOCMockのいいところは`andDo:block`と`andCall:selector`が非常に使いやすいところ。
Kiwiなんかは、特にBlocksが絡むとテスト用に拡張したオブジェクトに頼ったりする場面があるけど、OCMockはそれがほとんど必要ないのが良い。

個人的にはBlocksを結構使うので、OCMockが使いやすいですね。


## 選ぶ基準

そもそもObjective-CでBDDフレームワーク使ったことないならKiwi使っとくのが無難です。
元々GHUnitで書いていて、やっぱLRMocky使いたいわー＞＜とかならSpecta使うといいと思います。

ちなみに最近ホットなReactiveCocoaはSpecta使ってますね。



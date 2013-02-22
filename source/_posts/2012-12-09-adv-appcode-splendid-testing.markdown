---
layout: post
title: "AppCodeで華麗にテストをキメる！"
date: 2012-12-09 00:02
comments: true
categories: [Objective-C, AppCode, Testing, OCUnit, SenTestingKit]
---

こんにちは！うきょーです！！
[Objective-Cアドベントカレンダー2012](http://qiita.com/advent-calendar/2012/objective-c) 8日目の記事です。
みなさんiOSアプリを開発するときに当然テストを書くと思うんですが、このテストメソッドだけ試したいなーってときに、
XcodeだとManageShemesからチェックを外して・・・とか、できるんですが、非常に面倒ですよね！！！
そんなあなたにAppCodeがおすすめです！

## テストメソッド単位で実行をキメよう！

とりあえずキメてみましょう！テストフレームワークはSenTestingKitです。

普通にテストを書いていって・・・

{% img /images/adv-testing-test-case.png 420 %}

この`testMappedArrayUsingBlock`だけを実行して確かめてみたいなーと思っずたら、
キーバインドを弄っていない場合は`Command + Option + R`(多分あってるはず)なんかで、Run > Run... を呼び出します。
そうするとこんな感じのメニューが現れて・・・

{% img /images/adv-testing-select.png 420 %}

`2`を押すと、このコンテキストのみを実行してくれます。
すると・・・

{% img /images/adv-testing-result.png 420 %}

こういう感じに`testMappedArrayUsingBlock`だけ実行してくれます！便利ですね！！！

あとはガンガンテストをかいてガンガン実行してカイラクを得ましょう！！！

## ちなみにこれをXcodeで開いてみると・・・

{% img /images/adv-testing-xcode.png 320 %}

こんな感じになっています。


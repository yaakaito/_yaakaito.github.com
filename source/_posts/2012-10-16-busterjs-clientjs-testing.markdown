---
layout: post
title: "Buster.JSについて少しLTした"
date: 2012-10-16 22:24
comments: true
categories: [Testing, JavaScript, Buster.JS]
---

こんにちは！うきょーです。
突然LTすることになったので、最近気になってるBuster.JSネタで話してきました。
LTなのであんまり内容は濃くないです。iPhoneシュミレーターとかでも楽に動くし便利だね！というくらいです。

<script async class="speakerdeck-embed" data-id="507b79def901500002026b02" data-ratio="1.3333333333333333" src="//speakerdeck.com/assets/embed.js"></script>

スライドにはほとんど情報がないので、どんな感じで話したかを箇条書きで。

* yaakaito.orgが変なところに飛ばされとる (今はなおったみたい)
* みんなJSかきますよね！テストやってますか？？？
* フレームワークいろいろありますが、最近はBuster.JSがアツい
* JSTestDriverのようにブラウザ上での自動実行と、ヘッドレスでの実行がサポートされている
* "Test-Driven JavaScript Development"でも言及されていますが、実際の環境で動かせることは大事
* ちなみにこの本書いた人がBuster.JSの作者で、Buster.JSはこの本に書いてある理想を実際に行おうとしている
* けど、ヘッドレスでも動くしNodeも扱えるので、使い分けがしやすくて便利
* ヘッドレスはまだサポートされてると思った？残念、ベータちゃんでした！
* ともあれ便利なので使ってみよう
* npmで簡単インストール
* buster.jsというコンフィグを書いて、テストを書く
* サーバーを立ち上げてテスト対象になるブラウザでアクセス、対象として登録
* ChromeやFirefoxはもちろんiOSシュミレーターなんかでもばっちり！(実機もいけるよ！)
* RSpecみたいなBDDstyleの書き方もできるよ
* sinon.jsとwhen.jsが内蔵されているのでこれ一つで結構いろいろできるよ！ (ちなみにsinon.jsも同じ作者だったはず)
* クライアントサイドJSもテスト書こうね！

みたいな感じです！


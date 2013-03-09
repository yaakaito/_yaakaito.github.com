---
layout: post
title: "BrunchでJavaScriptアプリはじめる手順"
date: 2013-03-08 08:14
comments: true
categories: [JavaScript, CoffeeScript, Brunch]
---

こんにちは！うきょーです！
最近[Brunch](http://brunch.io/)というものを知ったので、とりあえずはじめてみたときのメモです。
正確には[Chaplin](http://chaplinjs.org/)を先に知って、結構良さげだし試してみよーとか思ったところが始まりなので、brunch-with-chaplinを前提にしてます。(今回はchaplinの話はしません。)

## brunchってそもそも何

gruntとか使っている人は、gruntにgiter8をくっつけて++というイメージが分かりやすいかと思います。Yeomanとかその系列のものです。
レイヤーが違うので比較してもあんまり意味はないんですが、grunt使っていた頃からすると、

* 最初からCoffeeScriptのことを考えているので、CoffeeScript使う場合は嬉しい (最近はそうでもないみたいだけど)
* プロジェクトのひな形作りやすいのは嬉しい
* ビルトインサーバーがあるので、`watch --server` みたいなの出来て嬉しい
* Mocha+phantomjsを最初から生成してくれるので、テストドリブンで始めるが楽
* ブラウザのオートリロードとかもあるよ！

という感じで、gruntみたいに自分でタスク組んで〜とやるよりは、サクッと開発を始められる感じです。
そこまで使ってないし、まだよくわからんので説明はこのくらいで。

## はじめる

brunchをインストールします。(node 0.6.10 ~)

```
$ npm install -g brunch
```

### プロジェクトのひな形を作る

Githubからテンプレートを引っ張ってきて作ります。今回はchaplinが使いたかったので、brunch-with-chaplinを引っ張ってきます。

```
$ brunch new <app-name> --skeleton https://github.com/paulmillr/brunch-with-chaplin
```

簡単ですね。`tree`してみるとこんな感じになってます。

```
$ tree -L 2 -F --dirsfirst
.
├── app/
│   ├── assets/
│   ├── controllers/
│   ├── lib/
│   ├── models/
│   ├── views/
│   ├── application.coffee
│   ├── initialize.coffee
│   ├── mediator.coffee
│   └── routes.coffee
├── generators/
│   ├── collection/
│   ├── collection-test/
│   ├── collection-view/
│   ├── controller/
│   ├── controller-test/
│   ├── generator/
│   ├── model/
│   ├── model-test/
│   ├── style/
│   ├── template/
│   ├── view/
│   └── view-test/
├── node_modules/
│   ├── chai/
│   ├── clean-css-brunch/
│   ├── coffee-script-brunch/
│   ├── css-brunch/
│   ├── handlebars-brunch/
│   ├── javascript-brunch/
│   ├── sinon/
│   ├── sinon-chai/
│   ├── stylus-brunch/
│   └── uglify-js-brunch/
├── test/
│   ├── assets/
│   ├── vendor/
│   ├── views/
│   └── test-helpers.coffee
├── vendor/
│   ├── scripts/
│   └── styles/
├── README.md
├── config.coffee
└── package.json
```

`config.coffee`にいろいろ設定が書いてあるんですが、長いので別で書きます。

とりあえずビルドしてみます。

```
$ brunch build
```

`/public`ができて、ここにもろもろ生成されたファイルが入っています。デプロイのときはここを使えばよいっぽい。

ビルトインサーバーを使ってアプリを起動してみる。

```
$ brunch watch --server
$ open http://localhost:3333/
```

{% img /images/hello-brunch.png %}

こういう感じにアプリが起動していることがわかります、やりましたね！

## テストを走らせる

サーバー起動してる状態で、`/public`の中に出来たテストランナーをブラウザで開けば普通にテストが走ります。
phantomjs使いたいときは、

```
$ brunch test
```

でよいらしいです。(使ってない)

## 他の環境でのセットアップ

brunchベースのプロジェクトにコミットするときは、
見た感じ、`npm install`すればよさそうなので試してみる

```
$ npm install
$ brunch w --server
```

出来た、これでよさげ。


使ってみてあーだこうだはもうちょっとしてから書こうと思います。
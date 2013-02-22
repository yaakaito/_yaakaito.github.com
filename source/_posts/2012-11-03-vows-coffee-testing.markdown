---
layout: post
title: "小さいCoffeeScriptのテストにはVowsが便利"
date: 2012-11-03 02:04
comments: true
categories: [CoffeeScript, Testing, Vows, Node.js]
---

こんにちは！うきょーです！
小さめのツールをCoffeeScriptで書く機会があったのですが、
テストに使った[Vows](http://vowsjs.org/)([日本語訳](http://vowsjs.jp/))というのが結構良かったです。
一言で言えば、BDDライクで非同期テストに強く、Coffeeと相性のよい、topicという独特の概念をもったフレームワークです。(長い)

## Vowsのセットアップと実行

npm経由でインストールします。

```
npm install vows
npm install -g vows
```

設定ファイルとかは特に必要なく、直接テストファイルを実行します。

```
vows test.js
```

CoffeeScriptをそのまま実行することができます。ここが重要

```
vows test.coffee --spec
```

`--spec`オプションを付けるといい感じの出力になります。

## Vowsのテストケース

簡単な例として文字列を反転する`reverse`というモジュールを考えたときのテストはこんな感じ。
特にCoffeeScriptで書くことにメリットを感じるので、サンプルは全部CoffeeScriptです。

```
vows    = require 'vows'
assert  = require 'assert'
reverse = require 'reverse'

vows
  .describe('reverse')
  .addBatch
    'example' :
      topic : ->
        reverse('abc')
      'should return cba' : (str) ->
        assert.equal str, 'cba'
```

`describe`を定義して、`addBatch`でテストのまとまりを追加します。
ポイントはさっきからちょっと出てきている`topic`で、これの実行結果がその下のテストケースへ渡ってきます。
この中は非同期でもokみたいで(今回は試していないけど)、`topic`の実行が終わったタイミングでテストが走る、という仕組みみたいです。便利ですね。
`topic`を使っていくとどうしてもモジュールを小さくせざるをえないので、きれいなコードを書くのにはよいと思います。

`topic`とかテストケースはネストすることもできて、例えばabcの他にdefgもテストしたい！とかって場合は、

```
.addBatch
  'example2' :
    topic : reverse
    'when abc' :
      topic : (f) -> f('abc')
      'should return cba' : (str) ->
        assert.equal str, 'cba'
    'when abc' :
      topic : (f) -> f('defg')
      'should return gfed' : (str) ->
        assert.equal str, 'gfed'
```

こういう感じで書くこともできます、一つ上の`topic`は次の`topic`に渡っていくので、テストするスコープを制限することができます。
ただ、ネストしすぎるとちょっと読みにくいですね。


## まとめ

個人的には、今回みたいにNodeを使って小さいコマンドラインツールとかを全部Coffeeで書くときに使うのがよいかなーという印象でした。
クライアントサイドのテストとかになってくると、長い目で見たときにBusterJSやその他Swarm系使った方がよさそうな印象。
大きめのプロジェクトになってくると、テストケースが要はでかいオブジェクトの定義なので、どうもしんどくなっていく気がする。

テストの為の中間ファイルとして.jsを吐く必要がないので、リポジトリがCoffeeScriptだけできれいに保てるのもポイント。
vowsでカバーできるサイズなら全部CoffeeScriptで書いてしまっても、それなりにモチベーションが保てる。(CoffeeScriptそんなに好きじゃない)


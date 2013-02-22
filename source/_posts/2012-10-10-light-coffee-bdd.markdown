---
layout: post
title: "CoffeeScriptでライトにBDDする"
date: 2012-10-10 00:54
comments: true
categories: [CoffeeScript, grunt, Jasmine, Testing]
---

こんにちは！うきょーです。
ちょろいCoffeeScript(単発ライブラリ程度)をBDDしながら書くとしたら、
どんな感じにするのが楽かなーと思ったのでちょっとやってみた。

選ぶ以前に、割と最初から何使うかなーってのは決めていて、
grunt+grunt-jasmine-taskの組み合わせです。

* [light-coffee-bdd](https://github.com/yaakaito/light-coffee-bdd)

## grunt-jasmine-task
grunt-jasmine-taskをnpmから引っ張ってきます。
これはgruntに標準でついてくるtest-taskと同じでphantomjsを使ってテストを実行するタスクです。
requirejs使うといい感じに書けるっぽいです。
package.jsonを適当に作って取ってきます。ついでにcoffee-taskも取ってきます。

```javascript
{
  "name": "project"
  , "version": "0.0.1"
  , "private": true
  , "dependencies": {
    "grunt-coffee" : "*"
    , "grunt-jasmine-task" : "*"
  }
}
```

## grunt.js
grunt.jsにcoffeeのコンパイルとjasmineのタスクを追加します。

```javascript
coffee : {
  app : {
    src : [
      'lib/*.coffee'
    ]
    , dest : 'build/'
  }

  , spec : {
    src : [
      'specs/*.coffee'
    ]
    , dest : 'spec_runner/spec/'
  }
  
  , runner : {
    src : [
      'spec_runner/main.coffee'
    ]
    , dest : 'spec_runner/' 
  }
}
, jasmine : {
  all : {
    src : ['spec_runner/index.html']
    , tasks : 'coffee:all'
  }
}
```

## ランナー
spec_runner/にランナーを用意します
libとかにjasmineとrequirejsを用意しておきます。
htmlはjasmineをブラウザで実行するときと一緒です。

```html
<!doctype html>
<html>
<head>
  <title>Jasmine Spec Runner</title>
  <link rel="shortcut icon" type="image/png" href="lib/jasmine-1.2.0/jasmine_favicon.png">
  <link rel="stylesheet" type="text/css" href="lib/jasmine-1.2.0/jasmine.css">
  <script type="text/javascript" data-main="main.js" src="lib/require-2.0.2.js"></script>
</head>
<body>
</body>
</html>
```

こんな感じ。
で、main.jsからspecを読み込んで実行してあげます。
main.js自体はgruntでcoffeeからコンパイルします。

```coffeescript
config =
  paths :
    'jasmine':       'lib/jasmine-1.2.0/jasmine'
    'jasmine.html':   'lib/jasmine-1.2.0/jasmine-html'
    'jasmine.helper': 'lib/jasmine-1.2.0/jasmine-helper'
  shim :
    'jasmine' :
      'exports' : 'jasmine'
    'jasmine.html':   ['jasmine']
    'jasmine.helper': ['jasmine']

require.config config

require ['jasmine', 'jasmine.html', 'jasmine.helper', 'spec/sample_spec'], (jasmine) ->
  jasmineEnv = jasmine.getEnv()
  jasmineEnv.updateInterval = 1000
  
  htmlReporter = new jasmine.HtmlReporter()
  
  jasmineEnv.addReporter htmlReporter

  jasmineEnv.specFilter = (spec) ->
    htmlReporter.specFilter spec

  jasmineEnv.execute()
```

これで準備完了です。適当なspecを書いてみましょう。

## sample_spec
gruntで一緒にコンパイルするので、specもcoffeeで書けばよかだと思います。今回は実装がないので何もdefineしてません。

```coffeescript
define [], ->
  describe 'A suite', ->
    it 'sample spec', ->
      expect(true).toBe true
```

あとはコンソールから

```
$ grunt jasmine
```

で動かします。watch-task書いて動かすのもよいと思います。

## まとめ
jasmineの部分だけ抽出してますが、実際はこれ+coffeelint+cancat+minifyみたいな構成で書いています。
gruntと使うと簡単なものなら早くセットアップできて、ブラウザもイチイチリロードしなくていいような環境になるので、楽チンですね。


---
layout: post
title: "buster-html-docとあとcoffee"
date: 2013-03-02 14:40
comments: true
categories: [JavaScript, CoffeeScript, Testing, BusterJS]
---

こんにちは！うきょーです。
[前回BusterJSのtestbedの話](http://yaakaito.github.com/blog/2013/03/01/buster-testbed-and-boot/)を書いたのですが、

{% blockquote @y_imaya https://twitter.com/y_imaya/status/307347977712848896 %}
@yaakaito HTMLを用意するまでもない場合は buster-html-doc とかも良いと思いますがどうでしょう！
{% endblockquote %}

という意見をもらったので、こっちのことも書いておこうと思いました。

## そもそもbuster-html-docって何

BusterJSはJSTestDriver形式で書かれたテストケースの実行をサポートしているのですが、JSTestDriverに[HTML Doc](http://code.google.com/p/js-test-driver/wiki/HtmlDoc)という昨日があります。
この部分だけをBusterJS用に切り出したのがbuster-html-docで、前回のようにHTMLを用意するまでもない場合に

```javascript
/*:DOC hoge = <p>aaaa</p>*/
assert.equals(this.hoge.innerHTML, 'aaaa');
```

という風にテスト毎にエレメントを生成することができます。

## 使い方

`buster-html-doc`をnpmからインストールします。

```
$ npm install buster-html-doc
```

`buster.js`でbuster-html-docを読み込むようにします。

```javascript
var config = module.exports;

config["browser test"] = {
  env : "browser",
  tests : [
    "test.js"
  ],
  extensions: [require("buster-html-doc")] // これ
}
```

こういう感じにテストを書きます。

```javascript
buster.testCase('hoge', {
	'test html doc' : function() {
		/*:DOC hoge = <p>aaaa</p>*/
		assert.equals(this.hoge.innerHTML, 'aaaa');
	}
})
```

これでテストを実行すると、テスト時に`/*:DOC hoge = <p>aaaa</p>*/`の部分が、

```javascript
this.hoge = (function () {
	var element = document.createElement("div");
	element.innerHTML = "<p>aaaa</p>";if (element.childNodes.length > 1) {
		throw new Error("HTML doc expected to only contain one root node, found " + element.childNodes.length); 
	}
	return element.firstChild; 
}());
```

という感じに変換されます。
あとはこのエレメントを使ってアサーションするなりできます。

上の例では`this.hoge`に対してエレメントを生成していますが、
そうではなく`body`とかに突っ込んでほしい場合は、`+=`を使って書く事もできます。

```javascript
/*:DOC += <p id="hoge">aaaa</p>*/
assert.equals(document.getElementById('hoge').innerHTML, 'aaaa');
```

という感じなのがbuster-html-docプラグインです。

## buster-coffee

続いてbuster-coffeeなのですが、名前の通りテスト実行時にCoffeeScriptをコンパイルしてくれるので、コードをCoffeeScriptで書けるよ、というものです。
これ自体は特にめんどくさくなくて、npmでインストールして、

```
$ npm install buster-coffee
```

```javascript
var runner = module.exports;

runner["browser test"] = {
  env : "browser",
  tests : [
    "test.coffee" // coffee
  ],
  extensions: [require("buster-coffee")]
}
```

という風に使えばよいのですが、buster-html-docと少し相性の問題があるみたいで、

```javascript
extensions: [require("buster-coffee"), require("buster-html-doc")]
```

こういう感じにして、

```coffeescript
buster.testCase 'hoge', 
	'test html doc' : ->
		###:DOC hoge = <p>aaaa</p>###
		assert.equals(this.hoge.innerHTML, 'aaaa');
```

こう書いても、

> TypeError: Cannot read property 'innerHTML' of undefined

となります。

コンパイルされるとHTML Docの部分は

```javascript
/*:DOC hoge = <p>aaaa</p>
*/
```

こうなるはずなので、一見大丈夫そうに思えるんですが、うまくいきません。
というか自分でコンパイルするとちゃんと動くので、プラグインの実行順か、それぞれの実行タイミングが悪いのかみたいな話だと思います。

ハマりやすいので気をつけましょう。

回避策としてはプラグインのところ見直してプルリクエストが一番早そうなんですが、
僕は他の理由もあって先にcoffeeを別にコンパイルするようにしてしまいました。


## おまけ

HTML Doc形式の書式が結構便利で、最近関わっているプロダクトだと

```coffeescript
###:XHR /hoge = {
	fuga : 'fuga',
	piyo : 'piyo'
} 
###
# /hoge にアクセスしたらこのレスポンスが返ってくる (XHR部分のラッパー有)
```

みたいにして通信部分をモックできるようしてみた、便利。
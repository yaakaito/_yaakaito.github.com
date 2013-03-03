---
layout: post
title: "BusterJSでテストに使うHTMLの設定と手動実行"
date: 2013-03-01 02:35
comments: true
categories: [JavaScript, Testing, BusterJS]
---

こんにちは！うきょーです！
みなさんJavaScriptのテスト書いてますか？当然書いてますよね？？？

JSでテスト書く時に、こういうHTMLを使いたいんだけど的なことってあると思います。
BusterJSはデフォルトでいい感じのHTMLを作って使ってくれるので楽にテストをはじめられるんですが、まあ差し替えたいよねーってことで差し替えます。

BusterJSではこれをtestbedと呼んでいて、設定ファイル(だいたいの場合は`buster.js`)で設定することができます。
設定の仕方は[こんな感じ](http://docs.busterjs.org/en/latest/overview/#custom-test-beds)なんだけど、いまんところ`testbed`ってプロパティは設定できないので、`resources`から設定します。

```javascript
var config = module.exports;

config["test"] = {
    sources: ["lib/*.js"],
    tests: ["test/*-test.js"],
    resources : {
    	path : "/",
    	file : "hoge.html"
	}
};
```

というわけでこんな感じに設定を追加します。こうすると`hoge.html`を使ってテストを走らせてくれます。
で、テスト用のファイルを読み込まなきゃいけないわけで(sourcesとかtestsに設定してるやつ)、それがどこに埋め込まれるのかなーというと、コードを見てみた感じ、

1. `{% raw %}{{scripts}}{% endraw %}` という文字列を探して、あったらそこを置き換える
2. `</body>` を探して、あったらその直前に置く
3. `</html>` を探して、あったらその直前に置く
4. 何も見つからなかったら、一番最後に連結する

という順番になってた。(`resource-middleware`とか読めば分かる)
なので、例えばテストの時だけ `initialize` みたいな関数を、セットアップスクリプト走らせる前に置き換えたいんだけど的なときは、(↓みたいな感じの場合)

```html
<script src="hoge.js"></script><!-- この中に initialize って関数があるとする -->
<script>
// この時点までにテストのときだけinitializeを置き換えてほしい ＞＜
initialize({
	hoge : fuga
});
</script>
```

`{% raw %}{{scripts}}{% endraw %}` を使って、

```html
<script src="hoge.js"></script>
{% raw %}{{scripts}}{% endraw %}
<script>
initialize({
	hoge : fuga
});
</script>
```

こういう感じにする。で、`bootstrap.js`みたいなのを用意してあげて、
```javascript
// bootstrap.js
initialize = function() { } // 何もしないでーーー＾ー＾
```

これを`sources`に追加する。

```javascript
var config = module.exports;

config["test"] = {
    sources: ["lib/*.js", "bootstrap.js"],
    tests: ["test/*-test.js"],
    resources : {
    	path : "/",
    	file : "hoge.html"
	}
};
```

とすると、実際にテストで使われるファイルは、

```html
<script src="hoge.js"></script>
<script src="bootstrap.js"></script><script src="テストとか"></script>
<script>
initialize({ // なにも・・・なかった・・・
	hoge : fuga
});
</script>
```

こんな感じになる。便利！

で、次にテストの実行開始タイミングをコントロールしたいと思うんだけど、割と簡単んで、[この辺](http://docs.busterjs.org/en/latest/starting-testrun-manually/#starting-testrun-manually)にも書いてあるように、

```javascript
var config = module.exports;

config["test"] = {
	autoRun: false,
    sources: ["lib/*.js"],
    tests: ["test/*-test.js"],
    resources : {
    	path : "/",
    	file : "hoge.html"
	}
};
```

`autoRun`を`false`にして、さっきの例だと、`initialize`まで呼ばれたら実行してほしい！とかなら、

```javascript
// bootstrap.js
initialize = function() { 
	buster.run();
}
```

みたいな感じ


というわけでテストに使うHTMLも設定できましたし、ガンガンテストできますね！


そういや別にもう一個なんか書こうと思ってたんですが、忘れたので知ってる人いたら教えてください。



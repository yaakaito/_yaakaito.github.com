---
layout: post
title: "JekyllでエセAPI的なの作る話"
date: 2013-03-06 01:55
comments: true
categories: [Jekyll, Ruby]
---

こんにちは！うきょーです！クライアント開発のみなさんこんばんは！
元気にクライアントアプリを開発していますか？？？

クライアントアプリとか作るときに、とりあえずAPI出来るまではモックのAPIをJSONで用意したりとかすると思うんですよね。
ただなんか、複数個データ用意しちゃったときとかに、いちいち全部書き換えるのはめんどくさいし、わざわざモックサーバー書くのも面倒だし、
[ww](http://agile.esm.co.jp/ww/)とかもあるが、別にそこまで高機能じゃなくてもいい・・・。

というわけで[Jekyll](http://jekyllrb.com/)で作ることにした。特に難しいことはしません。
`_layouts`にいつもの感じでテンプレートを書くんですが、HTMLじゃなくて代わりにJSONを書きます。

```
---
---
{
  "title" : "{{ page.title }}",
}
```

準備完了！あとはAPIにしたい的なデータを

```
---
title : each
layout : function
---
```

こういう感じに書いて、拡張子なしで保存します。とりあえず`each`って名前のをかいたので、こんな感じになります

```
├── _layouts
│   └── function.json
└── feature
    └── each
```


そしてコンパイルします

```
$ jekyll
```

こんな感じになって、

```
└── _site
    └── feature
        └── each
```

あとは

```
python -m SimpleHTTPServer
```

とかやっておけば

```
$ curl http://localhost:8000/feature/each
{
  "title" : "each",
}
```

となります、やりましたね！！！！！
コレクションっぽいのがほしかったら、ジェネレーターとかサクっと書けばよいだけなので、楽チンですね！

---
title: "CLIアプリを名刺代わりにするカモ"
excerpt: "CLIで叩いてすぐにアピールできるCLIアプリを作ってみた"
toc: true
toc_sticky: true
toc_label: "CLIアプリ"

categories:
 - エンジニア勉強
tags:
 - CLI
 - 開発
last_modified_at: 2020-06-03T09:40:00
---

# はじめに
以前、Reactを触りはじめたら作りたいと思っていたCLIアプリがありました。

<div class="block-link"><div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://qiita.com/akameco/items/e0af9e3cdf1cdb6fca61" data-iframely-url="//cdn.iframe.ly/hYNlCyl?iframe=card-small"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script></div><br>

ちょうど転職してからReactを触りはじめて1か月が経とうとしていたこともあり、重い腰を上げて作って見ようとした結果、やっと作れるようになりました。

今回はこちらの記事を参考にしてCLIで叩くと自己紹介用のアウトプットが表示されるアプリを作ってみた時の話をします。
<hr size="65px;">

# 作るきっかけ
ちょうど2年くらい前に流行っていましたが、当時はなんとなく「こういうアプリもあるんだな」くらいしか感想がなかったです。

それが最近になって、転職をしたりブログを書こうと思い始めたりする中で何かのきっかけでエンジニアに自己紹介する時の手段が紙の名刺くらいしかないことに気づきました。

それによって以下のようなアピールがなかなかその場で伝えられないこともあるんじゃないかと考えました。

- こういうアウトプットをしています
- SNSやっているんでよかったらフォローしてください
- 最近、こういうのにハマっているんで良かったら見てください

などなど、伝えたいことはあるのにその場ですぐ見てもらえなかったら残念な気持ちになるんじゃないかと思います。<br>
（※少なくとも、私はそう思います）

そこで、簡単なコマンドで自己紹介できるアプリがあったら、その場で共有できなかったとしても後でコマンドラインから色々伝えられるのではないかと思い、さっそく作ってみました。
<hr size="65px;">

# 作ったものの紹介
リポジトリは以下です。

<div class="block-link"><div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://github.com/LeeDDHH/leeddhh" data-iframely-url="//cdn.iframe.ly/GlWqsJL"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script></div><br>

CLI上で以下のような感じで動きます

<div class="block-link"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/cli-movie.gif'></div><br>

アプリのソースには自己紹介文、紹介したいリンクとその名前だけ自分用のものに設定すれば基本的には画像のように動きます

<hr size="65px;">

# 躓いた&はじめて知った
基本的には冒頭で紹介した記事にも書いてありますが、元となるリポジトリ通りに書けば問題なく動きます。

ただそれだけのはずでしたが、最初からリポジトリを生成してつくると意外と分からないことが多かったのでそれをまとめてみます。

リポジトリの構成はこんな感じで作りました。
```css
.
├── node_modules
├── package-lock.json
├── package.json
└── src
    ├── avatar.png // アイコン用画像
    ├── cli.js     // CLIコマンドの入り口
    ├── const.js   // 設定ファイル
    └── ui.js      // ターミナルで表示するUI
```

## `package.json`
`npm init` で生成し、ちょっと中身を変えたファイルが以下です。

```json
{
  "name": "leeddhh",
  "version": "1.0.0",
  "description": "leeddhh CLI",
  "main": ".src/cli.js",
  "scripts": {
    "build": "node src/cli.js",
    "dev": "npm run build"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/LeeDDHH/leeddhh.git"
  },
  "keywords": [
    "cli-app",
    "cli",
    "leeddhh"
  ],
  "author": {
    "name": "DongHeon Lee",
    "email": "niche3600@gmail.com"
  },
  "engines": {
    "node": ">=8"
  },
  "files": [
    "src"
  ],
  "bin": {
    "leeddhh": "./src/cli.js"
  },
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/leeddhh/leeddhh/issues"
  },
  "homepage": "https://github.com/leeddhh/leeddhh#readme",
  "dependencies": {
    "import-jsx": "4.0.0",
    "ink": "2.7.1",
    "ink-select-input": "3.1.2",
    "meow": "7.0.1",
    "open": "7.0.4",
    "react": "16.13.1",
    "term-img": "5.0.0",
    "terminal-image": "1.0.1"
  }
}
```
とくにこの中で分からなかったのは以下の3点でした。

### `repository`
ソースコードを管理する場所を指定指定する記述です。
ここに記述がある場合、リポジトリ内で `npm docs` をすることでソースコードのページまで飛ばしてくれます。

### `files`
プロジェクトから配布するファイルだけを指定する記述です。
`.npmignore` みたいなファイルのブラックリストを指定する役割とは真逆で、使用する上で本当に必要なファイルだけを指定します。
ファイルだけでなく、フォルダー単位で指定することも可能です。

### `bin`
日本語訳されている `npm package.json 取扱説明書` の説明を引用しますと以下のようです。
> コマンド名のマップを package.json の bin 項目に記述して利用出来ます。インストール時に npm はコマンドを呼び出すためのシンボリックリンクを グローバルインストール時 (注釈： -g オプションをつけてのインストール) は prefix/bin に、ローカルインストールでは ./node_modules/.bin/ に作成します。

> 例えば npm ではこう指定されています:

> { "bin" : { "npm" : "./cli.js" } }

> なので、npm をインストールしたとき、シンボリックリンクが cli.js に向けて /usr/local/bin/npm に作成されます。

要するにローカルでインストール場合、 `./node_modules/.bin/` 以下で指定したキーを `コマンド名` として扱い、バリューは `実行時のシンボリックリンク`の役割を果たせるように指定する記述とのことです。

## `Node.js`の書き方
元のリポジトリにあった要素を別ファイルとして管理させるため、`export` を使おうとしたら `Array is not a function` というエラーに出くわしました。

普段、Reactを書いていたくせでついつい `export default 関数名` みたいに書いていたことから調べてみたら、公式ページでは以下のように書いてありました。

> If the module is not simply a list of named exports, but rather contains a unique function or object export like module.exports = function () { ... }

今回は単純に指定した配列をそのまま使う用途であったため、 `exports` を使うことにしました。

そして書き方は `exports.プロパティ名` にすることで使用するファイルの中では `const { プロパティ名 } = require("export先のファイル")` のように使うことができます。

## npmにデプロイ
ローカルで開発を終えていざデプロイしようと `npm publish` をしたものの、デプロイができず焦りましたw

調べた結果、以下のような大まかな手順を踏むことでデプロイができるようになります。

1. [npmjs](https://www.npmjs.com/)でユーザ登録する
2. 登録した情報を元にローカルの `npm` にユーザを追加する
3. `npm publish .` でデプロイをする

ここに至るまで色々とエラーを出しながらたどり着いたこともあり、今回以降の記事でちゃんとした手順をまとめられるようにしていきたいと思いました。
<hr size="65px;">

# 最後に
ここまででCLIアプリの作るきっかけとでき上がったものからどんな学びがあったのかをまとめました。

ここで紹介したCLIアプリのテンプレートを別途リポジトリとして用意しました。

よかったら以下のリポジトリから皆さんも自己紹介用のCLIアプリを作ってみてください！

<div class="block-link"><div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://github.com/LeeDDHH/cli-app-template" data-iframely-url="//cdn.iframe.ly/3FpupEt"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script></div><br>

P.S. CLIアプリを作る上で疑問点/不明点の受け付けは [Twitter](http://twitter.com/camomile_cafe) もしくは [Mail](mailto:niche3600@gmail.com) にてお願いいたします。

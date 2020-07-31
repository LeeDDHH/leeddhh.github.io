---
title: "Rust LT Onlineの参加レポート"
excerpt: "Rustで何ができるのかを知るため、参加してみた"
header:
  og_image: /assets/images/workshop.png
toc: true
toc_sticky: true

categories:
 - エンジニア勉強
tags:
 - Rust
 - 振り返り
last_modified_at: 2020-07-31T09:08:00
---

# はじめに
PHPとJavaScriptを触ってきて3年が経ち、そろそろ他の言語も学ぼうと思っていました。<br>
最近、音声テキストチャットアプリケーションの `Discord` が実装言語を `Go` から `Rust` へ変えたニュースを見かけたり、デバイスの低レベルの制御を行うときに `Rust` の名前が上がってきたりしていたので興味を持っていました。

<div class="block-link">
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://www.atmarkit.co.jp/ait/articles/2002/10/news038.html" data-iframely-url="//cdn.iframe.ly/hizWLMe?iframe=card-small"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
</div><br>

<div class="block-link">
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://www.rust-lang.org/ja" data-iframely-url="//cdn.iframe.ly/oVR9xTz?iframe=card-small"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
</div><br>

しかし、自分は `Rust` に関する知識がほぼゼロだったこともあり、いきなり本格的な `Rust` の勉強会に参加することはハードルが高かったです。

そんな中、今週の月曜日に `Rust` に関することであれば何でも発表してもいいオンラインLTが開かれていたので知識ゼロでも得られるものはあると思い、参加してみました。

<div class="block-link">
<div class="iframely-embed"><div class="iframely-responsive" style="padding-bottom: 40.8994%; padding-top: 120px;"><a href="https://rust.connpass.com/event/181336/" data-iframely-url="//cdn.iframe.ly/W24dtBc"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
</div><br>

<hr size="65px;">

# イベント概要
当イベント、 `Rust` に関することであれば何でもLTをして良いというイベントです。

> LTテーマはRustに関連すればなんでもOKです！ (初心者縛りなどではないです。)

開催されるまで発表タイトルが公開されなかったので、どんな内容が聞けるのかを楽しみにしていました。

<hr size="65px;">

# LT
今回のLTは以下の内容を扱った会となりました。

- `Rust` トレイトの紹介
- Introduction of Plotters
- RustでAPI連携する社内ツール作ってみました

## `Rust` トレイトの紹介
<div class="block-link">
<div style="left: 0; width: 100%; height: 0; position: relative; padding-bottom: 56.25%; padding-top: 30px;"><iframe src="https://docs.google.com/presentation/d/1PStq7Zk3pq0EKM_CZLjE4TxlkOEBF3oZIYXUPqwOwMo/preview?usp=embed_googleplus" style="border: 0; top: 0; left: 0; width: 100%; height: 100%; position: absolute;" allowfullscreen scrolling="no" allow="encrypted-media"></iframe></div>
</div><br>

`Rust` には `Class` がなく、 `trait` という単位（正確には `trait object`）でポリモーフィズムを実現できます。<br>
PHPにも `trait` というキーワードがあったため、個人的には親和性のある話題のLTでした。

LTではじめて知ったこととしては以下のことがありました。

- Rust自体で提供しているスタンダードなtraitがある
  - 名前、定義、戻り値で構成されている
- traitのパフィーマンスを考慮して、エラーが発生した場合、最大1回だけ処理するものもある
- 継承（Derive）できるトレイトが存在する

`trait` に関して知らない人にもわかりやすく、その使いどころに関して説明をしていただきました。

## Introduction of Plotters
<div class="block-link">
<div style="left: 0; width: 100%; height: 0; position: relative; padding-bottom: 56.25%; padding-top: 30px;"><iframe src="https://docs.google.com/presentation/d/1-zW0JqvQBisw8-kVjh8Opeop4Qa2GRE94HaHqJjmJsI/preview?usp=embed_googleplus" style="border: 0; top: 0; left: 0; width: 100%; height: 100%; position: absolute;" allowfullscreen scrolling="no" allow="encrypted-media"></iframe></div>
</div><br>

`Rust` で使えるライブラリ（`Plotter`）について紹介をしていただきました。<br>
データを可視化するときに使えるライブラリで、出力方法にWebAssemblyも対応しているものです。

`Plotter` を使えば、シンプルなチャートを書き出すことも簡単にできるみたいで、サンプルコードをもとに実装方法を説明いただきました。

スライド3枚目のサンプルコードを以下の実装していたとのことです。

1. 出力形式の定義
2. チャートの定義
3. チャートの出力

他にも、「画像検出のライブラリと併用する時の例」や「図形の書き出し」のサンプルコードについても簡単に説明していただき、 `Rust` を活用した実装のイメージがなんとなく分かりました。

## RustでAPI連携する社内ツール作ってみました
<div class="block-link">
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://qiita.com/boiyama/items/e608b78ef77fc3818488" data-iframely-url="//cdn.iframe.ly/k4nwY7e?iframe=card-small"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
</div><br>

座学で `Rust` を学びながら、現場で活用するものを自分で組んだ方の話でした。<br>
Slackのスラッシュコマンドで勤怠の打刻をするツールを作成した経験をもとにできあがったもののアーキテクチャ図を見せながら発表していただきました。

実装時に `Rust` 以外の `openssl` や `gcc` のエラーで苦労したり、 `JWT` のトークンを扱っていたりした話からツールを作る中で周辺技術の勉強にもなった話が聞けました。

他には `Serde` という `Rust` のデータ構造を効率的に扱うためのフレームワークを使っていた話からLTが盛り上がり、人気のあるツールだと感じました。

<div class="block-link">
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://github.com/serde-rs/serde" data-iframely-url="//cdn.iframe.ly/0TtQMRP"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
</div><br>

ちなみに、ソースコードが気になる方は以下のリポジトリから確認ができます。

<div class="block-link">
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://github.com/boiyama/examples-rust-slack-kot" data-iframely-url="//cdn.iframe.ly/1V7q1uv"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
</div><br>

<hr size="65px;">

# 最後に
`Rust` に関する知識がほぼゼロな状態でLTの勉強会へ参加してみました。

事前知識がない状態でも他の言語にある概念が `Rust` にもあり、比較しながら聞くことができました。<br>
他にも `ベクター` という概念を `Rust` ではよく扱っているみたいで、勉強でつまづいたときのキーワードとして検索できるものが増えました。

また、LTの中で今回のイベントを主催したコミュニティのSlackにて、初心者チャンネルで聞きながら勉強したという話に興味を持ち、さっそく自分もコミュニティに入ってみました。<br>
気になる方は以下の `Rust` グループページからSlackに登録してみてください。

<div class="block-link">
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://rust.connpass.com/" data-iframely-url="//cdn.iframe.ly/S5TKITy?iframe=card-small"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
</div><br>

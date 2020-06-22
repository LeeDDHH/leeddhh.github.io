---
title: "Electronを試す前に使ってみると良いデモアプリの紹介"
excerpt: "Electronのちょっとした機能を試してみたいときにちょうどいいアプリを紹介"
toc: true
toc_sticky: true
toc_label: "APIデモアプリ"

categories:
 - エンジニア勉強
tags:
 - Electron
 - API
 - 開発
last_modified_at: 2020-06-22T10:00:00
---

# はじめに
転職してはじめて実務でElectronを触ると、「いろいろできるんだな」という漠然とした感想をもちました。

そして、[Electronで使えるAPI](https://www.electronjs.org/docs/api)を参考しながら開発をしていますが、文章で仕様を見てもどんな動作になるのかがピンと来なかったです。

簡単な動作でいいからせめてAPIの動きが見れたらいいなとおもっていたところ、一部のAPIに関して簡単に操作してみるElectronの公式がデモアプリがあることを見つけました。

<div class="block-link"><div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://github.com/electron/electron-api-demos" data-iframely-url="//cdn.iframe.ly/mjpfNLh"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script></div><br>


今回はアプリのインストールから、どんなAPIが試せるのかについてまとめていきます。

なお、執筆時はデモアプリのバージョンが`2.0.2`であり、Electronのバージョンは`5.0.1`であるため、現在とは異なる可能性があります。

<hr size="65px;">

# インストールと起動
インストール時に必要なNodeのバージョンは`8.9.3`であり、それより上のバージョンであれば問題ありません。

インストールから起動の手順は以下です。

```zsh
$ git clone https://github.com/electron/electron-api-demos
$ cd electron-api-demos
$ npm install
$ npm start
```

アプリを最初に起動した場合、この画面が表示されればうまく起動されていることになります。

<div class="block-big-img"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/electron-about.png'></div><br>

さっそくGet Startedをクリックすると、試せるAPIがカテゴリごとに別れたページに遷移します。

<div class="block-big-img"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/electron-demo-app.png'></div><br>

<hr size="65px;">

# デモアプリの構成と試せるAPI一覧

デモアプリの構成は以下の3点で構成されています。

- 説明（英文）
- デモがある時の「View Demo」ボタン
- サンプルコード

すべての説明にあるとは限らないですが、「View Demo」ボタンをトリガーにコードが実行され、そのUIを操作できます。

そして、画面の左側で選択できるカテゴリと試せるAPIに関しては以下になります。

- ウィンドウ管理
  - [ウィンドウの作成と管理](https://www.electronjs.org/docs/api/browser-window)
  - [ハング、クラッシュ時の処理](https://www.electronjs.org/docs/api/browser-window#event-unresponsive)
- メニュー
  - [アプリケーション/コンテクストメニュー](https://www.electronjs.org/docs/api/menu)
  - [グローバルショートカット](https://www.electronjs.org/docs/api/global-shortcut)
- ネイティブUI
  - [外部リンクとシステムのファイルマネージャーの呼び出し](https://www.electronjs.org/docs/api/shell)
  - [通知、イメージつき通知の表示](https://www.electronjs.org/docs/tutorial/notifications)
  - [システムダイアログ](https://www.electronjs.org/docs/api/dialog)
  - [トレイ（Macだとステータスバー）の表示](https://www.electronjs.org/docs/api/tray)
  - [ファイルのドラッグアンドドロップ](https://www.electronjs.org/docs/tutorial/native-file-drag-drop)
- 通信
  - [同期/非同期メッセージ](https://www.electronjs.org/docs/api/ipc-main)
  - [非表示ウィンドウとの通信](https://www.electronjs.org/docs/api/web-contents#event-did-finish-load)
- システム
  - [アプリやユーザのシステム情報の取得](https://www.electronjs.org/docs/api/app)
  - [クリップボード](https://www.electronjs.org/docs/api/clipboard)
  - [プロトコールハンドラーに夜アプリの起動](https://www.electronjs.org/docs/api/app#appsetasdefaultprotocolclientprotocol-path-args)
- メディア
  - [スクリーンショットの取得](https://www.electronjs.org/docs/api/desktop-capturer)

個人的に「こういうのもできるのか」と思った機能について2点ほど紹介しますと、「OSで表示させる形式での通知」と「トレイ（ステータスバー）の表示」でした。

<div class="block-big-img"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/electron-notification.gif'></div><br>

この通知機能を使うことで以下のケースにおいての実装がElectronだけで完結できます。
- ユーザに変更完了を知らせる
- 新しいアップデートがあることを知らせる

<div class="block-big-img"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/electron-tray.gif'></div><br>

また、トレイ表示機能を使うことで、設定項目を簡単に変えられるとか、ユーザの使用状況による切り替え機能とかが簡単に実装できます。

<hr size="65px;">

# 最後に
今回は、Electronの一部のAPIに関して簡単な操作が試せるデモアプリの紹介でした。

デモアプリを起動してはじめて知ったことですが、[Electron公式のチュートリアル](https://www.electronjs.org/docs/tutorial/quick-start)用に設定がされたクイックスタートリポジトリが存在していたみたいです。

デモアプリを操作してみて、チュートリアルからElectronを触って見たい方やデモアプリの機能を自分でカスタムいて使ってみたい方がいましたら、以下のリポジトリをクローンして学習するのも良いかも知れません。

<div class="block-link"><div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://github.com/electron/electron-quick-start" data-iframely-url="//cdn.iframe.ly/3EXPD1s"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
</div><br>

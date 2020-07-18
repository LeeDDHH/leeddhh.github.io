---
title: "Electronのプロセス間通信時の注意点と簡単な実装方法について"
excerpt: "Electron v.9.x.xでプロセス間通信時の注意点を通知機能を使って実装してみた"
header:
  og_image: /assets/images/job_sp.png
toc: true
toc_sticky: true

categories:
 - エンジニア勉強
tags:
 - Electron
 - 実装
 - 振り返り
last_modified_at: 2020-07-18T19:30:00
---

# はじめに
最近 `Electron` でOSのネイティブの通知APIを使った実装をする機会がありました。<br>
実装する中、今まで知っていたプロセス間の通信方法が非推奨の方法であり、他に推奨される方法が別途あることが分かりました。

今回はプロセス間通信の推奨する実装方法をOSネイティブの通知APIを使う例で説明します。

実装した例は以下のリポジトリから確認できます。

<div class="block-link">
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://github.com/LeeDDHH/electron-sample-code" data-iframely-url="//cdn.iframe.ly/zUNkPfa"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
</div><br>

<hr size="65px;">

# Electronの2つのプロセス
`Electron` のプロセスには大きく2つがあります。

1つ目は `メインプロセス` で、2つ目は `レンダラープロセス` です。

それぞれのプロセスがどのように動いているかについて公式ドキュメントの説明によると、 `メインプロセス` は以下です。

> Electronにおいて、package.json の main で指定されたスクリプトを実行するプロセスを メインプロセス (main process) と呼びます。
> メインプロセスで実行されるスクリプトは、ウェブページを生成することで GUI を表示できます。
> Electron アプリには常に1つのメインプロセスがありますが、これ以上はありません。

そして、 `レンダラープロセス` は以下です。

> Electron はウェブページを表示するために Chromium を使用しているため、 Chromium のマルチプロセスアーキテクチャも使用されます。
> Electronにおける各 Web ページはそれぞれのプロセスとして動作します。
> これをレンダラプロセス (renderer process) と呼びます。

1つの `メインプロセス` に複数の `レンダラープロセス` が存在し得ることが特徴です。

`Chrome` ブラウザに例えると、起動しているブラウザが1つの `メインプロセス` で、表示されているそれぞれのタブが `レンダラープロセス` になります。

<div class="block-big-img"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/mainandrenderer.png'></div><br>

[Electron - アプリケーションアーキテクチャ](https://www.electronjs.org/docs/tutorial/application-architecture){:target="_blank"}

また、それぞれのプロセスでは `node.js` のAPIへフルアクセスできます。<br>
つまり、 `レンダラープロセス` でも `require` を使うことでモジュールの呼び出しができてしまいます。<br>
これはリモートコンテンツをロードしようとする場合、セキュリティに気をつけないといけないことを意味します。<br>
今回の記事でセキュリティに関することの中でも、プロセス間通信時に使う `ipc通信` の利用で気をつけることを下記にまとめていきます。

<hr size="65px;">

# 実装時の注意点
## プロセスをまたいだ呼び出しをしない
`レンダラープロセス` には `remote` という `メインプロセス` のモジュールを `レンダラープロセス` から使えるAPIがあります。

`レンダラープロセス` からプロセス間で通信するためのイベントを設定しなくても `メインプロセス` による処理（OSを参照したり、ファイルを書き込んだりするような）が使える点では便利です。

<div class="block-big-img"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/remoteapi.png'></div><br>

[Electron - remote](https://www.electronjs.org/docs/api/remote){:target="_blank"}


ただし、これを使った実装にすると**オブジェクトにアクセスする速さ**と**セキュリティ上の理由**でこのAPIを使うことを非推奨する呼びかけもあります。

<div class="block-link">
<div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://medium.com/@nornagon/electrons-remote-module-considered-harmful-70d69500f31" data-iframely-url="//cdn.iframe.ly/XqHG0wO?iframe=card-small"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script>
</div><br>

とくに、リモートコンテンツによる悪意のある行為があった場合、XSS対策がないとそのままスクリプトが実行される可能性もあります。

<div class="block-big-img"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/badassaccess.png'></div><br>

このようなことが起きないようにするための方法として、 `メインプロセス` から `レンダラープロセス` を生成する際に `nodeIntegration` を無効化するオプションがあります。

```javascript
const mainWindow = new BrowserWindow({
  ︙
  他のオプション
  ︙
  webPreferences: {
    nodeIntegration: false, // このオプションを使う
  },
  ︙
});
```

[Electron - context-isolation](https://www.electronjs.org/docs/tutorial/context-isolation){:target="_blank"}

しかし、このオプションを使うと `レンダラープロセス` から `require` でモジュールを呼び出すことができなくなります。

```javascript
Uncaught ReferenceError: require is not defined
```

## プロセス間でコンテキストを共有する
では、XSS対策をしつつプロセス間通信を有効にするためにはどうすればいいかというと、以下の設定を加えることで可能になります。<br>

- `レンダラープロセス` を生成する時、 `メインプロセス` と通信できるAPIを限定的に公開するオプションをつける
- `レンダラープロセス` で使うAPIを `preload` オプションに追加する

```javascript
const mainWindow = new BrowserWindow({
  ︙
  webPreferences: {
    nodeIntegration: false,
    contextIsolation: true, // 限定的なAPI公開のフラグ
    preload: (限定的に公開するAPIモジュールのパス), // レンダラープロセスに公開するAPIのファイル
  },
  ︙
});
```

このように設定することでプロセス間の通信は以下のような形で実現できます。

<div class="block-big-img"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/preload.png'></div><br>

<hr size="65px;">

# 実装の説明
簡単ですが、今回実装したサンプルの構造は以下になります。

```javascript
.
├── index.html // レンダラープロセスで表示するファイル
├── main.js // メインプロセスファイル
├── preload.js // レンダラープロセスに限定的に公開するAPIのファイル
└── renderer.js // レンダラープロセスファイル
```

また、コードは以下の順で説明します。

- `メインプロセス` で `レンダラープロセス` を生成する書き方
- 限定公開用のAPIモジュールの書き方
- `レンダラープロセス` でイベントコールする書き方

## `メインプロセス` で `レンダラープロセス` を生成する書き方
**main.js**
```javascript
︙
const mainWindow = new BrowserWindow({
  ︙
  webPreferences: {
    nodeIntegration: false, // XSS対策としてnodeモジュールを使わないように設定
    contextIsolation: true, // 限定的にAPIを公開する設定
    preload: path.resolve("./preload.js"), // レンダラープロセスに公開するAPIのファイル
  },
  ︙
});
︙
```

上記で説明を書いた通り、 `レンダラープロセス` を生成する時のフラグと限定公開するAPIのモジュールを設定します。

```javascript
︙
// レンダラープロセスに公開するAPI
ipcMain.on("require-send-notice", (e) => {
  // OSネイティブの通知機能インスタンスを生成
  const notification = new Notification({
    title: "基本的な通知",
    body: "簡単なメッセージ",
    silent: false,
  });

  // 通知表示
  notification.show();

  // 一定時間が経つと自動で消えるように設定
  setTimeout(
    (notification) => {
      notification.close();
    },
    5000,
    notification
  );
});

ipcMain.handle("is-notification-supported", (event) => {
  // OSがネイティブの通知機能をサポートしているかどうかを確認する
  return Notification.isSupported();
});
︙
```

今回、OSネイティブの通知機能を使うためのAPIを定義します。<br>
通知機能に関する仕様は以下のドキュメントから確認できます。

[Electron - 通知](https://www.electronjs.org/docs/api/notification){:target="_blank"}

また、 `レンダラープロセス` と通信する際に `ipcMain` というAPIを使って同期/非同期的に扱います。<br>
扱い方に関しては以下のドキュメントから確認ができます。

[Electron - ipcMain](https://www.electronjs.org/docs/api/ipc-main){:target="_blank"}

## 限定公開用のAPIモジュールの書き方
**preload.js**
```javascript
const { contextBridge, ipcRenderer } = require("electron");

contextBridge.exposeInMainWorld("electron", {
  isSupportedNotice: () => ipcRenderer.invoke("is-notification-supported").then(result => result).catch(err => console.log(err)),
  noSupportedNotice: () => false,
  notice: () => ipcRenderer.send("require-send-notice"),
});
```

`contextBridge` を使って、 `メインプロセス` と `レンダラープロセス` 間で限定的に扱うAPIを定義します。<br>

`exposeInMainWorld` で第1引数に `レンダラープロセス` で扱うJavaScriptの `window` から参照するキーを指定します。<br>
第2引数には、 `レンダラープロセス` で呼び出し時に扱うAPIのオブジェクトを設定します。

`contextBridge` と扱えるAPIオブジェクトの仕様に関しては以下のドキュメントから確認できます。

[Electron - contextBridge](https://www.electronjs.org/docs/api/context-bridge){:target="_blank"}

また、 `メインプロセス` と通信する際に `ipcRenderer` というAPIを使って同期/非同期的に扱います。<br>
扱い方に関しては以下のドキュメントから確認ができます。

[Electron - ipcRenderer](https://www.electronjs.org/docs/api/ipc-renderer){:target="_blank"}

## `レンダラープロセス` でイベントコールする書き方
**renderer.js**

```javascript
const notification = document.getElementById("notice");
︙
notification.addEventListener("click", async () => {
  const result = await window.electron.isSupportedNotice();
  console.log("Notification: ", result);
  if (result) {
    window.electron.notice();
    return;
  }
});
```

`preload.js` で定義したAPIをイベント発火させたい要素に登録することで画面から呼び出すことができます。<br>
呼び出し方は、 `window.レンダラープロセスで扱うAPIキー、オブジェクトの呼び出しに使うキー()` で使えます。

このように設定することで、 `Electron` のアプリでは以下のように使えます。

<div class="block-big-img"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/sample-app.gif'></div><br>

<hr size="65px;">

## 最後に
今回たまたま実務で触って見たことがきっかけでして、 `レンダラープロセス` 生成時の注意点について説明しました。

あくまでもセキュリティ面で気をつけることの1つとしてXSS対策を兼ねたイベントの定義/登録方法について触れました。<br>
他にも `Electron` のセキュリティに関して興味がある方は以下の公式ドキュメントを参考にすることをオススメします。

[Electron - セキュリティ、ネイティブ機能、あなたの責任](https://www.electronjs.org/docs/tutorial/security){:target="_blank"}

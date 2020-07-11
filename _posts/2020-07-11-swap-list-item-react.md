---
title: "React内でリストの要素を前後の要素と入れ替えてみる"
excerpt: "配列要素の前後交換をReact内でやってみました。"
header:
  og_image: /assets/images/jouhou_koukan.png
toc: true
toc_sticky: true

categories:
 - エンジニア勉強
tags:
 - React
 - JavaScript
 - 実装
 - 振り返り
last_modified_at: 2020-07-11T22:40:00
---

# はじめに
<p>
  Reactで配列の要素を入れ替える実装方法を検索しても<strong>ドラッグアンドドロップライブラリを使う</strong>実装方法や<strong>配列の <code>sort</code> メソッドを使う</strong>実装方法しかなく、すぐには見つかりませんでした。
</p>
<p>
  そこで今回は、実装時のファイル構造をもとに、どのように振る舞いを実装したのかを紹介します。<br>
  また、実際の実装はJavaScriptの記法だけで実装ができたので難しく考える必要はありません。
</p>

<p>
  実装した例は以下のリポジトリから確認できます。
</p>

<div class="block-link"><div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://github.com/LeeDDHH/react-list-swap-item" data-iframely-url="//cdn.iframe.ly/WDPwQxG"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script></div><br>


# サンプルの説明
<p>
  やりたかった実装としては、<strong>テーブルで表示した行をユーザが選択し、その行の前後順番を操作する</strong>という動作を実現したかったです。<br>
  実際にどんな動作をするのかについては以下のイメージを参考にしてください。
</p>

<div class="block-big-img"><img src='{{ site.url }}{{ site.baseurl }}/assets/images/swapItem.gif'></div><br>

また、以下の説明はリポジトリのコードを見ながら読んでいただけますと理解しやすいです。

## 構造
<p>今回の実装でのファイルの構成と役割は以下になります。</p>
```javascript
.
├── index.jsx // jsxで書いた内容をhtmlにレンダリングする
├── App.jsx // stateの管理、振る舞いの定義を集めたコンポーネント（親）
├── Memes.js // 表示するためのデータ
└── parts
    ├── List.jsx // データをテーブル形式に表示する（子）
    └── ItemControl.jsx // データの順番を入れ替えるイベントを発火する（子）
```

<p>
  基本的には親コンポーネントの <code>App.jsx</code> でデータ/stateの管理、振る舞いの定義をします。<br>
  子コンポーネントでは親コンポーネントから渡されたデータを選択された状態も含めて表示したり、イベント発火をする入り口として扱います。<br>
  データはレスポンスで受けた後の想定であり、 <code>state</code> で管理するデータの形は以下になります。
</p>

```javascript
this.state = {
  memes: {受け取った配列のデータ},
  selectedId: null,
};
```

## 振る舞い
### リストから要素を選択
今回の実装では、配列の中の各オブジェクトには要素を特定するためのユニークな `id` を指定しています。<br>

```javascript
// 例
  {
    id: 1,
    name: "ぴえん",
    category: "流行り語",
  }
```

テーブルから行を選択すると、親コンポーネントで定義した振る舞いまでイベントが伝播します。<br>
定義した振る舞いでは、**現在選択している行**を参照して行の `id` を取得します。

```javascript
︙
handleCheckedItem = (e) => {
  // eはブラウザのネイティブイベントを操作するSyntheticEventオブジェクトのインスタンス
  this.setState({
    // 選択した行のidを取得し、数値型へ変換してstateを更新する
    selectedId: Number(e.currentTarget.children[0].children[0].id)
  });
}
︙
```

これで、選択した行の `id` を親コンポーネントで管理できます。

ちなみに、親コンポーネントへ伝播したときに扱う `SyntheticEvent` オブジェクトの**ネイティブイベント属性**や**伝播できるイベントの種類**に関しては以下のページから確認ができます。

<div class="block-link"><div class="iframely-embed"><div class="iframely-responsive" style="height: 140px; padding-bottom: 0;"><a href="https://ja.reactjs.org/docs/events.html" data-iframely-url="//cdn.iframe.ly/UPZblAG?iframe=card-small"></a></div></div><script async src="//cdn.iframe.ly/embed.js" charset="utf-8"></script></div><br>

### 配列の要素の入れ替え
行を選択した上で、「上へ」もしくは「下へ」をクリックすると親コンポーネントで定義した振る舞いまでイベントが伝播します。<br>
定義した振る舞いでは、**クリックしたイベントのタイプ**をもとに、 `state` で管理するデータの**配列の要素を入れ替え**をします。<br>

```javascript
// ItemControlコンポーネント
// data-typeで配列の順番を替えるタイプを設定
︙
<input type="button" value="上へ" data-type="up" onClick={props.moveItem} />
<input type="button" value="下へ" data-type="down" onClick={props.moveItem} />
︙

// Appコンポーネント
︙
handleMoveItem = (e) => {
  if (!this.state.selectedId) return;

  // クリックしたボタンのタイプをタイプを取得する
  const type = e.currentTarget.dataset.type;
  // stateで管理している配列から新しい配列をコピーする
  const newList = this.state.memes.slice();

  // 選択した行のidをもとに、順番の入れ替えをするオブジェクトの配列の中でのインデックスを取得する
  const targetIndex = newList.findIndex(item => item.id === this.state.selectedId);

  switch (type) {
    case "up":
      if (this._noMoreGoUp(targetIndex, newList)) return;
      // 前のインデックスのオブジェクトと順番を入れ替える
      newList.splice(targetIndex - 1, 2, newList[targetIndex], newList[targetIndex - 1]);
      break;

    case "down":
      if (this._noMoreGoDown(targetIndex, newList)) return;
      // 後のインデックスのオブジェクトと順番を入れ替える
      newList.splice(targetIndex, 2, newList[targetIndex + 1], newList[targetIndex]);
      break;

    default:
      break;
  }

  // 順番を入れ替えた配列をstateに更新する
  this.setState({
    memes: newList
  });
}
︙
```
<br>

ここでのポイントは `splice` メソッドを使って順番を入れ替えていることです。

```javascript
配列.splice(入れ替える要素のインデックス, 第1引数から取り除く要素の数, 取り除いた要素の位置から追加する順の要素)

//case "up"
配列.splice(入れ替える要素の1個前のインデックス, 2(1個前の要素と入れ替える要素分), 入れ替える要素、 1個前の要素)

//case "down"
配列.splice(入れ替える要素, 2(入れ替える要素と1個後の要素分), 1個前の要素, 入れ替える要素)
```

このように `splice` メソッドを使って要素を置き換えるときは以下の引数の順番で指定すれば、前後の入れ替えができます。

## 最後に
Reactを使って配列の前後要素を入れ替える実装方法を探しましたが、案外実際にコードで書いて見るとJavaScriptの配列のメソッドだけで実装ができました。<br>

筆者のようにReactで配列の前後要素を入れ替える必要な方にこの記事が参考になりましたら幸いです。

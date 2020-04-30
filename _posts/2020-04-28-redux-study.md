---
title: "Redux勉強の記録"
excerpt: "個人でRedux勉強したときに理解したことを書き足すポスト"
toc: true
toc_sticky: true
toc_label: "Redux"

categories:
 - エンジニア勉強
tags:
  - redux
  - 基礎
  - 初心者
last_modified_at: 2020-04-30T15:51:00
---

# Redux

- `state`管理のライブラリ

- 複雑なアプリケーションを`React`単体より簡単に作れる

## Reduxサイクル

|Redux|保険会社に例える|
|-----|-------------|
|Action Creator|保険の申請を出す|
|Action|フォーム|
|dispatch|受付人|
|Reducer|担当部署|
|State|保存された部署のデータ|

- 保険会社はお客様からの保険の申請から始まる

  - `Redux`は`Action Creator`からすべてが始まる

  - JavaScriptのオブジェクトを作るか返す役割を持つ関数

- 保険の申請には保険会社にどんなデータを持ってどう変えてほしいのかが書かれている

  - データをどのように更新したいのか、そしてそのデータは何なのかが`Action`に記載されている

  - `type`はデータをどう変えたいかについて意図を表す

    - アクションの種類を一意に識別できる文字列またはシンボル

  - `payload`は加えたい変更に関するコンテキスト

    - `Action`の実行に必要な任意のデータ

- 保険会社の受付人が申請のフォームをコピーして担当の部署に送る

  - `dispatch`はActionの中のオブジェクトをコピーして`Reducer`に送る

- 担当部署では、それぞれの部署に合った保険会社のデータを扱う

  - `Reducer`はActionから送られた`type`と`payload`をもとにデータを更新し、そのデータ群が集まるところに返す

- 部署で扱ったデータは中央集約管理される場所に集まって管理される

  - `State`はReducerが作り出したデータをReduxで管理しやすいように中央集約しておいたデータ群

  - アプリケーション内で扱うデータをすべて管理するため、Reduxが使える形にしたデータの集まり

  - これにより、各々の`Reducer`で現在の`state`を確認する必要がなくなった

## Action CreatorとAction
```javascript
// People dropping off a form (Action Creator)
const createPolicy = (name, amount) => {
  // Action Creatorは生のJavaScriptオブジェクトを返す
  // 生のオブジェクトにtypeとpayloadという中身を入れてActionという参照を使う
  return { // Action (a form in our analogy)
    type: 'CREATE_POLICY',
    payload: {
      name: name,
      amount: amount
    }
  };
};
```

## dispatch
dispatchはRedux自体に含まれているメソッド

## Reducers
- `Action Creator`で作られた`Action`から`Reducer`が呼ばれる

- 何があっても`Reducer`内で既存の`State`を新しい状態に更新しない

  - `Reducer`内から返すのはいつも新しいオブジェクトにする

  - `Reducer`の役割に沿った`Action`の`type`があったら、既存の`State`に`Action`の`payload`を加えた新しいオブジェクトを返す
    ```javascript
    return [...oldListOfClaims, action.payload];
    ```
    [スプレッド構文 - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

    [JSのスプレッド構文を理解する - Qiita](https://qiita.com/akisx/items/682a4283c13fe336c547)

  - `push`を使って既存の`State`に`Action`の`payload`を追加して返すような書き方は厳禁

  - 既存の`State`から特定の`payload`を取り除く場合も新しいオブジェクトとして　生成して返す
    ```javascript
    return oldListOfClaims.filter(claimName => claimName !== action.payload.claimName);
    ```
    [Array.prototype.filter() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

- `combineReducers`で指定するキーは`state`で扱うための名称になる

## Store

- `Redux`を使って`State`を操作するときに、`createStore`と`combineReducers`を使う

- `combineReducers`では複数のファイルでそれぞれ指定した`Reducer`の設定を1つにするためのメソッド

  - 複数の`Reducer`において指定された`Action.type`と`Action Creator`から生成されたオブジェクト内の`type`を比較して一致する条件だけに処理が走るようになる

- `createStore`は`combineReducers`で1つに束ねた`Reducer`をもとに1つのオブジェクトとして`State`管理を管理する

## State

- `Store`された`state`に直接アクセスしてデータを変更することはできない

- `Action Creator`から生成されたオブジェクト（`Action`）を通してはじめてデータの変更ができる

# Redux + JavaScript
[A Pen by vue+typescript勉強中カモ](https://codepen.io/camomile_cafe/pen/zYvNZNE?editors=0010)

# React-Redux

## 構造
```markdown
- src
|- actions    // Action Creatorに関するファイル
|- components // Component
|- reducers   // Reducer
index.js      // ReactとReduxの両方を準備する。Componentで開始時に表示するファイルを指定する
```

## 初期化時の呼び出し順
```markdown
index.js
reducers
↓
Provider
↓
App
↓
Component
connect
↓
mapStateToProps(Storeのstateを初期化時に適用する)
↓
mapDispatchToProps
↓
App
Component
↓
render
```

[Reduxの処理の流れ - Qiita](https://qiita.com/takahiron/items/9c2156a8efbd2ea98246)

## ファイル内での定義
- `index.js`

  - `Store`を生成して使うため、`redux`から`createStore`を使う

  - `Store`の`state`を`react`のDOMの中でも使えるように`Provider`を使う

  - `state`をどのように変化させるかの定義をした`reducer`を初期化するため、インポートする
  ```javascript
  import React from 'react'
  import ReactDOM from 'react-dom'

  import { createStore } from "redux";
  import { Provider } from "react-redux";

  import reducer from "./reducers";

  import App from './components/App'

  // Storeを生成し、Reducerと紐づくことでstateの更新ができるようになる
  // Storeのstateはstore.dispatch({action:content})といった形で更新できる
  const store = createStore(reducer);

  ReactDOM.render(
    <Provider store={store}>// Reactのルートからstoreを管理する
      <App />
    </Provider>,
    document.getElementById("root")
  );
  ```

- `reducers/index.js`

  - 複数の`reducer`の定義をまとめる`combineReducers`を`redux`からインポートする

    - `combineReducers`では、`state`内で扱うためのキー名とそのキーの値となるオブジェクトを返す`reducer`を指定する
    ```javascript
    export default combineReducers({
      キー1: reducer1
      キー2: reducer2
    });
    ```

  - ルートComponentで`Store`生成時に`Reducer`を初期化する


- `actions/index.js`

  - `state`に変化を与えるための関数`Action Creator`

    - 名前付きエクスポート（`named export`）で関数名を決める

  - 変化させるタイプやその値の規格を定義したオブジェクト`Action`

    - `type`と`payload`のセットにする

- `components/App.js`

  - 制御する`Component`をインポートする

- `redux`で`state`を使うComponent内

  - 基本的に`export default connect(mapStateToProps,{アクション名})(コンポーネント名)`という形式で扱う

  - `state`を受け取る場合
    ```javascript
    // 以下の形式でStateからComponentのpropsへ代える
    const mapStateToProps = (state) => {
      return { Component内で扱うpropsのキー: state.State内で扱っていたキー }
    }
    ```

  - `Action`をComponent内で扱う場合
    ```javascript
    import アクション名 from 'アクションが定義されたファイルのパス'
    ︙
    export default connect(mapStateToProps,{アクション名})(コンポーネント名)
    ```

  - この書き方で、`action`関数は`Store`に向けての処理を`dispatch`することができるようになる

  - `state`管理されているかどうかは`render`で`console.log(this.props)`

※明確に言語化できていない部分
- Component内で`Action Creator`がトリガーされた時、どうやって`Reducer`が反応する/できるのか

- おそらく、`Action Creator`がトリガーされ、`Action`をリターンすると`combineReducers`に登録された`キー：reducer`の順で処理が走るだろうと予想する

## `connect()()`の呼び出しの謎
```javascript
function connect(a=1,b=2) {
  console.log(a);
  this.arg1 = a;
  this.arg2 = b;

  //connect()で無名関数をリターンする
  return function(component='a') {
    console.log(arg1,arg2,component);
  }
}

//connect()()でリターンされた無名関数を実行する
connect(3,4)('b');
```

[connect()()の動きをconsoleで見る](https://codepen.io/camomile_cafe/pen/wvKdJzR?editors=0010)

[react-reduxのconnect()を図解する - Javaエンジニア、React+Redux+Firebaseでアプリを作る](http://yucatio.hatenablog.com/entry/2018/09/21/225716)

- どうやら`Currying`というES6の書き方があるらしい

# Redux-Thunk

## 役割

- `Action Creator`からAPIのリクエストを送り、レスポンスを受け取る
- レスポンスの結果を`Action`の`payload`に格納する

## 書き方

- `index.js`

  - `Store`を生成して使うため、`redux`から`createStore`を使う

  - `Store`の`state`を`react`のDOMの中でも使えるように`Provider`を使う

  - `state`をどのように変化させるかの定義をした`reducer`を初期化するため、インポートする

  - `reducer`に`Action`を送る前に非同期処理をするミドルウェアをミドルウェアを追加しておくために`applyMiddleware`を使う

  - `Action Creator`から`Action`を関数として返しても遅延して実行するよう、`redux-thunk`を使う
    ```javascript
    import React from 'react'
    import ReactDOM from 'react-dom'

    import { createStore, applyMiddleware } from "redux";
    import { Provider } from "react-redux";
    import thunk from 'redux-thunk';

    import reducer from "./reducers";

    import App from './components/App'

    // Storeを生成し、Reducerと紐づくことでstateの更新ができるようになる
    // Storeのstateはstore.dispatch({action:content})といった形で更新できる
    // Storeを生成する際に、thunkミドルウェアを使うようにする
    const store = createStore(reducers, applyMiddleware(thunk));

    ReactDOM.render(
      <Provider store={store}>// Reactのルートからstoreを管理する
        <App />
      </Provider>,
      document.getElementById("root")
    );
    ```

- `Action Creatorを記載したファイル`

  - `Action Creator`の処理を手動的に`dispatch`することで非同期処理が使える

  - `redux-thunk`の`createThunkMiddleware`に`dispatch`する

    - `Action Creator`の返り値が生のオブジェクトなら通常通り、`reducer`に処理を渡す

    - `Action Creator`の返り値が関数ならその関数を実行する

      - 関数実行後、非同期処理が走り、`dispatch`で生のオブジェクトを返すようにすると通常通り、`reducer`に処理を渡す
        ```javascript
        export const asyncAction = () => {
          return async (dispatch) => {
            // 非同期処理をしてactionのpayloadを生成
            const response = await someAsyncFunc();

            // 手動でdispatchし、redux-thunkへactionを送る
            dispatch({
              type: "ASYNC_ACTION",
              payload: response
            });
          }
        }
        ```
        [redux-thunk/index.js at master · reduxjs/redux-thunk](https://github.com/reduxjs/redux-thunk/blob/master/src/index.js)

        [Thunks in Redux: The Basics - Fullstack Academy - Medium](https://medium.com/fullstack-academy/thunks-in-redux-the-basics-85e538a3fe60)

        [도대체 무슨 '척'?](https://daveceddia.com/what-is-a-thunk/)

        [redux-thunk](https://velog.io/@bigbrothershin/redux-thunk)

        [Thunk - Wikipedia](https://en.wikipedia.org/wiki/Thunk)

        [리덕스(Redux)란 무엇인가? | 텅 빈 충만의 블로그](https://voidsatisfaction.github.io/2017/02/24/what-is-redux/)

        [리덕스 미들웨어, 그리고 비동기 작업 (외부데이터 연동) | VELOPERT.LOG](https://velopert.com/3401)

## Reduxによるデータロード順

```markdown
Componentが画面にレンダーされる
↓
Componentの`componentDidMount`メソッドが呼ばれる
↓
`componentDidMount`から`Action Creator`を呼ぶことで必要なデータをフェッチする
↓
`Action Creator`がAPIリクエストを送る
↓
レスポンスを受け取り、`Action`の`payload`に格納する
↓
`reducer`が`Action`の`type`を見て`state`を返す
↓
Componentの`mapStateToProps`を介して`render`関数で扱える状態になる
```

## 注意点

- `Action Creator`内で非同期処理のレスポンスを直接受け取って使うと以下のようなエラーが発生する
  ```
  Error: Actions must be plain objects. Use custom middleware for async actions.
  ```

- エラー発生の原因：`async/await`を使うことで想定してイなかったオブジェクトが返って来ている

  - `await`部分で呼び出した関数が生のJavaScriptのオブジェクトを返すと思うかも知れないが、実は`リクエストオブジェクト`を返している

  - 試しに`async/await`の処理を[babeljs](https://babeljs.io/)の`Try it out`でコピペして見ると想定してなかったオブジェクトが`return`されていることがわかる

    - 注意点として、`PRESETS`の設定項目はすべて`ON`にした状態で試す

  - なので、`async/await`を使っても生のJavaScriptオブジェクトは帰って来ず、`Action`で返すべきオブジェクトのタイプが合わない

- 非同期処理を使わずに同期的にデータをレスポンスしようとすると、`Action Creator`がリクエスト投げている間、`Action`は先に`dispatch`で`Reducer`までたどり着いてしまう

  - そうすると、Componentで表示するデータがないまま、画面が表示される

- そこで、`redux`内でもリクエスト/レスポンスの受送信ができるように手伝うミドルウェアが必要になり、`redux-thunk`がその役割を担う

## ReduxにおいてのMiddleware

- `dispatch`するすべての`Action`で呼び出される

  - `Action > dispatch > Middleware > Reducers`

- `Action`を止めたり、変えたりする方法で操作が可能になる

- 非同期的な`Action`を使うのが一般的な方法

## `Action Creator`のルールの違い

- `Redux`自体の場合

  - `Action Creator`は必ず`Action`オブジェクトを返す

  - `Action`は必ず`type`を持つ

    - 場合によっては`payload`もつける

- `redux-thunk`の場合

  - `Action Creator`は`Action`オブジェクトもしくは関数を返す

    - 関数を返す場合、`関数の呼び出し`もやってくれる

  - `Action`オブジェクトが返ってきたら必ず`type`を持つべき

    - 場合によっては`payload`もつけられる

## `redux-thunk`の詳細な動き

```markdown
＜Action Creator＞
↓
オブジェクトもしくは関数の「何か」
↓
※`dispatch`
↓
＜redux-thunk内＞
↓
「何か」
↓
関数かどうか？　→　＜関数の場合＞　→　`getState`と`dispatch`を使って関数の呼び出しをする　→　リクエストが終わるまで待つ　→　リクエストが終わったら、手動で`Action`を※`dispatch`する
↓
＜関数ではない場合＞
↓
Reducers
```

# reducerのルール

## 返り値
- 「undefined」以外の値を返す必要がある

## `state`の取り扱い
- 「`state`」、または以前の`state`とアクションのみを使用してアプリ内で使用されるデータを生成する

  - 初期化時の`reducer`は`state`が何も決まっていない状態であるが、`reducer`に渡すときの引数を`state = null`のようにデフォルトバリューで`undefined`以外に指定することでエラー発生せず、初期化された`state`が生成できる

  - 初期以降は生成された`state`と`action`をもとに`reducer`を通して新たな`state`を生成する

## Reducer内で扱える値について
- どの値を返すかを決定するために、「Reducerが受け付けた値」以外で返してはなりません。

  - `state`と`action`で受け付けたもの以外にAPIリクエストを投げたり、ファイルの読み込みをしたり、DOMを直接介したりして返す値を決めてはいけない

  - `state`と`action`のみで新しい`state`を返すべき

## ミスリーディングに注意する文脈
- 入力の「`state`」引数を変更してはいけない

  - 入力の「`state`」とは、引数で渡ってきた`state`のこと

  - `state`の形式が配列やオブジェクトの場合、いかにも容易く変えられてしまう
    ```javascript
    //配列
    const colors = ['red','green'];
    colors.push('purple') // ['red','green','purple']
    colors.pop() // ['red','green']
    colors[0] = 'PINK' // ['PINK','green']

    //オブジェクト
    const profile = { name: 'Alex' }
    profile.name = 'Sam' // { name: 'Sam' }
    profile.age = 30 // {name: "Sam", age: 30}

    //文字列
    const name = 'Sam'
    name[0] // 'S'
    name[0] = 'X' // name == "Sam"
    ```

  - 配列やオブジェクトの場合、定義した値（A）と同じ値を別で定義した値（B）とで厳格な比較をしても同一性はない。
  なぜかと言うと配列やオブジェクトは`参照として`比較される
    ```javascript
    //オブジェクト同士の厳格な比較
    const obj1 = {foo:1};
    const obj2 = {foo:1};

    obj1 === obj2 // false メモリ上のレファレンスが異なるため

    const obj3 = obj2;

    obj2 === obj3 // true メモリ上のレファレンスが同じ

    //配列同士の厳格な比較
    const arr1 = [1,2,3];
    const arr2 = [1,2,3];

    arr1 === arr2 // false メモリ上のレファレンスが異なるため

    const arr3 = arr2;

    arr2 === arr3 // true メモリ上のレファレンスが同じ

    //厳密に言うと、配列もオブジェクトの一種
    const arr = [1,2,3];
    const obj = { var: "foo" };

    typeof arr //"object"
    typeof obj //"object"
    ```
    [Array - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array)

  - [reduxのcombineReducerソースコード](https://github.com/reduxjs/redux/blob/master/src/combineReducers.ts)を見る限り、`reducer`内に引数で「渡ってきた`state`」と「現在の`state`」を比較して`state`の更新の有無を判定している。
  つまり、`reducer`内で`state`を直接変えてしまうと`state`に更新がかかるため、思わぬ動作を引き起こす可能性がある

# reducerで安全にstateを更新する
- 基本的に新しいオブジェクトや配列を返すことで安全に更新できる

```javascript
//オブジェクト
//変更
{...state, 変更するキー:'変更する値'}
//追加
{...state, 追加するキー:'追加する値'}
//削除
{...state, 削除するキー: undefined}
もしくは
_.omit(state,'削除するキー') // Lodash Documentを参考

//配列
//変更
state.map(elem => elem === 変更対象の値 ? 変更後の値 : elem)
//追加
[...state, 追加する値]
//削除
state.filter(elem => elem !== 削除する値)
```

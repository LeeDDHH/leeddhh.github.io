---
title: "React勉強の記録"
excerpt: "個人でReact勉強したときに理解したことを書き足すポスト"
toc: true
toc_sticky: true
toc_label: "React"

categories:
 - エンジニア勉強
tags:
  - react
  - 基礎
  - 初心者
last_modified_at: 2020-04-28T16:25:00
---

# Reactの目的は？
- 単一のJavaScriptライブラリ
- 画面に表示される有用なコンテンツ（HTML）を作る
- インタラクティブな処理をする

# reactでよく書いているclassは？
- JavaScriptのclass（ES2015）
  - アロー関数、インスタンス属性、class属性など
- ReactのComponentを使うことは、Reactの一番基本的な部分の1つ

# HTMLみたいに書いているものは？
- JSX
- HTMLではない
- React Componentがレンダリングされ、画面に表示されるときの形を指している
- でも、HTMLの書き方に近いところはある
※Reactを書く上でJSXの書き方を覚えるのは必要

# 画面内で動きを出したいときは？
- event Handler
- ユーザとアプリのインタラクティブなアクションを感知してアプリやComponentを更新したり、表示することができる

# reactとreact-domを別々で使う理由は？
- reactでは各Componentを定義するJavaScriptのコードが含まれている
- つまり、reactはライブラリ全体のプロセスがちゃんと動くことを担当する
- react-domはreactで書いたComponentをDOM内部に表したり、HTMLとして表したりする
- react、react-domはそれぞれ「Componentの定義に使う」「画面へComponentを表すために使う」

# create-react-appでReactのプロジェクトを立てる
## 大きな流れ
- Install / Update Node JS
- Install create-react-app
- Generate a project
- Build project

## 実行環境
- `node v12.14.1`
- `npm 6.14.4`
- `yarn 1.22.4`

## Reactのプロジェクト生成の方法
`npx create-react-app {appname}`
↓
もし、すでにグローバルでcreate-react-appがインストールされている場合は以下のコマンドを実行する必要がある
`npm uninstall -g create-react-app`
↓
Mac/Linuxの場合、以下のコマンドも実行する必要がある
`rm -rf /usr/local/bin/create-react-app`

## npxインストール時
npxインストール時に
`react@16.13.1`
`react-dom@16.13.1`
`react-scripts@3.4.1`
`cra-template@1.0.3`
がインストールされる

## Reactプロジェクト下で使うコマンド
Reactのプロジェクト内で以下のコマンドで実行が可能
`yarn start`
開発用のサーバを開始する。

`yarn build`
製品版のための静的ファイルをバンドル（1つのファイルに束ねる）する。

`yarn test`
テストランナーを起動する。

`yarn eject`
Removes this tool and copies build dependencies, configuration files
and scripts into the app directory. If you do this, you can’t go back!

## Reactプロジェクトをインストールしたときに注目しておく依存関係
### Babel
- ES5以降、毎年更新されているJavaScriptのバージョン（ES2018,ES2019...）があり、バージョンが上がるたびに良い機能が追加されていく
- できるだけ新しいバージョンを使いたいが、最終的にはユーザのブラウザでも実行されるように保証しなければならない
- しかし、ユーザが必ずしも最新のブラウザを使っているとは限らない
- ES5はすべてのブラウザでサポートされており、最新のJSのバージョンになるにつれ、ブラウザによっては保証されない可能性が高くなる
- つまり、ユーザに使ってもらうことを前提に作るためには後方互換性を意識しなければならない
- そこで、前のバージョンでも動作できるようにJSのソースコードをES5の書き方に変える必要がある
- その変換（トランスパイル）をやってくれるのがBabel
- ES5に書き換えると、前のバージョンでしか動作しないブラウザであってもJSの動きが保証されることになる
- そして、**create-react-appを実行するとBabelがデフォルトで含まれている!**

### webpack
- 複数のJavaScriptファイルを1つにして、圧縮する
- Component内で`import`するときに`.js`の指定をしなくても自動でファイルを探そうとすることができる

### Dev Server


## create-react-appフォルダーの中について
- src
  - 作成したソースコードを入れるところ
- public
  - 変更されない静的ファイルを保存するところ
- node_modules
  - プロジェクトの依存関係がすべて含まれているところ
  - 手動でnode_modulesをいじる必要はない
- package.json
  - プロジェクトの依存関係と構成を書くファイル
- package-lock.jsonもしくはyarn.lock
  - インストールされたパッケージの正確なバージョンが書かれているファイル

## JavaScriptのModuleシステムについて
- webpackのバンドルシステムを使う
- アプリケーションに必要なすべてのModuleをマッピングし、その結果物として1つ以上のバンドルを生成する
- Moduleの辞書的意味は**プログラムを機能ごとに分割した論理的な一部分**
- webpackにおいては**JavaScriptアプリケーションを構成する機能を分割した単位**で考えても良い
- import文を使うと他のファイルで定義したModuleをそのまま使うことができる
  ```
  // a.jsで定義
  export function sayHello(){
  console.log('Hello');
  }

  // index.js
  import { sayHello } from './a.js';
  sayHello();
  ```
- Module単位で書くことによって、別のところで書いたコードを他のコードとの依存性や衝突を気にせず、共有することも可能になった
[[Webpack] 사내 webpack 도입기 (2) - webpack 이란? - 오늘의 기억](https://memory.today/dev/22)
- ES2015以前のコードではCommonJS Modulesの書き方を見かけていたのかもしれない
- しかし、ES2015 Modulesを使うのであれば、importで十分
  ```
  // CommonJS Modules
  const React = require('react');

  // ES2015 Modules
  import React from 'react';
  ```
[import - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/import)

# componentについて
- componentはFunctionもしくはClassでできている
- componentはHTMLを生成してユーザに見せる
  - この時、書き方はJSXを使う
- componentはユーザのフィードバックを処理する
  - この時、Event Handlerを使う

# Function Componentについて
```
// こんな感じのComponent
const App = function () {
  return <div>Hi there!</div>
};

//アロー関数で書くとこんな感じ
const App = () => {
  return <div>Hi there!</div>
}
```

# componentをレンダーリングするためにやること
- reactで作ったページへアクセスするたびに`public/index.html`へ移動する
- reactで生成されたアプリケーションの規則として、idがrootになっているdiv内でComponentをレンダリングする
- [ReactDOM.render](https://ja.reactjs.org/docs/react-dom.html#render)の引数は以下のように設定する
  - 第1引数 : Reactコンポーネントへの参照
  - 第2引数 : React要素をレンダーするためのDOM

# JSXについて
- JSXはHTMLのように見えるが、**HTMLではない**
- 前提として、ES2015以降の形式で作成されたJSファイルはBabelを使うことでES5環境のブラウザでも動かすことができるようになること
- わたしたちのブラウザでは`<App />`のような書き方のタグが事前に用意されているわけではない
- それでもブラウザが認識してくれるのは、JSX形式で書いたコードがユーザのブラウザに送られたとき、正常に動作するJSとして変換されるから
- 正確には、JSXは見た目こそHTMLに近いものの、インポートしたReactライブラリ内の独自に定義された関数を呼び合うことでJavaScriptの制御をしている
  - その上、Babelを通すとES5環境のブラウザでも動かせるように変換してくれる
- まとめると
  - JSX形式で書くことはちょっと特殊なJavaScriptを書くこと
  - ブラウザはJSX形式で書いたコードを理解しているわけではなく、ブラウザで動かすときにJavaScript形式へ変わる
    - インポートしたReact,ReactDOMのライブラリ内でDOMを生成したり、イベントをハンドリングしたりする
  - その上、Babelを通すとES5環境のブラウザでも動かせるように変換してくれる
- [Babel公式ホームページ](https://babeljs.io/repl#?browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=Q&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=true&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015%2Creact%2Cstage-2&prettier=false&targets=&version=7.9.0&externalPlugins=)のTry it outからReactのコードがどうやってES5のJavaScriptへ変わるのかが試せる
- JSXは`React.createElement(component, props, ...children)`のsyntax sugar
- **子要素を持たない場合**、自己クローズ（self-closing）タグを利用することができる
  ```
  <div className="sidebar" />
  ```
- **子要素がある場合**、開始、終了タグを使う
  ```
  <Parent>
    <Child />
  </Parent>
  ```

# JSXとHTMLの違いについて
- 普通にHTMLの書き方でJSXを書くとエラーが発生する
- 理由は以下
  - JSX独自の属性がある
    - 要素にカスタムスタイルを追加すると違う文法になる
    - 要素にクラスを追加すると違う文法になる
  - JSXはJavaScriptの変数を参照できる

# Componentの3要素
- Componentの入れ子構造（Nesting）
  - Componentは別のComponentの内部に表示できる
  - `export`で他のComponentでも使えるようにできる

- Componentの再利用性（Reusability）
  - アプリケーション全体で簡単に再利用できるに作る

- Componentの構成（Configuration）
  - Componentが作成されたとき、構成できるものにする

# Component命名規則
- 基本的にパスカルケース（pascal case）で命名する
  - ファイル名、使用するComponent名の両方とも
  - ex)PascalCase、ComponentSystem

# カスタムしたComponentを他のComponentで使用するためにやっておくこと
- **使われる側のComponent**にexport処理を書く
- **使う側のComponent**にimport処理を書く

# ComponentをJSX構文でレンダリングするためには
```
# 基本的な型
<Component名 />
```

# 入れ子状態でComponentを渡す方法
- Component自体や使いたいデータを引き渡すときに使う
- 親Componentタグで子Componentを囲む
  ```
  <Parent>
    <Child />
  </Parent>
  ```
- 親Componentから子Componentを使う場合、`props.children`で子Componentを扱うことができる
  ```
  const Parent = (props) => {
    return (
      <div>props.children</div> //ChildのComponent自体
    );
  };
  ```
- 親Componentから子Componentの**プロパティ**を使う場合、`props.children.props`で使うことができる
  ```
  const Parent = (props) => {
    return (
      <div>props.children.props.参照したい値のキー</div>
    );
  };
  ```
[JSX を深く理解する – React#子要素としての JSX 要素](https://ja.reactjs.org/docs/jsx-in-depth.html#jsx-children)

[React基本(入れ子のコンポーネント) - Qiita](https://qiita.com/yousuke_asami/items/bd17e7b0f5667f9c4503)

# 関数ComponentとクラスComponentの違い
- 関数（Functional）Component
  - Good for simple content
  - 渡されたデータを元にJSXの生成をするような簡単な処理を書くときに使う
  - 複雑ではないロジックでComponentを作る時に使う
  - レンダリングされるデータを固定する
    - レンダリングされるまでデータを維持する
- クラス（Class）Component
  - Good for just about everything else
  - ネットワークの通信が必要だったり、データの加工が必要だったりする処理を書くときに使う
  - 簡単ではない、複雑なロジックでComponentを作る時に使う

# クラスComponentを使う利点
- （一般的に、）コードを構成することが簡単になる
  - ファイル内のコードを見て**何をするためのComponentなのか**がエンジニアの視点からして分かりやすくなる
- **state**が使える
  - インタラクティブなアプリケーションを作る上で必要な部分
  - ユーザの入力値を処理するときに使う
  - 非同期処理の機能が扱える
- **Life Cycle Event**を理解するにもメリットがある

# クラスComponentの原則
- JavaScriptのClassを使うこと
  - ES2015から導入されたあのClass
  - PrototypeというOOPで使われるイメーしするClassとは若干違う
    - でも、大体はOOPに近い
- `React.Component`を継承する
  - React.Componentを`サブクラス化`するとも言う
  - React.Componentの機能を借りる
- `render`メソッドを定義し、JSXでリターンする

# stateの原則
- クラスComponentでしか使えない
- propsとstateで混同するかもしれない:(
- 'State'はComponentに関連する一定のデータを含んだJavaScriptオブジェクト
- Componentのstateを更新すると、Componentがほぼ即座に再レンダリングされる
  - `setState`で`state`を更新すると`render`メソッドが再度呼び出される
  [state とライフサイクル – React](https://ja.reactjs.org/docs/state-and-lifecycle.html#adding-local-state-to-a-class)
  - Componentのリレンダリングをすることは、stateを更新することを意味する
- Componentが生成された時、stateを初期化する必要がある
  - constructorが使える
    - インスタンスが生成される度に呼ばれる
    - Componentのインスタンスが画面に表示されると最初に呼び出される
    - 必須ではないが、Componentの最初のレンダリングで、初期値の設定がしたい場合に使えば良い
    - `state`も初期値の設定はできる
    - `super`関数を使うことで親（`React.Component`）のconstructorに対する参照ができる
      [super - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/super)
      - `super`を使ってはじめて`this`が使える
        - もし、`super`をする前に`this`を使うと親クラスで定義されているはずのプロパティを使う事なんてできない
        ```javascript
        //親クラス
        class Parent {
          constructor(lastName) {
            this.lastName = lastName;
          }
        }

        //子クラス
        class Child extends Parent {
          constructor(lastName) {
            this.introduction(); // constructorで呼び出された時点ではthisが定義されていない
            super(lastName);
          }

          introduction() {
            return `My last name is ${this.lastName}.`;
          }
        }
        ```
        - このようなことが起きないようにするため、`super`で親クラスを呼び出し、`this`オブジェクトが使えるようにする
        [なぜsuper(props) を書くの? - React界のカリスマ「Dan Abramov」のブログ - Qiita](https://qiita.com/hand-dot/items/61a4b808f110b12e4281)
        [なぜsuper(props) を書くの? — Overreacted](https://overreacted.io/ja/why-do-we-write-super-props/)
      - これにより、propsに入っているデータをReactのComponent内で使うことができる
- stateが`setState`関数を使用してのみ更新できる
  - 例外があるとしたらconstructorの`this.state`で初期値を設定するときのみ

# propsの語源
- properties（特性、属性）の略
- propagation（伝播）の意味合いも間違ってはない

# propsの使い方
- immutable data（不変のデータ）
- can't change it（変更不可）
- passed in from parent（親から渡される）
- can be defaulted & validated（デフォルト値の設定と検証が可能）
- Componentを使う際に、内部で変更されないデータを保存するときに使う
- 2つのプロセスがある
  - 1つ目は、データをComponentへ提供する
  - 2つ目は、渡されたデータを使う
- 一般的に、親Componentから子Componentに渡される値
  - 子Componentをカスタマイズしたり、構成したりするのが目的
  - Componentの見た目、インタラクティブなアクションを設定する
  - 子Componentから親Componentにpropsを直接的に渡すことはできない
  - 慣習的に`props`で受け取る
- getDefaultPropsで定義されたデフォルト値か、親Componentから渡された値のどちらかをもつ
- クラスComponent内で扱うときは、`this.props`で書く
  [state とライフサイクル – React](https://ja.reactjs.org/docs/state-and-lifecycle.html)

# `props`と`this`の扱いの違い
- `props`は不変の値
- `this`は変更可能な値
  - `this`がクラスに存在する目的
  - `render`、ライフサイクルメソッドを呼び出した時、更新された値を読み込める
- 視覚的に見えるComponentがレンダリングされる時、`イベントハンドラー`も一緒に含まれる
  - つまり、イベントハンドラーが`あるpropsとstate`を持って`renderに属される`こと
  - だが、クラスComponentの場合、`this.props`がrenderに属されなくなる

[함수형 컴포넌트와 클래스, 어떤 차이가 존재할까? — Overreacted](https://overreacted.io/ko/how-are-function-components-different-from-classes/)

# stateの使い方
- mutable data（可変のデータ）
- can change it（変更可）
- maintained by component（コンポーネントによって保持）
- should ne considered private（プライベートであるべき）
- stateの変更は、仮想DOMとの差分から実際のDOMを更新してComponentを再描画するために使われる
- UIに関連した状態（pulldownの状態のような）、外部のデータに関連する状態（ログイン中のユーザ情報みたいな）になることがある
  - 外部からデータを取得してから表示するケースがこれに当たる
  - データを取得する前にすでにJSXでレンダリングされる画面から、それ以降に更新されたデータの結果を反映させないといけないときにstateが使われる
  - 非同期処理でデータをレスポンスするまで待つ処理時に使われる
  - リレンダリングするときに使われる
- Component内でthis.setState()を呼び出して状態を更新することができる

# Life Cycle Method
```
constructor
※1回限りのセットアップを行う
※公式、コミュニティの慣習として、データ読み込みの処理はここに書かず、componentDidMountに書くことを推奨する
↓
render
画面にコンテンツを表示する
※JSXを返す以外、何もしない
↓
componentDidMount
この関数は、Componentが最初に画面にレンダリングされるときに1回だけ自動的に呼び出される
※初期データ読み込みを行うときに使われる
※「初期データ読み込み処理をここでやる」という明快な役割が決められる
↓
componentDidUpdate
Componentが更新されるたびに自動的に呼び出される
※技術的に、componentDidUpdateの実行前にrenderが一度実行され、JSXを返して画面に表示する処理が走る
※state/propsが変わって、データ読み込みをするときに使われる
※ユーザからの入力値、外部からのデータ読み込みなどを含めたComponent内の更新があったらここでやる
↓
componentWillUnmount
Componentをそれ以上表示しない場合に呼び出される
※React以外のものを片付けるときに使われる

まれに使われるメソッド
shouldComponentUpdate
getDerivedStateFromProps(最初のマウント時、更新時にrenderされる直前に呼ばれる)
getSnapshotBeforeUpdate
```
[React.Component – React](https://ko.reactjs.org/docs/react-component.html)

## state初期化に代替できる書き方
- 普通に書くと、以下のようになる
  ```javascript
  class App extends React.component {
    constructor(props) {
      super(props);

      this.state = { 初期化したい内容 };
    }
  ︙
  }
  ```
- ただ、`Babel`がES5以下の環境でも動かせてくれるように変換することを利用し、`constructor`なしでも書ける
  ```javascript
  class App extends React.component {
    state = { 初期化したい内容 };
  ︙
  }
  ```
  - 良いところは、コードの量が減らせること

## stateを親Componentから子Componentにpropsとして渡す
- 親Componentで`this.state`に指定した値を子Componentに`prop`で渡す
  ```javascript
  //親Component
  ︙
  this.state = { key:value }
  ︙
  render(){
    return <Child passKey={this.state.key} />
  }

  //子Component
  const Child = (props) => {
    return (
      <div>props.passKey</div>
    )
  }
  ```

# イベントハンドラーの入力値を受け取る方法
- `onClick`、`onChange`、`onSubmit`
  - 引数は`event`のようにイベント発生による引数であることを明示的に表す
- 行いたい処理を書いた関数を`this`で指定してイベントハンドラーに渡せば変化がイベントの変化が起きたときにコールバックする
  ```javascript
  onInputChange(event) {
    ︙
    行いたいしたい処理
    ︙
  }

  render(){
    return(
      <div>
        <input onChange={this.onInputChange}>
      </div>
    )
  }
  ```
- 無名関数をワンライナーで書くこともできる
  ```javascript
  render(){
    return(
      <div>
        <input onChange={(event) => console.log(event.target.value)}>
      </div>
    )
  }
  ```

# レコードを表示する方法
- `map`関数で表示する

# 雑学
- node_modules配下にあるライブラリ構成を見るとcjsフォルダーで区切られていることがあるが、CommonJSの略のように見える
[.mjs とは何か、またはモジュールベース JS とエコシステムの今後 | blog.jxck.io](https://blog.jxck.io/entries/2017-08-15/universal-mjs-ecosystem.html)

- ESMはECMAScript Modulesの略
[Node.jsのECMAScript Modulesの紹介 - 技術探し](https://blog.hiroppy.me/entry/node-esm)

[複数のモジュール形式(CommonJS, ES Modules, UMD)をサポートしたnpmパッケージの作り方 in TypeScript - dackdive's blog](https://dackdive.hateblo.jp/entry/2019/09/23/100000)

[JavaScript モジュールの現状 | POSTD](https://postd.cc/the-state-of-javascript-modules/)

- webpackでnode_modulesを1つのファイルとする過程が気になる場合は読む
[Webpackでnode_modulesを1つのファイルにする – Mzlog 水島企画](https://lepetit-prince.net/ios/?p=7688)

# Tips
- HTMLの`class`や`for`はJSXでは`className`と`htmlFor`になる
  - 理由は、JSXでも同じ使われる予約語であるため
- JSX構文において中括弧で囲むセット数によって、表現することが違う。
  - 1セット : JavaScriptの記述ができる
    - 「JavaScriptの変数を参照する」という意味
    - 変数を直接指定したり、演算式を書いたりすることができる
    ```javascript
    const text = "Hello world";
    ReactDOM.render(
      <div>
        <h1> {text} </h1>
        {1+2+3}
      </div>
      ,document.getElementById('root')
    );
    ```
  - 2セット : オブジェクト技法でデータをセットできる
    - HTMLのように直接style属性を指定する場合は、`{{}}`(double curly braces)を使ってオブジェクト技法で指定する
    - つまり、**JavaScriptの記述** + **オブジェクトのセット**のように覚えればいい
    ```html
    <a style={{ color:'white' }}>Hello, world</a>
    ```
  - 3セット : HTMLエスケープをしないオブジェクト技法
    - 2セットではHTMLの特殊文字コードへ変換されてしまうHTMLエスケープが防げる
    ```
    //オブジェクト
    { "name": "<b>Hello</b>"}

    //2セット状態
    {{ name }}
    ↓
    &lt;b&gt;Hello&lt;/b&gt;

    //3セット状態
    {{{ name }}}
    ↓
    <b>Hello</b>
    ```
[mustache記法について簡単にまとめてみた - Qiita](https://qiita.com/sengok/items/1d958348215647a5eaf0)

[JSXの基本 – React入門 - to-R Media](https://www.to-r.net/media/react-tutorial04/)

[javascript-React의 JSX 구문에서 이중 중괄호의 목적은 무엇입니까? - 스택 오버플로](https://stackoverflow.com/questions/22671582/what-is-the-purpose-of-double-curly-braces-in-reacts-jsx-syntax)

- 要素のstyleをインラインで直接指定する場合、CSSの`ケバブケース`（単語をハイフンでつなぐ）を`キャメルケース`（単語の先頭が大文字）に変更する必要がある
  - style指定時、key:value形式のvalueの方は、必ず文字列にする
[React CSS](https://www.w3schools.com/react/react_css.asp)

- JSX構文においてのクォートの使い方は、JavaScript界隈の慣習（community convention）で以下のように使い分けているケースがある
  - シングル : JSX構文以外のプロパティ
  - ダブル : JSX構文のプロパティ

- JSX構文でJavaScriptオブジェクトを直接扱うことができない
  ```
  //エラーが発生する
  const App = () => {
    const buttonText = {text: 'Click me!'};

    return (
      <div>
        {buttonText} //代わりにbuttonText.textまで指定してすることでエラーが発生しない
      </div>
    );
  };
  ```
- JSXとHTMLの違い
  - インラインでスタイル指定する場合、mustacheでオブジェクト技法を使う
  - forとclassはJavaScriptでも使う予約語なのでhtmlForとclassNameに代わる
  - JavaScriptの変数が使える

- [Semantic UI CDN Link](<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.4.1/semantic.min.css">)
  - bootstrapに飽きたら、これもいい選択
  - cdnのいろんなバージョンがみたいときは[ここ](https://cdnjs.com/libraries/semantic-ui)を参考にする
  - reactプロジェクト内に含めたいなら[この方法](https://react.semantic-ui.com/usage)もあり
  - [公式のgit](https://github.com/Semantic-Org/Semantic-UI-React)
  - 導入の仕方の例
    - [React webでsematic-ui-reactを使い基本的なサイトを作成する – joppot](https://joppot.info/2018/07/27/4178)
    - [Semantic UI Reactを導入した | キクナントカドットコム](https://kikunantoka.com/2019/04/10--install-semantic-ui/)

- [Marak/faker.js: generate massive amounts of realistic fake data in Node.js and the browser](https://github.com/marak/Faker.js/)
  - ダミデータを使うときに良いライブラリ
  - [ReactでFaker.jsを利用してダミーデータを生成する – joppot](https://joppot.info/2018/11/09/4243)
    - 導入例

- JSXのベターな書き方について
  - [JSXが実はベターな解だったのではないか？｜erukiti｜note](https://note.com/erukiti/n/n6f673021469e)
  - [メンテナブルな React Component を目指すための小技集 - Qiita](https://qiita.com/terrierscript/items/372727448b58d4738018)

- VueとReactの比較記事
  - [私たちはなぜReactではなくVue.jsを選んだのか | POSTD](https://postd.cc/why-we-chose-vuejs-over-react/)
  - [他のフレームワークとの比較 — Vue.js](https://jp.vuejs.org/v2/guide/comparison.html)

- 韓国語のReact公式サイト
  - [React – 사용자 인터페이스를 만들기 위한 JavaScript 라이브러리](https://ko.reactjs.org/)

- 他のアプリを作るときにも参考にして使えるチャレンジ課題
  - ユーザの物理的な地理情報を取得する
  - 現在の日時年月を取得、表示
  - 地理情報と現在の日時}年月をもとにテキストとスタイリングを変える

- CSS指定時
  - スタイリングするComponentの一番ルートとなる要素にComponent名をケバブケース（Kebab Case）で書く
  - あくまで1つの手法
  ```javascript
  //Component
  const KamoEatGyoza = props => {
    return (
      <div className="kamo-eat-gyoza">
        <div>kamo</div>
        <div>gyoza</div>
      </div>
    );
  }
  ```

- 要素に直接直接スタイリングを指定する場合
  - styleのkeyをキャメルケースで記述
  ```javascript
  <div style]{{ textAlign: "center" }}>
  ```

- `props`にデフォルトの値を設定する方法
  - Componentを定義し、定義より下に設定する
  ```javascript
  // Component定義
  const Something = (props) => {
    return (
      <div>{props.text}</div>
    );
  }

  Something.defaultProps = {
    text: 'Loading...'
  }
  ```
[PropTypes を用いた型チェック – propsのデフォルト値](https://ja.reactjs.org/docs/typechecking-with-proptypes.html#default-prop-values)

- 要素の2つの属性
  - Controlled
    - `state`管理されている
    - Reactの世界の中でデータが管理されている
    - `rerender`されてもReactの管理下でデータが保持される
    - HTMLタグにReactで管理しているデータをオーバーライトする

  - UnControlled
    - `state`管理されていない
    - HTMLの世界のタグの中でデータが管理されている
    - `rerender`されたら消える可能性がある

- Virtual DOMによる`key`指定について
  - リスト表示の際に、その要素がユニークであることを示さないといけない
  - APIのresponseによっては固有のidも添えて送られる場合があるのでそれを使用する
  - returnさせる時、個々の要素の一番ルートにidを指定しておく必要がある

- React Refs
  - 単一のDOM要素へのアクセスを提供する
  - コンストラクターで参照を作成し、インスタンス変数に割り当て、特定のJSX要素にプロパティとして渡す
  - 一個一個の独立した要素に対して個別の設定や処理をさせたいときに使う
  ```javascript
  class A extends React.Component {
    constructor(props) {
      super(props);

      this.Ref = React.createRef();
    }

    render() {
      return (
        <div>
          <img ref={this.Ref} />
        </div>
      );
    }
  }
  ```
  [Ref と DOM – React](https://ja.reactjs.org/docs/refs-and-the-dom.html)

- `props`/`state`/`Component`概念
[Reactの基礎(JSX, ライフサイクル, state, props) - わくわくBank](https://www.wakuwakubank.com/posts/701-react-basic/)

[컴포넌트에 함수 전달하기 – React](https://ko.reactjs.org/docs/faq-functions.html)

[React 최상위 API – React](https://ko.reactjs.org/docs/react-api.html#reactfragment)

- `props.children`について
  - 子要素に対して共通の設定を指定する場合は、`props`
  - 子要素に対してそれぞれ違う設定を指定する場合は、`props.children`
[Reactで複数のchildrenを扱いたい場合の実装パターン - Qiita](https://qiita.com/terrierscript/items/3afee284621bbe126332)

[props.childrenを渡す方法 - Qiita](https://qiita.com/makkkiy/items/30dd32b815c63efdb64d)

[Reactのchildren探訪 - Qiita](https://qiita.com/jkr_2255/items/2e40f7c9ecf300e8d945)

[コンポジション vs 継承 – React](https://ja.reactjs.org/docs/composition-vs-inheritance.html)

# styled-components
[styled-componentsを使ったCSS設計 - Qiita](https://qiita.com/taneba/items/4547830b461d11a69a20)

[styled-components: Basics](https://styled-components.com/docs/basics)
